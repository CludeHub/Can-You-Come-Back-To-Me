# Neverlose UI Library — Roblox Lua
A Roblox Lua UI library styled after the Neverlose CS2 cheat menu. Features acrylic blur, animated toggles, color pickers, config saving, and a full tab/section/element system.
Table of Contents
```
Setup
Window
Tabs & Sub-Tabs
Sections
Elements
Toggle
Slider
Dropdown
Colorpicker
Toggle + Colorpicker
Dropdown + Colorpicker
Accordion
Settings Panel (per-element)
Settings: Toggle
Settings: Checkbox
Settings: Slider
Settings: Dropdown
Settings: Colorpicker
Accordion Elements
Accordion: Toggle
Accordion: Slider
Accordion: Dropdown
Config System
Available Icons
```
# Setup
Wrap the library and call Library:AddWindow(...) to get started:
local Window = Library:AddWindow(hubTitle, hubImage, gameTitle)
Parameter
Type
Description
hubTitle
string
Title shown in the header
hubImage
string
rbxassetid for the header logo
gameTitle
string
Game name shown in the header
Toggle the UI: Press H or click the floating Close [H] button.
Window
local Window = Library:AddWindow("Neverlose", "rbxassetid://...", "Counter Strike 2")
The window includes:
Acrylic blur background
FPS and ping (MS) overlay
User avatar + username display
Window settings panel (main color picker, UI scale, language)
Config save/load system
Search bar (filters all elements by name)
Tabs & Sub-Tabs
AddTab
local MyTab = Window:AddTab(name, icon)
Parameter
Type
Description
name
string
Tab label text
icon
string
Icon name (see Available Icons)
AddTabLabel
Inserts a non-clickable separator/label between tabs in the sidebar.
Window:AddTabLabel("Common")
AddSubTab
Adds a sub-tab row inside a tab (renders as a horizontal sub-nav bar).
local SubTab = MyTab:AddSubTab(name, icon)
Example:
local LegitTab = Window:AddTab("Legit", "crosshair")

Window:AddTabLabel("Common")

local VisualsTab = Window:AddTab("Visuals", "image")
local PlayersSubTab = VisualsTab:AddSubTab("Players", "user")
local WorldSubTab   = VisualsTab:AddSubTab("World", "earth")
Sections
Sections are two-column panels inside a tab or sub-tab.
local Section = Tab:AddSection(title, side)
Parameter
Type
Options
Description
title
string
any
Section header label
side
string
"left", "right"
Which column
Example:
local AimbotSection  = LegitTab:AddSection("AIMBOT", "left")
local TriggerSection = LegitTab:AddSection("TRIGGERBOT", "right")
Elements
All elements are added on a Section object.
Toggle
local toggle = Section:AddToggle(text, default, callback)
Parameter
Type
Description
text
string
Label text
default
boolean
Initial state (true/false)
callback
function
Called with (bool) on change
Methods:
toggle:Set(bool)     -- set state programmatically
toggle:Get()         -- returns current bool value
toggle:AddSettings() -- attaches a per-element settings panel (returns SettingsObj)
Example:
local bhop = Section:AddToggle("Bunny Hop", false, function(v)
    print("Bunny Hop:", v)
end)
bhop:AddSettings()
Slider
local slider = Section:AddSlider(text, min, max, default, callback, suffix)
Parameter
Type
Description
text
string
Label text
min
number
Minimum value
max
number
Maximum value
default
number
Initial value
callback
function
Called with (number) on change
suffix
string
Unit appended to value (e.g. "ms", "%")
Methods:
slider:Set(number)   -- set value programmatically
slider:Get()         -- returns current number
slider:AddSettings() -- attaches a settings panel
Example:
local fov = Section:AddSlider("FOV", 0, 180, 90, function(v)
    print("FOV:", v)
end, "°")
fov:AddSettings()
Dropdown
local dropdown = Section:AddDropdown(text, options, default, callback)
-- or (legacy 3-arg form):
local dropdown = Section:AddDropdown(text, options, callback)
Parameter
Type
Description
text
string
Label text
options
table of strings
List of selectable options
default
string or nil
Default selected option
callback
function
Called with (selectedString) on change
Methods:
dropdown:Set(val, silent) -- set value; silent=true skips callback
dropdown:Get()            -- returns current selected string
Example:
local hitbox = Section:AddDropdown("Hitbox", {"Head", "Chest", "Legs"}, "Head", function(v)
    print("Hitbox:", v)
end)
Colorpicker
local picker = Section:AddColorpicker(text, defaultColor, callback)
Parameter
Type
Description
text
string
Label text
defaultColor
Color3
Initial color
callback
function
Called with (Color3) on change
Methods:
picker:Set(Color3)  -- set color programmatically
picker:Get()        -- returns current Color3
Example:
local color = Section:AddColorpicker("Chams Color", Color3.fromRGB(255, 100, 100), function(c)
    print("Color:", c)
end)
Toggle + Colorpicker
A toggle switch with an embedded color picker button on the same row.
local tc = Section:AddToggleColorpicker(text, defaultEnabled, defaultColor, callback, colorCallback)
Parameter
Type
Description
text
string
Label text
defaultEnabled
boolean
Initial toggle state
defaultColor
Color3
Initial color
callback
function
Toggle callback (bool) — or (bool, Color3) if no colorCallback
colorCallback
function
Separate color callback (Color3) (optional)
Example:
Section:AddToggleColorpicker("Bullet Impacts", false, Color3.fromRGB(255, 100, 100),
    function(v) print("Enabled:", v) end,
    function(c) print("Color:", c) end
)
Dropdown + Colorpicker
A dropdown with an embedded color picker button on the same row.
local dc = Section:AddDropdownColorpicker(text, options, defaultColor, callback, colorCallback)
Parameter
Type
Description
text
string
Label text
options
table of strings
Dropdown options
defaultColor
Color3
Initial color
callback
function
Dropdown callback (selectedString) — or combined if no colorCallback
colorCallback
function
Separate color callback (Color3) (optional)
Methods:
dc:Set(val)           -- set dropdown value
dc:Get()              -- get dropdown value
dc:SetColor(Color3)   -- set color value
dc:GetColor()         -- get Color3
Example:
Section:AddDropdownColorpicker("Log Events", {"All", "Hits", "Deaths"}, Color3.fromRGB(255,255,255),
    function(v) print("Log:", v) end
)
Accordion
A collapsible sub-panel inside a section. Can hold its own elements.
local acc = Section:AddAccordion(text)
Parameter
Type
Description
text
string
Accordion header label
Only one accordion can be open at a time per section.
Example:
local accScope = ViewSection:AddAccordion("Scope Options")
accScope:AddToggle("Remove Scope", false, function(v) print(v) end)
accScope:AddSlider("Zoom Level", 1, 10, 5, function(v) print(v) end)
Settings Panel (per-element)
Some elements support :AddSettings(), which appends a small settings sub-panel below the element. The panel is toggled by a ... gear button.
local settings = toggle:AddSettings()
-- or
local settings = slider:AddSettings()
The returned SettingsObj supports its own elements:
Settings: Toggle
settings:AddToggle(text, default, callback)
Settings: Checkbox
settings:AddCheckbox(text, default, callback)
Settings: Slider
settings:AddSlider(text, min, max, default, callback, suffix)
Settings: Dropdown
settings:AddDropdown(text, options, default, callback)
Settings: Colorpicker
settings:AddColorpicker(text, defaultColor, callback)
Full example:
local grenadeTrajectory = WorldEspSection:AddToggle("Grenade Trajectory", false, function(v)
    print(v)
end)

