-- FlowAuth LRM compatibility variables
LRM_IsUserPremium=true
LRM_LinkedDiscordID="0"
LRM_ScriptName="Strongest Battlegrounds"
LRM_TotalExecutions=0
LRM_SecondsLeft=86332
LRM_UserNote="Checkpoint Key (Visitor IP: IP)"
LRM_ScriptVersion="0.0.0.4"
LRM_ExecutionTrace="fa1_yudfD_5oTtPJpy0FVu4Nvin2"
do
    local environment = getgenv()
    environment._premTag = function(name)
    return tostring(name)
end
environment._isPremiumUser = function()
return true
end
end

do
    local environment = getgenv()
    local runtime = environment.__AzedoHubUIRuntime or {}

    function runtime.corner(radius, parent)
        if typeof(parent) ~= "Instance" then return nil end
        local object = Instance.new("UICorner")
        object.CornerRadius = UDim.new(0, tonumber(radius) or 8)
        object.Parent = parent
        return object
    end

    function runtime.stroke(color, thickness, parent)
        if typeof(parent) ~= "Instance" then return nil end
        local object = Instance.new("UIStroke")
        object.Color = color or Color3.fromRGB(85, 213, 254)
        object.Thickness = tonumber(thickness) or 1
        object.Parent = parent
        return object
    end

    function runtime.gradient(parent, firstColor, secondColor, rotation)
        if typeof(parent) ~= "Instance" then return nil end
        local object = Instance.new("UIGradient")
        object.Color = ColorSequence.new(
        firstColor or Color3.fromRGB(18, 24, 64),
        secondColor or firstColor or Color3.fromRGB(24, 32, 85)
        )
        object.Rotation = tonumber(rotation) or 0
        object.Parent = parent
        return object
    end

    function runtime.shadow(parent)
        if typeof(parent) ~= "Instance" then return nil end
        local object = Instance.new("ImageLabel")
        object.Name = "Azedo HUBShadow"
        object.BackgroundTransparency = 1
        object.Position = UDim2.new(0, -18, 0, -18)
        object.Size = UDim2.new(1, 36, 1, 36)
        object.Image = "rbxassetid://6015897843"
        object.ImageColor3 = Color3.new(0, 0, 0)
        object.ImageTransparency = 0.42
        object.ScaleType = Enum.ScaleType.Slice
        object.SliceCenter = Rect.new(49, 49, 450, 450)
        object.ZIndex = 0
        object.Parent = parent
        return object
    end

    function runtime.tw(instance, properties, duration, easingStyle, easingDirection)
        if typeof(instance) ~= "Instance" or type(properties) ~= "table" then return nil end
        local tween = game:GetService("TweenService"):Create(
        instance,
        TweenInfo.new(
        tonumber(duration) or 0.2,
        easingStyle or Enum.EasingStyle.Quad,
        easingDirection or Enum.EasingDirection.Out
        ),
        properties
        )
        tween:Play()
        return tween
    end

    environment.__AzedoHubUIRuntime = runtime
    environment.corner = runtime.corner
    environment.stroke = runtime.stroke
    environment.gradient = runtime.gradient
    environment.shadow = runtime.shadow
    environment.tw = runtime.tw
end
if not getgenv()._nagPremium then
    local function _nagPremium()

    end
    getgenv()._nagPremium = _nagPremium
end

local TweenService     = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local RunService       = game:GetService("RunService")
local Players          = game:GetService("Players")
local HttpService      = game:GetService("HttpService")
local CoreGui          = game:GetService("CoreGui")
local LocalPlayer      = Players.LocalPlayer

do

    local function safeGetGlobal(name)
        local ok, v = pcall(function() return _G[name] end)
        if ok and v then return v end
        local ok2, v2 = pcall(function() return getgenv()[name] end)
        if ok2 and v2 then return v2 end
        return rawget(_ENV or _G, name)
    end
    local function safeSetGlobal(name, value)
        pcall(function() getgenv()[name] = value end)
    end

    pcall(function()
    getgenv().__realGetRawMT     = getgenv().__realGetRawMT     or safeGetGlobal("getrawmetatable")
    getgenv().__realHookNamecall = getgenv().__realHookNamecall or safeGetGlobal("hookmetamethod")
    getgenv().__realNewCClosure  = getgenv().__realNewCClosure  or safeGetGlobal("newcclosure")
end)

local function stubGetRaw(_obj)
    return setmetatable({}, {__index = function() return function() end end, __newindex = function() end})
end

local function stubHookNC(_obj, _method, _fn)
    return function() return nil end
end

safeSetGlobal("getrawmetatable", stubGetRaw)
safeSetGlobal("hookmetamethod",  stubHookNC)
safeSetGlobal("setreadonly",     function() end)
safeSetGlobal("setrawmetatable", function(t, _mt) return t end)

warn("[Azedo HUBHub] Anti-detect stub installed (Delta-compatible).")
end

pcall(function()

if not setclipboard then
    if toclipboard then setclipboard = toclipboard
    elseif Clipboard and Clipboard.set then setclipboard = function(s) Clipboard:set(s) end
    end
end
end)

if not mouse1press then
    local VIM = game:GetService("VirtualInputManager")
    getgenv().mouse1press = function()
    pcall(function() VIM:SendMouseButtonEvent(0, 0, 0, true, game, 1) end)
end
getgenv().mouse1release = function()
pcall(function() VIM:SendMouseButtonEvent(0, 0, 0, false, game, 1) end)
end

end

if not getgenv().Drawing or type(getgenv().Drawing) ~= "table" or not getgenv().Drawing.new then
    local DrawingObj = {
    new = function(_kind)
    local fake = setmetatable({Visible = false, Remove = function(self) end,
    Destroy = function(self) end},
    {__newindex = function() end, __index = function() return function() end end})
    return fake
end,
Fonts = {UI = 0, System = 1, Plex = 2, Monospace = 3},
}
pcall(function() getgenv().Drawing = DrawingObj end)
end

if not request then
    if syn and syn.request then request = syn.request
    elseif http and http.request then request = http.request
elseif http_request then request = http_request end
end
pcall(function() if request then getgenv().request = request end end)

if not queue_on_teleport then
    getgenv().queue_on_teleport = function() end
end

if not gethui then
    getgenv().gethui = function() return game:GetService("CoreGui") end
end

if not identifyexecutor then
    getgenv().identifyexecutor = function() return "Unknown", "0.0.0" end
end

local Theme = {
WindowWidth   = 720,
WindowHeight  = 520,
HeaderHeight  = 48,
TabBarHeight  = 44,
BannerHeight  = 62,
Radius        = 24,
RadiusSm      = 16,
RadiusXs      = 12,
RadiusPill    = 999,

BgPrimary     = Color3.fromRGB(18, 24, 64),
BgSecondary   = Color3.fromRGB(24, 32, 85),
BgCard        = Color3.fromRGB(35, 45, 110),
BgCardHover   = Color3.fromRGB(45, 60, 140),
BgCardActive  = Color3.fromRGB(40, 65, 160),
BgHeader      = Color3.fromRGB(14, 18, 50),
BgInput       = Color3.fromRGB(20, 28, 70),
BgTabBar      = Color3.fromRGB(20, 28, 70),

Accent        = Color3.fromRGB(85, 213, 254),
AccentBright  = Color3.fromRGB(135, 235, 255),
AccentDark    = Color3.fromRGB(50, 170, 220),
AccentDim     = Color3.fromRGB(30, 90, 130),
AccentGlow    = Color3.fromRGB(180, 240, 255),
AccentSoft    = Color3.fromRGB(100, 200, 255),

Premium       = Color3.fromRGB(255, 200, 50),
PremiumDark   = Color3.fromRGB(180, 130, 20),
PremiumDim    = Color3.fromRGB(50, 40, 10),

BannerBg      = Color3.fromRGB(15, 45, 100),
BannerActive  = Color3.fromRGB(25, 70, 150),

TextPrimary   = Color3.fromRGB(230, 245, 255),
TextSecondary = Color3.fromRGB(160, 190, 230),
TextMuted     = Color3.fromRGB(80, 110, 160),
TextDim       = Color3.fromRGB(50, 70, 110),

Border        = Color3.fromRGB(30, 45, 100),
BorderHi      = Color3.fromRGB(60, 100, 220),

Success       = Color3.fromRGB(50, 220, 120),
Error         = Color3.fromRGB(255, 55, 75),
Warning       = Color3.fromRGB(255, 180, 50),
}

