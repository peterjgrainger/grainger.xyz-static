---
  
  title: Backup and restore MongoDB with X.509 authorisation
  date: 2019-08-14
---
  
### Background
Before I start I just want to say the [mongo docs](https://docs.mongodb.com/) are great, really detailed but they can't cover every scenario (i.e. my scenario).

### Securing a database with X.509 certificates
There is [great documentation](https://docs.mongodb.com/manual/core/security-x.509/) on this! Without (too) many issues I was able to follow the many blogs out there of people doing just this, connecting from the client etc.  However I came against an issue finding any documentation on backing up and restoring a database with this security, no joy.

### A bit about the X.509 keys
I'm not going to go into all the details of how I made these keys.  If you haven't already get X.509 keys, read my post [Creating x.509 certificates for mongodb](https://www.grainger.xyz/creating-x-509-certificates-for-mongodb/).

* dba.pem (database admin)
* persistence.pem (read/write user)
* ca.pem (certificate authority used to sign the keys)

### My users and roles
I started out with 2 users:  

* A the Mongo built in DBA role: ```userAdminAnyDatabase``` for the ```admin``` db
* A read/write user for the collections to be used by my app (I made a role for this with access to relevant collections)

### What did work
I was able to log in using the ```mongo``` command line utility included with the mongo installation with this command.

```
mongo --ssl --sslPEMKeyFile /etc/ssl/certs/persistence.pem --sslCAFile /etc/ssl/certs/ca.pem --authenticationMechanism MONGODB-X509 --authenticationDatabase='$external' --host persistence-mongo
```

- ```/etc/ssl/certs/persistence.pem``` is the location of my X.509 file for the read/write user
- ```/etc/ssl/certs/ca.pem``` is the location of my ca file
- ```persistence-mongo``` is the hostname of the server running mongo (if running locally this will be localhost)

It works!!!! 😺  I can now log in and change things.  Now on to the backup and restore.

## Backup 
This is where things got tricky...

I used the same flags as before and used my database admin key instead of the read/write user key.

```
mongodump --ssl --sslPEMKeyFile /etc/ssl/certs/dba.pem --sslCAFile /etc/ssl/certs/ca.pem --authenticationMechanism MONGODB-X509 --authenticationDatabase='$external' --host persistence-mongo
```

And received this helpful error

```
Failed: error getting database names: not authorised on admin to execute command { listDatabases: 1 }
```

### Next failure
After much frustrated clicking around I finally just asked and surprisingly they had the answer.  The [--username or -u](https://docs.mongodb.com/manual/reference/program/mongo/#cmdoption-username) flag.  

```
mongodump --ssl --sslPEMKeyFile /etc/ssl/certs/dba.pem --sslCAFile /etc/ssl/certs/ca.pem --authenticationMechanism MONGODB-X509 --authenticationDatabase='$external' --host persistence-mongo --out dump -u "CN=dba,OU=MY AWESOME OU,O=MY ORGANISATION,L=TIMBUKTU,ST=Mali,C=AF"

```
And the return message was again 

```
Failed: error getting database names: not authorized on admin to execute command { listDatabases: 1 }
```
Still no joy, then I stumbled on this [post](https://stackoverflow.com/questions/23943651/mongodb-admin-user-not-authorized).  So maybe my issue now was nothing to do with the X.509 keys, but with the role my user had.

### Last hurdle 
So now all I had to do was add a role that can back things up and restore.  I *could* just change my admin user to have the built in role ```root```, however this is pretty bad practice.  Mongo have helpfully added a role for this, [Backup and restore roles](https://docs.mongodb.com/manual/reference/built-in-roles/#backup-and-restoration-roles).  Add these roles, with the corresponding X.509 keys (backup.pem/restore.pem), change the command to use these new users and...

```
mongodump --ssl --sslPEMKeyFile /etc/ssl/certs/backup.pem --sslCAFile /etc/ssl/certs/ca.pem --authenticationMechanism MONGODB-X509 --authenticationDatabase='$external' --host persistence-mongo --out dump -u "CN=backup,OU=DNG-CLIENT,O=DWP,L=Newcastle upon Tyne,ST=Tyne and Wear,C=GB"
```

No errors, great success!!!

 



  