local rayfield = loadstring(game:HttpGet("https://sirius.menu/rayfield"))()

local players = game:GetService("Players")
local workspace = game:GetService("Workspace")
local replicatedStorage = game:GetService("ReplicatedStorage")
local runService = game:GetService("RunService")
local coreGui = game:GetService("CoreGui")
local virtualInput = game:GetService("VirtualInputManager")
local tweenService = game:GetService("TweenService")
local httpService = game:GetService("HttpService")

local localPlayer = players.LocalPlayer

local settings = {
    STAMINA_VALUE = 100,
    KILL_AURA_RANGE = 25,
    ESP_COLOR = Color3.fromRGB(255, 0, 0),
    ESP_ENABLED = true,
    AUTO_PARRY_ENABLED = true,
    NO_COOLDOWN_ENABLED = true,
    KILL_AURA_ENABLED = true,
    IMMUNITY_ENABLED = true,
    HITBOX_ENABLED = true,
    HITBOX_SIZE = 4,
    HEALTH_GUI_ENABLED = true
}

local healthGUIs = {}

local function getCharacter()
    return localPlayer.Character or workspace:FindFirstChild(localPlayer.Name)
end

local function getHumanoid()
    local char = getCharacter()
    return char and char:FindFirstChildOfClass("Humanoid")
end

local function getHumanoidRootPart()
    local char = getCharacter()
    return char and char:FindFirstChild("HumanoidRootPart")
end

local function getEquippedTool()
    local char = getCharacter()
    if not char then return nil end
    for _, child in ipairs(char:GetChildren()) do
        if child:IsA("Tool") then
            return child
        end
    end
    return nil
end

local function getCombatEvent()
    local events = replicatedStorage:FindFirstChild("Events")
    return events and events:FindFirstChild("CombatEvent")
end

local combatEvent = getCombatEvent()

local function attack()
    local tool = getEquippedTool()
    if not tool then return end
    
    pcall(function()
        tool:Activate()
    end)
    
    if combatEvent then
        combatEvent:FireServer("M1", tool.Name)
        combatEvent:FireServer("Attack", tool.Name)
        combatEvent:FireServer("Swing", tool.Name)
    else
        virtualInput:SendMouseButtonEvent(0, 0, 0, true, game, 1)
        virtualInput:SendMouseButtonEvent(0, 0, 0, false, game, 1)
    end
end

local function findNearestPlayer(range)
    range = range or settings.KILL_AURA_RANGE
    local rootPart = getHumanoidRootPart()
    if not rootPart then return nil end
    
    local nearest = nil
    local nearestDistance = range
    
    for _, player in ipairs(players:GetPlayers()) do
        if player ~= localPlayer then
            local character = player.Character
            if character then
                local targetRoot = character:FindFirstChild("HumanoidRootPart")
                if targetRoot then
                    local distance = (rootPart.Position - targetRoot.Position).Magnitude
                    if distance < nearestDistance then
                        nearest = player
                        nearestDistance = distance
                    end
                end
            end
        end
    end
    
    return nearest, nearestDistance
end

task.spawn(function()
    local originalHitboxData = {}
    
    while true do
        for _, player in ipairs(players:GetPlayers()) do
            if player ~= localPlayer and player.Character then
                local head = player.Character:FindFirstChild("Head")
                if head and head:IsA("BasePart") then
                    if not originalHitboxData[head] then
                        originalHitboxData[head] = {
                            Size = head.Size,
                            Transparency = head.Transparency,
                            CanCollide = head.CanCollide,
                            Massless = head.Massless
                        }
                    end
                    
                    if settings.HITBOX_ENABLED then
                        head.Size = Vector3.new(settings.HITBOX_SIZE, settings.HITBOX_SIZE, settings.HITBOX_SIZE)
                        head.Transparency = 0.6
                        head.CanCollide = false
                        head.Massless = true
                    else
                        local original = originalHitboxData[head]
                        if original then
                            head.Size = original.Size
                            head.Transparency = original.Transparency
                            head.CanCollide = original.CanCollide
                            head.Massless = original.Massless
                        end
                    end
                end
            end
        end
        task.wait(0.1)
    end
end)