local SUPPORTED_GAMES = {
{ name = "BedWars", icon = utf8.char(0x1F6CF), aliases = {"bedwars", "bed wars"}, placeIds = {6872265039, 6872274481}, features = 69, img = "rbxassetid://6034281798" },
{ name = "BedWars", icon = utf8.char(0x1F6CF), aliases = {"bedwars", "bed wars"}, placeIds = {6872265039, 6872274481}, features = 69, img = "rbxassetid://6034281798" },
{ name = "Taxi Boss", icon = utf8.char(0x1F695), aliases = {"taxi boss", "taxiboss", "taxi"}, placeIds = {7305309231}, features = 40, img = "rbxassetid://6034281798" },
{ name = "BloxStrike", icon = utf8.char(0x1F3AF), aliases = {"bloxstrike", "blox strike"}, placeIds = {114234929420007}, features = 45, img = "rbxassetid://6034281798" },
{ name = "Anime Expeditions", icon = utf8.char(0x26A1), aliases = {"anime expeditions", "expeditions", "anime expedition"}, placeIds = {84515722934860}, features = 40, img = "rbxassetid://6034281798" },
{ name = "Arsenal",      icon = utf8.char(0x1F3AF),  placeIds = {286090429, 2133507413},   features = 20, img = "rbxassetid://6034281798" },
{ name = "Jujutsu Shenanigans", icon = utf8.char(0x26A1), aliases = {"jujutsu shenanigans", "jujutsu", "shenanigans"}, placeIds = {9391468976}, universeIds = {3508322461}, features = 15, img = "rbxassetid://6034281798" },
{ name = "Abyss",        icon = utf8.char(0x1F30A),  placeIds = {8841631016, 6499213745},  features = 22, img = "rbxassetid://6034281798" },
{ name = "Jailbreak",    icon = utf8.char(0x1F6A8),  placeIds = {606849621},               features = 50,  img = "rbxassetid://6034281798" },
{ name = "Sniper Duels", icon = utf8.char(0x1F3AF),  placeIds = {2957674229, 3519873232, 109397169461300},  features = 12, img = "rbxassetid://6034281798" },
{ name = "Swing Obby",   icon = utf8.char(0x1F3A2),  placeIds = {5334680521},              features = 10, img = "rbxassetid://6034281798" },
{ name = "Brookhaven",   icon = utf8.char(0x1F3E0),  placeIds = {4924922222},              features = 15, img = "rbxassetid://6034281798" },
{ name = "99 Nights",    icon = utf8.char(0x1F319),  placeIds = {13271790403, 14502339080}, features = 28, img = "rbxassetid://6034281798" },
{ name = "Blox Fruits",  icon = utf8.char(0x1F34E),  aliases = {"blox fruits", "blox fruit", "bloxfruits"}, placeIds = {2753915549, 4442272183, 7449423635}, features = 18, img = "rbxassetid://6034281798" },
{ name = "Pet Sim 99",   icon = utf8.char(0x1F43E),  aliases = {"pet simulator 99", "pet sim 99", "petsim99"}, placeIds = {8737899170, 15532244896, 15588347101}, universeIds = {3317771874}, features = 15, img = "rbxassetid://6034281798" },
{ name = "Bee Swarm",    icon = utf8.char(0x1F41D),  placeIds = {1537690962},              features = 12, img = "rbxassetid://6034281798" },
{ name = "SAB",          icon = utf8.char(0x1F916),  aliases = {"steal a brainrot", "sab"}, placeIds = {18668065096, 1234567890, 10495922230, 109983668079237}, universeIds = {109983668079237}, features = 24, img = "rbxassetid://6034281798" },
{ name = "Ultimate Battlegrounds", icon = utf8.char(0x1F52A), placeIds = {11815767793}, features = 18, img = "rbxassetid://6034281798" },
{ name = "Strongest Battlegrounds", icon = utf8.char(0x1F525), placeIds = {10495922230}, features = 15, img = "rbxassetid://6034281798" },
{ name = "Heroes Battlegrounds", icon = utf8.char(0x1F9B8), placeIds = {13076380114}, features = 16, img = "rbxassetid://6034281798" },
{ name = "Legend Battlegrounds", icon = utf8.char(0x1F5E1), placeIds = {15269951959}, features = 18, img = "rbxassetid://6034281798" },
{ name = "Jump for Brainrots", icon = utf8.char(0x1F9E0), placeIds = {139299356663913}, features = 12, img = "rbxassetid://6034281798" },
{ name = "+1 Jump Brainrot", icon = utf8.char(0x1F45F), placeIds = {121192350097093}, features = 15, img = "rbxassetid://6034281798" },
{ name = "Grab Ores!", icon = utf8.char(0x26CF), placeIds = {82806856224736}, features = 45, img = "rbxassetid://6034281798" },
{ name = "Murderers VS Sheriffs", icon = utf8.char(0x1F52A), placeIds = {135856908115931}, features = 22, img = "rbxassetid://6034281798" },
{ name = "Sailor Piece", icon = utf8.char(0x2693), aliases = {"sailor piece", "sailor peace"}, placeIds = {77747658251236}, features = 38, img = "rbxassetid://6034281798" },
{ name = "MM2",          icon = utf8.char(0x1F5E1),  aliases = {"murder mystery 2", "mm2"}, placeIds = {142823291, 80005698042109}, features = 40, img = "rbxassetid://6034281798" },
{ name = "Kick a Lucky Block", icon = utf8.char(0x1F340), aliases = {"kick a lucky block", "lucky block", "kick lucky"}, placeIds = {16955097151, 89469502395769}, universeIds = {89469502395769}, features = 30, img = "rbxassetid://6034281798" },
{ name = "Rivals",       icon = utf8.char(0x1F52B), aliases = {"rivals", "rivals fps", "matchmaking"}, placeIds = {17625359962, 14721849686, 117398147513099}, universeIds = {6035872082}, features = 22, img = "rbxassetid://6034281798" },
{ name = "Speed Keyboard Escape", icon = utf8.char(0x2328), aliases = {"speed keyboard escape", "keyboard escape", "1 speed keyboard escape", "escape candy chocolate", "1-speed-keyboard-escape", "keyboard"}, placeIds = {95082159892680}, features = 16, img = "rbxassetid://6034281798" },
{ name = "Speed Monkey Escape", icon = utf8.char(0x1F412), aliases = {"speed monkey escape", "monkey escape", "1 speed monkey escape", "monkey"}, placeIds = {114697347887839}, features = 20, img = "rbxassetid://6034281798" },
{ name = "Sell Lemons", icon = utf8.char(0x1F34B), aliases = {"sell lemons", "lemon tycoon", "lemons"}, placeIds = {79268393072444}, features = 14, img = "rbxassetid://6034281798" },
{ name = "Demonology", icon = utf8.char(0x1F47B), aliases = {"demonology", "ghost hunter", "demon"}, placeIds = {18199615050}, features = 12, img = "rbxassetid://6034281798" },
{ name = "Grow a Garden 2", icon = utf8.char(0x1F331), aliases = {"grow a garden 2", "grow a garden", "gag2", "gag 2"}, placeIds = {97598239454123, 1588579651, 126884695634066}, universeIds = {7436755782}, features = 25, img = "rbxassetid://6034281798" },
{ name = "Welcome to Bloxburg", icon = utf8.char(0x1F3E0), aliases = {"welcome to bloxburg", "bloxburg"}, placeIds = {185655149}, features = 16, img = "rbxassetid://6034281798" },
{ name = "Adopt Me", icon = utf8.char(0x1F436), aliases = {"adopt me", "adoptme"}, placeIds = {920587237}, features = 12, img = "rbxassetid://6034281798" },
{ name = "Dandy's World", icon = utf8.char(0x1F3A8), aliases = {"dandys world", "dandy", "dandys"}, placeIds = {16267229903}, features = 10, img = "rbxassetid://6034281798" },
{ name = "Blade Ball", icon = utf8.char(0x1F52A), aliases = {"blade ball", "bladeball", "bb"}, placeIds = {13772394625, 14777999650, 15182888200}, features = 25, img = "rbxassetid://6034281798" },
{ name = "KAT",          icon = utf8.char(0x1F52A), placeIds = {621129760}, universeIds = {228028122}, features = 11, img = "rbxassetid://6034281798" },
{ name = "One Scope",    icon = utf8.char(0x1F3AF), placeIds = {135648408848758}, features = 7, img = "rbxassetid://6034281798" },
{ name = "Da Hood",      icon = utf8.char(0x1F3E0), aliases = {"da hood", "dahood", "dh"}, placeIds = {2788229376, 105305881594005, 98343466176190, 93778920465645, 129449254792324, 122120252103378, 110917236678062, 130879446158509}, features = 120, img = "rbxassetid://6034281798" },
{ name = "Evade",        icon = utf8.char(0x1F3C3), aliases = {"evade", "rise"}, placeIds = {9872472334}, features = 45, img = "rbxassetid://6034281798" },
{ name = "Bridge Duel",  icon = utf8.char(0x1F5E1), aliases = {"bridge duel", "bridge duels", "uoro"}, placeIds = {139566161526375}, features = 35, img = "rbxassetid://6034281798" },
{ name = "Drain the Lake", icon = utf8.char(0x1F4A7), aliases = {"drain the lake", "drain lake"}, placeIds = {138381251771774}, universeIds = {10267363348}, features = 9, img = "rbxassetid://6034281798" },
{ name = "Prison Life", icon = utf8.char(0x1F6A8), aliases = {"prison life", "prison"}, placeIds = {155615604}, universeIds = {73885730}, features = 9, img = "rbxassetid://6034281798" },
{ name = "Fish It", icon = utf8.char(0x1F41F), aliases = {"fish it", "fisch", "fishit"}, placeIds = {121864768012064, 16732694052, 17445763782}, features = 25, img = "rbxassetid://6034281798" },
{ name = "Volleyball Legends", icon = utf8.char(0x1F3D0), aliases = {"volley ball legends", "volleyball legends", "volleyball", "volley ball"}, placeIds = {85395484560711}, universeIds = {6931042565}, features = 20, img = "rbxassetid://6034281798" },
{ name = "Flee the Facility", icon = utf8.char(0x1F3C3), aliases = {"flee city", "flee the facility", "flee"}, placeIds = {893973440}, universeIds = {372226183}, features = 30, img = "rbxassetid://6034281798" }
}
getgenv().SUPPORTED_GAMES = SUPPORTED_GAMES

local _detectedGame = nil
local _currentPlaceId = tonumber(game.PlaceId) or 0
local _currentGameId = tonumber(game.GameId) or 0
local _gameName = ""

pcall(function()
local info = game:GetService("MarketplaceService"):GetProductInfo(_currentPlaceId)
if info and info.Name then _gameName = info.Name:lower() end
end)

for _, gd in ipairs(SUPPORTED_GAMES) do
    if gd.placeIds then
        for _, pid in ipairs(gd.placeIds) do
            if _currentPlaceId == tonumber(pid) then
                _detectedGame = gd
                break
            end
        end
    end
    if not _detectedGame and gd.universeIds then
        for _, uid in ipairs(gd.universeIds) do
            if _currentGameId == tonumber(uid) then
                _detectedGame = gd
                break
            end
        end
    end
    if _detectedGame then break end
end

if not _detectedGame and _gameName ~= "" then
    for _, gd in ipairs(SUPPORTED_GAMES) do
        local gNameLower = gd.name:lower()
        if _gameName:find(gNameLower, 1, true) then
            _detectedGame = gd
            break
        end
        if gd.aliases then
            for _, alias in ipairs(gd.aliases) do
                local aLower = alias:lower()
                if #aLower >= 3 and _gameName:find(aLower, 1, true) then
                    _detectedGame = gd
                    break
                end
            end
        end
        if _detectedGame then break end
    end
end

if not _detectedGame then
    if _gameName:find("bedwars") or _gameName:find("bed wars") then
        for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "BedWars" then _detectedGame = gd; break end end
    elseif _gameName:find("bedwars") or _gameName:find("bed wars") then
    for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "BedWars" then _detectedGame = gd; break end end
