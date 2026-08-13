--// Simple ESP + Team Check
--// Para testes autorizados no seu próprio jogo Roblox

local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer

--// Configurações
local ESP_ENABLED = false
local TEAM_CHECK = true

local ESP_COLOR_ENEMY = Color3.fromRGB(255, 70, 70)
local ESP_COLOR_FRIEND = Color3.fromRGB(70, 170, 255)

local highlights = {}

--// Cria/atualiza ESP
local function updatePlayer(player)
    if player == LocalPlayer then
        return
    end

    if not ESP_ENABLED then
        return
    end

    local character = player.Character
    if not character then
        return
    end

    local old = highlights[player]
    if old then
        old:Destroy()
        highlights[player] = nil
    end

    -- Team Check
    local sameTeam = false

    if TEAM_CHECK then
        sameTeam = player.Team == LocalPlayer.Team
    end

    local highlight = Instance.new("Highlight")
    highlight.Name = "TestESP"
    highlight.DepthMode = Enum.HighlightDepthMode.AlwaysOnTop
    highlight.FillTransparency = 0.65
    highlight.OutlineTransparency = 0

    if sameTeam then
        highlight.FillColor = ESP_COLOR_FRIEND
        highlight.OutlineColor = Color3.fromRGB(120, 200, 255)
    else
        highlight.FillColor = ESP_COLOR_ENEMY
        highlight.OutlineColor = Color3.fromRGB(255, 120, 120)
    end

    highlight.Parent = character
    highlights[player] = highlight
end

--// Remove ESP de um jogador
local function removePlayer(player)
    if highlights[player] then
        highlights[player]:Destroy()
        highlights[player] = nil
    end
end

--// Atualiza todos
local function updateAll()
    for _, player in ipairs(Players:GetPlayers()) do
        if player ~= LocalPlayer then
            if ESP_ENABLED then
                updatePlayer(player)
            else
                removePlayer(player)
            end
        end
    end
end

--// Jogador entrou
Players.PlayerAdded:Connect(function(player)
    player.CharacterAdded:Connect(function()
        task.wait(0.5)
        updatePlayer(player)
    end)
end)

--// Jogador saiu
Players.PlayerRemoving:Connect(function(player)
    removePlayer(player)
end)

--// Respawn
for _, player in ipairs(Players:GetPlayers()) do
    if player ~= LocalPlayer then
        player.CharacterAdded:Connect(function()
            task.wait(0.5)
            updatePlayer(player)
        end)
    end
end

--// Atualização de Team
for _, player in ipairs(Players:GetPlayers()) do
    player:GetPropertyChangedSignal("Team"):Connect(function()
        if ESP_ENABLED then
            updatePlayer(player)
        end
    end)
end

--------------------------------------------------
--// INTERFACE
--------------------------------------------------

local gui = Instance.new("ScreenGui")
gui.Name = "TestARABE_GUI"
gui.ResetOnSpawn = false
gui.Parent = LocalPlayer:WaitForChild("PlayerGui")

local main = Instance.new("Frame")
main.Size = UDim2.new(0, 220, 0, 150)
main.Position = UDim2.new(0, 20, 0.5, -75)
main.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
main.BorderSizePixel = 0
main.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 10)
corner.Parent = main

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundTransparency = 1
title.Text = "ESP • TEST MODE"
title.TextColor3 = Color3.fromRGB(255, 255, 255)
title.TextSize = 18
title.Font = Enum.Font.GothamBold
title.Parent = main

--// Botão ESP
local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(1, -20, 0, 40)
espButton.Position = UDim2.new(0, 10, 0, 48)
espButton.BackgroundColor3 = Color3.fromRGB(170, 50, 50)
espButton.Text = "ESP: OFF"
espButton.TextColor3 = Color3.fromRGB(255, 255, 255)
espButton.TextSize = 15
espButton.Font = Enum.Font.GothamBold
espButton.Parent = main

local buttonCorner = Instance.new("UICorner")
buttonCorner.CornerRadius = UDim.new(0, 7)
buttonCorner.Parent = espButton

--// Botão Team Check
local teamButton = Instance.new("TextButton")
teamButton.Size = UDim2.new(1, -20, 0, 40)
teamButton.Position = UDim2.new(0, 10, 0, 98)
teamButton.BackgroundColor3 = Color3.fromRGB(50, 110, 170)
teamButton.Text = "Team Check: ON"
teamButton.TextColor3 = Color3.fromRGB(255, 255, 255)
teamButton.TextSize = 15
teamButton.Font = Enum.Font.GothamBold
teamButton.Parent = main

local teamCorner = Instance.new("UICorner")
teamCorner.CornerRadius = UDim.new(0, 7)
teamCorner.Parent = teamButton

--------------------------------------------------
--// BOTÕES
--------------------------------------------------

espButton.MouseButton1Click:Connect(function()
    ESP_ENABLED = not ESP_ENABLED

    if ESP_ENABLED then
        espButton.Text = "ESP: ON"
        espButton.BackgroundColor3 = Color3.fromRGB(50, 170, 80)
    else
        espButton.Text = "ESP: OFF"
        espButton.BackgroundColor3 = Color3.fromRGB(170, 50, 50)
    end

    updateAll()
end)

teamButton.MouseButton1Click:Connect(function()
    TEAM_CHECK = not TEAM_CHECK

    if TEAM_CHECK then
        teamButton.Text = "Team Check: ON"
        teamButton.BackgroundColor3 = Color3.fromRGB(50, 110, 170)
    else
        teamButton.Text = "Team Check: OFF"
        teamButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    end

    updateAll()
end)

local UserInputService = game:GetService("UserInputService")

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then
        return
    end

    if input.KeyCode == Enum.KeyCode.Delete then
        ESP_ENABLED = not ESP_ENABLED

        if ESP_ENABLED then
            espButton.Text = "ESP: ON"
            espButton.BackgroundColor3 = Color3.fromRGB(50, 170, 80)
        else
            espButton.Text = "ESP: OFF"
            espButton.BackgroundColor3 = Color3.fromRGB(170, 50, 50)
        end

        updateAll()
    end
end)
