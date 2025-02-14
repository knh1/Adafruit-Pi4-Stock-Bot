<div align="center">
  
# Adafruit Pi4 Stock Alert Bot &nbsp;&nbsp;
  <a href="">[![GitHub tag](https://img.shields.io/github/tag/EthyMoney/Adafruit-Pi4-Stock-Bot?include_prereleases=&sort=semver&color=blue)](https://github.com/EthyMoney/Adafruit-Pi4-Stock-Bot/releases/)</a>
  <a href="">[![License](https://img.shields.io/badge/License-MIT-blue)](https://github.com/EthyMoney/Adafruit-Pi4-Stock-Bot/blob/main/LICENSE)</a>
  <a href="">[![issues - Adafruit-Pi4-Stock-Bot](https://img.shields.io/github/issues/EthyMoney/Adafruit-Pi4-Stock-Bot)](https://github.com/EthyMoney/Adafruit-Pi4-Stock-Bot/issues)</a>
  <a href="">[![Made For - Discord](https://img.shields.io/static/v1?label=Made+For&message=Discord&color=%235865F2&logo=discord)](https://discord.com/)</a>
  <a href="">[![Made For - Slack](https://img.shields.io/static/v1?label=Made+For&message=Slack&color=%234A154B&logo=slack&logoColor=%23E01E5A)](https://slack.com/)</a>
  <a href="">[![Node.js - >=18.12.1](https://img.shields.io/badge/Node.js->=18.12.1-brightgreen?logo=node.js)](https://nodejs.org/en/)</a>
  <a href="">[![Discord.js - 14.12.1](https://img.shields.io/badge/Discord.js-14.12.1-blue?logo=discord&logoColor=https%3A%2F%2Fdiscord.js.org%2F%23%2F)](https://discord.js.org/)</a>
  
</div>

<br>
<p align="center">
  <img src="https://imgur.com/ndaGhdY.png" alt="Alert Icon" width="20%" height="auto">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://imgur.com/aldX3qJ.png" alt="Raspberry Pi 4 Model B" width="35%" height="auto">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://imgur.com/ndaGhdY.png" alt="Alert Icon" width="20%" height="auto">
</p>
<br><br>

## What This Is
A simple Discord and/or Slack bot that checks the stock status of all the Raspberry Pi 4 (model B) models on Adafruit and sends a message to a Discord/Slack channel when one is in stock. This bot is designed to be self-hosted and run for use in your own Discord server or Slack workspace.

## Why?
Because Adafruit's stock notification system sucks. It's a FIFO queue where the whole queue gets cleared any time any stock comes in. This means that your notification subscription will get removed even if your notification never got triggered during restock because the restock quantity was smaller than the queue size. This means that every time any restock happens at all, even if it's small and doesn't trigger your notification, you'll have to go back and re-subscribe to the notifications. This bot removes the need for that by allowing you to quickly get a @mention in your Discord server or a message in your Slack channels every time there is a restock!

## How It Works
On a set interval, the bot will query Adafruit's product page for the Pi 4 model B and watch for any of the stock statuses to change to "In stock". If one or more of the models come in stock, a notification is sent out to the configured Discord server channel with accompanying @role mentions. For Slack, it will send the notification to the specified channels since Slack doesn't have roles like Discord does. In either case, the notification will contain a direct link to the page of the SKU that's in stock so you can buy it right away. Stock statuses are tracked between update intervals, so you won't have to get spammed with the same notification on every check if the bot has already sent a notification for a current stock event of a particular model. This is handled in a smart way to ensure you always get *one* notification every time any model comes in stock, while never missing a restock!

## How to Set Up and Run
* Install [Node.js](https://nodejs.org) LTS edition for your specific environment using the site or a package manager. Node.js is supported basically everywhere which allows this bot to be multi-platform!
* Clone the repo, then run `npm install` from a terminal in the root project folder. This installs all necessary dependencies for you.
* Follow the below instructions for setting up a Discord or Slack bot (or even both!). Be sure to complete the [final steps](#final-configuration-steps-and-bot-startup) as well once you finish the Discord and Slack specific instructions.

### Discord Bot Set Up
* Go to the [Discord Developer Portal](https://discord.com/developers/applications) and click "New Application" at the top.
* Give your bot application a name and hit create.
* From the left side navigation column, select the "Bot" tab (has a puzzle piece icon), click "Add Bot" and confirm with the "Yes, do it!" button.
* From here, go ahead and set a username and avatar for your new bot. You'll want to uncheck the "Public Bot" option as well.
* Now you need to make an invite link so you can add the bot to your server. From the left side navigation column, select the "General Information" tab.
* Copy your "Application ID" shown there. You will put this into the following template link so it can identify your bot.
* Use this invite link as your template: `https://discord.com/oauth2/authorize?client_id=APPLICATION_ID_HERE&scope=bot&permissions=412652817488`
* Replace `APPLICATION_ID_HERE` in that link with your actual application ID you copied earlier.
* Now go ahead and use that link to add your bot to your server. Be sure to leave all permissions checked! These are pre-configured for you.
* It's important that you add the bot to your server before you proceed. The bot program expects to already have access to the server when it starts up.
* Now, you need to configure the `config.json` file for your use. Open the file in a text editor.
* Enter your bot's token. This can be found back in the developer portal under the "Bot" tab again. Click on "Reset Token" and copy it. KEEP THIS SAFE AND PRIVATE!
* Now enter the ID number of the server you added the bot to earlier. You can get from within Discord by right clicking on the server icon (with developer options enabled in settings)
* Now enter the name of the channel in your server where you'd like to have updates posted. You can leave this blank if you want the bot to create a new one for you.

### Slack Bot Set Up
* Go to the [Slack App API](https://api.slack.com/) and click "Create an app", then select "From scratch" in the popup that appears.
* Give your Slack App a name and select your workspace you'd like to add the bot to, then click "Create App".
* Under the "Add features and functionality" section select the "Bots" option.
* Along the left side navigation under the "Features" section, select "OAuth & Permissions". Once selected, scroll down to the "Scopes" section.
* Under "Bot Token Scopes", click the "Add an OAuth Scope" button and then add these scopes:
  * "chat:write",
  * "links:write",
  * "channels:manage",
  * "chat:write.customize", 
  * and "chat:write.public".
* Now scroll back up and click the "Install to Workspace" button. Allow the app access to your workspace using the "Allow" button on the screen that appears.
* You will now be shown a page with your bot token. Copy the "Bot User OAuth Token" and paste it in the slackBotToken parameter in the `config.json`. KEEP THIS TOKEN SAFE AND PRIVATE!
* Create at least one channel for the bot to post into. You can utilize up to four channels to each receive @channel notifications for specific models of Raspberry Pi's.
* Put the channel names in the `config.json` into the respective fields. For example, if you wanted 1GB updates to post to #general, put "#general" in the slackChannel1GB field. If you want to have a single channel mentioned for all models of raspberry pi's put the same channel into every slackChannel config or create mutliple channels.

### Final Configuration Steps and Bot Startup
* In the `config.json` file:
  * Indicate whether you are using the Discord bot, Slack bot, or even both, using the `enableDiscordBot`, or `enableSlackBot` fields of the config file. These are both on(true) by default, adjust them accordingly if needed.
  * Enter the update interval in seconds for `updateIntervalSeconds` (default is 30 seconds). 
  * Set any models you don't wish to monitor to false, using `watch1GigModel`, `watch2GigModel` and so on... (all are enabled(true) by default).
  * Choose whether or not you want to have sleep mode enabled using `enableSleepMode`. Sleep mode just prevents the bot from querying Adafruit overnight when restocks aren't happening (this is enabled(true) by default). Prevents needless spam to Adafruit's servers!
* Yay! You are now ready to start your bot! Go ahead and run `npm start` in a terminal of the project directory to launch the bot!.
* If you are using the Discord bot, be sure to make use of the roles that the bot created! Add them to yourself and others so you get mentioned when stock comes in.
* That's it! I hope you get your pi! :)
<br>

## Here's What the Notification Messages Look Like
<p align="center">
  <img src="https://imgur.com/55fdrDT.png" alt="Discord Message" width="42%" height="auto">
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
  <img src="https://imgur.com/OOzKLZH.png" alt="Slack Message" width="51%" height="auto">
</p>
<br>

## One More Thing
Like this bot? Show some support! Give me a star on this repo and share it with your friends! :)<br>
Contributions are welcome and encouraged! Feel free to open a pull request or issue for things you notice or want to fix/improve.<br>
If you want to chat, you can find me in the support Discord server of my other popular bot that I made called TsukiBot, [Join Here!](https://discord.gg/t7Ka9ycEyD)
