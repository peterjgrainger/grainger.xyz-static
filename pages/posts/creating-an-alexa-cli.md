---
  
  title: How to make an Amazon Alexa skill without the annoying Skills Kit UI.
  date: 2018-03-08
---
# Problem
Alexa skills are pretty fun to create, but I don't like how messy it is.  There is no really easy way to test the skill without using a test harness to simulate an event and lambda's seem too much like magic 🧙‍♀️. 

My other problem is the UI to create an skill.  They've put a new front end on recently but I'm not sure how many developers they have consulted about it.  Most people I know use shortcuts wherever they can.  The UI is good for beginners but once you have made a few it gets to repetitive and copy and pasty.

Also to make a skill in all the English speaking languages requires loads of manual process :( 

**Also** it's stressful to make sure that all the slots are correct on the custom skill and on the definition in the UI. 

# Goal
* To encapsulate the parts of the process that are mostly the same and automate. 
* Include a testing framework that gives you a high level of confidence in all scenarios of the skill
* No interaction required with the alexa skills kit UI. 

## How to get there

There are 2 parts to this.  One creating and deploying the endpoint Alexa will talk to and two setting up the Alexa skills kit config that will be downloaded to the user.

## Tools we will be using.

* [Alexa App](https://www.npmjs.com/package/alexa-app) -
 an abstraction to create valid JSON responses alexa will understand
* [ASK (Alexa Skills kit) CLI](https://developer.amazon.com/docs/smapi/ask-cli-command-reference.html#create-skill-subcommand) - to create, deploy and submit the skill for certification instead of using the alexa skills kit UI
* [Now](https://now.sh/) - server to deploy the logic for our app.

## Creating a repeatable process

For this to work everything has to be automated with ideally zero configuration to get it up and running.  The first step is to create a boilerplate to base all the common code.

### The code
My first go at the code is here: https://github.com/peterjgrainger/alexa-skill-boilerplate


### How to get it.

On my TODO list is to write some more documentation on how to use this thing but until then:

##### Make a new repo to store your skill

Go to [github](www.github.com) sign up and [create a new repo](https://help.github.com/articles/create-a-repo/)

Lets assume you've called your repo `awesome-skill`

##### Copy the repo

Make an [exact copy](https://help.github.com/articles/duplicating-a-repository/) of the repo https://github.com/peterjgrainger/alexa-skill-boilerplate

#### Install dependencies

navigate to the folder and run npm to install dependences

```
cd awesome-skill
npm install
```
This will take a while, get some ☕️

### How to use it

Any kind of skill can be created with this method but the example that comes with the kit is a custom skill, so we can start with that one.

#### Attempt to deploy the example skill.

To make sure all went well deploy this hello world skill

##### Log in to AWS and now

You will need to log into amazon using your normal details (if you have already registered, otherwise you will have to sign up)

Now requires an email address for the account, you will need to verify the address before continuing.

```
npm run init
```

##### Deploy

Deploy to AWS developer account and now account.

```
npm run deploy
```

##### Manually test

Open the testing page

```
npm run ask:testing
```

Ask alexa `Alexa, ask hello world I would like to say hello to the world and bob` alexa should respond `Hello to you bob`

##### Done

That's it.  You've successfully created an alexa skill.  See below for how to make it your own.

#### Make your own skill.

There are some key parts to the skill that need updated.  

##### **WARNING**

Using this method creates an app deployed on [now.sh](now.sh).  After 30 mins of no requests it will freeze and go to sleep.  To avoid this happening you can set up an automated ping request in [new relic](https://newrelic.com/).  It's in the synthetics section.

##### Images

An image to sum up your skill.  The image gets people using your app, no matter how good your skill is if your image is crap no one will use it.

* Big image - must be size 512x512 and be named big-image.png
* small image - must be size 108x108 and be named small-image.png

put both of these files in the project under the folder `img`

##### Publishing Information

Information that is displayed on the skill marketplace.  This details what the skill is and how to use it.  Should all be pretty self explanatory by the names.  Make sure the app name is lowercase with no special characters. 

##### now.json

Pick a unique name for the app.  This ensures a fixed address to access the skill once it has gone live.  Set `name` and `alias` to the same value e.g. `my-awesome-unique-name`.  Your skill will go live with the address `my-awesome-unique-name.now.sh`

##### package.json

Not required, but it's good practice to update the `name` property

##### Skill Definition

This is the fun part.  Creating the logic for the skill that responds to requests.

###### Launch

Alter the response to a friendly welcome

###### intents

[Built in intents](https://developer.amazon.com/docs/custom-skills/implement-the-built-in-intents.html) and [custom intents](https://developer.amazon.com/docs/custom-skills/best-practices-for-sample-utterances-and-custom-slot-type-values.html).  There is an example in the `src/intents/hello-world` for a custom type.  See `src/intents/stop` for an example of a built in type.  See [alexa app](https://www.npmjs.com/package/alexa-app) documentation for more information on how to write your intents.

###### slots

There are several built in [slot types](https://developer.amazon.com/docs/custom-skills/slot-type-reference.html).  See the stop intent `src/skill-definition/intents/stop` for details on how to use a built in amazon slot type and  `src/skill-definition/intents/hello-world` for how to use an custom slot type.  Custom slots are `src`

###### alexa app

See `src/skill-definition/models/slots/slot-types` for any custom slot types.  Make sure any new types are of the same format.  If you have no custom slots remove all the types. 