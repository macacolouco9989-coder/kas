-- ============================================
-- 🎯 MENU PRETO/CINZA + AIMBOT + FOV NA CÂMERA
-- ============================================
-- 📌 Aperte DELETE para abrir/fechar o menu
-- ============================================

-- ============================================
-- SERVIÇOS
-- ============================================

local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local Workspace = game:GetService("Workspace")
local Camera = Workspace.CurrentCamera

local player = Players.LocalPlayer
local mouse = player:GetMouse()

-- ============================================
-- CONFIGURAÇÕES DO AIMBOT
-- ============================================

local aimbotConfig = {
    enabled = true,
    targetPart = "Head",
    fov = 120,
    smoothness = 1,
    drawFOV = true,
    checkVisible = true,
    checkAlive = true,
    teamCheck = true,
    distance = 500,
    silent = false,
    triggerbot = false,
    esp = false,
    magicBullet = false,
    pullStrength = 100,
    keybind = "MouseButton2",
}

-- ============================================
-- VARIÁVEIS
-- ============================================

local target = nil
local lockedTarget = nil
local fovCircle = nil
local espObjects = {}
local keybindHeld = false

-- ============================================
-- CRIA A GUI
-- ============================================

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "MenuPreto"
screenGui.Parent = player.PlayerGui
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling

-- ============================================
-- MENU PRINCIPAL
-- ============================================

local menu = Instance.new("Frame")
menu.Size = UDim2.new(0, 420, 0, 520)
menu.Position = UDim2.new(0.5, -210, 0.5, -260)
menu.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
menu.BackgroundTransparency = 0.05
menu.BorderSizePixel = 1
menu.BorderColor3 = Color3.fromRGB(60, 60, 60)
menu.Visible = false
menu.Parent = screenGui

local menuCorner = Instance.new("UICorner")
menuCorner.CornerRadius = UDim.new(0, 12)
menuCorner.Parent = menu

-- ============================================
-- TOPO DO MENU
-- ============================================

local topBar = Instance.new("Frame")
topBar.Size = UDim2.new(1, 0, 0, 45)
topBar.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
topBar.BorderSizePixel = 0
topBar.Parent = menu

local topCorner = Instance.new("UICorner")
topCorner.CornerRadius = UDim.new(0, 12)
topCorner.Parent = topBar

local title = Instance.new("TextLabel")
title.Size = UDim2.new(0.6, 0, 1, 0)
title.Position = UDim2.new(0.05, 0, 0, 0)
title.BackgroundTransparency = 1
title.Text = "🎯 AIMBOT PREMIUM"
title.TextColor3 = Color3.fromRGB(220, 220, 220)
title.TextSize = 18
title.TextXAlignment = Enum.TextXAlignment.Left
title.Font = Enum.Font.GothamBold
title.Parent = topBar

local version = Instance.new("TextLabel")
version.Size = UDim2.new(0.3, 0, 1, 0)
version.Position = UDim2.new(0.65, 0, 0, 0)
version.BackgroundTransparency = 1
version.Text = "v3.0"
version.TextColor3 = Color3.fromRGB(120, 120, 120)
version.TextSize = 12
version.TextXAlignment = Enum.TextXAlignment.Right
version.Font = Enum.Font.Gotham
version.Parent = topBar

-- ============================================
-- BOTÃO FECHAR
-- ============================================

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 30, 0, 30)
closeBtn.Position = UDim2.new(1, -40, 0, 8)
closeBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(200, 200, 200)
closeBtn.TextSize = 16
closeBtn.Font = Enum.Font.GothamBold
closeBtn.BorderSizePixel = 0
closeBtn.Parent = topBar

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(1, 0)
closeCorner.Parent = closeBtn

closeBtn.MouseButton1Click:Connect(function()
    menu.Visible = false
end)

closeBtn.MouseEnter:Connect(function()
    closeBtn.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
end)

closeBtn.MouseLeave:Connect(function()
    closeBtn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
end)

-- ============================================
-- SCROLL FRAME
-- ============================================

