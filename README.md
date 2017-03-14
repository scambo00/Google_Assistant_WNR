# Integrating Wink Node Red and a custom "Action on Google" using API.AI
This will detail step by step how to import a prebuilt "Assistant Action" and how to keep it private (ie. you can use the Action in your own Google Home device, but it will not be publicly available).
The idea is to create an flow for wink node-red (webhook) that is triggered by an Action built with API.AI.  
## 1)API.AI
API.AI is a development tool for conversational end points. It allows to create a natural language interactions action for Google Home.
### 1.1)Create an account
You must create the account with the same Google email that you use to control your Google Home.
* Create an account on [API.AI](https://api.ai/):

### 1.2)Create Agent
Once we have signed up, we will be taken straight to the Api.ai interface where we can create our virtual AI assistant. Each assistant we create and teach specific skills to is called an “agent” in Api.ai. So, to begin, we create our first agent by clicking the “Create Agent” button on the top left hand side:
<img src='/images/createAgent2.png'/>
On the next screen, we enter in our agent’s details, including:
* Name: This is just for your own reference to differentiate agents in the api.ai interface. You could call the agent anything you would like, (I chose Wink Node Red).
* Description: A human readable description so you can remember what the agent’s responsible for. This is optional and might not be needed if your agent’s name is self-explanatory.
* Language: The language which the agent works in. For this tutorial, we will be choosing English.

When you have input your agent’s settings, choose “Save” next to the agent’s name to save everything:

### 1.3)Import Agent
Download the following file which is a prepopulated Agent for the wink node-red interface.  This file will be imported as described.
* [winkNodeRed_0_1.zip](winkNodeRed_0_1.zip)

<img src='/images/importAgent.jpg'/>

### 1.4)Finding your Api.ai API keys
The API keys we will need are further down on this agent page. Scroll down and you will find the “API keys” section. Copy and paste the “Developer access token” somewhere safe. This will be need to make queries to the Wink Node Red (context.global.googleHomeKey):

<img src='/images/agentAPIKeys.png'/>

### 1.5)Connect your API.AI action to your webhook
* Open ‘Fulfillment’ window on the right panel.
* Copy the URL of your application ‘https://your.mybluemix.net/red/googleActions’ to the URL field in API.AI.
* Leave "BASIC AUTH" blank
* Fill in "HEADERS" as show
* Set to "ENABLED"
* Save the Fulfillment.
* In any relevant Intent, enable the Fulfillment for the response: open ‘Intents’ window, open the intent that you want to connect, scroll down to ‘Fulfillment’ section, check “Use webhook”.
* Make sure all domains in the ‘Domains’ window are turned off.

<img src='/images/fulfillment.png'/>

## 2)Node-Red
The behavior of the Assistant when triggered by an Intent.  For example, what should the Assistant do when you ask “Turn On the Bedroom light”. API.AI recognize the words ‘On’(@command), and ‘Bedroom’(@winkName) but we need an application that process those words,  carry out the command and give us a responce. This is called a webhook. 

* Create new tab  - give it a meaningful name (Google Home Integration)
* Import from clipboard - [googleHomeFlow.json](googleHomeFlow.json)
* Set Global Defines - context.global.googleHomeKey="your developer access token";
* Deploy flow

Details of the "Process Request" function node will be added at sometime in the future that I can find the time to write it up.

## 3)Test Integration API.AI=>Node-Red=>API.AI
We can now test out our new Agent by executing the "update my devices" Intent. 
<img src='/images/updateDeviceIntent.jpg'/>

Click on the "Entities" selection on the left pane and type the test statement "update devices" into the test console on the right.
<img src='/images/testUpdateDevices.jpg'/>

The agent responds back with the number of wink devices WNR detected.    
<img src='/images/newDevices.jpg'/>

Clicking on the @winkName entity should display all of your wink devices.  The left column is the winkName and the right is synonyms that you can add to.
<img src='/images/winkNameEntity.jpg'/>

Again Details of the Intents and Enities will be added at sometime in the future that I can find the time to write it up.  I would suggest reading [Get started in 5 steps](https://docs.api.ai/docs/get-started)

## 4)Enable Actions on Google
* In the API.AI console, select Integrations in the left-hand panel.
* Enable the toggle switch on the Actions on Google card, as the following example shows:
<img src='/images/enableActions.jpg'/>

After you have enabled Actions on Google, you can configure your Conversation Action to prepare it for previewing 

### 4.1)To set your invocation name for testing:
* In the API.AI console, select Integrations in the left-hand panel.
* Click the SETTINGS link. The Actions on Google view displays.
* In the Invocation name for testing field, enter a name for your Conversation Action that you want to use
<img src='/images/invocationName.jpg'/>

### 4.2)SELECTING A VOICE
You can select one of several voice options when integrating your agent with Actions on Google. Select a voice that best matches the tone and style of your application. The default voice for the Home is a female voice, so if you want to ensure that your app contrasts with the default voice, you can choose a male voice.
### 4.3 Previewing your Conversation Action
After enabling Actions on Google integration, you can publish a preview of your Conversation Action so that you can try it out with the Google Home Web Simulator. This lets you test your agent without having to register it with Google
* In the API.AI console, select Integrations in the left-hand panel.
* Click the SETTINGS link. The Actions on Google view displays.
* If the AUTHORIZE link is visible in the lower right, click it and follow the instructions to give your client access to the new Conversation Action on the simulator.
* In the lower right corner of the Actions on Google view, click the PREVIEW button.
* If the PREVIEW button is not available, be sure that you have:
* Authorized your client to access the Conversation Action (click the AUTHORIZE button)
* Set the value of the invocation name for testing
* Selected a Welcome Intent
* If successful, a link to the Google Home Web Simulator appears in the lower right corner:

### 4.3)Web Simulator
The [web simulator](https://developers.google.com/actions/tools/web-simulator) lets you preview actions that you've built in API.AI in an easy-to-use interface with debugging and voice input. It helps you make sure that your Conversation Actions actually sound conversational and let you use the device without having hardware

### 4.4)Action Package

## 5) Extend your Actions preview
The command line interface will be used to extend your action forever
### 5.1)Download gactions-cli
* https://developers.google.com/actions/tools/gactions-cli

### 5.2)Downaload these two file
* [action.json](action.json)
* [command.txt](command.txt)

### 5.3)Edit action.json
<img src='/images/editAction.png'/>
### 5.4)Edit coomand.txt
<img src='/images/editCommand.png'/>
### 5.5)Run command from CMD window
Place the two files and the gactions.exe file into the same directory and from a cmd window and then execute the command.
This command will send the Action (‘action.json’ file, same as the the API.AI project) to your Google Home and will run it as a preview for 9999999 minutes (about 20 years).



