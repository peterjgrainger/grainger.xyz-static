---
  
  title: Development Testing Nuance Dragon Naturally Speaking on mac
  tag: olly web development
  date: 2017-10-20
---
  
If you are looking for somewhere on how to explain how to test the Nuance product Dragon Naturally Speaking then you are in the right place.

### Setup
There is a mac version of Dragon, but it isn't anywhere near as good as the Windows version.  First we need to set up a VM with windows and then install a trial version of dragon to test it out.

### Creating a virtual Windows environment
Microsoft offers [free virtual VMs](https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/) from Windows 7 to 10 for testing.  You will also need [virtual box](https://www.virtualbox.org/wiki/Downloads) to run the downloaded VMs.

### Installing Dragon
At the minute I can only find version 13 for free on [this site](http://shop.nuance.co.uk/store/nuanceeu/en_GB/Content/pbPage.201508_dnspremium13trial_uk)

### Put everything together
You need to put the downloaded file somewhere the VM can reach it.  You can see where this is setup on your machine from the properties window in virtualbox 
![virtualbox properties](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2017/10/Screen+Shot+2017-10-08+at+06.41.51.png)

### Local testing
If you want to test something running locally on your mac then in the VM add `10.0.2.2 localhost` to your host file `C:\windows\system32\drivers\etc\hosts`

You should be able to access `localhost:<port>` now through the VM
![edit host file](https://s3.eu-west-2.amazonaws.com/ghost-blog-grainger/2017/10/Screen+Shot+2017-10-20+at+11.09.51.png)


### Install
Start up the windows VM.  Run the downloaded dragon .exe file and follow the instructions.  It might take a while to download.

### Test!
You should end up with a running version of Windows and Dragon for 30 days. 