local scroll = Instance.new("ScrollingFrame")
scroll.Size = UDim2.new(1, 0, 1, -45)
scroll.Position = UDim2.new(0, 0, 0, 45)
scroll.BackgroundTransparency = 1
scroll.BorderSizePixel = 0
scroll.CanvasSize = UDim2.new(0, 0, 0, 900)
scroll.ScrollBarThickness = 4
scroll.ScrollBarImageColor3 = Color3.fromRGB(80, 80, 80)
scroll.ScrollBarImageTransparency = 0.5
scroll.Parent = menu

local container = Instance.new("Frame")
container.Size = UDim2.new(1, 0, 0, 850)
container.BackgroundTransparency = 1
container.Parent = scroll

-- ============================================
-- FUNÇÃO CHECKBOX
-- ============================================

local function criarCheckbox(parent, yPos, texto, estadoInicial)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 32)
    frame.Position = UDim2.new(0, 0, 0, yPos)
    frame.BackgroundTransparency = 1
    frame.Parent = parent
    
    local caixa = Instance.new("TextButton")
    caixa.Size = UDim2.new(0, 18, 0, 18)
    caixa.Position = UDim2.new(0.03, 0, 0.5, -9)
    caixa.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    caixa.Text = ""
    caixa.BorderSizePixel = 1
    caixa.BorderColor3 = Color3.fromRGB(80, 80, 80)
    caixa.Parent = frame
    
    local caixaCorner = Instance.new("UICorner")
    caixaCorner.CornerRadius = UDim.new(0, 3)
    caixaCorner.Parent = caixa
    
    local checkMark = Instance.new("TextLabel")
    checkMark.Size = UDim2.new(1, 0, 1, 0)
    checkMark.BackgroundTransparency = 1
    checkMark.Text = "✓"
    checkMark.TextColor3 = Color3.fromRGB(200, 200, 200)
    checkMark.TextSize = 14
    checkMark.Font = Enum.Font.GothamBold
    checkMark.Visible = estadoInicial or false
    checkMark.Parent = caixa
    
    local textoLabel = Instance.new("TextLabel")
    textoLabel.Size = UDim2.new(0.85, 0, 1, 0)
    textoLabel.Position = UDim2.new(0.09, 0, 0, 0)
    textoLabel.BackgroundTransparency = 1
    textoLabel.Text = texto
    textoLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    textoLabel.TextSize = 13
    textoLabel.TextXAlignment = Enum.TextXAlignment.Left
    textoLabel.Font = Enum.Font.Gotham
    textoLabel.Parent = frame
    
    local estado = estadoInicial or false
    
    caixa.MouseButton1Click:Connect(function()
        estado = not estado
        checkMark.Visible = estado
    end)
    
    caixa.MouseEnter:Connect(function()
        caixa.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    end)
    
    caixa.MouseLeave:Connect(function()
        caixa.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
    end)
    
    return frame, function() return estado end, function(novoEstado)
        estado = novoEstado
        checkMark.Visible = estado
    end
end

-- ============================================
-- FUNÇÃO DROPDOWN
-- ============================================

local function criarDropdown(parent, yPos, texto, opcoes, valorPadrao)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 32)
    frame.Position = UDim2.new(0, 0, 0, yPos)
    frame.BackgroundTransparency = 1
    frame.Parent = parent
    
    local textoLabel = Instance.new("TextLabel")
    textoLabel.Size = UDim2.new(0.35, 0, 1, 0)
    textoLabel.Position = UDim2.new(0.03, 0, 0, 0)
    textoLabel.BackgroundTransparency = 1
    textoLabel.Text = texto
    textoLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
    textoLabel.TextSize = 13
    textoLabel.TextXAlignment = Enum.TextXAlignment.Left
    textoLabel.Font = Enum.Font.Gotham
    textoLabel.Parent = frame
    
    local botao = Instance.new("TextButton")
    botao.Size = UDim2.new(0.5, 0, 1, 0)
    botao.Position = UDim2.new(0.45, 0, 0, 0)
    botao.BackgroundColor3 = Color3.fromRGB(35, 35, 35)
    botao.Text = valorPadrao or opcoes[1]
    botao.TextColor3 = Color3.fromRGB(200, 200, 200)
    botao.TextSize = 13
    botao.Font = Enum.Font.Gotham
    botao.BorderSizePixel = 1
    botao.BorderColor3 = Color3.fromRGB(60, 60, 60)
    botao.Parent = frame
    
    local botaoCorner = Instance.new("UICorner")
    botaoCorner.CornerRadius = UDim.new(0, 4)
    botaoCorner.Parent = botao
    
    local indice = 1
    
    botao.MouseButton1Click:Connect(function()
        indice = indice % #opcoes + 1
        botao.Text = opcoes[indice]
    end)
    
    return frame, function() return opcoes[indice] end
