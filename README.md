-- Serviços
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local Lighting = game:GetService("Lighting")
local LocalPlayer = Players.LocalPlayer

-- Variáveis
local espAtivo = true
local modoLeveAtivo = false
local ESPFolder = Instance.new("Folder", workspace)
ESPFolder.Name = "ESPFolder"

-- ScreenGui principal
local gui = LocalPlayer:WaitForChild("PlayerGui")
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "PainelCompleto"
ScreenGui.Parent = gui

-- Função para tornar qualquer UI arrastável
local function makeDraggable(frame, handle)
    local dragging = false
    local dragInput, dragStart, startPos

    handle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = frame.Position

            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    handle.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            frame.Position = UDim2.new(
                startPos.X.Scale, startPos.X.Offset + delta.X,
                startPos.Y.Scale, startPos.Y.Offset + delta.Y
            )
        end
    end)
end

-- BOTÃO ZERB
local ZerbButton = Instance.new("TextButton")
ZerbButton.Name = "ZERB"
ZerbButton.Size = UDim2.new(0, 60, 0, 60)
ZerbButton.Position = UDim2.new(0.05, 0, 0.5, -30)
ZerbButton.BackgroundColor3 = Color3.fromRGB(0, 120, 255)
ZerbButton.Text = "ZERB"
ZerbButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ZerbButton.Font = Enum.Font.SourceSansBold
ZerbButton.TextSize = 14
ZerbButton.BorderSizePixel = 0
ZerbButton.Parent = ScreenGui

local UICorner = Instance.new("UICorner")
UICorner.CornerRadius = UDim.new(1, 0)
UICorner.Parent = ZerbButton

-- PAINEL PRINCIPAL
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 400, 0, 250)
MainFrame.Position = UDim2.new(0.3, 0, 0.3, 0)
MainFrame.BackgroundColor3 = Color3.fromRGB(10, 30, 60)
MainFrame.BorderSizePixel = 0
MainFrame.Visible = false
MainFrame.Parent = ScreenGui

local Title = Instance.new("TextLabel")
Title.Size = UDim2.new(1, 0, 0, 30)
Title.BackgroundColor3 = Color3.fromRGB(20, 50, 90)
Title.Text = "Painel Script"
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.Font = Enum.Font.SourceSansBold
Title.TextSize = 18
Title.Parent = MainFrame

-- Container de abas
local Tabs = Instance.new("Frame")
Tabs.Size = UDim2.new(0, 100, 1, -30)
Tabs.Position = UDim2.new(0, 0, 0, 30)
Tabs.BackgroundColor3 = Color3.fromRGB(15, 40, 80)
Tabs.BorderSizePixel = 0
Tabs.Parent = MainFrame

local Content = Instance.new("Frame")
Content.Size = UDim2.new(1, -100, 1, -30)
Content.Position = UDim2.new(0, 100, 0, 30)
Content.BackgroundColor3 = Color3.fromRGB(20, 55, 100)
Content.BorderSizePixel = 0
Content.Parent = MainFrame

-- Função criar aba
local function CriarAba(nome, ordem)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(1, 0, 0, 40)
    Button.Position = UDim2.new(0, 0, 0, (ordem - 1) * 40)
    Button.BackgroundColor3 = Color3.fromRGB(30, 70, 120)
    Button.Text = nome
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.SourceSansBold
    Button.TextSize = 16
    Button.Parent = Tabs
    return Button
end

-- Abas e Frames
local ESPButton = CriarAba("ESP", 1)
local TPButton = CriarAba("TP", 2)
local ModoLeveButton = CriarAba("Modo Leve", 3)

local ESPFrame = Instance.new("Frame")
ESPFrame.Size = UDim2.new(1, 0, 1, 0)
ESPFrame.BackgroundTransparency = 1
ESPFrame.Visible = true
ESPFrame.Parent = Content

local TPFrame = Instance.new("Frame")
TPFrame.Size = UDim2.new(1, 0, 1, 0)
TPFrame.BackgroundTransparency = 1
TPFrame.Visible = false
TPFrame.Parent = Content

local ModoLeveFrame = Instance.new("Frame")
ModoLeveFrame.Size = UDim2.new(1, 0, 1, 0)
ModoLeveFrame.BackgroundTransparency = 1
ModoLeveFrame.Visible = false
ModoLeveFrame.Parent = Content

