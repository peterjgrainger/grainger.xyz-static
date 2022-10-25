---
  type: posts
  title: Using self hosted gitlab npm modules in Docker container
  date: 2018-01-31
---
  
### Intro

I've been containerising all my projects at work recently to make the builds more consistent and ultimately to allow us to use those builds for production.  I recently ran into the issue on our on premise installation of gitlab of how to pull git repo's as npm modules from our protected internal gitlab during continuous integration (CI) builds when using docker.

So in my package.json I had
```
    "awesome-project": "git+ssh://git@gitlab.itsshared.net:some-group/awesome-project.git#1.1.0",

```
Which works when ran via the terminal on my machine but when running in a Docker, the ssh keys from my machine aren't there...

#### Local builds

You have two options for local builds, for dev etc

###### Copy the ssh keys over (multi-stage builds)

I go over this in [a previous post](https://www.grainger.xyz/using-ssh-keys-in-docker/) on how to do this securely.

###### Use an access token

This requires a bit of setup for each person that wants to use it but if your aim is to build the same container locally without too many changes then this is your best bet.

In your Dockerfile just before running npm install run the command

```shell
RUN sed -i 's/git+ssh:\/\/git@gitlab.itsshared.net:/git+https:\/\/oauth2:'$BUILD_TOKEN'@gitlab.itsshared.net\//' ./package.json
```

This command will search and replace and reference to gitlab and change your npm modules to be downloaded using your token rather than your ssh keys.  Your package.json will now look like this:

```shell
"awesome-project": "git+https://oauth2:<token-value>@gitlab.itsshared.net:some-group/awesome-project.git#1.1.0"
```

Two changes here:

* The method of transfer has been changed from `git` to `git+https`
* The user has been changed from `git` to `oauth2:<token-value>` where `<token-value>` is the access token created in gitlab.


You will also now need to run the build step with the `--build-arg` option like so:

```shell
docker build --build-arg BUILD_TOKEN=<your access token> .
```

### CI build

We can't do exactly the same in the CI build as it's not best practice to use your personal access token in CI builds.

Instead of using `oauth2:'$BUILD_TOKEN'` as the user to connect to gitlab in the `package.json` we use a special user named `gitlab-ci-token` with the job token `$CI_JOB_TOKEN`

So in your `Dockerfile` before the npm install add the following:

```shell
RUN sed -i 's/git+ssh:\/\/git@gitlab.itsshared.net:/git+https:\/\/gitlab-ci-token:'$BUILD_TOKEN'@gitlab.itsshared.net\//' ./package.json
```

and in your `gitlab-ci.yml` add the build arg to pass through the job token.

```shell
docker build --build-arg BUILD_TOKEN=$CI_JOB_TOKEN -t my-app .
```

And you're done!  Now to make everything into a module!