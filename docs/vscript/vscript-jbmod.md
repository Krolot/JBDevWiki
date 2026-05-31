# VScript in JBMod
In this guide we will cover:

- Where to put the scripts: We will learn the folders and the naming of scripts.
- How to use the console: What can you do with the console generally.
- How to execute them in game: Using commands such as script_execute and others.
- What are Functions, Callbacks and Bindings?
- Fun scripts: What you have been waiting for.
	
## Before Starting

!!! note "Prerequisite"

    You must have JBMod installed via Steam on the **github** branch for these instructions to work correctly.

If you don't know how to code in the VScript language, Squirrel, you can go back to **[this](vscript-learn.md)** article to learn the basics.

## Scripts Folder

The first thing we need to do is open Steam, click on JBMod and go to the right on the settings icon, click on it. Select **Properties** and then select **Installed Files** you will see a folder like this:

[Image]

Click on the one that says `jbmod` (all lowercase). Then click on the folder `scripts` if theres no scripts folder you create a new one and name it like that.

Inside the `scripts` folder create a new folder called `vscripts`. Inside that folder create `gamemodes` and `weapons`

### What is this?

This is the main folder, here you will have something like this

[Image]

These are some things you can do with vscript: 

- **[Gamemodes](vscript-main.md)** (article not ready)
- **[Weapons](vscript-main.md)** (article not ready)
- **Simple files** that can be executed through console

We will be learning the latter.

## Console

This is the heart of JBMod modding. It is essential if you want to start making scripts. To enable it you first need to open JBMod, go to options, and on the Keybind section click in **"Advanced"**, then click on **"Enable Developer Console"**

[Gif]

Now you can open your console using the letter "`" on an English keyboard. If you dont have an english keyboard you can change the keybind for the console in the same section

[Image]

Once you open the console you might get a little overwhelmed about all the stuff on your screen, you can type stuff on the textbox below it.

[Image]

Try typing in the console this inside any map (it does not work in the main menu): 

```squirrel
script printl("Hello World!")
```

If the message appears in the console, congratulations, you have made your first line of code inside JBMod.

!!! warning "Must know"

    The `script` command is a simple way to execute code directly from the console. Everything written after `script` is treated as a single line of code. `script_execute`, on the other hand, executes an entire `.nut` file located inside the `vscripts` folder.
	
## How to execute scripts

This is very simple, inside the folder `jbmod/scripts/vscripts/` we are going to create a new file, as you learned in the last article, Create a new file and rename its extension to ```.nut```. In this case we will do something like **`test.nut`**

Inside the `test.nut` file, type the same example as before 

```squirrel
printl("Hello World!")
```

The last thing you need to do is to start a server, or just opening a map using the `map jb_buildingblocks` command.

!!! note "Must know"

    You dont have to close the game when you want to change your scripts, you only  have to use the command `script_execute test` every time you change your script. They can only be executed INSIDE a map, not in the main menu. 
	
Finally just use the command `script_execute test` and the message should appear in the console. Well done.

## Entities/Handles

Everything is an entity in JBMod, the player is an entity, the props on the map are entities. Even things like the world itself.

A handle is a reference to an entity, its like a variable that points to it.

```squirrel
local player = GetPlayerByIndex(1);

player.SetHealth(100);
```

So this script when it gets executed it tells the server that the FIRST player on the server should have the health to a hundred, in local servers usually the first player is the one that hosts it.

You can see that we first define the local variable player, this variable uses the function `GetPlayerByIndex()` that searches for player through the server list, in this case we tell it to search for the first player. Once it founds the first player we tell it `player.SetHealth(100)` the dot means that we tell the script to access something from the entity, in this case its health and we tell it to set it to a hundred. You must do this for any entity you want to modify.  

Theres a lot of ways to access entities that we will see later.

!!! warning "Must Know"

	A handle becomes invalid if the entity is removed or disconnected, if its invalid the code will have errors.

## Entity Find

In VScript, you cannot directly access a full list of entities. Instead, you must iterate through them one by one using search functions.

The most common function is:

```squirrel
Entities.FindByClassname(startEntity, className);
```

This function will search for entities that match the classname, starting in the entity we tell it to. If you want it from the start use `null`

```squirrel
local ent = null;

ent = Entities.FindByClassname(ent, "prop_physics*")

printl(ent)
```

In this case it will look for the first prop_physics it finds and it will print it, soon we will see it with loops.

## Functions