end

-- ============================================
-- FUNÇÃO SLIDER
-- ============================================

local function criarSlider(parent, yPos, texto, min, max, valorPadrao)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 35)
    frame.Position = UDim2.new(0, 0, 0, yPos)
    frame.BackgroundTransparency = 1
    frame.Parent = parent
    
    local textoLabel = Instance.new("TextLabel")
    textoLabel.Size = UDim2.new(0.4, 0, 1, 0)
    textoLabel.Position = UDim2.new(0.03, 0, 0, 0)
    textoLabel.BackgroundTransparency = 1
    textoLabel.Text = texto
    textoLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
    textoLabel.TextSize = 12
    textoLabel.TextXAlignment = Enum.TextXAlignment.Left
    textoLabel.Font = Enum.Font.Gotham
    textoLabel.Parent = frame
    
    local valorLabel = Instance.new("TextLabel")
    valorLabel.Size = UDim2.new(0.1, 0, 1, 0)
    valorLabel.Position = UDim2.new(0.87, 0, 0, 0)
    valorLabel.BackgroundTransparency = 1
    valorLabel.Text = tostring(valorPadrao)
    valorLabel.TextColor3 = Color3.fromRGB(200, 200, 200)
    valorLabel.TextSize = 12
    valorLabel.TextXAlignment = Enum.TextXAlignment.Right
    valorLabel.Font = Enum.Font.GothamBold
    valorLabel.Parent = frame
    
    local sliderBg = Instance.new("Frame")
    sliderBg.Size = UDim2.new(0.42, 0, 0, 4)
    sliderBg.Position = UDim2.new(0.43, 0, 0.5, -2)
    sliderBg.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    sliderBg.BorderSizePixel = 0
    sliderBg.Parent = frame
    
    local sliderCorner = Instance.new("UICorner")
    sliderCorner.CornerRadius = UDim.new(1, 0)
    sliderCorner.Parent = sliderBg
    
    local fill = Instance.new("Frame")
    fill.Size = UDim2.new((valorPadrao - min) / (max - min), 0, 1, 0)
    fill.BackgroundColor3 = Color3.fromRGB(150, 150, 150)
    fill.BorderSizePixel = 0
    fill.Parent = sliderBg
    
    local fillCorner = Instance.new("UICorner")
    fillCorner.CornerRadius = UDim.new(1, 0)
    fillCorner.Parent = fill
    
    local valor = valorPadrao or 0
    local arrastando = false
    
    local function atualizarSlider(posX)
        local relX = math.clamp((posX - sliderBg.AbsolutePosition.X) / sliderBg.AbsoluteSize.X, 0, 1)
        valor = math.round(min + (max - min) * relX)
        fill.Size = UDim2.new(relX, 0, 1, 0)
        valorLabel.Text = tostring(valor)
    end
    
    sliderBg.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            arrastando = true
            atualizarSlider(input.Position.X)
        end
    end)
    
    UserInputService.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement and arrastando then
            atualizarSlider(input.Position.X)
        end
    end)
    
    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            arrastando = false
        end
    end)
    
    return frame, function() return valor end, function(novoValor)
        valor = math.clamp(novoValor, min, max)
        local relX = (valor - min) / (max - min)
        fill.Size = UDim2.new(relX, 0, 1, 0)
        valorLabel.Text = tostring(valor)
    end