local function destroyHealthGUI(player)
    if healthGUIs[player] then
        healthGUIs[player]:Destroy()
        healthGUIs[player] = nil
    end
end

local function createHealthGUI(player)
    if player == localPlayer or not settings.HEALTH_GUI_ENABLED then
        return
    end
    
    destroyHealthGUI(player)
    
    local character = player.Character
    if not character then return end
    
    local head = character:FindFirstChild("Head") or character:WaitForChild("Head", 3)
    local humanoid = character:FindFirstChildOfClass("Humanoid") or character:WaitForChild("Humanoid", 3)
    
    if not head or not humanoid then return end
    
    local gui = Instance.new("BillboardGui")
    gui.Name = "EchidnaHealthGUI_" .. player.Name
    gui.Size = UDim2.new(0, 90, 0, 18)
    gui.StudsOffset = Vector3.new(0, 2.5, 0)
    gui.AlwaysOnTop = true
    gui.Adornee = head
    gui.Parent = (gethui and gethui()) or coreGui
    
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 1, 0)
    frame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
    frame.BorderSizePixel = 0
    frame.Parent = gui
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 4)
    corner.Parent = frame
    
    local healthBar = Instance.new("Frame")
    healthBar.Size = UDim2.new(1, 0, 1, 0)
    healthBar.BackgroundColor3 = Color3.fromRGB(50, 220, 50)
    healthBar.BorderSizePixel = 0
    healthBar.Parent = frame
    
    local healthBarCorner = Instance.new("UICorner")
    healthBarCorner.CornerRadius = UDim.new(0, 4)
    healthBarCorner.Parent = healthBar
    
    local textLabel = Instance.new("TextLabel")
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.BackgroundTransparency = 1
    textLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
    textLabel.Font = Enum.Font.SourceSansBold
    textLabel.TextSize = 11
    textLabel.Parent = frame
    
    local function updateHealthBar(health)
        local maxHealth = math.max(humanoid.MaxHealth, 1)
        local percent = math.clamp(health / maxHealth, 0, 1)
        healthBar.Size = UDim2.new(percent, 0, 1, 0)
        textLabel.Text = math.floor(health) .. " / " .. math.floor(humanoid.MaxHealth)
        
        if percent > 0.5 then
            healthBar.BackgroundColor3 = Color3.fromRGB(50, 220, 50)
        elseif percent > 0.25 then
            healthBar.BackgroundColor3 = Color3.fromRGB(230, 180, 40)
        else
            healthBar.BackgroundColor3 = Color3.fromRGB(230, 40, 40)
        end
    end
    
    humanoid.HealthChanged:Connect(updateHealthBar)
    updateHealthBar(humanoid.Health)
    
    healthGUIs[player] = gui
end

local function setupPlayerHealthGUI(player)
    if player == localPlayer then return end
    
    player.CharacterAdded:Connect(function()
        task.wait(0.5)
        createHealthGUI(player)
    end)
    
    if player.Character then
        createHealthGUI(player)
    end
end

local function refreshAllHealthGUIs()
    for player in pairs(healthGUIs) do
        destroyHealthGUI(player)
    end
    
    if not settings.HEALTH_GUI_ENABLED then return end
    
    for _, player in ipairs(players:GetPlayers()) do
        if player ~= localPlayer then
            setupPlayerHealthGUI(player)
        end
    end
end

for _, player in ipairs(players:GetPlayers()) do
    setupPlayerHealthGUI(player)
end

players.PlayerAdded:Connect(setupPlayerHealthGUI)
players.PlayerRemoving:Connect(destroyHealthGUI)