local s = grenadeTrajectory:AddSettings()
s:AddCheckbox("Only Teammates", false, function(v) print(v) end)
s:AddSlider("Thickness", 1, 5, 2, function(v) print(v) end)
Accordion Elements
Accordions support their own mini-element set:
Accordion: Toggle
acc:AddToggle(text, default, callback)
Accordion: Slider
acc:AddSlider(text, min, max, default, callback, suffix)
Accordion: Dropdown
acc:AddDropdown(text, options, default, callback)
Config System
The library includes a built-in preset/config manager accessible from the header Save button. All registered elements are automatically serialized.
Features:
Create named configs
Load / Save configs to file (writefile / readfile required)
Duplicate configs
Delete configs (moved to Recently Deleted, recoverable)
Search configs by name
Auto-save (saves the active config every 30 seconds)
All elements register themselves automatically. Supported value types: boolean, number, string, Color3.
Available Icons
Pass these strings as the icon argument in AddTab or AddSubTab:
Name
Description
"ads"
Ads / cursor icon
"list"
List icon
"folder"
Folder icon
"earth"
Globe / world icon
"locked"
Lock icon
"home"
Home icon
"mouse"
Mouse icon
"user"
Person / user icon
"cosmetics"
Cosmetics icon
"sun"
Sun / visuals icon
"shop"
Shop icon
"farm"
Farm icon
"code"
Code / dev icon
"crosshair"
Crosshair icon
"image"
Image / photo icon
"layers"
Layers / inventory
"side"
Side icon
"grenade"
Grenade icon
"bio"
Bio icon
"camera"
Camera icon
"car"
Car icon
"run"
Run / movement icon
"box"
Box icon
"carcolor"
Car color icon
"avacolor"
Avatar color icon
"locker"
Locker icon
"retry"
Retry / refresh icon
"gun"
Gun icon
"sword"
Sword icon
"knife"
Knife icon
"egg"
Egg icon
"sprinkler"
Sprinkler icon
"gear"
Gear / misc icon
Full Example
local Window = Library:AddWindow("Neverlose", "", "Counter Strike 2")

-- Tab
local LegitTab = Window:AddTab("Legit", "crosshair")

-- Sections
local AimbotSection   = LegitTab:AddSection("AIMBOT", "left")
local TriggerSection  = LegitTab:AddSection("TRIGGERBOT", "right")

-- Aimbot elements
local aimbotToggle = AimbotSection:AddToggle("Enable Aimbot", false, function(v)
    print("Aimbot:", v)
end)
aimbotToggle:AddSettings()

AimbotSection:AddDropdown("Bone", {"Head", "Chest", "Stomach"}, "Head", function(v)
    print("Bone:", v)
end)

local fov = AimbotSection:AddSlider("FOV", 0, 180, 90, function(v)
    print("FOV:", v)
end, "°")
fov:AddSettings()

-- Accordion inside a section
local ViewSection = LegitTab:AddSection("VIEW", "right")
local accView = ViewSection:AddAccordion("View Options")
accView:AddToggle("No Scope Overlay", false, function(v) print(v) end)
accView:AddSlider("FOV Multiplier", 50, 150, 100, function(v) print(v) end, "%")

-- Visuals tab with sub-tabs
Window:AddTabLabel("Common")
local VisualsTab = Window:AddTab("Visuals", "image")
local PlayersSubTab = VisualsTab:AddSubTab("Players", "user")

local PlayersSection = PlayersSubTab:AddSection("PLAYERS", "left")
PlayersSection:AddToggle("ESP Enabled", false, function(v) print("ESP:", v) end)
PlayersSection:AddToggleColorpicker("Chams", false, Color3.fromRGB(255, 100, 100),
    function(v) print("Chams:", v) end,
    function(c) print("Chams Color:", c) end
)