As we have learned before, functions are blocks of code that are executed when explicitly called by the script or another system. A real JBMod example could be something like:
```squirrel
function HealPlayer(player)
{
    player.SetHealth(100);
}

HealPlayer(GetPlayerByIndex(1));
```
This simple function will set the first player health to a hundred.

## Callbacks

Callbacks are functions that are automatically called by the engine when a specific event occurs.
The script does not call them directly.

```squirrel
function OnPlayerSpawn(player)
{
    ClientPrint(player, Constants.EHudNotify.HUD_PRINTTALK, "A player has Spawned")
}
```

The function `OnPlayerSpawn` only gets executed when any player spawns, in this case it will print the message into chat. You DONT call the function, the function gets called by the engine.

The function automatically knows what player spawned so you dont need to search them by index or any other way.

## Bindings

Bindings are the connection between the engine and your script. They define which function will be executed when an event happens.

```squirrel
RegisterScriptGameEventListener("player_say");

function OnGameEvent_player_say(event)
{
    local msg = event.text.tolower();

    if (msg == "!hello")
    {
        Say(null, "Hello!", false);
    }
}

__CollectGameEventCallbacks(this);
```
As you can see here, we use the function `RegisterScriptGameEventListener()` to register the GameEvent `player_say`

!!! warning "Must know"

    We dont know yet all the game events on JBMod. This one was discovered through an experimental process.
	
Then we define a callback called `OnGameEvent_player_say()`, which is executed whenever the engine fires the player_say event. In this script, we check the message sent by the player, and if it matches `!hello`, the script responds in chat.

Finally, we use `__CollectGameEventCallbacks();` This function scans the current script scope and automatically registers all functions that start with `OnGameEvent_` as listeners for game events and then binds them with engine events.

This is one of the simplest ways to build chat-based command systems in JBMod.

## Extras

Yes, i know you might feel a little overwhelmed by this, not even me understands Bindings. So lets get into some simpler concepts to continue.

### Client VS Server

The client is your local game, it loads the graphics the UI, models and everything else you see while playing.

The server makes the math, where the bullets go, where you are, makes sure that everything is connected between clients and that they dont desync.

Something interesting is that some functions are exclusive to the server while others are exclusive to the client. Theres not only one `script_execute` command, theres also `script_execute_client` that only executes code on your client.

You can also access the SERVER or the CLIENT inside a script.

```squirrel
if ( SERVER )
{
	printl("Hi, im the server")
}
if ( CLIENT )
{
	printl("Hi, im the client")
}
```
try using the command `script_execute test`, if you see the server message you are accessing the server, then try `script_execute_client test` if you see the client message you are accessing the client.

!!! note "Shared"

	If you dont put any of those two, you are accesing the SHARED state, both client and server.
	
### Null Checks

When something doesnt exist it becomes `null` and VScript doesnt like when something doesnt exist, so you must do checks.

```squirrel
local player = GetPlayerByIndex(2);

printl(player.GetHealth());
```

this code looks for the SECOND player in the server, since theres no second player it leaves an error on console.

```squirrel
AN ERROR HAS OCCURRED [the index 'GetHealth' does not exist]

CALLSTACK
*FUNCTION [main()] test.nut line [3]

LOCALS
[player] NULL
[vargv] ARRAY
[this] TABLE
Error running script named test
```

Something like this should appear on console, when it says that the index `GetHealth` doesnt exist, its because theres no entity with it.

Try using the same script but put this command on console `jbmod_bot_add` in this case it actually prints the health of the second player, the bot.

The script shouldnt have errors even if the second player is desconected so we are going to make a null check

```squirrel
local player = GetPlayerByIndex(2);

if (!player)
	return

printl(player.GetHealth());
```

In this case we use the return feature with an if statement, `if (!player)` it means that if the player doesnt exist, stop the function, if you try using this in a game with no second player you shouldnt recieve any errors.

This is necessary so your scripts dont break mid-game.

### Debugging

You can use the `printl()` to print stuff to console and debug if it works, as we already learned, by just using printl you can see which parts of the code get executed at what moment, it helps to understand logic errors.

### Includes

You can include other `.nut` files inside the one you are making by simply using the function `IncludeScript()`

```squirrel
// this is the main file located inside the vscripts folder inside a custom folder called "myscripts"
// this file is called SayHi.nut

hello <- "hi";

function SayHi()
{
	printl(hello);
}
```

This is the file that we are going to include.

```squirrel
IncludeScript("myscripts/SayHi.nut");

hello = "bye"

SayHi();
```

