# How to create your own career in Flight Build Sandbox Simulator

## Introduction

The game supports the creation and import of third-party careers. This documentation explains in detail the principles of career creation, as well as the project structure to simplify the development and integration process.
Career files are located at the following path: `Android/data/com.GullSoft.AircraftSimulator/files/Career/<career name>`.

## Project Structure

The career project is organized into several folders, each containing resources for various aspects of the game.

⚠️ Be careful when creating the basic folders. Missing or incorrectly named base folders/files in the career structure will result in game errors. Below is the required folder and file architecture. ⚠️

```plaintext
/Main Career Folder
│
├── /Aircrafts
├── /Maps
├── /Cache
├── /Demo
│   ├── NameKey.txt
│   ├── DescriptionKey.txt
│   └── icone.(jpg/png)
├── /Localization
│   └── English.txt
├── /Skripts
│   ├── SaveKey.txt
│   ├── Main.lua
│   └── Exit.lua
├── /Textures
└── /Audios
```

### Folders:

- **Aircrafts**  
  Contains save files of all builds.

- **Maps**  
  Contains save files of all maps.

- **Audios**  
  Contains music files. Files must have the `.ogg` format.

- **Cache**  
  Temporary files and data created during the game process.

- **Demo**  
  Contains three elements: `DescriptionKey.txt`, `icone.jpg`, `NameKey.txt`. (File names cannot be changed.)
  - `DescriptionKey.txt` contains the localization key for the basic career description in the list of other careers.
  - `NameKey.txt` contains the localization key for the career name.
  - `icone.jpg` — the career icon image in 16:3 format, for example, 800x150.

- **Localization**  
  Localization files for translating the game text into different languages.

- **Skripts**  
  Contains scripts responsible for game logic and managing the career process.

- **Textures**  
  Contains images in `.png` or `.jpg` format.

## Explanation of Basic Mechanics

### Console

To open the console, touch the screen with four fingers simultaneously. In the console, you can see Lua syntax errors and some other errors.

### Missions and Maps

Place the maps and buildings used in the career into the corresponding folders: `Aircrafts` and `Maps`. To reduce the file size of the career in the game, there is a system that finds textures in the first save of a specific career. For example, if there are 5 maps that use the same textures, you can delete all these textures from the Textures folder in almost all builds, leaving them only in one.

As you may have seen in the buildings and maps editor, there is an item for sorting missions. These elements are used to create maps with missions. Read the description of each element, especially in the map editor, to understand what each does. To do this, click on the element in the details list.

### Data

When a mission with a new key is loaded for the first time, the following data is always set: Completed Mission IDs [0], Explored Vehicle IDs [0], Selected Vehicle ID: 0. Thus, the vehicle with ID 0 (starting vehicle) will be immediately acquired and selected.

### Localization

FBS (Flight Build Sandbox Simulator) uses a custom localization system. As you may have seen, there is a Localization folder in the game files. Currently, it contains the files English.txt and Russian.txt. These are the localization files. They store elements in the format `Key_1 = Value 1`. When creating a key, use `_` instead of spaces.

You can also create a file for a new language, for example, `German.txt`, and rename it based on `English.txt`. After translating all values into German, you will be able to easily switch the language in the game settings. An important aspect is that the localization folder of the career must contain files with the same names as in the main folder for the system to correctly complement the base localization files. If you do not plan to localize into different languages, the career localization folder must include the `English.txt` file, as English is the main language of the game.

Example `Heaven's legacy`:
```plaintext
Career_Name = Heaven's legacy
Career_Description = In the heart of a forested mountain area, far from the bustle of the city, the hero decides to start a new chapter of his life. Having inherited an airplane from his ancestors, he leaves the noisy city and goes to the clean air and picturesque landscapes. Now his task is not only to explore the magnificent natural spaces, but also to become a pilot who solves various tasks: delivering cargo, transporting passengers, and helping local residents.
Specification_Description_B = Cargo Transportation
Specification_Description_P = Passengers Transportation
Cessna_172_Description = The Cessna 172 is a lightweight, single-engine airplane ideal for transporting small cargo. With its robust design and easy handling, the Cessna 172 is used to transport small boxes and other lightweight items to remote locations.
Robinson_R44_Description = The Robinson R44 is a compact light helicopter designed to carry up to one passenger. Thanks to its maneuverability and reliability, the R44 is well suited for flying to hard-to-reach places and performing small transport tasks.
Mission_Name_1 = First flight
Mission_Description_1 = Your first mission on an old but reliable airplane. You need to get used to the controls and get a feel for the machine. Just take a test flight to the nearest landing site. This easy task will help you get used to the controls and get a feel for the airplane in the sky.
```

