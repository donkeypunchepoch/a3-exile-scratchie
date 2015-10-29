## Scratchies (*lottery like* minigame for Exile Mod) v0.2

![Buy a scratch, get the prize](buyget.jpg "Buy a scratch, get the prize")
![Use the Scratchie](usexm8.jpg "Use the scratchie in XM8")

Videos: [PART #1](https://www.youtube.com/watch?v=zVPXYhhYrbU) [PART #2](https://www.youtube.com/watch?v=2MC45ycnOkc) - thanks to Rythron

[Changelog](CHANGELOG.txt)

This extension is licensed under the Arma Public Licence - Author: ole1986

## Installation
### Required Tools

+ PBO Manager (I use cpbo from http://www.kegetys.fi/category/gaming/armamods/)
+ Notepad++ or any other Text Editor (https://notepad-plus-plus.org/)
+ Exile Mod version 0.9.35

### Prerequisite

Before you can start it is necessary to **unpack** the &lt;MissionFile&gt; and the &lt;ExileServer&gt; using your favorite pbo manager

Placeholder            | File
---------------------- | -------------
&lt;MissionFile&gt;    | Exile.&lt;Mapname&gt;.pbo (E.g. Exile.Altis.pbo )
&lt;ExileServerMod&gt; | @ExileServer Exile server mod folder located in game directory.

### Database setup

+ Import the mysql file *mysql\lottery.sql* into your exile database (through mysql or phpmyadmin for example).
+ Copy and repalce the *mysql\exile.ini* with the file located in &lt;ExileServerMod&gt;\extDB\sql_custom_v2\exile.ini

### Exile Mission modifications

+ Copy the folders *MissionFile\overrides* and *MissionFile\addons* into your &lt;MissionFile&gt; directory
+ Modify the &lt;MissionFile&gt;\config.cpp and add the below line inside `class CfgExileCustomCode`

```
ExileClient_gui_xm8_slide_apps_onOpen = "overrides\ExileClient_gui_xm8_slide_apps_onOpen.sqf";
```

+ Modify the &lt;MissionFile&gt;\description.ext and add the below line inside  `class CfgRemoteExec -> class Functions`

```
class ExileServer_lottery_network_request { allowedTargets=2; };
```

### Exile Server modifications

**CHANGED IN VERSION >= 0.2**

+ Copy the ExileServerMod\scratchie_server.pbo into your &lt;ExileServerMod&gt;\addons directory

*PLEASE MAKE SURE YOU HAVE REMOVED ALL PREVIOUS FILES FROM THE exile_server.pbo*


### Buy / Get Prize code line

*The below code can be used to buy a scratchie from any object you decide*

`["buy",ExileClientSessionId, player, ""] remoteExecCall ["ExileServer_lottery_network_request", 2];`

*The below code can be used to get the prize, when player has won*

`["get",ExileClientSessionId, player, ""] remoteExecCall ["ExileServer_lottery_network_request", 2];`

**Example implementation to Buy/Get Prize from the office (&lt;MissionFile&gt;\initPlayerLocal.sqf)**
```
_officeTrader = [
    "Exile_Trader_Office",
    "GreekHead_A3_04",
    ["InBaseMoves_SittingRifle1"],
    [0, -0.15, -0.45],
    180.008,
    _chair
]
call ExileClient_object_trader_create;
// add the buy scratchie and get prize as action menu to the office trader
_officeTrader addAction ["<t color='#FFFFFF'>Buy Scratchie(200,-)</t>", { ["buy",ExileClientSessionId, player, ""] remoteExecCall ["ExileServer_lottery_network_request", 2]; }];
_officeTrader addAction ["<t color='#c72651'>Get Prize!</t>", { ["get",ExileClientSessionId, player, ""] remoteExecCall ["ExileServer_lottery_network_request", 2]; }];
```

### Battleye

+ add the below to the end of line `7 remoteexec` (line number 19?!) in your scripts.txt

 `!="remoteExecCall ["ExileServer_lottery_network_request\","`

+ add the below to the end of line `7 ""` in your remoteexec.txt

 `!"ExileServer_lottery_network_request"`