elseif _gameName:find("blox fruit") or _gameName:find("bloxfruit") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Blox Fruits" then _detectedGame = gd; break end end
elseif _gameName:find("fish it") or _gameName:find("fisch") or _gameName:find("fishit") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Fish It" then _detectedGame = gd; break end end
elseif _gameName:find("steal a brainrot") or _gameName:find("sab") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "SAB" then _detectedGame = gd; break end end
elseif _gameName:find("volley") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Volleyball Legends" then _detectedGame = gd; break end end
elseif _gameName:find("speed keyboard") or _gameName:find("keyboard escape") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Speed Keyboard Escape" then _detectedGame = gd; break end end
elseif _gameName:find("speed monkey") or _gameName:find("monkey escape") or _gameName:find("monkey") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Speed Monkey Escape" then _detectedGame = gd; break end end
elseif _gameName:find("anime expedition") or _gameName:find("expedition") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Anime Expeditions" then _detectedGame = gd; break end end
elseif _gameName:find("grow a garden") or _gameName:find("gag2") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Grow a Garden 2" then _detectedGame = gd; break end end
elseif _gameName:find("prison life") or _gameName:find("prison") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Prison Life" then _detectedGame = gd; break end end
elseif _gameName:find("da hood") or _gameName:find("dahood") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Da Hood" then _detectedGame = gd; break end end
elseif _gameName:find("bloxstrike") or _gameName:find("blox strike") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "BloxStrike" then _detectedGame = gd; break end end
elseif _gameName:find("taxi boss") or _gameName:find("taxiboss") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Taxi Boss" then _detectedGame = gd; break end end
elseif _gameName:find("evade") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Evade" then _detectedGame = gd; break end end
elseif _gameName:find("flee the facility") or _gameName:find("flee city") or _gameName:find("flee") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Flee the Facility" then _detectedGame = gd; break end end
end
end

if not _detectedGame then
    pcall(function()
    local RS = game:GetService("ReplicatedStorage")
    if RS:FindFirstChild("TS") or lplr.PlayerScripts:FindFirstChild("TS") then
        for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "BedWars" then _detectedGame = gd; break end end
    elseif RS:FindFirstChild("TS") or lplr.PlayerScripts:FindFirstChild("TS") then
    for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "BedWars" then _detectedGame = gd; break end end
elseif RS:FindFirstChild("Remotes") and RS.Remotes:FindFirstChild("CommF_") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Blox Fruits" then _detectedGame = gd; break end end
elseif RS:FindFirstChild("events") and RS.events:FindFirstChild("reels") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Fish It" then _detectedGame = gd; break end end
elseif RS:FindFirstChild("Volleyball") or workspace:FindFirstChild("Volleyball") or RS:FindFirstChild("Volley") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Volleyball Legends" then _detectedGame = gd; break end end
elseif RS:FindFirstChild("Brainrots") or workspace:FindFirstChild("Brainrots") then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "SAB" then _detectedGame = gd; break end end
elseif workspace:FindFirstChild("ComputerTrigger", true) or workspace:FindFirstChild("FreezePod", true) then
for _, gd in ipairs(SUPPORTED_GAMES) do if gd.name == "Flee the Facility" then _detectedGame = gd; break end end
end
end)
end

if not _detectedGame then
    _detectedGame = { name = "Strongest Battlegrounds", features = 0 }
end

local AzedoHubLib = {}

local _notifHolder = nil
local function _pushNotif(title, text, dur, gold)
    dur = dur or 4
    if not _notifHolder then
        local scr = Instance.new("ScreenGui", CoreGui)
        scr.Name = "Azedo HUBNotifs"; scr.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
        _notifHolder = scr
    end

    local f = Instance.new("Frame", _notifHolder)
    f.Size = UDim2.new(0, 340, 0, 80)
    f.Position = UDim2.new(1, 360, 1, -100)
    f.BackgroundColor3 = Theme.BgCard; f.ZIndex = 500
    corner(Theme.RadiusSm, f); stroke(gold and Theme.Premium or Theme.Accent, 1.5, f)
    gradient(f, Theme.BgSecondary, Theme.BgCard, 90)

    local accent = Instance.new("Frame", f)
    accent.Size = UDim2.new(0, 5, 0.6, 0); accent.Position = UDim2.new(0, 10, 0.2, 0)
    accent.BackgroundColor3 = gold and Theme.Premium or Theme.Accent
    accent.BorderSizePixel = 0; accent.ZIndex = 501; corner(Theme.RadiusPill, accent)

    local tl = Instance.new("TextLabel", f)
    tl.Size = UDim2.new(1, -40, 0, 22); tl.Position = UDim2.new(0, 24, 0, 10)
    tl.BackgroundTransparency = 1; tl.Text = title
    tl.TextColor3 = gold and Theme.Premium or Theme.AccentBright
    tl.Font = Enum.Font.GothamBlack; tl.TextSize = 14; tl.ZIndex = 501
    tl.TextXAlignment = Enum.TextXAlignment.Left

    local dl = Instance.new("TextLabel", f)
    dl.Size = UDim2.new(1, -40, 0, 36); dl.Position = UDim2.new(0, 24, 0, 34)
    dl.BackgroundTransparency = 1; dl.Text = text
    dl.TextColor3 = Theme.TextSecondary; dl.Font = Enum.Font.Gotham
    dl.TextSize = 12; dl.TextWrapped = true; dl.ZIndex = 501
    dl.TextXAlignment = Enum.TextXAlignment.Left

    local prog = Instance.new("Frame", f)
    prog.Size = UDim2.new(1, -20, 0, 3); prog.Position = UDim2.new(0, 10, 1, -8)
    prog.BackgroundColor3 = gold and Theme.Premium or Theme.Accent; prog.ZIndex = 501
    corner(Theme.RadiusPill, prog)
    tw(prog, {Size = UDim2.new(0, 0, 0, 3)}, dur, Enum.EasingStyle.Linear)

    tw(f, {Position = UDim2.new(1, -360, 1, -100)}, 0.5, Enum.EasingStyle.Back)

    task.delay(dur, function()
    tw(f, {Position = UDim2.new(1, 380, 1, -100), BackgroundTransparency = 0.5}, 0.4)
    task.wait(0.45); pcall(function() f:Destroy() end)
end)
end

function AzedoHubLib:Notify(title, text, dur, gold)
    task.spawn(_pushNotif, title, text, dur, gold)
end

local _lastPremNag      = 0
local _activePremModal  = nil
local PREM_SITE         = "getsnowy.xyz/pricing"
local PREM_SITE_URL     = "https://getsnowy.xyz/pricing"