Thus, the localization structure ensures ease of adding new languages and supports flexibility in changing text in the game. In the future, we can retrieve values by key in the Lua script, allowing dynamic text retrieval based on the selected language.

### Scripting

In FBS, the Lua programming language is used to create careers. Script files are stored in the Skripts folder. They can have the `.txt` or `.lua` extension. The Skripts folder must contain the files `Main.(lua, txt)`, `Exit.(lua, txt)`, and `SaveKey.txt`.

SaveKey.txt is the file that stores the key for saving career data. You can replace it, and then all progress in the career will be lost. Try to create this key from random letters and numbers to avoid repetitions in other careers. For example: `sdfgsd2323ri3k4jr23ioh423ihnf85f3o`.

Main.lua is the file that runs when the career menu is loaded. Its basic structure is as follows:

```lua
-- Your main code here

-- System
UpdateTreeScrollArea()
InitializeTreeData()
SetTotalMissionCount(10) -- your mission count
UpdateFastList()
```

Exit.lua — is the file that runs before returning to the main menu. It can be empty, but you can use it, for example, to save the current playing time of the music so it can resume from the same point.

#Example Career `Heaven's legacy`

Main.lua
```lua
LoadMap('Base Lobby')

SetMoneySymbol('✪')

Read('Aircraft Specification Types')

Read('Tech Tree')

Read('UI Colors')

Read('Map')

--System
UpdateTreeScrollArea()
InitializeTreeData()
SetTotalMissionCount(8)
UpdateFastList()
```

Aircraft Specification Types.lua
```lua
RegistryAircraftSpecificationType('B', 'Specification_Description_B', HexColor('#46c43b'))
RegistryAircraftSpecificationType('P', 'Specification_Description_P', HexColor('#3badc4'))
```

Tech Tree.lua
```lua
arrowNotPassedColor = HexColor('#c2c2c2')
arrowPassedColor = HexColor('#cdf4d5')
techItemColor = HexColor('#52795e')

SpawnTechTreeItem(0, 0, 'Cessna 172', 0, 
    {SpecificationType('P', 1), SpecificationType('B', 1)}, 
    'Cessna 172', 'Cessna_172_Description', 
    Pos2(0, 0), techItemColor)

SpawnArrow(Pos2(0, 0), 'Left', IsAircraftPurchased(0) and arrowPassedColor or arrowNotPassedColor)

SpawnTechTreeItem(1, 0, 'Robinson-R44', 3000,
    {SpecificationType('P', 2)}, 
    'Robinson-R44', 'Robinson_R44_Description', 
    Pos2(1, 0), techItemColor)
```

UI Colors.lua
```lua
colorMain = HexColor('#07493b')
colorInfo = HexColor('#028e51')

SetUiElementColor('Tech Tree Fone', HexColor('#213026'))

SetUiElementColor('Play Button', colorMain)
SetUiElementColor('Tech Tree Button', colorMain)
SetUiElementColor('Exit Button', colorMain)
SetUiElementColor('Tech Tree Back Button', colorMain)
SetUiElementColor('Mission Back Button', colorMain)
SetUiElementColor('Money Tab', colorInfo)
SetUiElementColor('Info Tab', colorInfo)
```

