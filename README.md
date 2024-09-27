# How to create your own career in Flight Build Sandbox Simulator

## Введение

Игра поддерживает создание и импорт сторонних карьер. В этой документации подробно описаны принципы создания карьеры, а также структура проекта, чтобы упростить процесс разработки и интеграции.
Файлы карьеры находятся по пути Файлы карьеры находятся по следующему пути: `Android/data/com.GullSoft.AircraftSimulator/files/Career/<имя карьеры>`.

## Структура проекта

Проект карьеры организован в виде нескольких папок, каждая из которых содержит ресурсы для различных аспектов игры

⚠️Внимательно создавайте базовые папки. Отсутствие или неправильное наименование базовых папок/файлов в структуре карьеры приводит к ошибкам игры. Ниже указана обязательная архитектура папок и файлов⚠️

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
    ├── SaveKey.txt
│   ├── Main.lua
│   └── Exit.lua
├── /Textures
└── /Audios
```

### Папки:

- **Aircrafts**  
  Содержит файлы сохранений всех билдов.

- **Maps**  
  Содержит файлы сохранений всех карт.

- **Audios**  
  Содержит файлы музыки. Файлы должны обязательно иметь разрешение `.ogg`.

- **Cache**  
  Временные файлы и данные, которые создаются в процессе игры.

- **Demo**  
  Содержит три элемента: `DescriptionKey.txt`, `icone.jpg`, `NameKey.txt`. (Менять название файлов запрещено.) 
  - `DescriptionKey.txt` содержит ключ локализации для базового описания карьеры в списке других карьер. 
  - `NameKey.txt` содержит ключ локализации для названия. 
  - `icone.jpg` — картинка иконки карьеры в формате 16:3, например 800x150.

- **Localization**  
  Файлы локализации для перевода текста игры на разные языки.

- **Skripts**  
  Содержит скрипты, отвечающие за логику игры и управление процессом карьеры.

- **Textures**  
  Содержит изображения в формате `.png` или `.jpg`.

## Объяснение базовых механик

### Консоль

Чтобы открыть консоль, прикоснитесь к экрану 4 пальцами одновременно.

### Миссии и Карты

Поместите используемые в карьере карты и постройки в соответствующие папки: `Aircrafts` и `Maps`. Чтобы уменьшить размер файла карьеры в игре, существует система нахождения текстур в первом попавшемся сохранении конкретной карьеры. Например, если есть 5 карт, которые используют одни и те же текстуры, вы можете удалить все эти текстуры из папок Textures почти во всех билдах, оставив их лишь в одном.

Как вы могли видеть в редакторе построек и карт, есть пункт сортировки миссий. Эти элементы используются для создания карт с миссиями. Почитайте описание каждого элемента, особенно в редакторе карт, чтобы понять, что к чему. Для этого кликните по элементу в списке деталей.

### Локализация

В FBS (Flight Build Sandbox Simulator) используется кастомная система локализации. Как вы могли видеть, в файлах игры есть папка Localization. На данный момент в ней лежат файлы English.txt, Russian.txt. Это и есть файлы локализации. В них хранятся элементы формата `Ключ_1 = Значение 1`. При создании ключа используйте `_` вместо пробелов.

Вы также можете создать файл для нового языка, например, `German.txt`, и переименовать его, взяв за основу `English.txt`. После перевода всех значений на немецкий язык, вы сможете легко переключать язык в настройках игры. Важным аспектом является то, что в папке локализации карьеры должны находиться файлы с такими же именами, как и в основной папке, чтобы система могла правильно дополнить базовые файлы локализации. Если вы не планируете делать локализацию на разные языки в файлах локализации карьеры обязательно должен быть файл `English.txt` так как Англиский основной язык игры.

Пример `Heaven's legacy`:
```plaintext
Career_Name = Heaven's legacy
Career_Description = In the heart of a forested mountain area, far from the bustle of the city, the hero decides to start a new chapter of his life. Having inherited an airplane from his ancestors, he leaves the noisy city and goes to the clean air and picturesque landscapes. Now his task is not only to explore the magnificent natural spaces, but also to become a pilot who solves various tasks: delivering cargo, transporting passengers and helping local residents.
Specification_Description_B = Cargo Transportation
Specification_Description_P = Passengers Transportation
Cessna_172_Description = The Cessna 172 is a lightweight, single-engine airplane ideal for transporting small cargo. With its robust design and easy handling, the Cessna 172 is used to transport small boxes and other lightweight items to remote locations.
Robinson_R44_Description = The Robinson R44 is a compact light helicopter designed to carry up to one passenger. Thanks to its maneuverability and reliability, the R44 is well suited for flying to hard-to-reach places and performing small transport tasks.
Mission_Name_1 = First flight
Mission_Description_1 = Your first mission on an old but reliable airplane. You need to get used to the controls and get a feel for the machine. Just take a test flight to the nearest landing site. This easy task will help you get used to the controls and get a feel for the airplane in the sky.
```

Таким образом, структура локализации обеспечивает простоту добавления новых языков и поддерживает гибкость в изменении текста в игре. В дальнейшем мы можем получить значение по ключу в скрипте Lua, что позволяет динамически получать текст в зависимости от выбранного языка.

### Скриптинг

В FBS для создания карьеры используется язык программирования Lua.
В папке Skripts хранятся файлы скриптов. Они могут иметь разрешение `.txt` или `.lua`.
В папке Skripts обязательно должны храниться файлы `Main.(lua, txt)`, `Exit.(lua, txt)` и `SaveKey.txt`.

SaveKey.txt — файл, в котором хранится ключ для сохранения данных о карьере. Вы можете его заменить, и тогда весь прогресс карьеры будет утрачен. Постарайтесь делать этот ключ из случайных букв и цифр, чтобы избежать повторений в других карьерах. Например: `sdfgsd2323ri3k4jr23ioh423ihnf85f3o`.

Main.lua — файл, который выполняется при загрузке меню карьеры. Его базовая структура представляет собой:

```lua
-- Your main code here

-- System
UpdateTreeScrollArea()
InitializeTreeData()
SetTotalMissionCount(10) -- your mission count
UpdateFastList()
```

Exit.lua — файл, который выполняется перед выходом в главное меню. Он может быть пустым, но вы можете использовать его, например, для сохранения текущего проигрываемого времени музыки, чтобы воспроизвести её с того же момента.

# Пример карьеры `Heaven's legacy`

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

## Синтаксис

### Основные типы данных
- **nil** - отсутствующее значение, используется для обозначения пустоты.
- **boolean** - логические значения: true и false.
- **number** - числовые значения (например, 42, 3.14).
- **string** - строки, заключенные в одинарные или двойные кавычки (например, 'Hello', "World").
- **tabl**e - структуры данных, аналогичные массивам или словарям, создаваемые с помощью фигурных скобок (например, {1, 2, 3} или {key = "value"}).
- **function** - функции, которые можно присваивать переменным и передавать как аргументы.
- **Передача** значений
- **Числа**: передаются как есть, например, SetTotalMissionCount(8).
- **Строки**: передаются в виде строковых литералов, например, LoadMap('Base Lobby').
- **Таблицы**: передаются в виде литералов, например, SpawnTechTreeItem(0, 0, 'Cessna 172', 0, {SpecificationType('P', 1)}, 'Cessna 172', 'Cessna_172_Description', Pos2(0, 0), techItemColor).
- **Функции**: можно передавать как аргументы.

### Описание доступных методов
