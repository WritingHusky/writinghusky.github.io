---
title: Twilight Princess AP World
date: 2025-02-15 -0700
categories: [AP World]
tags: [project]
---

# Overview

Archipelego is a multi world randomizer, which allows for players to randomize multiple games together into one big randomized game.

At the begining of the year I began working on a Twilight Princess intergration with Archipelego. 

# Process

There had been some work done to create the world before but the progress was slow. Twilight is a long game when it comes to randomizers. There are 474 locations that are randomized in the game, and each location needs logic to track accessiblity as well as information about where in memory that location can be checked (more about that later.) 

Starting this project I did not want to have to write it all out by hand, so the first thing I did was to see what other people have done with the game. I had played the normal Twilight Princess randomizer many times before so I knew where to look. After looking through docs and joinging the discord I was able to find the code for the web generator that TPR uses to create seeds to run the game. Within the web generator there is a folder of all the world data used. This inculdes information about every location ("checks") and every region ("rooms").

After finding this data I knew that with it I would be able to use it to generate the code for the my randomizer. This data was under the MIT linsence so I got a copy of it and began to learn it and what I needed to do with it. 

At the begining of the year I learnt about Cursor and wanted to try it out for something. So I tried playing around with it trying to get it to build the program to generate the code I wanted. It took me a while to both understand how Archipelego works and how to generate what I wanted. At the end of the process I ended up using what I learnt from "vibe coding" the generator to create a nice generator to build out what I needed. 

## So much data

In addition to the data from the web generator I also found a spread sheet of locations and memory addresses, also I would later find a spreedsheet that layed out the architecture of the save data for Twilight Princess. With this I was able to combine it with the world data to create the mass file that contains all the info about each location. Upon release there was a bit of smoothing over of the data as some things were labled wrong or just not correct (474 locations is a lot so its inevitable.)

In addition to the location data there is also the items. Giving a player an item is not as simple as writing some data in there inventory and ta-dah they have the item. In zelda games, when link recieves an item he usualy holds it up to the camera to show the player. (I believe the following is a randomizer feature) It is possible the player recives an item and they are not able display the animation of showing it and item giving is not simple, so there is what I call an "item queue." Each item is given an id and the item can be added to the queue and when possible links does the "show the player the item" animation and then the item is collected. This is very usefull for AP as when the player recieves an item I can add it to the queue and then they have the item. TPR also handles progressive items here so I don't have to worry about them, which makes my life so much easier.

## Logic

The final thing is that in order for the randomizer to be playable there needs to be logic that determines what is needed to access locations. The web generator that I got the world data from also has logic so I used that to help build the logic. The logic is split into to catergories, the macros and the loction/region rules. Macros are the functions that will do the checks needed, for example has_sword() returns True if the player has a sword, can_defeat_Bublin() returns True/False based on if the player has the things to beat a Bublin. The location/region rules are logical combinations of macros that determine weither or not the player can access it, for example the rule for the "Snowpeak Cave Ice Poe" location the player needs the shadow crystal and the ball and chain. Region access also works on previous location rules, ie if you need shadow crystal to access regions prior then it can assume you have shadow crystal in that region (This makes the rules simpler) 

# Alpha Release

Once the apworld was created an basic functionality worked, I released it as Early Alpha. For the first version there were fequent releases that focused on getting this to work consistently and properly. There were many fixes by Lunarsoap that address incorrect data in locations and items. As releases rolled out settings were implemented and the apworld got closer to stability.

Some major improvements include: Adding settings for Small keys, Big Keys, and Map & Compass. Unit testing that covers a large portion of the possible settings. Integration of the Fuzzing tool for full setting exploration.

## Current state

As of writing this, (March 28, 2025)the apworld has reached a point where the fuzz testing hasn't found errors in generation. The only way the world fails to generate is if both overworld and dungeon locations are disabled, in which case the apworld with throw an OptionError (Which makes the fuzzer ignore that generation). 

I am very proud of what I have been able to achieve with the project. I have more plans for things to do moving forward, it's still in Alpha, including intergration with the Twilight princess randomizer. 
