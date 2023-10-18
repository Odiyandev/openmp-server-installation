# open.mp Server Installation

<a href="https://www.open.mp"><img src="/screenshots/open-mp-logo.png" width=50 height=50/></a>
[Open Multiplayer](https://www.open.mp) server installation tutorial!

**This tutorial is for those who want to transfer their gamemode from SA:MP server to open.mp server**

> [!NOTE] 
> *If you are using the FCNPC plugin, please stop for now because this plugin does not work for open.mp currently.*

## Steps

- [Step 1](#step-1)
- [Step 2](#step-2)
- [Step 3](#step-3)
- [Step 4](#step-4)
- [Step 5](#step-5)
- [Step 6](#step-6)
- [Step 7](#step-7)
- [Step 8](#step-8)
- [Step 9](#step-9)
- [Step 10](#step-10)
- [Step 11](#step-11)
- [Step 12](#step-12)

## Step 1

Download the latest version of open.mp server files from https://github.com/openmultiplayer/open.mp/releases

<kbd>![](/screenshots/Screenshot%20(1).png)</kbd>

- `open.mp-win-x86.zip` **Windows** Server
- `open.mp-linux-x86.tar.gz` **Linux** Server

## Step 2

Extract the `.zip` or `.tar.gz` archive contents on your disk

<kbd>![](/screenshots/Screenshot%20(3).png)</kbd>

## Step 3

Create these folders in the **Server** directory:
- filterscripts
- gamemodes
- plugins
- scriptfiles
  
<kbd>![](/screenshots/Screenshot%20(4).png)</kbd>

## Step 4

Put your gamemode `.pwn` file in the **gamemodes** folder

## Step 5

Put required includes (e.g. `sscanf2.inc`, `streamer.inc`) in the **qawno/include** folder

## Step 6

Put required plugins (e.g. `sscanf.dll`, `streamer.dll`) in the **plugins** folder

*______________________________________________________________________*

> [!IMPORTANT] 
> If you use the following plugins in table, you must put a version of the plugin that is compatible with omp!
> 
> Put the following plugins in the **../components** folder, not in the **../plugins** folder!

| Plugin  | OMP |
| ------ | --- |
| Pawn.CMD  | https://github.com/katursis/Pawn.CMD/releases/tag/3.4.0-omp |
| Pawn.RakNet  | https://github.com/katursis/Pawn.RakNet/releases/tag/1.6.0-omp |
| sampvoice  | https://github.com/AmyrAhmady/sampvoice/releases/tag/v3.1.5-omp |
| SKY  | Use Pawn.RakNet instead |
| YSF  | You don't need YSF because open.mp already declared most of the same natives |
| FCNPC  | Currently not supported |

## Step 7

Open the qawno IDE program located at **Server/qawno/qawno.exe**

<kbd>![](/screenshots/Screenshot%20(5).png)</kbd>

## Step 8

Press **CTRL + O** then open your gamemode `.pwn` file in the **../gamemodes** folder

## Step 9

Find 
```pawn
#include <a_samp>
```
replace with
```pawn
#include <open.mp>
```
then press **F5** to compile.

> [!NOTE]
> If you are get error or warning, See [Compiler errors and warnings](#compiler-errors-and-warnings)

## Step 10

Open **config.json** file with Notepad or other IDEs

<kbd>![](/screenshots/Screenshot%20(9).png)</kbd>

## Step 11

Edit **config.json**

<kbd>![](/screenshots/Screenshot%20(11).png)</kbd>

Find
```json
"main_scripts": [
    "test 1"
],
```
replace with
```json
"main_scripts": [
    "your_gamemode_amx_file_name"
],
```

*______________________________________________________________________*

Find
```json
"legacy_plugins": [],
```
Specify required plugins
```json
"legacy_plugins": [
    "crashdetect",
    "mysql",
    "sscanf",
    "streamer",
    "PawnPlus",
    "pawn-memory"
],
```

*______________________________________________________________________*

Find
```json
"side_scripts": [],
```
Specify your filterscripts
```json
"side_scripts": [
    "filterscripts/file_name"
],
```

Press **CTRL + S** to save changes.

## Step 12

Run the server

- **Windows**

Open the `omp-server.exe` program

<kbd>![](/screenshots/Screenshot%20(10).png)</kbd>

- **Linux**

```bash
./omp-server
```

## Compiler errors and warnings
- **warning 213: tag mismatch: expected tag "?", but found none ("_")**:

e.g.

```pawn
SetPlayerControllable(playerid, 1);
// ->
SetPlayerControllable(playerid, true);

TextDrawFont(textid, 1);
// ->
TextDrawFont(textid, TEXT_DRAW_FONT:1);
```

But you can ignore it for now:
```pawn
#define NO_TAGS
#include <open.mp>

// If the warning still occurs
// Use #pragma warning disable 213
```

*______________________________________________________________________*

- **warning 234: function is deprecated (symbol "TextDrawColor") Use `TextDrawColour**

Press **CTRL + F** in qawno and replace all `TextDrawColor` to `TextDrawColour`

<kbd>![](/screenshots/Screenshot%20(7).png)</kbd>

> [!NOTE]
> There are many other deprecated functions, you just need to replace them with the new function name.

*______________________________________________________________________*

- **warning 214: possibly a "const" array argument was intended: "?"**
- **warning 239: literal array/string passed to a non-const parameter**

e.g.
```pawn
public MyFunction(string[])
```
->
```pawn
public MyFunction(const string[])
```

*______________________________________________________________________*

- **error 025: function heading differs from prototype**

e.g.
```pawn
public OnVehicleMod(playerid, vehicleid, componentid)

public OnVehiclePaintjob(playerid, vehicleid, paintjobid)

public OnVehicleRespray(playerid, vehicleid, color1, color2)

public OnPlayerEditAttachedObject(playerid, response, index, modelid, boneid, Float:fOffsetX, Float:fOffsetY, Float:fOffsetZ, Float:fRotX, Float:fRotY, Float:fRotZ, Float:fScaleX, Float:fScaleY, Float:fScaleZ)
```
->
```pawn
public OnVehicleMod(playerid, vehicleid, component)

public OnVehiclePaintjob(playerid, vehicleid, paintjob)

public OnVehicleRespray(playerid, vehicleid, colour1, colour2)

public OnPlayerEditAttachedObject(playerid, EDIT_RESPONSE:response, index, modelid, boneid, Float:fOffsetX, Float:fOffsetY, Float:fOffsetZ, Float:rotationX, Float:rotationY, Float:rotationZ, Float:scaleX, Float:scaleY, Float:scaleZ)
```

## Help
If you still have issues running the server, please join the [official open.mp Discord server](https://discord.gg/samp)

https://discord.gg/samp

Ask at [#openmp-support](https://discord.com/channels/231799104731217931/966398440051445790)
