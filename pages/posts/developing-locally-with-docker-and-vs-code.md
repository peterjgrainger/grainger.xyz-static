---
  type: posts
  title: Developing locally with Docker and VS code
  date: 2018-07-22
---
  
Coding is fun, debugging issues on other team members machines isn't.  It works on my machine doesn't cut it anymore!

This post outlines how to run your code in a container rather than directly on your machine including how to debug and run tests, why you should do it and what the trade offs are.

![works on my machine](https://imgs.xkcd.com/comics/inexplicable.png "XKCD comic")

## It works on my machine

It's a bit of a joke between old school developers from the days where sharing code involved passing physical media between each other.   But even today with all the fancy CI/CD it costs serious money and causes massive frustration.  If you have ever worked on a team where you rely on code created by a team member, there is at least one time during the project that you just can't get it to work.  You call the person who developed it across, try everything you can think of, delete everything, reinstall, do it again, then you both sit with blank expressions staring at the screen slowly loosing the will to live.

### How do containers solve this problem?

Like most software solutions this problem was modeled against one that was solved decades ago in the 1950's.  Before the 50's most cargo ships were transporting entire lorries including the trailer and the cab.  Then [Malcolm McLean](https://en.wikipedia.org/wiki/Malcom_McLean) came along.  He owned a lorry company and saw what a massive waste of space this system of transporting the whole vehicle, so he made detachable containers that fit both on the trailers of the lorries and in a specially designed cargo ship and container ships as we know it were born.

![some containers](https://res.cloudinary.com/di4veouyn/image/upload/c_scale,e_auto_brightness,q_46:420,w_914/v1533371379/blog/frank-mckenna-252014-unsplash.webp "some containers")



If we translate this back to software, containers are self contained processes on your machine that share some of your underlying OS and add some additional functionality.  All the isolation of a VM without the duplication of an operating system.  This isn't a new concept and can be traced back to [Unix V6](https://en.wikipedia.org/wiki/Version_6_Unix) but it took until 2013 for containers to become easy for the likes of us mortals to use.

![vms vs containers](https://i.stack.imgur.com/exIhw.png "VMs vs Containers")

So, back to using the things.

### Why you would want to do this.

- Versions of any runtimes (e.g. node) are synchronized across the team.
- All dependencies managed by a dependency manager (e.g. npm) will be the same version and compiled on the same OS
- Code can use built-in optimized OS specific functionality like `find` or `grep` without having to build in a shim for different operating systems.
- It is easier to deploy to production later either using a container orchestration systems or use the image definition to build up the bare metal.
- Have the ability to share these pre-built images between developers
- Easier to create a releasable production product without any more configuration, testing 
- All local settings are now in code, environment variables, arguments, port numbers etc.

### The trade offs

- Some complexity to begin with
- Adding additional maintenance
- Requires staff to have additional skills / knowledge of linux / chosen OS (could be also a benefit to upskill?).
- Probably overkill if you are on a team of one or prototyping and not relevant to single page apps

## Now the fun bit.

To achieve this nirvana we will be using [docker](https://www.docker.com/). You can download this for Mac, Windows or Linux.  Just have a look at the site. 

We will also be using [nodejs](https://nodejs.org/en/) and [vscode](https://code.visualstudio.com/), and [git](https://git-scm.com/) to get the source code  You can use any language, OS, editor or ide you feel comfortable with, but I feel comfortable with linux, node and vscode.


### Setting up your development environment

The best way to learn this stuff is just to get stuck in and try an break things.  I've created a repo with a simple static page that is returned.  You can expand to whatever the new hotness is.

I'll be going through an example code base that I prepared earlier with all of this set up, for the first bit all you need to install is:

- [docker](https://www.docker.com/)
- [git](https://git-scm.com/)

### Get the code

Open a terminal and clone the code locally
```bash
git clone git@github.com:peterjgrainger/development-with-docker.git
```

### Start the container

Open a new terminal, this will display the logs of the running node process.
```bash
cd development-with-docker
docker-compose up
```

This might take a while ☕️

### Done!

Navigate to http://localhost:3000 to see your new beautiful site.

![site display](http://res.cloudinary.com/di4veouyn/image/upload/c_scale,f_auto,w_681/v1533375028/blog/Screen_Shot_2018-08-04_at_10.29.04.png "site display")

### Debugging and editing

For debugging you need to install an vscode, you could also use the node debugger in chrome.  Installing node if you don't already have it also quiets the vscode intellisense

#### install dependencies

Open a new terminal and navigate to the code we downloaded in the previous steps

```bash
cd development-with-docker
npm install
```

#### Open vscode

Open vscode with the command line tools.

```bash
code .
```

#### Go to the debug tab

Go to the Debug tab (shortcut on mac: <kbd>⇧</kbd><kbd>⌘</kbd><kbd>D</kbd>)
![debug thumb](http://res.cloudinary.com/di4veouyn/image/upload/t_media_lib_thumb/v1533374524/blog/Screen_Shot_2018-08-04_at_10.21.22.png "debug")

#### Attach to the running docker 
Select `attach to Docker` from the dropdown and press <kbd>F5</kbd> or the ▶️ button beside the dropdown

![debug tab](https://msdnshared.blob.core.windows.net/media/2017/09/ps1.png "debug tab")

#### Go back to the code view
Navigate to the `src/index.js` file (file finder shortcut: <kbd>⌘</kbd><kbd>p</kbd)

Click in the left margin to add a breakpoint on the line 15

Refresh http://localhost:3000 

You should be automatically focused on that line in vscode.  You are now debugging!

Change the code and refresh http://localhost:3000 to see your changes.

### What about testing.

You can do that too!  Open a new terminal and run the tests inside the container.

```bash
docker exec site npm test
```

You should get the output
```bash
> ava tests/*.js


  ✔ foo
  ✔ bar

  2 tests passed
```

## Developing with docker

I hope this gives you an idea of the pros and cons of using Docker for your development environment.  It's a bit of pain upfront but is worth it in the end.  

If you have any issues with the instructions or need any help with a more complicated setup use the comments or get in touch on twitter [@peterjgrainger](https://twitter.com/peterjgrainger)