This is the file that executes the included code, as you can see, when we include a script we can access its functions and variables, in this case we modified the variable so instead of saying hi it says goodbye.

!!! warning "Must Know"

	You cant use local variables from other scripts, it only works with global variables.
	
### Loops

You can do some flashy stuff with loops, specially finding all entities of something:

```squirrel
local ent = null;

while ((ent = Entities.FindByClassname(ent, "prop_physics")) != null)
{
    ent.Destroy()
}
```

This is a cleanup script, as we learned before it looks for all entities named of the class `prop_physics` using the function `Entities.FindByClassname` until it finds them all and it destroys them.

```squirrel
local ent = null;

while ((ent = Entities.FindByClassname(ent, "prop_*")) != null)
{
    printl(ent)
}
```
You can also use `*` to search for all the stuff after what you written, even you can put the `*` by itself and it will search for EVERY entity.

```squirrel
for (local i = 1; i <= MaxClients(); i++)
{
    local player = GetPlayerByIndex(i);

    if (!player)
        continue;

    printl(player.GetPlayerName());
	printl(player);
}
```

You could use the same method from the last example but this one is more customizable, in this case it checks for all players using a for loop that stops at the maxplayers number using the function `MaxClients()`, so if you have 32 it will do it 32 times.

It prints the players name and its index.

## EntFire

Every entity has inputs, and EntFire can trigger these inputs, it works as a command but also as a function

```squirrel
EntFire(targetName, action, value, delay, activator);
```

The target is what we want to change, the action is what we are changing, the value is what changes, the delay its self explanatory (in seconds), and the activator is who triggers it.

```squirrel
local ent = null;

EntFire("prop_physics*", "DisableMotion", "", 0, null);
```

This will freeze every prop_physics on the server, this is what the physgun does to the prop you are holding when you right click, so if you grab them with it they unfreeze.

This version of entfire is specially good for map making, because it uses a targetname we can tell it to do specific stuff to an entity with a specific name:

```squirrel
EntFire("door_main", "Open", "", 0);
```

This triggers the input "Open" to any entity called "door_main".

```squirrel
EntFireByHandle(handle, string, string, float, handle, handle)
```

Instead of using the name of the entity we are looking for, we use a handle. Its more precise and flexible.

```squirrel
local player = null;

while ((player = Entities.FindByClassname(player, "player")) != null)
{
    EntFireByHandle(player, "Color", "255 128 0", 0, null, null);
}
```

This changes every player color to JBMod Orange.

As you can see we use the handle instead of the name, this works better with functions like `OnPlayerSpawn(player)` because player is a handle and not a string.

## NetProps

NetProps (known as Networked Properties) are the internal variables of an entity. Some of them have a public function, while others dont.

A player can have health, armor, speed, position, weapons, active weapon, render color and tons of other stuff, everything can be modified through NetProps

```squirrel
player.SetHealth(100);
```

This is the public function to set the health of a player.

```squirrel
NetProps.SetPropInt(player, "m_iHealth", 100);
```

This is how you do it with NetProps, in this case we should always use the public function because is the secure way of doing, but. What happens when you dont have any function for whatever you want to do?

```squirrel
NetProps.GetPropString(player, "m_szNetworkIDString")
```

right know there is NO function for getting a steam id, so this is the way of doing it.

```squirrel
for (local i = 1; i <= MaxClients(); i++)
{
    local player = GetPlayerByIndex(i);

    if (!player)
        continue;

	local SteamID = NetProps.GetPropString(player, "m_szNetworkIDString")
	
	if (SteamID == "[U:1:1201533202]")
		printl("The user is Cayama0811")
	else if (SteamID == "BOT")
		printl("The user is a BOT")
	else
		printl("The user is not Cayama0811, nor a BOT.")
		
	printl(SteamID)
}
```

In this case this code checks all the players on the game (like a previous example) and then it gets the string (text) of the SteamID of all the players, if the SteamID equals to mine, yes my SteamID, the one from the guy who writes all this stuff, then it will print that is mine, if the string equals to "BOT" then it will print that the ID doesnt exist and its actually a BOT, and then every other player that isnt me or a bot will get a diferent print. At the end it prints the actual SteamID.