end

-- ============================================
-- FUNÇÃO TÍTULO DE SEÇÃO
-- ============================================

local function criarTitulo(parent, yPos, texto)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(1, 0, 0, 25)
    frame.Position = UDim2.new(0, 0, 0, yPos)
    frame.BackgroundTransparency = 1
    frame.Parent = parent
    
    local linha = Instance.new("Frame")
    linha.Size = UDim2.new(0.03, 0, 1, 0)
    linha.Position = UDim2.new(0.02, 0, 0, 0)
    linha.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    linha.BorderSizePixel = 0
    linha.Parent = frame
    
    local textoLabel = Instance.new("TextLabel")
    textoLabel.Size = UDim2.new(0.85, 0, 1, 0)
    textoLabel.Position = UDim2.new(0.08, 0, 0, 0)
    textoLabel.BackgroundTransparency = 1
    textoLabel.Text = texto
    textoLabel.TextColor3 = Color3.fromRGB(160, 160, 160)
    textoLabel.TextSize = 13
    textoLabel.TextXAlignment = Enum.TextXAlignment.Left
    textoLabel.Font = Enum.Font.GothamBold
    textoLabel.Parent = frame
    
    return frame
end

-- ============================================
-- FUNÇÃO SEPARADOR
-- ============================================

local function criarSeparador(parent, yPos)
    local frame = Instance.new("Frame")
    frame.Size = UDim2.new(0.94, 0, 0, 1)
    frame.Position = UDim2.new(0.03, 0, 0, yPos)
    frame.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    frame.BorderSizePixel = 0
    frame.Parent = parent
    return frame
end

-- ============================================
-- CONSTRUINDO O MENU
-- ============================================

local y = 10

-- AIMBOT
criarTitulo(container, y, "AIMBOT")
y = y + 30

local cb1, getAimbot = criarCheckbox(container, y, "Aimbot Ativo", true)
y = y + 35

local dd1, getKeybind = criarDropdown(container, y, "Botão de Ativação:", {"MouseButton1", "MouseButton2", "MouseButton3", "F", "R", "Shift", "Control", "Q", "E", "X", "Z", "C", "V"})
y = y + 38

local cb2, getHead = criarCheckbox(container, y, "Mira na Cabeça", true)
y = y + 35

local cb3, getVisible = criarCheckbox(container, y, "Checar Visibilidade", true)
y = y + 35

local cb4, getTeam = criarCheckbox(container, y, "Não Mirar em Aliados", true)
y = y + 35

local cb5, getDrawFOV = criarCheckbox(container, y, "Desenhar FOV", true)
y = y + 35

criarSeparador(container, y)
y = y + 15

-- AJUSTES
criarTitulo(container, y, "AJUSTES")
y = y + 30

local sl1, getFOV = criarSlider(container, y, "FOV:", 10, 360, 120)
y = y + 40

local sl2, getSmooth = criarSlider(container, y, "Suavidade:", 1, 20, 5)
y = y + 40

local sl3, getDist = criarSlider(container, y, "Distância Máxima:", 50, 1000, 500)
y = y + 40

criarSeparador(container, y)
y = y + 15

-- SILENT AIM
criarTitulo(container, y, "SILENT AIM")
y = y + 30

local cb6, getSilent = criarCheckbox(container, y, "Silent Aim", false)
y = y + 35

criarSeparador(container, y)
y = y + 15

-- MAGIC BULLET
criarTitulo(container, y, "MAGIC BULLET")
y = y + 30

local cb7, getMagicBullet = criarCheckbox(container, y, "Magic Bullet", false)
y = y + 35

local sl4, getPullStrength = criarSlider(container, y, "Força do Puxão:", 1, 100, 100)
y = y + 40

criarSeparador(container, y)
y = y + 15

-- TRIGGERBOT
criarTitulo(container, y, "TRIGGERBOT")
y = y + 30

