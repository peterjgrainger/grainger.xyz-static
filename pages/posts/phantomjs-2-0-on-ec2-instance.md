---
  type: posts
  title: Phantomjs 2.0 on EC2 instance
  date: 2017-08-22
---
  
## What’s this all about?

I’ve been working on a project that uses phantomjs v1.9.x to output a PDF report. Because of the seemingly unrelated vunerabilities to SSL due to the POODLE attacks attacking a vunerability in TLS v1.0-1.2 meaning I needed to upgrade to TLS v2.0 if I still want to use a secure https connection… which isn’t supported in phantomjs v1.9.x

## So, what does that all mean?

That means that phantomjs stops working if you upgrade your https certificate! There is some hope though with phantomjs v2.0, this new release uses the new QT and webkit library (which does support the new encryption). Phantomjs 2.0 binary is available to download for windows and mac, but no Linux! As we are running on AWS EC2 instances and using the default AMI based on redhat there is no binary already compiled…

## What? So what happens now?

You can download the source and follow the build instructions to build on your version of linux. I’ve compiled a binary to work on the amazon AMI for your convenience you can download here this should save you pretty much a day compiling, see here for the link: https://github.com/TheSmokingGnu/compiledPhantomjsBin
