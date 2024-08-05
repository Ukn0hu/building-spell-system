```
 _____     _ _   _ _            _____         _ _    _____         _             
| __  |_ _|_| |_| |_|___ ___   |   __|___ ___| | |  |   __|_ _ ___| |_ ___ _____ 
| __ -| | | | | . | |   | . |  |__   | . | -_| | |  |__   | | |_ -|  _| -_|     |
|_____|___|_|_|___|_|_|_|_  |  |_____|  _|___|_|_|  |_____|_  |___|_| |___|_|_|_|
                        |___|        |_|                  |___|                  
```

# 🏗️ Building Spell System

Place a gameobject in the game using an AoE spell and build something cool with greater precision and ease !

Use player orientation for gameobject orientation.

### ⚠️ Warning

All gameobject are not supposed to be spawned. (like large building) 
Thus, some spell models show you a little white and blue cube while casting. 

### ⚙️ Tweaks

- [DBC documentation](https://wowdev.wiki/DBC)
- [Spell dbc documentation](https://wowdev.wiki/DBC/Spell)

Obviously it's not really documented so my advice is to see how other spells work

Edit spawn duration: 
>Here, gameobjects has no duration, take a look at the column `DurationIndex` in `Spell.dbc` to change that. 
Pay attention to documentation if you find, it's an index not a time value. For example : "32" value means 3 seconds.

Edit spell range:
>Take a look at the column `RangeIndex` in `Spell.dbc`, 20 meters -> "3" value.

# ✨ Installation

We will need to patch `Spell.dbc` in game client and server's dbc.

- [MPQ Editor](http://www.zezula.net/en/mpq/download.html)
- [DBC Editor](https://github.com/WowDevTools/WDBXEditor/releases)

 We'll do :

- sql import is for creating all gameobject entities.
- patching `GameObjectDisplayInfo.dbc` is used to be able to see all models in gameobject. In addition with `patch-D.MPQ` for the game client.
- patching `Spell.dbc` is to register to server's dbc that we have created all building spells. Same, associated MPQ patch is here for game client.

>For information: DBC and MPQ patches here are the same thing. We just must have both server's files and game client files synchronized.


Step 1 and 2 are not mine, I taked this from [open-wow repo](https://github.com/Open-Wow/archive/tree/main/Fait/14-all-buildings-models-etc-as-gameobjects).


1. Import `gameobject_template.sql` to the world database (29324 gameobjects in total)
2. Allows you to see and add all the 3d models of the game in the form of a gameobject.
    1. Put the `patch-D.MPQ` file in Wow client (Wow 3.3.5/Data/\<here>)
    2. Replace `GameObjectDisplayInfo.dbc` in server's DBC folder
3. Ok, now with all the new gameobjects added, we create its associated spell
    1. Backup/Copy your server's DBC `Spell.dbc` somewhere just in case
    2. Server's DBC edit
        1. Start `WDBX Editor.exe`
        2. Open your `Spell.dbc` and import (CSV) all spells with `Gameobject_spell.csv`
        3. Save as `Spell.dbc` in your dbc directory 
    3. MPQ edit
        1. Start `MPQEditor.exe` and create a mpq file with default config. 
        (the file must respect this notation: `patch-[LETTER OR A NUMBER]` example: `patch-4` or `patch-U`, alphabetical or digital order and not before existing patch/files)
        2. Create a folder named `DBFilesClient`
        3. Drag'n'drop your new `Spell.dbc` into
        4. Close the MPQ properly in the editor.
        5. Put it in Wow client
4. Done ! go check if it's working

>Pay attention to localized game client, you need to extract your DBC yourself instead of download enUS dbc files online.
>
>I've don't see any core extractors binaries online so you need to generate them.

### 🔥 How to generate core extractors (optionnal for enUS game client)
**You need to be able to compile your core server.**
1. Open CMake and find `"TOOLS"` in configuration, tick it.
2. Click on `Configure` then `Generate` then open VisualStudio
3. Now, its time to build the server, right click to `ALL_BUILD->generate`

The extractors will be generated in the directory where your `authserver.exe` and `worldserver.exe` are generated.
(`azerothcore/build/bin/RelWithDebInfo/...` or `azerothcore/build/bin/Release/...`)

# 📖 Usage
### 🏞️ Ingame
Now ingame you can `.lookup spell <gameobject_name>` and `.learn ID` to try some building. (shift+left click just after you write `.learn ...` to do not have to write the spell ID)
Short version: `.lo sp <gameobject_name>`
⬆️ Use ALT+UpArrow to get older chat msg when taping msg ingame. It's helps.

### 👍 Searching models
You can use wow.tools or WoW Model Viewer 0.7.x (supporting WOTLK).
I used the genuine name for each model to make it easy to search. You are free to change.

### 👍 Use added gameobject
Gameobjects ids are from : 500000 and 539743 and 4000000 and 4002169

### 👍 Add custom building spell
Use the `template.csv` to create your own building spell.
With this, you have to follow the step 3 of the installation process to add your own spell.


| TO DO CHANGE                  | Description                     |
| ----------------------------- | ------------------------------- |
| [SPELL ID]                    | id of your spell                |
| [DurationIndex HERE]          | go check dbc documentation      |
| [RangeIndex HERE]             | go check dbc documentation      |
| [GAMEOBJECT ENTRY]            | id of the gameobject to spawn   |
| [enUS NAME SPELL]             | original name spell             |
| [frFR NAME SPELL]             | french name spell               |
| [enUS DESCRIPTION SPELL]      | original description spell      |
| [frFR DESCRIPTION SPELL]      | french description spell        |
| [enUS AURA DESCRIPTION SPELL] | original aura description spell |
| [frFR AURA DESCRIPTION SPELL] | french aura description spell   |


For further usage you can :
- create a c++ spell script or lua script to 
	- persist it in database when you spawn a gameobject.
	- create a logic with player
	- set a spell on item and create limited use
	- vendors
	- player housing
	- etc

# 🫶 Credit 

- Saperlipopute (✌️)
- Dark_Spyro_003 (for original content of gameobjects patch)