local cb8, getTrigger = criarCheckbox(container, y, "Triggerbot", false)
y = y + 35

criarSeparador(container, y)
y = y + 15

-- ESP
criarTitulo(container, y, "ESP")
y = y + 30

local cb9, getESP = criarCheckbox(container, y, "ESP Ativo", false)
y = y + 35

local cb10, getESPBox = criarCheckbox(container, y, "ESP Box", true)
y = y + 35

local cb11, getESPName = criarCheckbox(container, y, "ESP Nomes", true)
y = y + 35

scroll.CanvasSize = UDim2.new(0, 0, 0, y + 50)

-- ============================================
-- FUNÇÃO FOV CIRCLE (AGORA NA CÂMERA)
-- ============================================

local function criarFOVCircle()
    if fovCircle then fovCircle:Destroy() end
    
    local viewportSize = Camera.ViewportSize
    local centerX = viewportSize.X / 2
    local centerY = viewportSize.Y / 2
    
    local circle = Drawing.new("Circle")
    circle.Visible = aimbotConfig.drawFOV
    circle.Radius = aimbotConfig.fov
    circle.Thickness = 1.5
    circle.Color = Color3.fromRGB(150, 150, 150)
    circle.Transparency = 0.4
    circle.Filled = false
    circle.NumSides = 30
    circle.Position = Vector2.new(centerX, centerY) -- CENTRALIZADO NA CÂMERA
    
    fovCircle = circle
    return circle
end

criarFOVCircle()

-- ============================================
-- ATUALIZA FOV QUANDO A TELA MUDA
-- ============================================

Camera:GetPropertyChangedSignal("ViewportSize"):Connect(function()
    if fovCircle then
        local viewportSize = Camera.ViewportSize
        fovCircle.Position = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
    end
end)

-- ============================================
-- FUNÇÕES DO AIMBOT
-- ============================================

local function getEnemies()
    local enemies = {}
    
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("Humanoid") then
            local char = plr.Character
            local humanoid = char:FindFirstChild("Humanoid")
            
            if humanoid.Health <= 0 then
                continue
            end
            
            if aimbotConfig.teamCheck and plr.Team == player.Team then
                continue
            end
            
            local targetPart = char:FindFirstChild("Head") or char:FindFirstChild("HumanoidRootPart")
            if not targetPart then
                continue
            end
            
            local distance = (targetPart.Position - Camera.CFrame.Position).Magnitude
            if distance > aimbotConfig.distance then
                continue
            end
            
            if aimbotConfig.checkVisible then
                local ray = Ray.new(Camera.CFrame.Position, (targetPart.Position - Camera.CFrame.Position).Unit * distance)
                local hit, position = Workspace:FindPartOnRay(ray, player.Character, false, true)
                if hit and not hit:IsDescendantOf(char) then
                    continue
                end
            end
            
            table.insert(enemies, {
                player = plr,
                character = char,
                humanoid = humanoid,
                part = targetPart,
                distance = distance,
                position = targetPart.Position,
            })
        end
    end
    
    return enemies
end

local function getAngleToTarget(targetPosition)
    local cameraPos = Camera.CFrame.Position
    local direction = (targetPosition - cameraPos).Unit
    local lookDirection = Camera.CFrame.LookVector
    
    local angle = math.acos(lookDirection:Dot(direction))
    return math.deg(angle)
end

local function aimAt(targetPosition)
    if not targetPosition then return end
    
    local cameraPos = Camera.CFrame.Position
    local direction = (targetPosition - cameraPos).Unit
    
    if aimbotConfig.magicBullet then
        local pull = aimbotConfig.pullStrength / 100
        local randomOffset = Vector3.new(
            (math.random() - 0.5) * (1 - pull) * 0.1,
            (math.random() - 0.5) * (1 - pull) * 0.1,
            (math.random() - 0.5) * (1 - pull) * 0.1
        )
        direction = (direction + randomOffset).Unit
    end
    
    if aimbotConfig.smoothness > 1 then
        local targetCFrame = CFrame.new(cameraPos, cameraPos + direction * 100)
        local lerpFactor = 1 / aimbotConfig.smoothness
        Camera.CFrame = Camera.CFrame:Lerp(targetCFrame, lerpFactor)
    else
        Camera.CFrame = CFrame.new(cameraPos, cameraPos + direction * 100)
    end
