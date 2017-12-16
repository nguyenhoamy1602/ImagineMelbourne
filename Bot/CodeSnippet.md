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

- An active Microsoft Azure subscription. If you don't have one, [sign up for a free trial](http://aka.ms/WATK-FreeTrial).
- Twitter Account 


## Tutorials ##
https://blogs.msdn.microsoft.com/uk_faculty_connection/2017/10/28/how-to-build-a-user-interactive-bot-with-microsoft-bot-framework/
How to build a user interactive bot with Microsoft Bot Framework by Shu Ishida

Code snippets are here

1. Make your Bot cocky
  ```
  .matches(/^hey/i, (session, args) => {
    session.send("If you can't think of anything more interesting to say than '%s' don't message me!",
        session.message.text);
  })
  ```
  
  ^ denotes the start of a sentence, and ‘i' indicates that the search should be case-insensitive. These are called regular expressions, often used in text search and text replacement. The bot replies to you using session.message.text which contains your utterance.
  
1. Go to **the online code editor tab**, go to console, type
```
npm install --save dotenv twit
```
1. Create a **.env** file
Add
```
CONSUMER_KEY = 
CONSUMER_SECRET =
ACCESS_TOKEN = 
ACCESS_TOKEN_SECRET = 
```

1. Create **config.js**
```
require ('dotenv').config();

module.exports = {
	consumer_key: process.env.CONSUMER_KEY,
	consumer_secret: process.env.CONSUMER_SECRET,
	access_token: process.env.ACCESS_TOKEN,
	access_token_secret: process.env.ACCESS_TOKEN_SECRET,	
};
```
1. Finally, in the app.js file, set up twitbot (a Twit object that carries out the tweeting operations) configuring with the key information. Make sure you don’t call this just ‘bot’ – it will overlap with the ‘bot’ variable defined already.

```
var Twit = require('twit');
var config = require('./config');
var twitbot = new Twit(config);
```

1. Let’s append this to the response to ‘Greeting’ for the time being as shown below.
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


Once you create new intent, publish it several times!

