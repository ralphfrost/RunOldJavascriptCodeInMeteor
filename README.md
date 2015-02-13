# RunOldJavascriptCodeInMeteor
One way to get old or pre-existing  javascript code to run within a Meteor js app

So, I'm learning Meteor JS (version 1.03) and also learning javascript and I spent a day or so searching for instructions on how to include old  javascript code in a meteor app, or how to run old javascript games or programs within Meteor. 

Not finding much, within a few hours I found that the following procedure works.   The short answer is, paste the old js program code within a Meteor.myFunction definition in a lib/oldCode.js file and then call it (for instance, Meteor.myFunctions.playGame(ver)) from some click event in your main.js file. 


The longer answer is below and in illustrated in the repo files.   The  javascript code I use here in my example comes from https://github.com/alefrost/infinityDungeon. If you are learning, start, there.

First, install Meteor:

curl https://install.meteor.com/ | sh  //osx/linux

Second, create a new project:

Mac-HD: username ~/Meteor$  meteor create game
Mac-HD: username ~/Meteor$  cd game
Mac-HD: username ~/Meteor/game$ mkdir lib

...now copy/paste or use git commands to get files into your project:

game.html
game.js
game.css
lib/oldgamecode.js

and, when ready, run 'meteor' in your terminal within the project directory to test locally before deploying to one hosting option on the web via:   

Mac-HD: username ~/Meteor$  meteor deploy uniqueSubdomainName.meteor.com


Some more details for people new to Meteor js...

1. The game.html file contains suffienct html and template definitions relevant to Alex's specific (infinity/Dungeon) javascript program code included herein.
2. The game.js file contains code to initialize some Meteor Session variables and define template helpers and template event actions. 
3. In this instance "the old game/program code is slightly modified and pasted into the lib/meteorGame.js file which has the following structure:

lib/meteorGame.js:

// Wrap old js /program/game code in Meteor.myFunctions
// allowing calling Meteor.myFunctions.playGame(ver)
// from anywhere in Meteor app. (saved in /lib  2/5/2015 -ref-

Meteor.myFunctions = {
    playGame : function(gameversion) {
// Original old/external js code starts here...

// ... but I replaced 17, 29, 10 hard-coded values w/
// user controlled (Meteor) Session vars

var ROWS = Session.get("rows");
var COLS = Session.get("cols");
var ACTORS = Session.get("actors");
var MIN_LEAF_SIZE = Session.get("minLeafSize");
var MAX_LEAF_SIZE = Session.get("maxLeafSize");

var map;

var player;
var actorList = [];
var livingEnemies;
var actorMap = {};


var root = createLeaf(0, 0, COLS, ROWS);
var leaves = [root];
var halls = [];

// AND, 2/5/2015 adapted game to run in Meteor 1.03 -ref-
if (gameversion === 'basic')
  runBasic();
else
  runComplex();

   // ...
   // ALL remaining js code in the original javescript file pasted here...
   // ...

};  // end of Meteor.myFunctions wrapper



This js program gets called within game.js via:  

      Meteor.myFunctions.playGame(ver);  // start  new game
      
That's about it...    