!!! warning "Must know"
	
	m_szNetworkIDString only works with SteamID3, so the format should look something like this `[U:1:1201533202]`, you can visit [this](https://steamid.io/) website to check your SteamID3.

You can try changing the SteamID3 and the name to check how it works.

This is a very secure way of making player specific scripts, like admin commands and such.

### Types of NetProps

As you saw before we used `SetPropInt()` and `GetPropString()` but theres more than just that, we can get and set any value from the NetProps.

```squirrel
GetPropBool
GetPropEntity
GetPropFloat
GetPropInfo
GetPropInt
GetPropString
GetPropType
GetPropVector

HasProp

SetPropBool
SetPropEntity
SetPropFloat
SetPropInt
SetPropString
SetPropVector
```

With all of these functions you can Get/Set NetProps as you please, try experimenting with a few of them to check what you can do.

## Tables

As you saw in the vscript syntax guide we can also use tables.

```squirrel
for (local i = 1; i <= MaxClients(); i++)
{
    local player = GetPlayerByIndex(i);

    if (!player)
        continue;

    local playerData =
    {
        name = player.GetPlayerName(),
        kills = player.GetFragCount(),
        deaths = player.GetDeathCount(),
        SteamID = NetProps.GetPropString(player, "m_szNetworkIDString")
    };
    
    if (playerData.SteamID == "BOT")
        continue;

    foreach (key, value in playerData)
    {
        printl(key + " = " + value);
    }
}
```

This is a simple example that checks for everyone in the game, it prints their name, how much kills theyve got, their death count and their SteamID, useful to get a lot of stats from any player quickly, it also skips bots altogheter.

## Fun Scripts

This is what you all have been waiting for, the fun.

```squirrel
if (SERVER)
{
    printl("Scanning all players...");

    local p = null;

    while (p = Entities.FindByClassname(p, "player"))
    {
        if (!p.IsPlayer())
            continue;

        printl("Player found: " + p.entindex());

        p.TakeDamage( 100, 2, p )

    }
}
```

This simple script works as a global kill, it will kill any player that is alive.

```squirrel
RegisterScriptGameEventListener("player_say");

function OnGameEvent_player_say(event)
{
    msg <- event.text;
    player <- GetPlayerFromUserID(event.userid);
	
    if (!player)
        return;

    MessageSound("hello", "hi", "*vo/npc/male01/hi01.wav");
    MessageSound("police", "cps", "*vo/npc/male01/cps01.wav");
    MessageSound("yes", "yeah", "*vo/npc/male01/yeah02.wav");
    MessageSound("nice", null, "*vo/npc/male01/nice.wav");
}

function MessageSound(keyword1, keyword2, soundpath)
{
    if (!player || !player.IsValid())
        return;

    local text = msg.tolower();

    if (text.find(keyword1) != null || (keyword2 != null && text.find(keyword2) != null))
    {
        player.PrecacheScriptSound(soundpath);
        player.EmitSound(soundpath);
    }
}

__CollectGameEventCallbacks(this);
```

This is a more advanced one, it will play the sound that you tell it to when a player types the keyword you want, for example if a player types "yes" or "yeah" they will emit the "yeah" voiceline from the hl2 citizens. This script is optimized to use a specific function so it doesnt get bloated.

```squirrel
local old_kill = OnPlayerKilled;

function OnPlayerKilled(player, attacker)
{
	old_kill(player, attacker)
	local maxhealth = player.GetMaxHealth();
	attacker.SetHealth(maxhealth);
}
```

Health on kill! Very simple.

```
local ent = null;

while ((ent = Entities.FindByName(ent, "prop_roller")) != null)
{
    local pos = ent.GetOrigin();
    local ang = ent.GetAngles();
    
    local propTable = 
    {
        targetname = "roller",
        origin = pos,
        angles = ang,
        model = "models/roller.mdl"
    }
    
    local myProp = SpawnEntityFromTable("prop_dynamic_override", propTable);
    myProp.SetSolid(0);
}
```

This is more important than fun, but ill still add it, in this case it will create a prop in the position of any entity called "prop_roller", this allows us to create custom entities.

In hammer you will create an info_target entity and name it "prop_roller" everytime you run the script a roller will spawn in the position of the entity without physics nor collision.

```squirrel
for (local i = 1; i <= MaxClients(); i++)
{
    local player = GetPlayerByIndex(i);

    if (!player)
        continue;

	NetProps.SetPropString(player, "m_szNetname", "Snakez")

}
```

This will kinda change all players name into another one, in this case everyone will be named Snakez. It only seems to work for chat though.

## Challenges

Work in progress.

## Whats next?

We will learn how to make gamemodes/weapons with all the stuff we learned through this article, we will learn special functions/callbacks from gamemodes/weapons and we will learn how to use vscript for mapping.