runService.Stepped:Connect(function()
    if not settings.IMMUNITY_ENABLED then return end
    
    local humanoid = getHumanoid()
    if humanoid then
        humanoid:SetStateEnabled(Enum.HumanoidStateType.FallingDown, false)
        humanoid:SetStateEnabled(Enum.HumanoidStateType.Ragdoll, false)
        humanoid:SetStateEnabled(Enum.HumanoidStateType.PlatformStanding, false)
        
        if humanoid.PlatformStand then
            humanoid.PlatformStand = false
        end
        if humanoid.Sit then
            humanoid.Sit = false
        end
    end
end)

task.spawn(function()
    while true do
        if settings.KILL_AURA_ENABLED then
            local char = getCharacter()
            if char then
                local target, distance = findNearestPlayer(settings.KILL_AURA_RANGE)
                if target and distance <= settings.KILL_AURA_RANGE then
                    local targetChar = target.Character
                    if targetChar then
                        local targetRoot = targetChar:FindFirstChild("HumanoidRootPart")
                        local playerRoot = getHumanoidRootPart()
                        
                        if playerRoot and targetRoot then
                            playerRoot.CFrame = CFrame.new(
                                playerRoot.Position,
                                Vector3.new(targetRoot.Position.X, playerRoot.Position.Y, targetRoot.Position.Z)
                            )
                            attack()
                        end
                    end
                end
            end
            task.wait(0.01)
        else
            task.wait(0.1)
        end
    end
end)

local window = rayfield:CreateWindow({
    Name = "Azedo HUB v11.0",
    LoadingTitle = "Azedo HUB...",
    LoadingSubtitle = "Rayfield UI Interface",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "Azedo HUB",
        FileName = "Config"
    },
    KeySystem = false
})

local combatTab = window:CreateTab("Combat Features", 4483362458)
local visualsTab = window:CreateTab("Visuals & UI", 4483362458)
local playerTab = window:CreateTab("Player Tweaks", 4483362458)

combatTab:CreateToggle({
    Name = "Kill Aura",
    CurrentValue = settings.KILL_AURA_ENABLED,
    Flag = "KillAuraToggle",
    Callback = function(value)
        settings.KILL_AURA_ENABLED = value
    end
})

combatTab:CreateToggle({
    Name = "Auto Parry",
    CurrentValue = settings.AUTO_PARRY_ENABLED,
    Flag = "AutoParryToggle",
    Callback = function(value)
        settings.AUTO_PARRY_ENABLED = value
    end
})

combatTab:CreateToggle({
    Name = "No Cooldown",
    CurrentValue = settings.NO_COOLDOWN_ENABLED,
    Flag = "NoCooldownToggle",
    Callback = function(value)
        settings.NO_COOLDOWN_ENABLED = value
    end
})

combatTab:CreateToggle({
    Name = "Hitbox Head Expansion (4x)",
    CurrentValue = settings.HITBOX_ENABLED,
    Flag = "HitboxToggle",
    Callback = function(value)
        settings.HITBOX_ENABLED = value
    end
})

visualsTab:CreateToggle({
    Name = "Overhead Health Displays",
    CurrentValue = settings.HEALTH_GUI_ENABLED,
    Flag = "HealthGUIToggle",
    Callback = function(value)
        settings.HEALTH_GUI_ENABLED = value
        refreshAllHealthGUIs()
    end
})

visualsTab:CreateToggle({
    Name = "Player ESP",
    CurrentValue = settings.ESP_ENABLED,
    Flag = "ESPToggle",
    Callback = function(value)
        settings.ESP_ENABLED = value
    end
})

playerTab:CreateToggle({
    Name = "Anti-Stun & Anti-Ragdoll",
    CurrentValue = settings.IMMUNITY_ENABLED,
    Flag = "ImmunityToggle",
    Callback = function(value)
        settings.IMMUNITY_ENABLED = value
    end
})

rayfield:Notify({
    Title = "Echidna Klase Ready",
    Content = "All features and dynamic health monitoring active.",
    Duration = 5,
    Image = 4483362458
})
