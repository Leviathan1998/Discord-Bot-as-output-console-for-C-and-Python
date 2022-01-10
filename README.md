# Discord-Bot-as-output-console-for-C-and-Python
This tutorial will learn you how to make your own Discord Bot and make him work like a output console. Read README file..

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

PREQUISITIES: You will need:


>>> NodeJS
>>> Code Editor (VS Code, Atom, Sublime, Eclipse, CLion etc...) I recommend you VS Code  https://code.visualstudio.com/download
>>> Discord Account and Discord Server.

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

LETS START!! First you need to create a bot:

 1) >>> You'll need to  register your bot to  Discord Developer portal https://discord.com/login?redirect_to=%2Fdevelopers%2Fapplications


2) >>> After you login you will reach this screen then click on new application and you would be able to create a new application.

3) >>> On this page you can set the bot icon description about the bot etc. Select bot option, and add a bot. Now the bot is setup,

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

2 LETS CODE THE BOT :

1) >>> First lets create a directory for the bot, and also install the dependencies required for coding the bot.

>>> discord.js — The Discord library used to create our Discord bot.
>>> dotenv — This is going to allow us to hide certain variables, such as our bot’s client ID.
>>> request — This package is used to make http and https request.

2) >>> Lets start coding by opening preferred code editor.

3) >>> Now lets add .env and node_modules to .gitignore so that those files are not pushed to github repo in future.

4) >>> Now we will get the token for our bot from the developer portal (https://discord.com/login?redirect_to=%2Fdevelopers%2Fapplications),
click on bot option and then click on copy.

5) >>> Now paste the bot token in .env file:
   
   >>> CLIENT_TOKEN=<your token>
  
6) >>> Now in package json add a line inside script tag:
  
   >>> “start”:"node index.js"
  
7) >>> Now lets create a file index.js and add the required dependencies:
  
  >>> const Discord = require(“discord.js”);
 const client = new Discord.Client();
var request = require(“request”);
require(“dotenv”).config();
client.login(process.env.CLIENT_TOKEN);
  
8) >>> Now for the compiler we would be using jdoodle api (https://www.jdoodle.com/compiler-api/),
sign up and get the client Id and client Secret and paste it in .env file,
  
  >>> CLIENT_ID=<your client Id>
CLIENT_SECRET=<your client secret>
  
9) >>> Now lets add the code to run c and python program:
  
  const Discord = require("discord.js");
const client = new Discord.Client();
var request = require("request");
require("dotenv").config();

const prefix = "coder ";

client.on("ready", () => {
    console.log("coder-BOT is online!");
  });
  
  client.on("message", async (message) => {
    try {
      let args = message.content.substring(prefix.length).split(" ");
      switch (args[0]) {case "run":
      var input = "";
      var language, index;
      if (args[1] == "C" || args[1] == "c") {
        language = "c";
        index = "0";
      } else if (args[1] == "python") {
        language = "python3";
        index = "3";
      }
      var fileName = message.attachments.array()[0];
      for (var i = 2; i < args.length; i++) input += args[i] + " ";

      request.get(fileName.url, (err, response, body) => {
        const runRequestBody = {
          script: body,
          language: language,
          stdin: input,
          versionIndex: index,
          clientId: process.env.CLIENT_ID,
          clientSecret: process.env.CLIENT_SECRET,
        };
        request
          .post({
            url: "https://api.jdoodle.com/execute",
            json: runRequestBody,
          })
          .on("error", (error) => {
            console.log("request.post error", error);
            return;
          })
          .on("data", (data) => {
            const parsedData = JSON.parse(data.toString());
            if (parsedData.error) {
              return;
            } else {
              var output = "";
              for (var i = 0; i < parsedData.output.length; i++) {
                if (parsedData.output[i] == "\n") output += "\n";
                else output += parsedData.output[i];
              }
              let outputOfProgram = new Discord.MessageEmbed()
                .setAuthor(client.user.username)
                .setDescription("Result!!!")
                .setColor("33FFB3")
                .setDescription(output);
              message.channel.send(outputOfProgram);
              return;
            }
          });
      });
      break;
    }
}
catch (Exception) {
    console.log(Exception);
    let Errorbotembed = new Discord.MessageEmbed()
      .setAuthor(client.user.username)
      .setDescription("Error!!!")
      .setColor("FFFFFF")
      .setDescription("Error Encountered\nFor Help use\nDM help");
    message.channel.send(Errorbotembed);
  }
});


client.login(process.env.CLIENT_TOKEN);
                                                           
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                                                           
 SO LETS BREAK THE CODE:
                                                           
 1) >>> client.on ready is when the discord bot goes online and it would just print coder-BOT is online in the command prompt.
  
 2) >>> client.on message works when someone sends a message in the channel on which the discord bot has access to.
  
 3) >>> In our case what we do here is we split the message to check whether the starting of the message contains the pattern “coder”. If a message starts with coder then we understand that the message is to the coderBot. Now here I used switch statement since we have only 1 command the coderbot address that is the run command.
  
4) >>> Now when a message comes starting with “coder run” the first case would run and what we do in here is we check what is the argument that comes after the run command if it is c then c compiler is used and if it is python then we use the python interpreter. The index variable is set according to how the index is set in the jdoodle ide i.e, in our case we are using python 3.7.4 which is at index 3 in jdoodle python ide and gcc 5.3.0 c compiler at index 0.
  
5) >>> Now after the language is specified we specify the inputs to the file and it is.
  
6) >>> Now to capture the file that is uploaded along with the message we use :
  
  >>> var fileName = message.attachments.array()[0];
  
 7) >>> Now we send the request using jdoodle api and print the output accordingly.
  
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
ADDING BOT INTO SERVER :
  
1) >>> Navigate to the OAuth2 option in the developer portal,
  
2) >>> In scopes select bot,
  
3) >>> Then in bot permission select send message and manage roles.
  
4) >>> Copy the link and then go to the link and add to the discord server.
  
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  
NOW LETS TRY RUNNING YOUR BOT :
  
1) >>> Use the command in CMD :
     
  >>> npm start
  
2) >>> You will get coder-Bot online message in cmd.  