Map.lua
```lua
SetMapSprite(GetSprite('map.jpg'), 1, HexColor('#4b96ba'))

SetMissionItemCompletedColor(HexColor('#000000'))
	
SpawnMissionItem(1, 0, 'Mission 1', 100, 
	{SpecificationType('P', 1)}, 
	'Mission_Name_1', 'Mission_Description_1',
	Pos2(-75, -275))
	
SpawnMissionItem(2, 1, 'Mission 2', 200, 
	{}, 
	'Mission_Name_2', 'Mission_Description_2',
	Pos2(-175, -225))
	
SpawnMissionItem(3, 2, 'Mission 3', 300, 
	{SpecificationType('P', 1), SpecificationType('B', 1)}, 
	'Mission_Name_3', 'Mission_Description_3',
	Pos2(25, -25))
	
SpawnMissionItem(4, 3, 'Mission 4', 400, 
	{SpecificationType('B', 1)}, 
	'Mission_Name_4', 'Mission_Description_4',
	Pos2(-225, -25))
	
SpawnMissionItem(5, 4, 'Mission 5', 500, 
	{SpecificationType('P', 1)}, 
	'Mission_Name_5', 'Mission_Description_5',
	Pos2(197, 342))
	
SpawnMissionItem(6, 5, 'Mission 6', 600, 
	{}, 
	'Mission_Name_6', 'Mission_Description_6',
	Pos2(300, 335))
	
SpawnMissionItem(7, 6, 'Mission 7', 700, 
	{SpecificationType('P', 2)}, 
	'Mission_Name_7', 'Mission_Description_7',
	Pos2(250, 280))
	
SpawnMissionItem(8, 7, 'Mission 8', 800, 
	{SpecificationType('P', 2)}, 
	'Mission_Name_8', 'Mission_Description_8',
	Pos2(182, 205))

SetDependencieRunLines()
```

## Syntax

### Basic Data Types
- **nil** - a missing value, used to denote emptiness.
- **boolean** - logical values: true and false.
- **number** - numeric values (e.g., 42, 3.14).
- **string** - strings enclosed in single or double quotes (e.g., 'Hello', "World").
- **table** - data structures similar to arrays or dictionaries, created with curly braces (e.g., {1, 2, 3} or {key = "value"}).
- **function** - functions that can be assigned to variables and passed as arguments.

### Value Passing
- **Numbers**: passed as they are, e.g., SetTotalMissionCount(8).
- **Strings**: passed as string literals, e.g., LoadMap('Base Lobby').
- **Tables**: passed as literals, e.g., {SpecificationType('P', 1), SpecificationType('B', 2), SpecificationType('A', 5)}.
- **Functions**: can be passed as arguments.

### Description of Available Methods

**Print** - output the string to the console.
```lua
Print(object message)
Print('Hello FBS!')
```

**Pos2** - creating a vector of two values.
```lua
Pos2(float x, float y)
Pos2(10.0, 20.0)
```

**HexColor** - converts a string with a hexadecimal color value to a Color object.
```lua
HexColor(string hex)
HexColor('#FF5733')
```

**Color** - creates a color from 4 values (r, g, b, a).
```lua
Color(float r, float g, float b, float a)
Color(1.0, 0.0, 0.0, 1.0)  -- Красный цвет
```

**SpecificationType** - Creates an AircraftSpecificationTypeAircraftItem object with the specified label and efficiency.
```lua
SpecificationType(string label, int efficiency)
SpecificationType('Speed', 75)
```

**Read** - executes the script in the `Skripts` folder by its name.
```lua
Read(string scriptName)
Read('mission_start')
```

**IsAircraftPurchased** - checks to see if the airplane was purchased using his ID.
```lua
IsAircraftPurchased(int id)
IsAircraftPurchased(1)
```

**AddMoney** - adds the specified amount of money to the player.
```lua
AddMoney(int amount)
AddMoney(500)
```

**LoadMap** - loads a map from the `Maps` folder by its name as a background in the menu.
```lua
LoadMap(string mapName)
LoadMap('forest_map')
```

**PlayAudio** - plays the audio file by its name.
```lua
PlayAudio(string audioName)
PlayAudio('menu music')
```

**GetAudioTime** - returns the current music playback time.
```lua
GetAudioTime()
```

**SetAudioTime** - sets the current time of the audio track.
```lua
SetAudioTime(float time)
SetAudioTime(25.0)
```

**GetAudioLength** - returns the length of the current audio track.
```lua
GetAudioLength()
```

**SetMoneySymbol** - sets the currency symbol.
```lua
SetMoneySymbol(string symbol)
SetMoneySymbol('$')
```