-- Alternar abas
ESPButton.MouseButton1Click:Connect(function()
    ESPFrame.Visible = true
    TPFrame.Visible = false
    ModoLeveFrame.Visible = false
end)
TPButton.MouseButton1Click:Connect(function()
    ESPFrame.Visible = false
    TPFrame.Visible = true
    ModoLeveFrame.Visible = false
end)
ModoLeveButton.MouseButton1Click:Connect(function()
    ESPFrame.Visible = false
    TPFrame.Visible = false
    ModoLeveFrame.Visible = true
end)

-- Mostrar/ocultar painel
local aberto = false
ZerbButton.MouseButton1Click:Connect(function()
    aberto = not aberto
    MainFrame.Visible = aberto
end)

makeDraggable(ZerbButton, ZerbButton)
makeDraggable(MainFrame, Title)

--===================== MODO LEVE =====================
local ModoLeveButtonInside = Instance.new("TextButton")
ModoLeveButtonInside.Size = UDim2.new(0, 200, 0, 50)
ModoLeveButtonInside.Position = UDim2.new(0, 10, 0, 10)
ModoLeveButtonInside.BackgroundColor3 = Color3.fromRGB(0, 85, 255)
ModoLeveButtonInside.TextColor3 = Color3.fromRGB(255, 255, 255)
ModoLeveButtonInside.Text = "Ativar Modo Leve"
ModoLeveButtonInside.Font = Enum.Font.SourceSansBold
ModoLeveButtonInside.TextScaled = true
ModoLeveButtonInside.Parent = ModoLeveFrame

local function ativarModoLeve()
    Lighting.GlobalShadows = false
    Lighting.OutdoorAmbient = Color3.fromRGB(128,128,128)
    
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Explosion") then
            obj.Enabled = false
        end
        if obj:IsA("Texture") or obj:IsA("Decal") then
            obj:Destroy()
        end
        if obj:IsA("Smoke") or obj:IsA("Fire") then
            obj.Enabled = false
        end
        if obj:IsA("BasePart") then
            obj.Material = Enum.Material.Plastic
            obj.Reflectance = 0
            obj.CastShadow = false
        end
    end
end

local function desativarModoLeve()
    Lighting.GlobalShadows = true
    Lighting.OutdoorAmbient = Color3.fromRGB(255,255,255)
    
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("ParticleEmitter") or obj:IsA("Trail") or obj:IsA("Explosion") then
            obj.Enabled = true
        end
        if obj:IsA("Smoke") or obj:IsA("Fire") then
            obj.Enabled = true
        end
    end
end

ModoLeveButtonInside.MouseButton1Click:Connect(function()
    modoLeveAtivo = not modoLeveAtivo
    if modoLeveAtivo then
        ativarModoLeve()
        ModoLeveButtonInside.Text = "Desativar Modo Leve"
    else
        desativarModoLeve()
        ModoLeveButtonInside.Text = "Ativar Modo Leve"
    end
end)

--===================== TELEPORTE =====================
local function CreateTPGui()
    local PlayerListFrame = Instance.new("ScrollingFrame")
    PlayerListFrame.Size = UDim2.new(1, 0, 1, 0)
    PlayerListFrame.Position = UDim2.new(0, 0, 0, 0)
    PlayerListFrame.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
    PlayerListFrame.BorderSizePixel = 2
    PlayerListFrame.Parent = TPFrame
    PlayerListFrame.ScrollBarThickness = 6
    PlayerListFrame.AutomaticCanvasSize = Enum.AutomaticSize.Y
    PlayerListFrame.CanvasSize = UDim2.new(0,0,0,0)
    PlayerListFrame.ClipsDescendants = true
    PlayerListFrame.Active = true

    local UIListLayout = Instance.new("UIListLayout")
    UIListLayout.Parent = PlayerListFrame
    UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
    UIListLayout.Padding = UDim.new(0, 2)

    local function TeleportToPlayer(targetPlayer)
        if targetPlayer.Character and targetPlayer.Character:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character:MoveTo(targetPlayer.Character.HumanoidRootPart.Position + Vector3.new(0,3,0))
        end
    end

    local function UpdatePlayerList()
        for _, child in pairs(PlayerListFrame:GetChildren()) do
            if child:IsA("TextButton") then
                child:Destroy()
            end
        end

        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= LocalPlayer then
                local PlayerButton = Instance.new("TextButton")
                PlayerButton.Size = UDim2.new(1, 0, 0, 25)
                PlayerButton.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
                PlayerButton.TextColor3 = Color3.fromRGB(255, 255, 255)
                PlayerButton.Text = "→ " .. plr.Name
                PlayerButton.Parent = PlayerListFrame

                PlayerButton.MouseButton1Click:Connect(function()
                    TeleportToPlayer(plr)
                end)
            end
        end
    end

    Players.PlayerAdded:Connect(UpdatePlayerList)
    Players.PlayerRemoving:Connect(UpdatePlayerList)
    UpdatePlayerList()
