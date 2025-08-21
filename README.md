-- BLUE Hub Mini Premium v1.3
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local RunService = game:GetService("RunService")
local GuiService = game:GetService("CoreGui")
local TweenService = game:GetService("TweenService")

-- GUI Principal
local Gui = Instance.new("ScreenGui")
Gui.Name = "BLUEHubMiniPremium"
Gui.Parent = GuiService

-- Botão flutuante
local FloatButton = Instance.new("TextButton")
FloatButton.Size = UDim2.new(0, 60, 0, 60)
FloatButton.Position = UDim2.new(0, 20, 0.5, -30)
FloatButton.Text = "BLUE"
FloatButton.BackgroundColor3 = Color3.fromRGB(0, 170, 255)
FloatButton.TextColor3 = Color3.fromRGB(255, 255, 255)
FloatButton.Font = Enum.Font.GothamBold
FloatButton.TextScaled = true
FloatButton.AutoButtonColor = false
FloatButton.ZIndex = 2
FloatButton.Parent = Gui

-- Sombra do botão flutuante
local Shadow = Instance.new("UIStroke")
Shadow.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
Shadow.Color = Color3.fromRGB(0, 120, 255)
Shadow.Thickness = 2
Shadow.Parent = FloatButton

-- Frame do menu
local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 260, 0, 320)
MainFrame.Position = UDim2.new(0.5, -130, 0.5, -160)
MainFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 35)
MainFrame.Visible = false
MainFrame.Active = true
MainFrame.Draggable = true
MainFrame.Parent = Gui
MainFrame.ClipsDescendants = true
MainFrame.BorderSizePixel = 0
MainFrame.AutomaticSize = Enum.AutomaticSize.None

-- Sombra do frame principal
local FrameStroke = Instance.new("UIStroke")
FrameStroke.ApplyStrokeMode = Enum.ApplyStrokeMode.Border
FrameStroke.Color = Color3.fromRGB(0, 170, 255)
FrameStroke.Thickness = 2
FrameStroke.Parent = MainFrame

-- Barrinha superior estilo Windows
local TitleBar = Instance.new("Frame")
TitleBar.Size = UDim2.new(1, 0, 0, 30)
TitleBar.BackgroundColor3 = Color3.fromRGB(20, 20, 25)
TitleBar.BorderSizePixel = 0
TitleBar.Parent = MainFrame

local TitleLabel = Instance.new("TextLabel")
TitleLabel.Text = "BLUE Hub"
TitleLabel.Size = UDim2.new(0.8, 0, 1, 0)
TitleLabel.BackgroundTransparency = 1
TitleLabel.TextColor3 = Color3.fromRGB(0, 170, 255)
TitleLabel.Font = Enum.Font.GothamBold
TitleLabel.TextScaled = true
TitleLabel.Parent = TitleBar

local CloseButton = Instance.new("TextButton")
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0.2, 0, 1, 0)
CloseButton.Position = UDim2.new(0.8, 0, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(170, 0, 0)
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextScaled = true
CloseButton.AutoButtonColor = false
CloseButton.Parent = TitleBar
CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)

-- Função para criar botões premium com hover
local function CreateButton(name, callback, index)
    local Button = Instance.new("TextButton")
    Button.Size = UDim2.new(0.9, 0, 0, 35)
    Button.Position = UDim2.new(0.05, 0, 0, 40 + (index-1)*40)
    Button.Text = name
    Button.BackgroundColor3 = Color3.fromRGB(40, 40, 45)
    Button.TextColor3 = Color3.fromRGB(255, 255, 255)
    Button.Font = Enum.Font.GothamBold
    Button.TextScaled = true
    Button.AutoButtonColor = false
    Button.Parent = MainFrame

    -- Gradiente sutil
    local Gradient = Instance.new("UIGradient")
    Gradient.Color = ColorSequence.new(Color3.fromRGB(0, 170, 255), Color3.fromRGB(0, 120, 255))
    Gradient.Rotation = 90
    Gradient.Parent = Button

    -- Hover animation
    Button.MouseEnter:Connect(function()
        TweenService:Create(Button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(0, 170, 255)}):Play()
    end)
    Button.MouseLeave:Connect(function()
        TweenService:Create(Button, TweenInfo.new(0.2), {BackgroundColor3 = Color3.fromRGB(40, 40, 45)}):Play()
    end)

    Button.MouseButton1Click:Connect(callback)
