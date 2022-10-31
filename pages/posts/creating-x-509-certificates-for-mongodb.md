---
  
  title: Creating X.509 certificates for MongoDB
  date: 2017-08-31
---
  
## Overview
After all the fun make it work bit is over now comes the bit where you have to think about architecture / security :(

I'm not going to go into the full explanation of how X.509 certificates actually work with mongo.  But rest assured it is the safest way to communicate to the database and you don't have to store passwords anywhere!!!  If you want to learn more about this pattern see the [mongo docs](https://docs.mongodb.com/manual/core/security-transport-encryption/)

### Prerequisites.

* mongo v3.2+
* openssl
* linux or mac (windows might work but I haven't tried it)

### Using Docker (Optional)

If you don't want to mess up your local config and have [Docker for Mac](https://www.docker.com/docker-mac) or [Docker for Windows](https://docs.docker.com/docker-for-windows/) installed I suggested running it in there, then you can delete when finished (or keep it :))

```
docker run -i -t mongo:3.4 /bin/bash
```

### Start the database
Either through docker or your install on your own system start the database with no security.  This step will allow you to add the user and the role.

``` 
mongod --fork --syslog
```

If you get the output ```child process started successfully, parent exiting``` it's working.

### Creating a read / write user
For this user make sure they have read, write and update access but only for the database and collection that it needs.  From Mongo's own words on security.

> Create roles that define the exact access a set of users needs. Follow a principle of least privilege. Then create users and assign them only the roles they need to perform their operations. A user can be a person or a client application.

I'm going to use a database named ```Users``` that will hold multiple user records in a ```records``` collection.  The user will be able to add/update/read a record.

#### Login to the Users database

```javascript
mongo Users
```

#### Create the `readWriteRole` role

```javascript
 db.createRole({
    role: 'readWriteRole',
    privileges: [{
        resource: {
            db: 'Users',
            collection: 'records'
        },
        actions: ['find', 'update', 'insert']
    }],
    roles: []
});
```

#### Create the user

Now that we have a role we can create the user with this role.  As this user will be linked to the X.509 SSL certificate the user will be the same as the user we will put in our certificate. 

* CN - common name (in our case the user)
* OU - organisation unit 
* O - organisation
* L - location
* ST - state
* C - Country

```javascript
db.getSiblingDB('$external').runCommand({ createUser: "C=GB,ST=Greater London,L=London,O=Dev Team,OU=Widgets Inc,CN=readWrite", roles:[{role: 'readWriteRole', db: 'Users'}] });
```

Exit out of mongo `exit` and go back to your shell prompt.

### Certificates

To create a valid x.509 certificate you need to sign with a certificate authority.  If you are just choosing a certificate authority I would recommend [let's encrypt](https://letsencrypt.org/), it's open and free!  Otherwise like I had to, you can have self signed certificates.

#### CA certificate

This will create a CA certificate that will be used to sign the key the user is going to use.  (You can use a proper CA if you want instead of this one)

```shell
openssl req -passout pass:password -new -x509 -days 3650 -extensions v3_ca -keyout $CERTS_DIR/ca_private.pem -out $CERTS_DIR/ca.pem -subj "/CN=CA/OU=Widgets Inc/O=Dev Team/L=London/ST=Greater London/C=GB"
```

#### User certificate
First create the CSR file
```javascript
openssl req -newkey rsa:2048 -nodes -out read-write.csr -keyout read-write.key -subj '/CN=readWrite/OU=Widgets Inc/O=Dev Team/L=London/ST=Greater London/C=GB' 

```
Using the ca file in the previous step sign the key and write out the crt file 

```
openssl x509 -passin pass:password -sha256 -req -days 365 -in read-write.csr -signkey read-write.key -CA ca.pem -CAkey ca_private.pem -CAcreateserial -out read-write.crt 
```
Now combine that into one key file.

```
cat read-write.crt read-write.key > read-write.pem
```

#### Database certificate
Using the same procedure as above create a key signed by the same CA to start the database

```javascript
openssl req -newkey rsa:2048 -nodes -out db.csr -keyout db.key -subj '/CN=localhost/OU=webmaster/O=Dev Team/L=London/ST=Greater London/C=GB' 

```
```
openssl x509 -passin pass:password -sha256 -req -days 365 -in db.csr -signkey db.key -CA ca.pem -CAkey ca_private.pem -CAcreateserial -out db.crt 
```

```
cat db.crt db.key > db.pem
```


#### Shutdown the server
We now need to stop the server to start it back up in SSL and authentication mode.

```
mongo admin
db.shutdownServer()
exit
```

#### Start the server with SSL and authentication enabled 

Create a mongo.conf file to specify that we require SSL to connect.

`mongod.conf`

```
net:
  port: 27018 
  bindIp: 127.0.0.1
  ssl:
      mode: requireSSL
      auth: true
      PEMKeyFile: ./db.pem
      PEMKeyPassword: password
      CAFile: ./ca.pem
```

You can create this file on the server using the command

```
echo -e "net:\n    ssl:\n        mode: requireSSL\n        PEMKeyFile: ./db.pem\n        PEMKeyPassword: password\n        CAFile: ./ca.pem" > mongod.conf
```

Now start mongo using the config file.

```
mongod --config ./mongod.conf --fork --syslog --auth
```

### Log into the database

You can now log in using your newly created keys.  We can check the security is there by trying to run the `mongo` command without any authorisation.  The output should be (among other messages):

```
mongo

AssertionException handling request, closing client connection: 17189 The server is configured to only allow SSL connections
```

Now try to log in to the Users database with the User key
```
mongo --ssl --sslPEMKeyFile read-write.pem --sslCAFile ca.pem --authenticationMechanism MONGODB-X509 --authenticationDatabase='$external' --host localhost -u "C=GB,ST=Greater London,L=London,O=Dev Team,OU=Widgets Inc,CN=readWrite" Users

```

### Test functionality

Insert a record
```
db.records.insert({lookup: 23}) // output WriteResult({ "nInserted" : 1 })
```

Find a record
```
db.records.find({lookup: 23}) // output { "_id" : ObjectId("59a2638021fcdb790e220cd1"), "lookup" : 23 }
```

Update a record
```
db.records.update({lookup:23}, {lookup: 24}) // output WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })
```

Negative test: look for a record in a collection you don't have access to
```
db.wrongCollection.find() // output Error: error: {
        "ok" : 0,
        "errmsg" : "not authorized on Users to execute command { find: \"wrongCollection\", filter: {} }",
        "code" : 13,
        "codeName" : "Unauthorized"
}
```

### Enhancements

There are a few other users to consider before deploying this.

#### Other users
* Admin user - Using the built in [userAdminAnyDatabase](https://docs.mongodb.com/manual/reference/built-in-roles/#userAdminAnyDatabase) to allow an admin commands including creating new users.
* Backup user - Using the built in [backup role](https://docs.mongodb.com/manual/reference/built-in-roles/#backup)
* restore role - Using the built in [restore role](https://docs.mongodb.com/manual/reference/built-in-roles/#restore)

I normally have one role per application using the database to make sure there is no accidental changes made by each separate application.

#### Vault
Automatically renew keys and store the cert passwords and private CA so only the app can use a key not a user (i.e. you). 
 See [Vault TLS docs](https://www.vaultproject.io/docs/auth/cert.html)