end

CreateTPGui()
LocalPlayer.CharacterAdded:Connect(CreateTPGui)

--===================== ESP =====================
local ESPGui = Instance.new("ScreenGui")
ESPGui.Name = "ESPGui"
ESPGui.ResetOnSpawn = false
ESPGui.Parent = gui

local botaoESP = Instance.new("TextButton")
botaoESP.Size = UDim2.new(0, 150, 0, 50)
botaoESP.Position = UDim2.new(0, 20, 0, 20)
botaoESP.BackgroundColor3 = Color3.new(0.2,0.2,0.2)
botaoESP.TextColor3 = Color3.new(1,1,1)
botaoESP.Text = "ESP: Ligado"
botaoESP.Parent = ESPFrame

botaoESP.MouseButton1Click:Connect(function()
    espAtivo = not espAtivo
    botaoESP.Text = espAtivo and "ESP: Ligado" or "ESP: Desligado"
    for _, obj in pairs(ESPFolder:GetChildren()) do
        obj.Enabled.Value = espAtivo
    end
end)

-- Função ESP
local function setupESP(character, player)
    local root = character:WaitForChild("HumanoidRootPart")
    local espContainer = Instance.new("Folder", ESPFolder)
    espContainer.Name = player.Name

    local box = Instance.new("BoxHandleAdornment")
    box.Adornee = root
    box.Size = Vector3.new(2,5,1)
    box.Color3 = Color3.new(1,0,0)
    box.Transparency = 0.5
    box.AlwaysOnTop = true
    box.ZIndex = 2
    box.Parent = espContainer

    local billboard = Instance.new("BillboardGui")
    billboard.Size = UDim2.new(0,100,0,50)
    billboard.StudsOffset = Vector3.new(0,3,0)
    billboard.AlwaysOnTop = true
    billboard.Adornee = root
    billboard.Parent = espContainer

    local label = Instance.new("TextLabel")
    label.Size = UDim2.new(1,0,1,0)
    label.BackgroundTransparency = 1
    label.TextColor3 = Color3.new(1,0,0)
    label.TextScaled = true
    label.Font = Enum.Font.ArialBold
    label.Text = player.Name
    label.Parent = billboard

    local enabledValue = Instance.new("BoolValue", espContainer)
    enabledValue.Name = "Enabled"
    enabledValue.Value = espAtivo

    local conn
    conn = RunService.RenderStepped:Connect(function()
        if not root.Parent then
            conn:Disconnect()
            espContainer:Destroy()
            return
        end

        local ativo = enabledValue.Value
        box.Visible = ativo
        billboard.Enabled = ativo

        if ativo and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (LocalPlayer.Character.HumanoidRootPart.Position - root.Position).Magnitude
            label.Text = player.Name .. "\n" .. math.floor(distance) .. "m"
        end
    end)
end

local function criarESP(player)
    if player == LocalPlayer then return end

    if player.Character then
        setupESP(player.Character, player)
    end
    player.CharacterAdded:Connect(function(char)
        setupESP(char, player)
    end)

    player.CharacterRemoving:Connect(function()
        local oldESP = ESPFolder:FindFirstChild(player.Name)
        if oldESP then oldESP:Destroy() end
    end)
end

for _, player in pairs(Players:GetPlayers()) do
    criarESP(player)
end
Players.PlayerAdded:Connect(criarESP)
Players.PlayerRemoving:Connect(function(player)
    local oldESP = ESPFolder:FindFirstChild(player.Name)
    if oldESP then oldESP:Destroy() end
end)