local function _nagPremium(featureName)
    local now = tick()
    if now - _lastPremNag < 1.2 then return end
    _lastPremNag = now
    if _activePremModal then pcall(function() _activePremModal:Destroy() end); _activePremModal = nil end

    local ok, err = pcall(function()
    local label = featureName and tostring(featureName) or "That feature"

    local scr = Instance.new("ScreenGui")
    scr.Name               = "Azedo HUBPremiumModal"
    scr.ZIndexBehavior     = Enum.ZIndexBehavior.Sibling
    scr.IgnoreGuiInset     = true
    scr.ResetOnSpawn       = false
    scr.DisplayOrder       = 2147483647
    local _parented = false
    if not RunService:IsStudio() then
        if syn and syn.protect_gui then pcall(function() syn.protect_gui(scr); scr.Parent = CoreGui; _parented = true end) end
        if not _parented and gethui then pcall(function() scr.Parent = gethui(); _parented = true end) end
        if not _parented and get_hidden_gui then pcall(function() scr.Parent = get_hidden_gui(); _parented = true end) end
    end
    if not _parented then
        pcall(function()
        scr.Parent = (RunService:IsStudio() and LocalPlayer.PlayerGui) or CoreGui
    end)
end
_activePremModal       = scr

local back = Instance.new("TextButton", scr)
back.Name                   = "Backdrop"
back.Size                   = UDim2.fromScale(1, 1)
back.BackgroundColor3       = Color3.new(0, 0, 0)
back.BackgroundTransparency = 1
back.BorderSizePixel        = 0
back.AutoButtonColor        = false
back.Text                   = ""
back.ZIndex                 = 9000
tw(back, { BackgroundTransparency = 0.45 }, 0.2)

local card = Instance.new("Frame", scr)
card.Name                   = "Card"
card.AnchorPoint            = Vector2.new(0.5, 0.5)
card.Position               = UDim2.fromScale(0.5, 0.5)
card.Size                   = UDim2.fromOffset(0, 0)
card.BackgroundColor3       = Theme.BgCard
card.BorderSizePixel        = 0
card.ZIndex                 = 9010
card.ClipsDescendants       = true
corner(18, card)
stroke(Theme.Premium, 2, card)
gradient(card, Theme.BgSecondary, Theme.BgCard, 125)
shadow(card)

local bar = Instance.new("Frame", card)
bar.Size                  = UDim2.new(1, 0, 0, 4)
bar.BackgroundColor3      = Theme.Premium
bar.BorderSizePixel       = 0
bar.ZIndex                = 9011
gradient(bar, Theme.PremiumDark, Theme.Premium, 0)

local lockContainer = Instance.new("Frame", card)
lockContainer.Name                 = "LockContainer"
lockContainer.Size                 = UDim2.new(0, 68, 0, 68)
lockContainer.Position             = UDim2.new(0.5, -34, 0, 28)
lockContainer.BackgroundColor3     = Theme.PremiumDim
lockContainer.BorderSizePixel      = 0
lockContainer.ZIndex               = 9012
corner(999, lockContainer)
stroke(Theme.Premium, 2, lockContainer)

local shackle = Instance.new("Frame", lockContainer)
shackle.Name                 = "Shackle"
shackle.Size                 = UDim2.new(0, 26, 0, 26)
shackle.Position             = UDim2.new(0.5, -13, 0.5, -23)
shackle.BackgroundTransparency = 1
shackle.ZIndex               = 9013
local shackleCorner = Instance.new("UICorner", shackle)
shackleCorner.CornerRadius   = UDim.new(0.5, 0)
local shackleStroke = Instance.new("UIStroke", shackle)
shackleStroke.Color          = Theme.Premium
shackleStroke.Thickness      = 3
shackleStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border

local lockBody = Instance.new("Frame", lockContainer)
lockBody.Name                 = "Body"
lockBody.Size                 = UDim2.new(0, 34, 0, 26)
lockBody.Position             = UDim2.new(0.5, -17, 0.5, -7)
lockBody.BackgroundColor3     = Theme.Premium
lockBody.BorderSizePixel      = 0
lockBody.ZIndex               = 9014
local bodyCorner = Instance.new("UICorner", lockBody)
bodyCorner.CornerRadius   = UDim.new(0, 4)

local keyholeCircle = Instance.new("Frame", lockBody)
keyholeCircle.Name                 = "KeyholeCircle"
keyholeCircle.Size                 = UDim2.new(0, 6, 0, 6)
keyholeCircle.Position             = UDim2.new(0.5, -3, 0.3, 0)
keyholeCircle.BackgroundColor3     = Theme.BgCard
keyholeCircle.BorderSizePixel      = 0
keyholeCircle.ZIndex               = 9015
local kcCorner = Instance.new("UICorner", keyholeCircle)
kcCorner.CornerRadius   = UDim.new(1, 0)

local keyholeLine = Instance.new("Frame", lockBody)
keyholeLine.Name                 = "KeyholeLine"
keyholeLine.Size                 = UDim2.new(0, 3, 0, 8)
keyholeLine.Position             = UDim2.new(0.5, -1.5, 0.3, 4)
keyholeLine.BackgroundColor3     = Theme.BgCard
keyholeLine.BorderSizePixel      = 0
keyholeLine.ZIndex               = 9015
local klCorner = Instance.new("UICorner", keyholeLine)
klCorner.CornerRadius   = UDim.new(0, 1)

local title = Instance.new("TextLabel", card)
title.Size                = UDim2.new(1, -30, 0, 28)
title.Position            = UDim2.new(0, 15, 0, 106)
title.BackgroundTransparency = 1
title.Text                = "PREMIUM FEATURE"
title.TextColor3          = Theme.Premium
title.Font                = Enum.Font.GothamBlack
title.TextSize            = 20
title.ZIndex              = 9012

local feat = Instance.new("TextLabel", card)
feat.Size                 = UDim2.new(1, -30, 0, 22)
feat.Position             = UDim2.new(0, 15, 0, 136)
feat.BackgroundTransparency = 1
feat.Text                 = "â" .. label .. "â is locked."
feat.TextColor3           = Theme.TextPrimary
feat.Font                 = Enum.Font.GothamSemibold
feat.TextSize             = 14
feat.TextTruncate         = Enum.TextTruncate.AtEnd
feat.ZIndex               = 9012

local sub = Instance.new("TextLabel", card)
sub.Size                  = UDim2.new(1, -30, 0, 20)
sub.Position              = UDim2.new(0, 15, 0, 160)
sub.BackgroundTransparency = 1
sub.Text                  = "Unlock every Azedo HUB Premium feature at"
sub.TextColor3            = Theme.TextSecondary
sub.Font                  = Enum.Font.Gotham
sub.TextSize              = 12
sub.ZIndex                = 9012

local siteBtn = Instance.new("TextButton", card)
siteBtn.Size              = UDim2.new(1, -60, 0, 44)
siteBtn.Position          = UDim2.new(0, 30, 0, 188)
siteBtn.BackgroundColor3  = Theme.Premium
siteBtn.BorderSizePixel   = 0
siteBtn.AutoButtonColor   = false
siteBtn.Text              = "✦  " .. PREM_SITE .. "    •  Click to copy"
siteBtn.TextColor3        = Color3.fromRGB(20, 15, 0)
siteBtn.Font              = Enum.Font.GothamBlack
siteBtn.TextSize          = 15
siteBtn.ZIndex            = 9012
corner(10, siteBtn)
stroke(Theme.PremiumDark, 1, siteBtn)
gradient(siteBtn, Theme.Premium, Theme.PremiumDark, 90)

siteBtn.MouseEnter:Connect(function() tw(siteBtn, { Size = UDim2.new(1, -52, 0, 46) }, 0.15) end)
siteBtn.MouseLeave:Connect(function() tw(siteBtn, { Size = UDim2.new(1, -60, 0, 44) }, 0.15) end)
siteBtn.Activated:Connect(function()
local copied = false
pcall(function()
if setclipboard   then setclipboard(PREM_SITE_URL);   copied = true end
end)
if not copied then
    pcall(function() toclipboard(PREM_SITE_URL); copied = true end)
end
siteBtn.Text = copied
and ("✔  Copied  " .. PREM_SITE_URL)
or ("Visit " .. PREM_SITE)
tw(siteBtn, { BackgroundColor3 = Theme.Success }, 0.15)
task.delay(1.4, function()
pcall(function()
siteBtn.Text = "✦  " .. PREM_SITE .. "    •  Click to copy"
tw(siteBtn, { BackgroundColor3 = Theme.Premium }, 0.2)
end)
end)
end)

local closeBtn = Instance.new("TextButton", card)
closeBtn.Size             = UDim2.new(1, -60, 0, 32)
closeBtn.Position         = UDim2.new(0, 30, 0, 242)
closeBtn.BackgroundColor3 = Theme.BgInput
closeBtn.BorderSizePixel  = 0
closeBtn.AutoButtonColor  = false
closeBtn.Text             = "Maybe later"
closeBtn.TextColor3       = Theme.TextSecondary
closeBtn.Font             = Enum.Font.GothamMedium
closeBtn.TextSize         = 13
closeBtn.ZIndex           = 9012
corner(8, closeBtn)
stroke(Theme.Border, 1, closeBtn)

local function dismiss()
    if not scr.Parent then return end
    tw(back, { BackgroundTransparency = 1 }, 0.2)
    tw(card, { Size = UDim2.fromOffset(0, 0) }, 0.18, Enum.EasingStyle.Back, Enum.EasingDirection.In)
    task.delay(0.22, function() pcall(function() scr:Destroy() end); if _activePremModal == scr then _activePremModal = nil end end)
end
closeBtn.Activated:Connect(dismiss)
back.Activated:Connect(dismiss)

card.Size = UDim2.fromOffset(0, 0)
tw(card, { Size = UDim2.fromOffset(400, 300) }, 0.35, Enum.EasingStyle.Back, Enum.EasingDirection.Out)

task.delay(7, function() if _activePremModal == scr then dismiss() end end)
end)

if not ok then
    local label = featureName and ("'" .. tostring(featureName) .. "'") or "That feature"
    AzedoHubLib:Notify("✦  PREMIUM LOCKED",
    label .. " is locked to Azedo HUB Premium. Upgrade at " .. PREM_SITE, 5, true)
end
end

local InternalLib
do
    local AzedoHub_UI_URL = "https://raw.githubusercontent.com/0x1s2s/Snowy-Ui-Library/5f1a093d63944fbeaf0801f6be09cef876b538ff/SnowyUi"

    local function loadAzedoHubUI()
        local ok, result = pcall(function()
        return loadstring(game:HttpGet(AzedoHub_UI_URL, true))()
    end)
    if not ok or type(result) ~= "table" or type(result.Window) ~= "function" then
        error("[Azedo HUBHub] Azedo HUB UI v2 could not be loaded: " .. tostring(result), 0)
    end
    return result
end

local AzedoHubUIV2 = loadAzedoHubUI()
local activeNativeWindow
local activeUsedIcons = {}

InternalLib = {
Toggled = true,
Accent = Color3.fromRGB(85, 213, 254),
}

local ICON_POOL = {
"house-bold", "crosshair-bold", "user-circle-bold", "eye-bold",
"gear-six-bold", "sliders-horizontal-bold", "sword-bold", "shield-check-bold",
"lightning-bold", "globe-hemisphere-west-bold", "map-pin-bold", "chart-line-up-bold",
"car-profile-bold", "leaf-bold", "fish-simple-bold", "coins-bold",
"backpack-bold", "rocket-launch-bold", "game-controller-bold", "wrench-bold",
"sparkle-bold", "skull-bold", "binoculars-bold", "person-simple-run-bold",
"target-bold", "flame-bold", "cube-bold", "package-bold",
"compass-bold", "magic-wand-bold", "crown-bold", "snowflake-bold",
}

local LEGACY_ICONS = {
home = "house-bold", target = "crosshair-bold", account = "user-circle-bold",
user = "user-circle-bold", eye = "eye-bold", settings = "gear-six-bold",
sliders = "sliders-horizontal-bold", swords = "sword-bold", shield = "shield-check-bold",
zap = "lightning-bold", globe = "globe-hemisphere-west-bold",
["map-pin"] = "map-pin-bold", ["bar-chart-2"] = "chart-line-up-bold",
layout = "squares-four-bold", search = "magnifying-glass-bold",
save = "floppy-disk-bold", flame = "flame-bold",
}

local function semanticIcon(title)
    local value = string.lower(tostring(title or ""))
    if value:find("combat", 1, true) or value:find("aim", 1, true) or value:find("gun", 1, true) then return "crosshair-bold" end
    if value:find("visual", 1, true) or value:find("esp", 1, true) then return "eye-bold" end
    if value:find("move", 1, true) or value:find("speed", 1, true) or value:find("travel", 1, true) then return "person-simple-run-bold" end
    if value:find("farm", 1, true) or value:find("collect", 1, true) then return "leaf-bold" end
    if value:find("setting", 1, true) or value:find("config", 1, true) then return "gear-six-bold" end
    if value:find("player", 1, true) or value:find("character", 1, true) then return "user-circle-bold" end
    if value:find("money", 1, true) or value:find("economy", 1, true) then return "coins-bold" end
    if value:find("teleport", 1, true) or value:find("world", 1, true) then return "globe-hemisphere-west-bold" end
    if value:find("misc", 1, true) or value:find("utility", 1, true) then return "wrench-bold" end
    return nil
end

local function wrapLabel(handle)
    local wrapped = { Instance = handle and handle.Instance }
    function wrapped:Set(value)
        if self.Instance and self.Instance:IsA("TextLabel") then
            self.Instance.Text = tostring(value)
        end
    end
    function wrapped:Get()
        return self.Instance and self.Instance.Text or nil
    end
    return wrapped
end