end

local function findBestTarget()
    local enemies = getEnemies()
    if #enemies == 0 then return nil end
    
    local bestTarget = nil
    local bestScore = math.huge
    
    for _, enemy in ipairs(enemies) do
        local angle = getAngleToTarget(enemy.position)
        
        if angle <= aimbotConfig.fov then
            local score = enemy.distance + angle * 10
            
            if score < bestScore then
                bestScore = score
                bestTarget = enemy
            end
        end
    end
    
    return bestTarget
end

-- ============================================
-- ESP
-- ============================================

local function updateESP()
    for _, espObj in pairs(espObjects) do
        pcall(function() espObj:Destroy() end)
    end
    espObjects = {}
    
    if not aimbotConfig.esp then return end
    
    for _, plr in ipairs(Players:GetPlayers()) do
        if plr ~= player and plr.Character and plr.Character:FindFirstChild("Humanoid") then
            local char = plr.Character
            local humanoid = char:FindFirstChild("Humanoid")
            
            if humanoid.Health <= 0 then continue end
            if aimbotConfig.teamCheck and plr.Team == player.Team then continue end
            
            local root = char:FindFirstChild("HumanoidRootPart")
            local head = char:FindFirstChild("Head")
            if not root then continue end
            
            local pos, onScreen = Camera:WorldToScreenPoint(root.Position)
            if not onScreen then continue end
            
            local headPos, headOnScreen = Camera:WorldToScreenPoint(head and head.Position or root.Position)
            
            local espBox = getESPBox()
            local espName = getESPName()
            
            if espBox then
                local dist = (root.Position - Camera.CFrame.Position).Magnitude
                local size = math.clamp(250 / dist * 10, 20, 100)
                
                local box = Drawing.new("Square")
                box.Size = Vector2.new(size, size * 1.3)
                box.Position = Vector2.new(pos.X - size/2, headPos.Y - size * 0.7)
                box.Color = Color3.fromRGB(150, 150, 150)
                box.Thickness = 1.5
                box.Visible = true
                box.Filled = false
                table.insert(espObjects, box)
            end
            
            if espName then
                local name = Drawing.new("Text")
                name.Text = plr.Name
                name.Position = Vector2.new(pos.X, headPos.Y - 60)
                name.Color = Color3.fromRGB(200, 200, 200)
                name.Size = 14
                name.Center = true
                name.Visible = true
                table.insert(espObjects, name)
            end
        end
    end
end

-- ============================================
-- TRIGGERBOT
-- ============================================

local function triggerbot()
    if not aimbotConfig.triggerbot then return end
    if not target then return end
    
    local angle = getAngleToTarget(target.position)
    if angle <= aimbotConfig.fov then
        pcall(function()
            mouse1click()
        end)
    end
end

-- ============================================
-- LOOP PRINCIPAL
-- ============================================

