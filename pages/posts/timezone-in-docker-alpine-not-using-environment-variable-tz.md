---
  
  title: Timezone in Docker Alpine not using environment variable TZ
  date: 2018-02-09
---
  
I've started using alpine images in all my Docker containers, there are many blogs on why this is a good idea, lower container size (~5MB) which means faster to download and safer as there is less to be exploited.

## Timezones

Dates are the bane of many a developer and javascript doesn't do them to well.  Most languages allow you to set a timezone when creating dates but not Javascript it seems.

I didn't notice this problem until I started using Docker to run my node apps.  Node will take the timezone of a running server and use your timezone when `new Date()` function is called.

``` 
docker run -it node /bin/bash
root@345245d:/# date // output Mon Sep  4 14:39:40 BST 2017
```

## node:latest (base: Ubuntu)

At first I was using the [Official Node image](https://hub.docker.com/_/node/) which has an Ubuntu base image.  I originally had an issue where Dates where created in UTC but adding the environment variable `TZ: 'Europe/London'` solved that. 

```
docker run -it -e "TZ=Europe/London" node /bin/bash
root@34543fs4:/# date // output Mon Sep  4 14:39:40 BST 2017
```

## node:alpine

I changed all my images to `node:alpine` and at first I didn't notice any issues, but on further integration testing my validation on micro-services were failing when checking the current date is after a stored one.  The client was sending the correct value, so when I looked in the logs of the server the dates were set to `GMT`, not `BST` (GMT+1).

```
docker run -it -e "TZ=Europe/London" node:alpine /bin/bash
root@34543fs4:/# date // output Mon Sep  4 13:39:40 GMT 2017 WHAT?!?!
```

From what I understand this isn't a "bug" with alpine but just not included in the base image to handle timezones.  See ["bug" details](https://bugs.alpinelinux.org/issues/5543).  The fix helpfully supplied by [Christian Kampka](https://bugs.alpinelinux.org/users/1105) is to install the package [tzdata](https://pkgs.alpinelinux.org/package/edge/main/x86/tzdata) (at 3.34MB nearly as big as Alpine OS!) then node will pick up the timestamp setting.

### New Dockerfile

To add the new package use `apk add`

```
FROM node:alpine
RUN apk add tzdata
```

Now when I use this Dockerfile with the env I get the correct timestamp

```
docker run -it -e "TZ=Europe/London" . /bin/bash
root@34543fs4:/# date // output Mon Sep  4 14:39:40 BST 2017 yey!
```