local function wrapSection(nativeSection)
    local section = {}

    function section:CreateToggle(name, default, callback)
        return nativeSection:Toggle({
        Text = tostring(name), Default = default == true, Callback = callback,
        })
    end

    function section:CreateButton(name, callback)
        return nativeSection:Button({
        Text = tostring(name), ButtonText = "Run", Callback = callback,
        })
    end

    function section:CreateSlider(name, minimum, maximum, default, callback)
        minimum, maximum = tonumber(minimum) or 0, tonumber(maximum) or 100
        if maximum < minimum then minimum, maximum = maximum, minimum end
        return nativeSection:Slider({
        Text = tostring(name), Min = minimum, Max = maximum,
        Default = math.clamp(tonumber(default) or minimum, minimum, maximum),
        Callback = callback,
        })
    end

    function section:CreateDropdown(name, items, default, callback)
        local options = type(items) == "table" and items or {}
        local selected = default
        if selected == nil and #options > 0 then selected = options[1] end
        local current
        local wrapped = {}

        local function create(values, value)
            current = nativeSection:Dropdown({
            Text = tostring(name), Options = values, Default = value,
            Placeholder = "Select an option", Callback = callback,
            })
            wrapped.Instance = current.Instance
        end

        create(options, selected)
        function wrapped:Set(value) return current:Set(value) end
            function wrapped:Get() return current:Get() end
                function wrapped:Refresh(values)
                    if current and current.Instance then current.Instance.Visible = false end
                    options = type(values) == "table" and values or {}
                    local value = #options > 0 and options[1] or nil
                    create(options, value)
                end
                return wrapped
            end

            function section:CreateLabel(content)
                local clean = tostring(content or ""):gsub("^──%s*", ""):gsub("%s*──$", "")
                return wrapLabel(nativeSection:Label(clean))
            end

            function section:CreateInput(name, placeholder, callback)
                return nativeSection:Input({
                Text = tostring(name), Placeholder = tostring(placeholder or "Type here..."),
                Default = "", Callback = callback,
                })
            end

            return section
        end

        function InternalLib:GetIcon(name)
            return LEGACY_ICONS[name] or "house-bold"
        end

        function InternalLib:MakeDraggable()
            return function() return false end
        end

        function InternalLib:CreateWindow(title)
            if not (activeNativeWindow and activeNativeWindow.gui and activeNativeWindow.gui.Parent) then
                activeNativeWindow = AzedoHubUIV2:Window({
                Title = "Azedo HUB Studios",
                Subtitle = tostring(title or (_detectedGame and _detectedGame.name) or "Azedo HUB Hub"),
                IconPack = "phosphor",
                Logo = "rbxassetid://96897412898182",
                LogoRectOffset = Vector2.new(40, 256),
                LogoRectSize = Vector2.new(945, 457),
                Keybind = Enum.KeyCode.RightShift,
                })
                activeUsedIcons = {}
            end
            local nativeWindow = activeNativeWindow
            local usedIcons = activeUsedIcons
            local function claimIcon(requested, tabName)
                local legacyKey = string.lower(tostring(requested or ""))
                local candidate = semanticIcon(tabName) or LEGACY_ICONS[legacyKey]
                if type(candidate) == "string" and candidate ~= "" and not usedIcons[candidate] then
                    usedIcons[candidate] = true
                    return candidate
                end
                for _, iconName in ipairs(ICON_POOL) do
                    if not usedIcons[iconName] then
                        usedIcons[iconName] = true
                        return iconName
                    end
                end
                local fallback = "number-circle-" .. tostring(#nativeWindow.tabs + 1) .. "-bold"
                usedIcons[fallback] = true
                return fallback
            end

            local wind = {
            Current = nil,
            MainContainer = nativeWindow.gui,
            NativeWindow = nativeWindow,
            Tabs = {},
            }

            function wind:CreateTab(name, requestedIcon)
                local tabName = tostring(name or "Main")
                local nativeTab = nativeWindow:Tab({
                Name = tabName,
                Icon = claimIcon(requestedIcon, tabName),
                Description = tabName .. " features and controls",
                })
                local tab = { SubTabs = {}, NativeTab = nativeTab, _sectionCount = 0 }

                function tab:CreateSubTab(subName)
                    self._sectionCount = self._sectionCount + 1
                    local subTab = { Name = tostring(subName or "Main"), ParentTab = self }

                    function subTab:CreateSection(sectionName)
                        local titleText = tostring(sectionName or self.Name)
                        local nativeSection = nativeTab:Section({
                        Title = titleText,
                        Column = ((self.ParentTab._sectionCount - 1) % 2) + 1,
                        })
                        return wrapSection(nativeSection)
                    end

                    function subTab:Select()
                        nativeWindow:Select(nativeTab.index)
                    end

                    table.insert(self.SubTabs, subTab)
                    return subTab
                end

                function tab:Select()
                    nativeWindow:Select(nativeTab.index)
                    wind.Current = { Tab = self }
                end

                function tab:SetVisible(state)
                    nativeTab.button.Visible = state ~= false
                    nativeWindow:LayoutRail()
                end

                self.Tabs[tabName] = tab
                if not self.Current then tab:Select() end
                return tab
            end

            function wind:Destroy()
                nativeWindow:Destroy()
            end

            return wind
        end

    end

    local function resolveDropdownArgs(d, list, cb)
        local items, def, callback
        if type(d) == "table" then
            items = d
            def = type(list) == "string" and list or nil
            callback = type(list) == "function" and list or cb
        else
        items = list
        def = nil
        callback = cb
    end
    return items or {}, def, callback
end

local Kavo = {}
function Kavo.CreateLib(hubName, _)
    local Window = InternalLib:CreateWindow(hubName)
    local BridgeWindow = { Tabs = {}, Window = Window }

    function BridgeWindow:NewTab(tabName)
        local icon = "home"
        local low = tabName:lower()
        if low:find("aim", 1, true) or low:find("combat", 1, true) or low:find("kill", 1, true) or low:find("gun", 1, true) then icon = "target"
        elseif low:find("player", 1, true) or low:find("user", 1, true) or low:find("char", 1, true) or low:find("move", 1, true) then icon = "user"
    elseif low:find("visual", 1, true) or low:find("esp", 1, true) or low:find("eye", 1, true) or low:find("render", 1, true) then icon = "eye"
elseif low:find("setting", 1, true) or low:find("config", 1, true) or low:find("ui", 1, true) then icon = "settings"
elseif low:find("util", 1, true) or low:find("misc", 1, true) or low:find("tool", 1, true) then icon = "sliders"
elseif low:find("game", 1, true) or low:find("hub", 1, true) or low:find("main", 1, true) then icon = "layout"
end

local ActualTab = Window:CreateTab(tabName, icon)
local BridgeTab = { ActualTab = ActualTab }

function BridgeTab:NewSection(secName)
    local sIcon = "layout"
    local slow = (secName or ""):lower()
    if slow:find("combat", 1, true) then sIcon = "swords"
    elseif slow:find("visual", 1, true) then sIcon = "eye"
elseif slow:find("main", 1, true) or slow:find("home", 1, true) then sIcon = "home"
elseif slow:find("setting", 1, true) or slow:find("config", 1, true) then sIcon = "settings"
end

local SubTab = self.ActualTab:CreateSubTab(secName or "Main", sIcon)
local Section = SubTab:CreateSection(secName or "Main")

local BridgeSec = {}
function BridgeSec:NewToggle(n, d, cb) return Section:CreateToggle(n, false, cb) end
    function BridgeSec:NewSlider(n, d, max, min, cb) return Section:CreateSlider(n, min, max, min, cb) end
        function BridgeSec:NewButton(n, d, cb) return Section:CreateButton(n, cb) end
            function BridgeSec:NewDropdown(n, d, list, cb)
                local items, def, callback = resolveDropdownArgs(d, list, cb)
                return Section:CreateDropdown(n, items, def, callback)
            end
            function BridgeSec:NewLabel(txt) return Section:CreateLabel(txt) end
                function BridgeSec:NewInput(n, placeholder, cb) return Section:CreateInput(n, placeholder, cb) end
                    function BridgeSec:NewTitle(txt) return Section:CreateLabel("✦ " .. txt) end
                        function BridgeSec:NewSection() return Section end

                            function BridgeSec:NewPremiumToggle(n, d, cb)
                                local handle
                                local suppressing = false
                                handle = Section:CreateToggle(_premTag(n), false, function(state)
                                if suppressing then return end
                                if _isPremiumUser() then
                                    if cb then cb(state) end
                                elseif state then
                                _nagPremium(n)
                                suppressing = true
                                task.defer(function()
                                pcall(function() handle:Set(false) end)
                                task.wait(); suppressing = false
                            end)
                        end
                    end)
                    return handle
                end
                function BridgeSec:NewPremiumButton(n, d, cb)
                    return Section:CreateButton(_premTag(n), function()
                    if _isPremiumUser() then if cb then cb() end else _nagPremium(n) end
                end)
            end
            function BridgeSec:NewPremiumSlider(n, d, max, min, cb)
                return Section:CreateSlider(_premTag(n), min or 0, max or 100, min or 0, function(v)
                if _isPremiumUser() then if cb then cb(v) end else _nagPremium(n) end
            end)
        end
        function BridgeSec:NewPremiumDropdown(n, d, list, cb)
            local items, def, callback = resolveDropdownArgs(d, list, cb)
            return Section:CreateDropdown(_premTag(n), items, def, function(v)
            if _isPremiumUser() then if callback then callback(v) end else _nagPremium(n) end
        end)
    end
    return BridgeSec
end

function BridgeTab:HideTabButton() self.ActualTab:SetVisible(false) end
    function BridgeTab:Select() self.ActualTab:Select() end
        return BridgeTab
    end

    return BridgeWindow
end

local nnLocalPlayer = game:GetService("Players").LocalPlayer
local nnUIS = game:GetService("UserInputService")
local nnRep = game:GetService("ReplicatedStorage")
local bhRS = game:GetService("RunService")
local nnCam = workspace.CurrentCamera

local function nnChar() return nnLocalPlayer.Character end
    local function nnHum() local c = nnChar() return c and c:FindFirstChildWhichIsA("Humanoid") end
    local function nnHRP() local c = nnChar() return c and c:FindFirstChild("HumanoidRootPart") end

    local function nnFindValue(...)
        local args = {...}
        for _, obj in pairs(workspace:GetDescendants()) do
            for _, name in pairs(args) do
                if obj.Name:lower() == name:lower() and (obj:IsA("IntValue") or obj:IsA("NumberValue")) then
                    return obj
                end
            end
        end
        return nil
    end

    local Config = { HubName = "Azedo HUB Studios", AccentColor = Color3.fromRGB(0, 162, 255) }
    local function notify(title, text, dur) AzedoHubLib:Notify(title, text, dur, false) end

        getgenv().Window = Kavo.CreateLib(Config.HubName, "Midnight")
        local Window = getgenv().Window

        do
            local environment = getgenv()
            local gameName = (_detectedGame and _detectedGame.name) or "Unknown"
            environment.__AzedoHubPayloadReady = {
            nonce = environment.__AzedoHubLaunchNonce,
            game = gameName,
            ui = true,
            at = os.clock(),
            }
            print("[Azedo HUBHub] Payload UI ready: " .. tostring(gameName))
        end

        getgenv().AzedoHubLib = AzedoHubLib; getgenv().Kavo = Kavo; getgenv().notify = notify

        task.defer(function()
        pcall(function()
        local overlay = getgenv().__AzedoHubLoadingOverlay
        if overlay then

            overlay:Destroy()
            getgenv().__AzedoHubLoadingOverlay = nil
        end
    end)
end)

local _gameTabCreated = {}

local function gameTab(gameName, tabDisplayName)
    if not (_detectedGame and _detectedGame.name:lower() == gameName:lower()) then
        local noop = function() end
        local stubSec = {}
        stubSec.NewToggle   = noop
        stubSec.NewButton   = noop
        stubSec.NewSlider   = noop
        stubSec.NewDropdown = noop
        stubSec.NewLabel    = noop
        stubSec.NewPremiumToggle   = noop
        stubSec.NewPremiumButton   = noop
        stubSec.NewPremiumSlider   = noop
        stubSec.NewPremiumDropdown = noop
        stubSec.NewSection  = function() return stubSec end
        local stub = {}
        stub.NewSection = function() return stubSec end
        stub.NewToggle  = noop
        stub.NewButton  = noop
        stub.NewSlider  = noop
        stub.NewDropdown = noop
        stub.NewPremiumToggle   = noop
        stub.NewPremiumButton   = noop
        stub.NewPremiumSlider   = noop
        stub.NewPremiumDropdown = noop
        return stub

    end

    local categorySideTabs = {}
    local categorySections = {}

    local function getOrCreateCategory(categoryName, categoryIcon)
        if categorySideTabs[categoryName] then
            return categorySections[categoryName]
        end
        local st  = Window.Window:CreateTab(categoryName, categoryIcon or "layout")
        local sub = st:CreateSubTab("Main", "layout")
        local sec = sub:CreateSection("Features")
        categorySideTabs[categoryName] = st
        categorySections[categoryName] = sec
        return sec
    end

    local Factory = {}
    function Factory:NewSection(secName)
        local rawName = secName or "General"
        local slow    = rawName:lower()
        local category, catIcon = "General", "globe"

        if slow:find("free feat") then
            category, catIcon = "Free", "home"
        elseif slow:find("premium feat") then
        category, catIcon = "Premium", "zap"
    elseif slow:find("combat") or slow:find("aimbot") or slow:find("aim")
    or slow:find("weapon") or slow:find("gun") or slow:find("silent")
    or slow:find("aura") then
    category, catIcon = "Combat", "swords"
elseif slow:find("esp") or slow:find("visual") or slow:find("wallhack")
or slow:find("radar") or slow:find("chams") or slow:find("render") then
category, catIcon = "Visuals", "eye"
elseif slow:find("move") or slow:find("fly") or slow:find("speed")
or slow:find("noclip") or slow:find("jump") or slow:find("travel")
or slow:find("tp") or slow:find("teleport") then
category, catIcon = "Movement", "bar-chart-2"
elseif slow:find("farm") or slow:find("auto") or slow:find("fish")
or slow:find("chest") or slow:find("collect") or slow:find("resource") then
category, catIcon = "AutoFarm", "zap"
elseif slow:find("survival") or slow:find("god") or slow:find("oxygen")
or slow:find("survive") or slow:find("anti") then
category, catIcon = "Survival", "shield"
elseif slow:find("troll") or slow:find("rage") or slow:find("hitbox") then
category, catIcon = "Rage", "flame"
elseif slow:find("misc") or slow:find("util") or slow:find("extra")
or slow:find("general") or slow:find("server") then
category, catIcon = "Utility", "sliders"
elseif slow:find("economy") or slow:find("money") or slow:find("shop") then
category, catIcon = "Economy", "bar-chart-2"
elseif slow:find("belt") then
category, catIcon = "Belts", "layout"
elseif slow:find("tank") then
category, catIcon = "Tanks", "layout"
elseif slow:find("slot") then
category, catIcon = "Slots", "layout"
end

local sec = getOrCreateCategory(category, catIcon)
sec:CreateLabel("── " .. rawName .. " ──")

local Bridge = {}

function Bridge:NewToggle(n, d, cb)   return sec:CreateToggle(n, false, cb) end
    function Bridge:NewButton(n, d, cb)   return sec:CreateButton(n, cb) end
        function Bridge:NewSlider(n, d, max, min, cb)
            return sec:CreateSlider(n, min or 0, max or 100, min or 0, cb)
        end
        function Bridge:NewDropdown(n, d, list, cb)
            local items, def, callback = resolveDropdownArgs(d, list, cb)
            return sec:CreateDropdown(n, items, def, callback)
        end
        function Bridge:NewLabel(txt) return sec:CreateLabel(txt) end
            function Bridge:NewInput(n, placeholder, cb) return sec:CreateInput(n, placeholder, cb) end
                function Bridge:NewTitle(txt) return sec:CreateLabel("✦ " .. txt) end
                    function Bridge:NewSection()  return sec end

                        function Bridge:NewPremiumToggle(n, d, cb)
                            local handle
                            local suppressing = false
                            handle = sec:CreateToggle(_premTag(n), false, function(state)
                            if suppressing then return end
                            if _isPremiumUser() then
                                if cb then cb(state) end
                            elseif state then
                            _nagPremium(n)
                            suppressing = true
                            task.defer(function()
                            pcall(function() handle:Set(false) end)
                            task.wait(); suppressing = false
                        end)
                    end
                end)
                return handle
            end
            function Bridge:NewPremiumButton(n, d, cb)
                return sec:CreateButton(_premTag(n), function()
                if _isPremiumUser() then if cb then cb() end else _nagPremium(n) end
            end)
        end
        function Bridge:NewPremiumSlider(n, d, max, min, cb)
            return sec:CreateSlider(_premTag(n), min or 0, max or 100, min or 0, function(v)
            if _isPremiumUser() then if cb then cb(v) end else _nagPremium(n) end
        end)
    end
    function Bridge:NewPremiumDropdown(n, d, list, cb)
        local items, def, callback = resolveDropdownArgs(d, list, cb)
        return sec:CreateDropdown(_premTag(n), items, def, function(v)
        if _isPremiumUser() then if callback then callback(v) end else _nagPremium(n) end
    end)
end
return Bridge

end

local noop = function() end
Factory.NewToggle  = noop
Factory.NewButton  = noop
Factory.NewSlider  = noop
return Factory
end
getgenv().gameTab = gameTab

local function createUIBridge(gameName)
    local tabFactory = gameTab(gameName, gameName)
    local Bridge = {}

    local freeSec = tabFactory:NewSection("Free Features")
    local premSec = tabFactory:NewSection("Premium Features")

    local function isFeaturePremium(title, desc)
        local t = (title or ""):lower()
        local d = (desc or ""):lower()
        local keywords = {
        "auto", "farm", "kill", "aura", "tp", "teleport", "fly", "speed",
        "click", "mansion", "oil", "ship", "airdrop", "rob", "npc", "turret",
        "glider", "items", "give"
        }
        for _, kw in ipairs(keywords) do
            if t:find(kw, 1, true) or d:find(kw, 1, true) then
                return true
            end
        end
        return false
    end

    function Bridge:CreateWindow(options)
        local win = {}
        function win:AddTab(tabOptions)
            local tabObj = {}
            function tabObj:AddSection(secName) return self end

                function tabObj:AddButton(opts)
                    local isPrem = isFeaturePremium(opts.Title or opts.Name, opts.Description or opts.Desc)
                    if isPrem then
                        premSec:NewPremiumButton(opts.Title or opts.Name or "Button", opts.Description or opts.Desc or "", opts.Callback)
                    else
                    freeSec:NewButton(opts.Title or opts.Name or "Button", opts.Description or opts.Desc or "", opts.Callback)
                end
            end

            function tabObj:AddToggle(id, opts)
                local options = opts or id
                local title = options.Title or options.Name or "Toggle"
                local desc = options.Description or options.Desc or ""
                local callback = options.Callback
                local default = options.Default or false

                local isPrem = isFeaturePremium(title, desc)
                local activeCallback = callback
                local handle

                if isPrem then
                    handle = premSec:NewPremiumToggle(title, desc, function(state)
                    if activeCallback then pcall(function() activeCallback(state) end) end
                end)
            else
            handle = freeSec:NewToggle(title, desc, function(state)
            if activeCallback then pcall(function() activeCallback(state) end) end
        end)
    end

    if default and handle and handle.Set then
        pcall(function() handle:Set(default) end)
    end

    local toggleWrapper = {}
    function toggleWrapper:OnChanged(cb)
        activeCallback = cb
        return toggleWrapper
    end
    function toggleWrapper:SetValue(val)
        pcall(function() handle:Set(val) end)
    end
    return toggleWrapper
end

function tabObj:AddSlider(id, opts)
    local options = opts or id
    local title = options.Title or options.Name or "Slider"
    local desc = options.Description or options.Desc or ""
    local callback = options.Callback
    local min = options.Min or 0
    local max = options.Max or 100
    local default = options.Default or min

    local isPrem = isFeaturePremium(title, desc)
    local activeCallback = callback
    local handle

    if isPrem then
        handle = premSec:NewPremiumSlider(title, desc, max, min, function(v)
        if activeCallback then pcall(function() activeCallback(v) end) end
    end)
else
handle = freeSec:NewSlider(title, desc, max, min, function(v)
if activeCallback then pcall(function() activeCallback(v) end) end
end)
end

if default and handle and handle.Set then
    pcall(function() handle:Set(default) end)
end

local sliderWrapper = {}
function sliderWrapper:OnChanged(cb)
    activeCallback = cb
    return sliderWrapper
end
function sliderWrapper:SetValue(val)
    pcall(function() handle:Set(val) end)
end
return sliderWrapper
end

function tabObj:AddDropdown(id, opts)
    local options = opts or id
    local title = options.Title or options.Name or "Dropdown"
    local desc = options.Description or options.Desc or ""
    local callback = options.Callback
    local values = options.Values or options.List or {}
    local default = options.Default or nil

    local isPrem = isFeaturePremium(title, desc)
    local activeCallback = callback
    local handle

    if isPrem then
        handle = premSec:NewPremiumDropdown(title, desc, values, function(v)
        if activeCallback then pcall(function() activeCallback(v) end) end
    end)
else
handle = freeSec:NewDropdown(title, desc, values, function(v)
if activeCallback then pcall(function() activeCallback(v) end) end
end)
end

if default and handle and handle.Set then
    pcall(function() handle:Set(default) end)
end

local ddWrapper = {}
function ddWrapper:OnChanged(cb)
    activeCallback = cb
    return ddWrapper
end
function ddWrapper:SetValue(val)
    pcall(function() handle:Set(val) end)
end
return ddWrapper
end

function tabObj:AddLabel(txt) freeSec:NewLabel(txt) end
    function tabObj:AddParagraph(opts) freeSec:NewLabel((opts.Title or "") .. ": " .. (opts.Content or "")) end
        function tabObj:AddKeybind(id, opts) return { OnChanged = function(self, cb) return self end, SetValue = function(self, val) end } end
            function tabObj:AddColorpicker(id, opts) return { OnChanged = function(self, cb) return self end, SetValue = function(self, val) end } end

                return tabObj
            end
            function win:SelectTab(idx) end
                return win
            end
            function Bridge:Notify(opts)
                pcall(function() AzedoHubLib:Notify(opts.Title or "Notification", opts.Content or opts.Text or "", opts.Duration or 3) end)
            end
            return Bridge
        end
        if not _detectedGame then
            getgenv().HomeTab = Window:NewTab("Home")
            getgenv().PlayerTab = Window:NewTab("Player")
            getgenv().VisualTab = Window:NewTab("Visuals")
            getgenv().UtilTab = Window:NewTab("Utility")
            getgenv().AimbotTab = Window:NewTab("Aimbot")
            getgenv().SettingsTab = Window:NewTab("Settings")

            getgenv().HomeSec = HomeTab:NewSection("Status")
            HomeSec:NewLabel(utf8.char(0x2744) .. " Azedo HUB HUB v6.0")

            getgenv().PlayerSection = PlayerTab:NewSection("General")
            getgenv().VisualSection = VisualTab:NewSection("ESP & Effects")
            getgenv().UtilSection   = UtilTab:NewSection("General Tools")
            getgenv().AimbotSection = AimbotTab:NewSection("Combat Settings")
            getgenv().UISec        = SettingsTab:NewSection("Interface")
            getgenv().InfoSec      = SettingsTab:NewSection("Info")

            getgenv().WalkSpeedValue = 16
            getgenv().JumpPowerValue = 50
            getgenv().GodModeEnabled = false

            if getgenv().PlayerModifierLoop then
                pcall(function() getgenv().PlayerModifierLoop:Disconnect() end)
                getgenv().PlayerModifierLoop = nil
            end

            getgenv().PlayerModifierLoop = game:GetService("RunService").RenderStepped:Connect(function(deltaTime)
            pcall(function()
            local char = game.Players.LocalPlayer.Character
            if char and char:FindFirstChild("Humanoid") and char:FindFirstChild("HumanoidRootPart") then
                local hum = char.Humanoid
                if getgenv().GodModeEnabled then
                    if hum.MaxHealth ~= math.huge then hum.MaxHealth = math.huge end
                    if hum.Health ~= math.huge then hum.Health = math.huge end
                    pcall(function() hum:SetStateEnabled(Enum.HumanoidStateType.Dead, false) end)
                end
            end
        end)
    end)

    PlayerSection:NewSlider("WalkSpeed", "Changes player speed", 250, 16, function(value)
    getgenv().WalkSpeedValue = value
    if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = value
    end
end)

PlayerSection:NewSlider("JumpPower", "Changes jump height", 250, 50, function(value)
getgenv().JumpPowerValue = value
if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
    game.Players.LocalPlayer.Character.Humanoid.JumpPower = value
end
end)

PlayerSection:NewSlider("Gravity", "Changes world gravity", 196, 0, function(value)
game.Workspace.Gravity = value
end)

PlayerSection:NewPremiumToggle("God Mode", "Instantly regenerates health", function(state)
getgenv().GodModeEnabled = state
if not state then
    pcall(function()
    local char = game.Players.LocalPlayer.Character
    if char and char:FindFirstChild("Humanoid") then
        char.Humanoid.MaxHealth = 100
        char.Humanoid.Health = 100
        char.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, true)
    end
end)
end
end)