local function updateAimbot()
    aimbotConfig.enabled = getAimbot()
    aimbotConfig.targetPart = getHead() and "Head" or "HumanoidRootPart"
    aimbotConfig.drawFOV = getDrawFOV()
    aimbotConfig.fov = getFOV()
    aimbotConfig.smoothness = getSmooth()
    aimbotConfig.distance = getDist()
    aimbotConfig.teamCheck = getTeam()
    aimbotConfig.checkVisible = getVisible()
    aimbotConfig.silent = getSilent()
    aimbotConfig.triggerbot = getTrigger()
    aimbotConfig.esp = getESP()
    aimbotConfig.magicBullet = getMagicBullet()
    aimbotConfig.pullStrength = getPullStrength()
    
    local keybindStr = getKeybind()
    aimbotConfig.keybind = keybindStr
    
    -- FOV CIRCLE - SEMPRE CENTRALIZADO NA CÂMERA
    if fovCircle then
        local viewportSize = Camera.ViewportSize
        fovCircle.Position = Vector2.new(viewportSize.X / 2, viewportSize.Y / 2)
        fovCircle.Radius = aimbotConfig.fov
        fovCircle.Visible = aimbotConfig.drawFOV and aimbotConfig.enabled
    end
    
    if aimbotConfig.enabled and keybindHeld then
        local newTarget = findBestTarget()
        
        if newTarget then
            if lockedTarget and lockedTarget.player == newTarget.player then
                target = newTarget
            else
                target = newTarget
                lockedTarget = target
            end
        else
            target = nil
            lockedTarget = nil
        end
        
        if target then
            local targetPos = target.part.Position
            if aimbotConfig.targetPart == "Head" then
                targetPos = targetPos + Vector3.new(0, 0.5, 0)
            end
            
            if not aimbotConfig.silent then
                aimAt(targetPos)
            end
            
            triggerbot()
        end
    else
        target = nil
        lockedTarget = nil
    end
    
    updateESP()
end

-- ============================================
-- DETECTA O BOTÃO DE ATIVAÇÃO
-- ============================================

local function getKeyEnum(keyStr)
    local keyMap = {
        MouseButton1 = Enum.UserInputType.MouseButton1,
        MouseButton2 = Enum.UserInputType.MouseButton2,
        MouseButton3 = Enum.UserInputType.MouseButton3,
        F = Enum.KeyCode.F,
        R = Enum.KeyCode.R,
        Shift = Enum.KeyCode.LeftShift,
        Control = Enum.KeyCode.LeftControl,
        Q = Enum.KeyCode.Q,
        E = Enum.KeyCode.E,
        X = Enum.KeyCode.X,
        Z = Enum.KeyCode.Z,
        C = Enum.KeyCode.C,
        V = Enum.KeyCode.V,
    }
    return keyMap[keyStr] or Enum.UserInputType.MouseButton2
end

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    local currentKey = getKeybind()
    local keyEnum = getKeyEnum(currentKey)
    
    if input.UserInputType == keyEnum or input.KeyCode == keyEnum then
        keybindHeld = true
    end
end)

UserInputService.InputEnded:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    
    local currentKey = getKeybind()
    local keyEnum = getKeyEnum(currentKey)
    
    if input.UserInputType == keyEnum or input.KeyCode == keyEnum then
        keybindHeld = false
    end
end)

-- ============================================
-- ABRIR/FECHAR COM DELETE
-- ============================================

UserInputService.InputBegan:Connect(function(input, gameProcessed)
    if gameProcessed then return end
    if input.KeyCode == Enum.KeyCode.Delete then
        menu.Visible = not menu.Visible
        if menu.Visible then
            scroll.CanvasPosition = Vector2.new(0, 0)
        end
    end
end)

-- ============================================
-- ARRASTAR MENU
-- ============================================

local arrastando = false
local inicioArraste, posicaoInicial

topBar.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        arrastando = true
        inicioArraste = input.Position
        posicaoInicial = menu.Position
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement and arrastando then
        local delta = input.Position - inicioArraste
        menu.Position = UDim2.new(
            posicaoInicial.X.Scale,
            posicaoInicial.X.Offset + delta.X,
            posicaoInicial.Y.Scale,
            posicaoInicial.Y.Offset + delta.Y
        )
    end
end)

UserInputService.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        arrastando = false
    end
end)

-- ============================================
-- LOOP PRINCIPAL
-- ============================================

RunService.RenderStepped:Connect(function()
    updateAimbot()
end)

-- ============================================
-- MENSAGEM FINAL
-- ============================================

print("✅ MENU + AIMBOT GRUDENTO CARREGADO!")
print("📌 Aperte DELETE para abrir/fechar")
print("🎯 FOV centralizado na câmera!")
print("🎯 Segure o botão configurado para ativar o aimbot")
print("⚙️ Configure tudo no menu!")
