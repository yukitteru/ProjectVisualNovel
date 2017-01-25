# ProjectVisualNovel (Fall 2016 - Present)

##Overview
The goal of this project is to create a branching dialogue system that can be used by game developers to add dialogue to [LibGDX] (https://libgdx.badlogicgames.com/) games. The original intent of the project was to be used to create **visual novels**, so the feature set is created with that in mind. However, the system should be flexible enough to be used for any type of dialogue, making it useful for other genres like RPGs or even just for cutscenes in games.   

Visual novels are a type of video game that feature little gameplay and instead are made mostly of dialogue, with most interaction being in the form of making choices that influence the story in some way. Visual novels are less common for American gamers, but are extremely popular in Japan. This project was inspired by visual novels such as *Ace Attorney* and *Dangan Ronpa* and some of the ideas used in structuring the conversation files were based on *Ren'PY*, a Python engine for making visual novels. 

I chose to make this project instead of using something like Ren'Py for several reasons. Writing the code myself allows me to have much more control and allows me to implement commands however I feel is most convenient. Commands that would be inconvenient or impossible in Ren'Py can be implemented more easily. Additionally, by making the system as flexible as possible, it can eaily be used for much more than visual novels. Ren'Py is great for visual novels, but doesn't necessarily have functionality for other types of games. And perhaps most importantly, I thought it would be good experience to try doing something like this myself. There's little point in reinventing the wheel, but learning to make a better wheel is still useful.

##Demonstration
*Note: Graphics are placeholder and not mine. They are just for demonstration purposes.*
![gif](https://dl.dropboxusercontent.com/u/25507891/visualnovel3test.gif)

The file containing the Conversation used in the example can be seen [here] (https://github.com/wizered67/ProjectVisualNovel/blob/master/android/assets/Conversations/demonstration.conv)

## How it works
Almost all of the code used for the visual novel system can be found in the [GUI package] (https://github.com/wizered67/ProjectVisualNovel/tree/master/core/src/com/wizered67/game/GUI). Many of the terms below are simply terms I created to refer to things in the dialogue system. Other visual novel systems may refer to them by other names.

The smallest component of dialogue is a [command] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/Commands/ConversationCommand.java). Each command makes some change to current status of the game. These changes can be things such as [playing an animation] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/Commands/CharacterAnimationCommand.java), [displaying a sequence of text] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/Commands/MessageCommand.java), or offering the player a [choice] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/Commands/ShowChoicesCommand.java) and reacting to it. (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/Commands/PlaySoundCommand.java). There are a wide variety of commands already implemented and more are being added still. 

A named sequence of commands is a **branch**. The game will go through all commands in the current branch and execute them in order, sometimes stopping between commands to let the previous one finish, depending on what command it was. For example, playing a sound effect does not pause the execution of commands, but displaying a message does pause the execution of commands until the players clicks to confirm that they want to continue. Writing the code in a way that allowed commands to determine if they should pause execution has already proven to be extremely helpful. I can even implement commands that sometimes pause and sometimes don't, such as the set animation command which, depending on a variable, will either go straight to the next command or wait for the animation to finish. 

A group of branches is called a [Conversation] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/Conversation.java). Branches within a Conversation are freely traveled between by using the [change branch] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/Commands/ChangeBranchCommand.java) command. An example where this would be useful is in the show choice command, which presents the player with several choices. Depending on which choice they make, a different command can be executed. Executing a change branch command lets the player go through a new series of dialogue depending on their choice. The player can even be returned to a branch they've already been through for looping dialogue. 

## Writing Conversations using XML
Because of the sheer number of commands that would be required to make a full visual novel, it has always been a priority to make it convenient and fast to write Conversations. After some contemplation, I settled on using **XML** files to write Conversations. This allows me to add tags that can have attributes and other elements within them. These tags are then converted to the corresponding commands in game by the [XML Loader] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/XmlIO/ConversationLoader.java). 

I have taken several steps to make writing XML conversations as fast, clean, and error free as possible. I have created an [XSD schema] (https://github.com/wizered67/ProjectVisualNovel/blob/master/core/src/com/wizered67/game/GUI/Conversations/XmlIO/ConversationSchema.xsd) for the conversation files. This adds simple validation, making sure the creator is immediately aware of missing attributes or misplaced elements. Additionally, in many IDEs, including IntelliJ, the conversation schema adds autocompletion to many of the tags. For example, mandatory attributes can be automatically added for the creator to fill in.

I have also simplified Conversation files by allowing some elements to use text instead of XML tags. For example, dialogue can be added by writing "Speaker: Text to say." instead of <message speaker="" text="">. This slightly reduces the autocompletion capabilities and validation, but it makes it much easier to read and write. I intend to write a more extensive validator in Java that would catch additional problems.

## Features
* Test
* Test 2