local Noclip = nil
local Clip = nil

function noclip()
    Clip = false
    local function Nocl()
        if Clip == false and game.Players.LocalPlayer.Character ~= nil then
            for _,v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                if v:IsA('BasePart') and v.CanCollide and v.Name ~= floatName then
                    v.CanCollide = false
                end
            end
        end
        wait(0.21)
    end
    Noclip = game:GetService('RunService').Stepped:Connect(Nocl)
end

function clip()
    if Noclip then Noclip:Disconnect() end
    Clip = true
end

PlayerSection:NewPremiumToggle("Noclip", "Walk through objects and walls", function(state)
if state then noclip() else clip() end
end)

local isInfJump = false
PlayerSection:NewPremiumToggle("Infinite Jump", "Jump in mid-air", function(state)
isInfJump = state
game:GetService("UserInputService").JumpRequest:Connect(function()
if isInfJump and game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
    game.Players.LocalPlayer.Character.Humanoid:ChangeState("Jumping")
end
end)
end)

VisualSection:NewSlider("Field of View", "Change camera FOV", 120, 70, function(value)
game.Workspace.CurrentCamera.FieldOfView = value
end)

VisualSection:NewToggle("Fullbright", "Removes shadows and darkness", function(state)
if state then
    game.Lighting.Ambient = Color3.new(1, 1, 1)
    game.Lighting.ColorShift_Bottom = Color3.new(1, 1, 1)
    game.Lighting.ColorShift_Top = Color3.new(1, 1, 1)
    game.Lighting.GlobalShadows = false
else
game.Lighting.Ambient = Color3.fromRGB(128, 128, 128)
game.Lighting.GlobalShadows = true
end
end)