end

-- Toggle do menu pelo botão flutuante
FloatButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = not MainFrame.Visible
end)

-- Variáveis HUB
local ESPEnabled = false
local ESPBoxes = {}
local ESPNames = {}
local Speed = 16
local Invisible = false
local NoclipEnabled = false

-- Funções existentes (ESP, Velocidade, Invisibilidade total, Noclip)
-- [Mesmas funções do script anterior: ESP com nome, box e atravessa paredes; Invisibilidade total; Noclip; Speed + / -]

-- Criação dos botões premium
CreateButton("Toggle ESP", function()
    ESPEnabled = not ESPEnabled
    if ESPEnabled then
        for _, player in pairs(Players:GetPlayers()) do
            if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                -- Box
                local box = Instance.new("BoxHandleAdornment")
                box.Adornee = player.Character.HumanoidRootPart
                box.Size = Vector3.new(2, 5, 1)
                box.Color3 = Color3.fromRGB(0, 170, 255)
                box.Transparency = 0.5
                box.AlwaysOnTop = true
                box.ZIndex = 2
                box.Parent = workspace
                ESPBoxes[player] = box

                -- Nome
                local nameTag = Instance.new("BillboardGui")
                nameTag.Adornee = player.Character:FindFirstChild("HumanoidRootPart")
                nameTag.Size = UDim2.new(0, 100, 0, 40)
                nameTag.AlwaysOnTop = true
                nameTag.Parent = player.Character:FindFirstChild("HumanoidRootPart")
                
                local label = Instance.new("TextLabel")
                label.Text = player.Name
                label.Size = UDim2.new(1,0,1,0)
                label.BackgroundTransparency = 1
                label.TextColor3 = Color3.fromRGB(0, 170, 255)
                label.Font = Enum.Font.GothamBold
                label.TextScaled = true
                label.Parent = nameTag
                ESPNames[player] = nameTag
            end
        end
    else
        for _, box in pairs(ESPBoxes) do box:Destroy() end
        for _, tag in pairs(ESPNames) do tag:Destroy() end
        ESPBoxes = {}
        ESPNames = {}
    end
end, 1)

CreateButton("Speed +", function()
    Speed = Speed + 5
    LocalPlayer.Character.Humanoid.WalkSpeed = Speed
end, 2)

CreateButton("Speed -", function()
    Speed = Speed - 5
    LocalPlayer.Character.Humanoid.WalkSpeed = Speed
end, 3)

CreateButton("Toggle Invisibility", function()
    Invisible = not Invisible
    for _, part in pairs(LocalPlayer.Character:GetChildren()) do
        if part:IsA("BasePart") or part:IsA("Accessory") or part:IsA("Hat") or part:IsA("CharacterMesh") then
            if part:IsA("Accessory") or part:IsA("Hat") then
                if part:FindFirstChild("Handle") then
                    part.Handle.Transparency = Invisible and 1 or 0
                end
            else
                part.Transparency = Invisible and 1 or 0
            end
        end
    end
end, 4)

CreateButton("Toggle Noclip", function()
    NoclipEnabled = not NoclipEnabled
end, 5)

RunService.Stepped:Connect(function()
    if NoclipEnabled then
        for _, part in pairs(LocalPlayer.Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = false
            end
        end
    else
        for _, part in pairs(LocalPlayer.Character:GetChildren()) do
            if part:IsA("BasePart") then
                part.CanCollide = true
            end
        end
    end
end)
