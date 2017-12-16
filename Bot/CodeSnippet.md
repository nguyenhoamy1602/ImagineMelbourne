<a name="HOLTitle"></a>
# Building Intelligent Bots with the Microsoft Bot Framework #

---

<a name="Overview"></a>
## Overview ##

Software bots are everywhere. You probably interact with them every day without realizing it. Bots, especially chat and messenger bots, are changing the way we interact with businesses, communities, and even each other. Thanks to light-speed advances in artificial intelligence (AI) and the ready availability of AI services, bots are not only becoming more advanced and personalized, but also more accessible to developers. 

Regardless of the target language or platform, developers building bots face the same challenges. Bots must be able process input and output intelligently. Bots need to be responsive, scalable, and extensible. They need to work cross-platform, and they need to interact with users in a conversational manner and in the language the user chooses.

The [Microsoft Bot Framework](https://dev.botframework.com/), combined with [LUIS](https://www.luis.ai/applications/), provides the tools developers need to build and publish intelligent bots that interact naturally with users using a range of services. In this lab, you will create a bot using Visual Studio Code and the Microsoft Bot Framework, and connect it to a knowledge base built with LUIS. Then you will interact with the bot using Facebook Messenger — one of many popular services with which bots built with the Microsoft Bot Framework can integrate.

<a name="Objectives"></a>
### Objectives ###

In this hands-on lab, you will learn how to:

- Create an Azure Bot Service to host a bot
- Get the bot to send tweet to Twitter
- Set up the bot on Facebook Messenger

<a name="Prerequisites"></a>
### Prerequisites ###

The following are required to complete this hands-on lab:

- Facebook Page and Developer Account
- Twitter Account 


## Tutorials ##
https://blogs.msdn.microsoft.com/uk_faculty_connection/2017/10/28/how-to-build-a-user-interactive-bot-with-microsoft-bot-framework/
How to build a user interactive bot with Microsoft Bot Framework by Shu Ishida

Code snippets are here

### Step 1: Change your bot response ###
1. Make your Bot cocky
  ```
  .matches(/^hey/i, (session, args) => {
    session.send("If you can't think of anything more interesting to say than '%s' don't message me!",
        session.message.text);
  })
  ```
  
  ^ denotes the start of a sentence, and ‘i' indicates that the search should be case-insensitive. These are called regular expressions, often used in text search and text replacement. The bot replies to you using session.message.text which contains your utterance.
  
### Step 2: Get your bot to tweet "Hello World!" ###

1. Go to **the online code editor tab**, go to console, type
```
npm install --save dotenv twit
```

2. Create a **.env** file, add in the information below. Add **.env** into **.gitignore**

```
CONSUMER_KEY = 
CONSUMER_SECRET =
ACCESS_TOKEN = 
ACCESS_TOKEN_SECRET = 
```

3. Create **config.js**
```
require ('dotenv').config();

module.exports = {
	consumer_key: process.env.CONSUMER_KEY,
	consumer_secret: process.env.CONSUMER_SECRET,
	access_token: process.env.ACCESS_TOKEN,
	access_token_secret: process.env.ACCESS_TOKEN_SECRET,	
};
```

4. Finally, in the app.js file, set up twitbot (a Twit object that carries out the tweeting operations) configuring with the key information. Make sure you don’t call this just ‘bot’ – it will overlap with the ‘bot’ variable defined already.

```
var Twit = require('twit');
var config = require('./config');
var twitbot = new Twit(config);
```

5. Let’s append this to the response to ‘Greeting’ for the time being as shown below.
```
.matches('Greeting', (session, args) => {
    session.send("If you can't think of anything more interesting to say than '%s' don't message me!",
        session.message.text);
    twitbot.post('statuses/update', {
        status: 'hello world sent from Azure!'    
    }, (err, data, response) => {
        if (err) {
            console.log(err);
        } else {
            console.log('${data.text} tweeted!');
        }
    });
})
```

### Step 3: Get Your Bot to Tweet What You Want ###
```
.matches('Tweet', (session, args) => {
    session.send("OK, I'll connect you to your Twitter account");
    session.beginDialog('/tweet');
})

```

Put this at the bottom of app.js
```
\\ Tweet Dialog
bot.dialog('/tweet', [
    function (session) {
        builder.Prompts.text(session, 'What would you like to tweet?');
    },
    function (session, results, next) {
        session.send("Got it! I'll tweet '%s'", results.response);
        twitbot.post('statuses/update', {
            status: results.response
        }, (err, data, response) => {
        if (err) {
            console.log(err);
        } else {
            console.log('${data.text} tweeted!');
        }
    });
    session.endDialog("Now your tweet is live!");
    }
]).cancelAction('cancelAction', 'OK, cancelled tweet.', {
    matches: /^nvm$|^nevermind$|^cancel$|^cancel.*tweet|^quit$/i,
    confirmPrompt: "Are you sure?"
});
```

Once you create new intent, publish it several times!


### Step 4: Get Your Bot into Facebook Messenger ###

Follow this tutorial to set up your Facebook App
[Creating A Facebook Bot Using Microsoft Bot Framework](http://aihelpwebsite.com/Blog/EntryId/7/Creating-A-Facebook-Bot-Using-Microsoft-Bot-Framework)

Further References
[Microsoft Bot Framework Documentation](https://docs.microsoft.com/en-us/bot-framework/)