local espHighlights = {}
VisualSection:NewPremiumToggle("Player ESP", "Highlights all players through walls", function(state)
if state then
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character then
            local highlight = Instance.new("Highlight")
            highlight.Parent = player.Character
            highlight.FillColor = Config.AccentColor
            highlight.OutlineColor = Config.SecondaryColor
            highlight.FillTransparency = 0.5
            table.insert(espHighlights, highlight)
        end
    end
else
for _, highlight in pairs(espHighlights) do
    if highlight and highlight.Parent then highlight:Destroy() end
end
espHighlights = {}
end
end)

VisualSection:NewPremiumSlider("Hitbox Expander", "Make enemies easier to hit", 20, 2, function(value)
_G.HeadSize = value
_G.Disabled = true
game:GetService('RunService').RenderStepped:connect(function()
if _G.Disabled then
    for i,v in next, game:GetService('Players'):GetPlayers() do
        if v.Name ~= game:GetService('Players').LocalPlayer.Name then
            pcall(function()
            v.Character.HumanoidRootPart.Size = Vector3.new(_G.HeadSize,_G.HeadSize,_G.HeadSize)
            v.Character.HumanoidRootPart.Transparency = 0.7
            v.Character.HumanoidRootPart.BrickColor = BrickColor.new("Really blue")
            v.Character.HumanoidRootPart.Material = "Neon"
            v.Character.HumanoidRootPart.CanCollide = false
        end)
    end
end
end
end)
end)

UtilSection:NewButton("Give Btools", "Gives building tools to inventory", function()
local tool1 = Instance.new("HopperBin", game.Players.LocalPlayer.Backpack)
tool1.BinType = "Clone"
local tool2 = Instance.new("HopperBin", game.Players.LocalPlayer.Backpack)
tool2.BinType = "Hammer"
local tool3 = Instance.new("HopperBin", game.Players.LocalPlayer.Backpack)
tool3.BinType = "Grab"
end)