**RegistryAircraftSpecificationType** - registers the specification of the technique.
```lua
RegistryAircraftSpecificationType(string name, string label, Color color)
RegistryAircraftSpecificationType('Jet', 'Speed', Color(0.5, 0.5, 0.5, 1.0))
```

**SpawnTechTreeItem** - creates an element in the technology tree.
```lua
SpawnTechTreeItem(int id, int requiredId, string mapBuildName, int reward,
AircraftSpecificationTypeAircraftItem[] specs, string nameTag,
string descriptionTag, Vector2 position, Color color)

SpawnTechTreeItem(1, 0, 'base_tech', 100, {SpecificationType('Speed', 75)},
'JetSpeed', 'Increases jet speed', Pos2(10.0, 20.0), Color(1.0, 0.0, 0.0, 1.0))
```

**SpawnArrow** - creates an arrow in the technology tree with the specified position, direction, and color. Available direction commands: `Left`, `Right`, `Up`, `Down`.
```lua
SpawnArrow(Vector2 position, string direction, Color color)
SpawnArrow(Pos2(15.0, 25.0), 'Right', Color(0.0, 0.0, 1.0, 1.0))
```

**SetUiElementColor** - changes the color of a UI element by tag. 
Доступные теги: `Play Button`, `Tech Tree Button`, `Exit Button`, `Tech Tree Fone`, `Tech Tree Back Button`, `Money Tab, Info Tab`, `Mission Back Button`.
```lua
SetUiElementColor(string tag, Color color)
SetUiElementColor('missionPanel', Color(0.0, 1.0, 0.0, 1.0))
```

**SetTotalMissionCount** - sets the total number of missions.
```lua
SetTotalMissionCount(int count)
SetTotalMissionCount(10)
```

**Save... / Load...** - Methods of saving / loading data by key.
```lua
--String
SaveString(string key, string value)
SaveString('player_name', 'John')

LoadString(string key)
LoadString('player_name')

--Float
SaveFloat(string key, float value)
SaveFloat('volume', 0.75)

LoadFloat(string key)
LoadFloat('volume')

--Int
SaveInt(string key, int value)
SaveInt('score', 100)

LoadInt(string key)
LoadInt('score')
```

**GetSprite** - loads a sprite by name from the `Textures` folder.
```lua
GetSprite(string name)
GetSprite('map')
```

**SetMapSprite** - sets a sprite on the map with size and color.
```lua
SetMapSprite(Sprite sprite, int size, Color color)
SetMapSprite(GetSprite('map'), 64, Color(1.0, 1.0, 1.0, 1.0))
```

**SpawnHelpCoordinates** - Creates coordinates on a map to help.
```lua
SpawnHelpCoordinates(float step, int sideItemsCount, float size)
SpawnHelpCoordinates(5.0, 10, 3.0)
```

**CompleteMission** - completes the mission by its ID.
```lua
CompleteMission(int id)
CompleteMission(1)
```

**SpawnMissionItem** - creates a mission element with the specified parameters.
```lua
SpawnMissionItem(int id, int requiredId, string mapBuildName,
int reward, AircraftSpecificationTypeAircraftItem[] specs,
string nameTag, string descriptionTag, Vector2 position)

SpawnMissionItem(2, 1, 'new_mission', 200, {SpecificationType('Speed', 80)},
'JetBoost', 'Boosts jet speed', Pos2(12.0, 24.0))
```

**SetMissionItemUnavailableColor** - sets the color for the icons of unavailable missions.
```lua
SetMissionItemUnavailableColor(Color color)
SetMissionItemUnavailableColor(Color(0.5, 0.5, 0.5, 1.0))
```

**SetMissionItemCompletedColor** - sets the color for icons of completed missions.
```lua
SetMissionItemCompletedColor(Color color)
SetMissionItemCompletedColor(Color(0.0, 1.0, 0.0, 1.0))
```

**SetMissionItemUncompletedColor** - sets the color for the icons of unfinished missions.
```lua
SetMissionItemUncompletedColor(Color color)
SetMissionItemUncompletedColor(Color(1.0, 0.0, 0.0, 1.0))
```

**SetRunLinesColor** - sets the color of the running lines of mission dependencies.
```lua
SetRunLinesColor(Color color)
SetRunLinesColor(Color(1.0, 0.0, 0.0, 1.0))
```
