# Simple Slackbot based on Botkit and Watson-Botkit-Middleware, extended for IBM Cloud Functions

[Watson Botkit Middleware](https://github.com/watson-developer-cloud/botkit-middleware) is a [chat service / natural language processing plugin](https://botkit.ai/docs/readme-middlewares.html) for the [Botkit framework](https://botkit.ai/getstarted.html). There is a [simple bot](https://github.com/watson-developer-cloud/botkit-middleware/tree/master/examples/simple-bot) example for Watson Botkit Middleware that shows how easy it is to connect IBM Watson Assistant to Slack.

Some time ago, I wrote a [tutorial showing how to use server actions with Watson Assistant](https://console.bluemix.net/docs/tutorials/slack-chatbot-database-watson.html) to hook up a Db2 database with a Slack chatbot. For the tutorial I used the serverless approach and the [Conversation connector](https://github.com/watson-developer-cloud/conversation-connector/) to bring Slack and Watson Assistant together. With the sample in this Github repository I provide code to reuse the functions for the server actions that I described in the tutorial. Use Botkit instead of Conversation connector to bring Slack and Watson Assistant together, but keep the server-side code for a database-driven chatbot!

The following blog entries are of relevance:
* [Enable Botkit Middleware for Watson Assistant for serverless actions](http://blog.4loeser.net/2018/06/enable-botkit-middleware-for-watson.html)
* [Use BotKit Middleware to create Watson-powered database interface](http://blog.4loeser.net/2018/06/use-botkit-middleware-to-create-watson.html)

## Install
To get everything up and running, follow the general instructions in the tutorial to set up Watson Assistant, the server actions with IBM Cloud Functions. Also, obtain a authorization token for the Slack bot. You can deploy the botkit middleware either as **Docker container** or directly using **npm**.
#### Docker
1. Copy the template for the environment settings over:
   ```
   cp .env.template .env
   ```   
2. Edit the file **.env** and fill in the necessary credentials for Watson Assistant, IBM Cloud Functions and Slack.

3. Build the Docker image:
   ```
   docker build --network=host -t mybotkit1 .
   ```   
   This creates a container image with Botkit, Watson Botkit Middleware and the required Express server. It also copies in the environment configuration.

4. Run the Docker container:
   ```
   docker run  -t --network=host -i  mybotkit1
   ```   
   The bot should turn "online" in your Slack app. You should be able to access the event information and create new events. See the [Slackbot tutorial](https://console.bluemix.net/docs/tutorials/slack-chatbot-database-watson.html) for details.

#### npm
1. Copy the template for the environment settings over:
   ```
   cp .env.template .env
   ```   
2. Edit the file **.env** and fill in the necessary credentials for Watson Assistant, IBM Cloud Functions and Slack.
3. Install the dependencies:
   ```
   npm install
   ```   
4. Start the app
   ```
   npm start
   ```   

## Details
In order to successfully call the server actions (IBM Cloud Functions actions), Watson Assistant needs the API key. That key is added to the message context as private variable in the middleware **before** method in [simple-bot-slack-extended.js](simple-bot-slack-extended.js). The code is similar to what is done in [pre-conversation-APIKey.js](https://github.com/IBM-Cloud/slack-chatbot-database-watson/blob/master/pre-conversation-APIKey.js) in the tutorial code.