UtilSection:NewPremiumButton("Teleport to Random Player", "TPs you to a random user", function()
local players = game.Players:GetPlayers()
if #players > 1 then
    local randomPlayer = players[math.random(1, #players)]
    repeat randomPlayer = players[math.random(1, #players)]
    until randomPlayer.Name ~= game.Players.LocalPlayer.Name
    if randomPlayer.Character and randomPlayer.Character:FindFirstChild("HumanoidRootPart") and game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = randomPlayer.Character.HumanoidRootPart.CFrame * CFrame.new(0, 0, 3)
    end
end
end)

UtilSection:NewButton("Rejoin Server", "Rejoins the same server", function()
game:GetService("TeleportService"):TeleportToPlaceInstance(game.PlaceId, game.JobId, game.Players.LocalPlayer)
end)

UtilSection:NewPremiumButton("Server Hop", "Joins a random new server", function()
local Http = game:GetService("HttpService")
local TPS = game:GetService("TeleportService")
local Api = "https://games.roblox.com/v1/games/"
local _place = game.PlaceId
local _servers = Api..tostring(_place).."/servers/Public?sortOrder=Asc&limit=100"
local function ListServers(cursor)
    local Raw = game:HttpGet(_servers .. ((cursor and "&cursor="..cursor) or ""))
    return Http:JSONDecode(Raw)
end
local Server, Next; repeat
local Servers = ListServers(Next)
Server = Servers.data[1]
Next = Servers.nextPageCursor
until Server
TPS:TeleportToPlaceInstance(_place, Server.id, game.Players.LocalPlayer)
end)

UtilSection:NewToggle("Anti-AFK", "Prevents Roblox disconnects", function(state)
local gc = getconnections or get_signal_cons
if state and gc then
    for i,v in pairs(gc(game.Players.LocalPlayer.Idled)) do
        if v["Disable"] then v["Disable"](v)
        elseif v["Disconnect"] then v["Disconnect"](v) end
    end
end
end)

local AimbotParams = {
Enabled = false, ShowFOV = false, FOVRadius = 80,
Smoothness = 0.5, TeamCheck = true, TargetPart = "Head"
}

local FOVCircle = Drawing.new("Circle")
FOVCircle.Position = Vector2.new(game.Workspace.CurrentCamera.ViewportSize.X / 2, game.Workspace.CurrentCamera.ViewportSize.Y / 2)
FOVCircle.Radius = AimbotParams.FOVRadius
FOVCircle.Filled = false; FOVCircle.Color = Color3.fromRGB(255, 255, 255)
FOVCircle.Visible = false; FOVCircle.Thickness = 1

local function getClosestPlayerToCursor()
    local closestDist = AimbotParams.FOVRadius
    local closestPlayer = nil
    local mousePos = game:GetService("UserInputService"):GetMouseLocation()
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild(AimbotParams.TargetPart) and player.Character:FindFirstChild("Humanoid") and player.Character.Humanoid.Health > 0 then
            if AimbotParams.TeamCheck and player.Team == game.Players.LocalPlayer.Team then end
            local screenPos, onScreen = game.Workspace.CurrentCamera:WorldToViewportPoint(player.Character[AimbotParams.TargetPart].Position)
            if onScreen then
                local dist = (Vector2.new(screenPos.X, screenPos.Y) - mousePos).Magnitude
                if dist < closestDist then closestDist = dist; closestPlayer = player end
            end
        end
    end
    return closestPlayer
end

game:GetService("RunService").RenderStepped:Connect(function()
if AimbotParams.ShowFOV then
    FOVCircle.Position = game:GetService("UserInputService"):GetMouseLocation()
    FOVCircle.Radius = AimbotParams.FOVRadius; FOVCircle.Visible = true
else FOVCircle.Visible = false end
if AimbotParams.Enabled and game:GetService("UserInputService"):IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then
    local target = getClosestPlayerToCursor()
    if target and target.Character and target.Character:FindFirstChild(AimbotParams.TargetPart) then
        local currentCam = game.Workspace.CurrentCamera
        local targetCFrame = CFrame.new(currentCam.CFrame.Position, target.Character[AimbotParams.TargetPart].Position)
        currentCam.CFrame = currentCam.CFrame:Lerp(targetCFrame, AimbotParams.Smoothness)
    end
end
end)

AimbotSection:NewPremiumToggle("Enable Aimbot (Right Click)", "Locks onto targets in FOV", function(state)
AimbotParams.Enabled = state
end)
AimbotSection:NewToggle("Show FOV Circle", "Displays the aimbot radius", function(state)
AimbotParams.ShowFOV = state
end)
AimbotSection:NewSlider("FOV Size", "Target area size", 300, 20, function(value)
AimbotParams.FOVRadius = value
end)
AimbotSection:NewPremiumSlider("Aimbot Smoothness", "100=Instant, 1=Slow", 100, 1, function(value)
AimbotParams.Smoothness = value / 100
end)
AimbotSection:NewToggle("Team Check", "Ignores teammates", function(state)
AimbotParams.TeamCheck = state
end)

UISec:NewButton("Test Notification", "Test the notification system", function()
AzedoHubLib:Notify("Test", "Notifications are working!", 4, false)
end)
UISec:NewButton("Test Premium Alert", "Premium-style notification", function()
AzedoHubLib:Notify("Premium Alert", "This is how premium alerts look!", 4, true)
end)
local uiTransparency = 0
UISec:NewSlider("UI Opacity", "Adjust hub transparency", 8, 0, function(v)
uiTransparency = v / 10
pcall(function()

for k, v in pairs(getgenv()) do
    if type(k) == "string" and k:sub(1,3) == "_h_" and typeof(v) == "Instance" and v:IsA("ScreenGui") then
        local m = v:FindFirstChild("Main")
        if m then m.BackgroundTransparency = uiTransparency end
    end
end
end)
end)

InfoSec:NewLabel("Right-click = Hold to aim (Aimbot)")
InfoSec:NewLabel("Drag title bar to move")
InfoSec:NewLabel("discord.gg/jX23ajWzxw")
InfoSec:NewButton("Copy Discord", "Copy invite link", function()
if setclipboard then setclipboard("https://discord.gg/jX23ajWzxw") end
AzedoHubLib:Notify("Copied!", "Discord link copied.", 3, false)
end)

ScriptSec:NewButton("Reload Hub", "Destroys and reloads the UI", function()
pcall(function()
for k, v in pairs(getgenv()) do
    if type(k) == "string" and k:sub(1,3) == "_h_" and typeof(v) == "Instance" and v:IsA("ScreenGui") then
        pcall(function() v:Destroy() end); getgenv()[k] = nil
    end
end
end)
task.wait(0.1)
getgenv().Window = Kavo.CreateLib(Config.HubName, "Midnight")
notify("Reloaded", "Hub UI restarted.", 3)
end)

ScriptSec:NewLabel("Azedo HUB HUB v6.0 | Built by Azedo HUB Studios")
ScriptSec:NewLabel(tostring(#SUPPORTED_GAMES) .. " Games Supported")
else

local function stubTab()
    local noop = function() end
    local stS = {}
    stS.NewToggle   = noop
    stS.NewButton   = noop
    stS.NewSlider   = noop
    stS.NewDropdown = noop
    stS.NewLabel    = noop
    stS.NewPremiumToggle   = noop
    stS.NewPremiumButton   = noop
    stS.NewPremiumSlider   = noop
    stS.NewPremiumDropdown = noop
    stS.NewSection  = function() return stS end

    return {
    NewSection    = function() return stS end,
    HideTabButton = noop,
    Select        = noop,
    ActualTab     = {SetVisible = noop}
    }
end

getgenv().HomeTab     = stubTab()
getgenv().PlayerTab   = stubTab()
getgenv().VisualTab   = stubTab()
getgenv().UtilTab     = stubTab()
getgenv().AimbotTab   = stubTab()
getgenv().SettingsTab = stubTab()

local stubS = HomeTab:NewSection()
getgenv().HomeSec       = stubS
getgenv().PlayerSection = stubS
getgenv().VisualSection = stubS
getgenv().UtilSection   = stubS
getgenv().AimbotSection = stubS
getgenv().UISec        = stubS
getgenv().InfoSec      = stubS
getgenv().ScriptSec    = stubS
end

if _detectedGame and _detectedGame.name == "Strongest Battlegrounds" then
    local TSBTab = gameTab("Strongest Battlegrounds", "Strongest Battlegrounds ")
    do
        local TSBMain = TSBTab:NewSection("Auto Farm")
        local TSBStats = TSBTab:NewSection("Statistics")
        local TSBInfo = TSBTab:NewSection("Credits")

        local Players = game:GetService("Players")
        local RunService = game:GetService("RunService")
        local UIS = game:GetService("UserInputService")
        local lp = Players.LocalPlayer
        local isMobile = UIS.TouchEnabled

        local lowestHpFarmEnabled = false
        local autoUltimateEnabled = false
        local autoEmoteEnabled = false
        local currentTarget = nil
        local farmConnection = nil
        local attackThread = nil

        local farmOffsetX, farmOffsetY, farmOffsetZ = 0, 0, 3

        local startTime = tick()
        local startKills = 0
        local leaderstats = lp:FindFirstChild("leaderstats")
        if leaderstats and leaderstats:FindFirstChild("Kills") then startKills = leaderstats.Kills.Value end
        local KPM_Label = TSBStats:NewLabel("Kills Per Minute: 0.00")

        task.spawn(function()
        while task.wait(1) do
            local currentKills = (leaderstats and leaderstats:FindFirstChild("Kills") and leaderstats.Kills.Value) or 0
            local elapsed = (tick() - startTime) / 60
            KPM_Label:Set(string.format("Kills Per Minute: %.2f", (currentKills - startKills) / math.max(0.01, elapsed)))
        end
    end)

    local function comm(args)
        local char = lp.Character
        if char and char:FindFirstChild("Communicate") then
            pcall(function() char.Communicate:FireServer(unpack(args)) end)
        end
    end

    local function getChar(plr)
        local live = workspace:FindFirstChild("Live")
        return (live and live:FindFirstChild(plr.Name)) or plr.Character
    end

    local function isAlive(plr)
        local c = getChar(plr)
        local h = c and c:FindFirstChildOfClass("Humanoid")
        return h and h.Health > 0
    end

    local function getTarget()
        local low, best = math.huge, nil
        for _, p in pairs(Players:GetPlayers()) do
            if p ~= lp and isAlive(p) then
                local h = getChar(p).Humanoid.Health
                if h < low then low = h; best = p end
            end
        end
        return best
    end

    local function fireHotbar()
        pcall(function()
        local hotbar = lp.PlayerGui:FindFirstChild("Hotbar")
        local backpack = hotbar and hotbar:FindFirstChild("Backpack")
        local hb = backpack and backpack:FindFirstChild("Hotbar")
        if not hb then return end

        local events = {"MouseButton1Click", "MouseButton1Down", "MouseButton1Up", "Activated"}

        for i = 1, 4 do
            local slot = hb:FindFirstChild(tostring(i))
            local btn = slot and slot:FindFirstChild("Base")
            if btn and (btn:IsA("TextButton") or btn:IsA("ImageButton")) then
                for _, eventName in ipairs(events) do

                    if firesignal then
                        firesignal(btn[eventName])
                    elseif getconnections then

                    for _, connection in pairs(getconnections(btn[eventName])) do
                        if connection.Fire then connection:Fire() end
                    end
                end
            end
        end
    end
end)
end

local function stopFarm()
    if farmConnection then farmConnection:Disconnect(); farmConnection = nil end
    if attackThread then task.cancel(attackThread); attackThread = nil end
    currentTarget = nil
end

local function startFarm()
    stopFarm()
    farmConnection = RunService.Heartbeat:Connect(function()
    if not lowestHpFarmEnabled then return end
    if not currentTarget or not isAlive(currentTarget) then currentTarget = getTarget() end
    if currentTarget then
        local tHrp = getChar(currentTarget):FindFirstChild("HumanoidRootPart")
        local myHrp = lp.Character and lp.Character:FindFirstChild("HumanoidRootPart")
        if tHrp and myHrp then
            myHrp.CFrame = tHrp.CFrame * CFrame.new(farmOffsetX, farmOffsetY, farmOffsetZ)
        end
    end
end)
attackThread = task.spawn(function()
while lowestHpFarmEnabled do
    if currentTarget and isAlive(currentTarget) then
        comm(isMobile and {{Goal = "LeftClick", Mobile = true}} or {{Goal = "LeftClick"}})
        comm({{Dash = Enum.KeyCode.W, Key = Enum.KeyCode.Q, Goal = "KeyPress"}})
        if autoUltimateEnabled then comm({{MoveDirection = Vector3.zero, Key = Enum.KeyCode.G, Goal = "KeyPress"}}) end
        if autoEmoteEnabled then comm({{Goal = "Emote Spin"}}) end
        fireHotbar()
    end
    task.wait(0.1)
end
end)
end

TSBMain:NewPremiumToggle("Auto Kill Farm (Lowest HP)", "Automatically hunts for players with low health", function(v)
lowestHpFarmEnabled = v
if v then startFarm() else stopFarm() end
end)
TSBMain:NewToggle("Auto Ultimate", "Auto-activates ultimate mode", function(v) autoUltimateEnabled = v end)
TSBMain:NewToggle("Auto Claim Emote", "Auto-spins after kills", function(v) autoEmoteEnabled = v end)

TSBMain:NewSection("Farm Axis Adjustments")
TSBMain:NewPremiumSlider("Offset X", "Horizontal position", 15, -15, function(v) farmOffsetX = v end)
TSBMain:NewPremiumSlider("Offset Y", "Vertical position", 15, -15, function(v) farmOffsetY = v end)
TSBMain:NewPremiumSlider("Offset Z", "Forward/Backward position", 15, -15, function(v) farmOffsetZ = v end)

TSBInfo:NewLabel("Integrated by Azedo HUB Hub")
TSBInfo:NewLabel("Script made by nismovxa")
TSBInfo:NewButton("Copy Official Discord", "Join our community", function()
setclipboard("https://discord.gg/jX23ajWzxw")
notify("Credits", "Link copied to clipboard!")
end)
end
end
