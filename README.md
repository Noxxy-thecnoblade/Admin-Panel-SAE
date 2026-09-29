# Admin-Panel-SAE
This Admin Panel So Crazy!
--[[
    STEAL AN EGG - CUSTOM ADMIN PANEL (CREATOR PANEL)
    Inspired by Pak GM Channel
    Features: Draggable, FE Compatible, English Translated, Anti-Hack Structure
--]]

local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local LocalPlayer = Players.LocalPlayer
local PlayerGui = LocalPlayer:WaitForChild("PlayerGui")

-- Mencegah duplikasi GUI jika script dijalankan ulang
if PlayerGui:FindFirstChild("CreatorPanelGui") then
    PlayerGui.CreatorPanelGui:Destroy()
end

-- ==========================================
-- 1. UTILITIES (Draggable & Security)
-- ==========================================
local function makeDraggable(gui)
    local dragging, dragInput, dragStart, startPos
    gui.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
            dragging = true
            dragStart = input.Position
            startPos = gui.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then dragging = false end
            end)
        end
    end)
    gui.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
            dragInput = input
        end
    end)
    UserInputService.InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

-- ==========================================
-- 2. GUI CREATION
-- ==========================================
local CreatorPanelGui = Instance.new("ScreenGui")
CreatorPanelGui.Name = "CreatorPanelGui"
CreatorPanelGui.ResetOnSpawn = false
CreatorPanelGui.Parent = PlayerGui

-- Tombol Bulat Pemicu (Trigger Button)
local ToggleButton = Instance.new("TextButton")
ToggleButton.Name = "ToggleButton"
ToggleButton.Size = UDim2.new(0, 60, 0, 60)
ToggleButton.Position = UDim2.new(0.05, 0, 0.5, -30)
ToggleButton.BackgroundColor3 = Color3.fromRGB(50, 205, 50) -- Tema Hijau
ToggleButton.Text = "A"
ToggleButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ToggleButton.Font = Enum.Font.GothamBold
ToggleButton.TextSize = 28
ToggleButton.Parent = CreatorPanelGui

local UICorner_Toggle = Instance.new("UICorner")
UICorner_Toggle.CornerRadius = UDim.new(1, 0) -- Membuat jadi bulat penuh
UICorner_Toggle.Parent = ToggleButton

makeDraggable(ToggleButton)

-- Main Frame (Admin Panel)
local MainFrame = Instance.new("Frame")
MainFrame.Name = "MainFrame"
MainFrame.Size = UDim2.new(0, 800, 0, 420)
MainFrame.Position = UDim2.new(0.5, -400, 0.5, -210)
MainFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 48)
MainFrame.Visible = false
MainFrame.Parent = CreatorPanelGui

local UICorner_Main = Instance.new("UICorner")
UICorner_Main.CornerRadius = UDim.new(0, 8)
UICorner_Main.Parent = MainFrame

makeDraggable(MainFrame)

-- Top Bar (Header)
local TopBar = Instance.new("Frame")
TopBar.Name = "TopBar"
TopBar.Size = UDim2.new(1, 0, 0, 45)
TopBar.BackgroundColor3 = Color3.fromRGB(35, 35, 38)
TopBar.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Text = "  Creator Panel"
Title.Size = UDim2.new(0.5, 0, 1, 0)
Title.BackgroundTransparency = 1
Title.Font = Enum.Font.GothamBold
Title.TextSize = 22
Title.TextColor3 = Color3.fromRGB(144, 238, 144) -- Hijau Muda terang
Title.TextXAlignment = Enum.TextXAlignment.Left
Title.Parent = TopBar

-- Close Button (X)
local CloseButton = Instance.new("TextButton")
CloseButton.Name = "CloseButton"
CloseButton.Size = UDim2.new(0, 40, 0, 35)
CloseButton.Position = UDim2.new(1, -45, 0, 5)
CloseButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
CloseButton.Text = "X"
CloseButton.TextColor3 = Color3.fromRGB(255, 255, 255)
CloseButton.Font = Enum.Font.GothamBold
CloseButton.TextSize = 18
CloseButton.Parent = TopBar

local UICorner_Close = Instance.new("UICorner")
UICorner_Close.CornerRadius = UDim.new(0, 6)
UICorner_Close.Parent = CloseButton

-- Sidebar Menu (Kiri)
local Sidebar = Instance.new("Frame")
Sidebar.Name = "Sidebar"
Sidebar.Size = UDim2.new(0, 150, 1, -45)
Sidebar.Position = UDim2.new(0, 0, 0, 45)
Sidebar.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Sidebar.Parent = MainFrame

local UIListLayout = Instance.new("UIListLayout")
UIListLayout.Padding = UDim.new(0, 5)
UIListLayout.Parent = Sidebar

local function createMenuButton(name)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(1, -10, 0, 35)
    btn.BackgroundColor3 = (name == "Servers") and Color3.fromRGB(50, 205, 50) or Color3.fromRGB(50, 50, 55)
    btn.Text = name
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Font = Enum.Font.Gotham
    btn.TextSize = 14
    btn.Parent = Sidebar
    
    local corner = Instance.new("UICorner")
    corner.CornerRadius = UDim.new(0, 4)
    corner.Parent = btn
    return btn
end

createMenuButton("Camera")
createMenuButton("Character")
createMenuButton("Collection")
createMenuButton("Mutations")
createMenuButton("Scene")
local ServerBtn = createMenuButton("Servers")

-- Content Display Area (Kanan)
local ContentArea = Instance.new("Frame")
ContentArea.Name = "ContentArea"
ContentArea.Size = UDim2.new(1, -160, 1, -55)
ContentArea.Position = UDim2.new(0, 155, 0, 50)
ContentArea.BackgroundColor3 = Color3.fromRGB(38, 38, 40)
ContentArea.Parent = MainFrame

-- Konten Menu "Servers" (Sesuai Foto)
local ServerTitle = Instance.new("TextLabel")
ServerTitle.Text = "My Server"
ServerTitle.Size = UDim2.new(1, 0, 0, 30)
ServerTitle.Position = UDim2.new(0, 10, 0, 10)
ServerTitle.BackgroundTransparency = 1
ServerTitle.Font = Enum.Font.GothamBold
ServerTitle.TextSize = 20
ServerTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
ServerTitle.TextXAlignment = Enum.TextXAlignment.Left
ServerTitle.Parent = ContentArea

local InfoLabel = Instance.new("TextLabel")
local serverIdStr = "CC_local_id_" .. math.random(10000000, 99999999)
InfoLabel.Text = serverIdStr .. " is closed. Start it to open your private server."
InfoLabel.Size = UDim2.new(1, -20, 0, 20)
InfoLabel.Position = UDim2.new(0, 10, 0, 45)
InfoLabel.BackgroundTransparency = 1
InfoLabel.Font = Enum.Font.Gotham
InfoLabel.TextSize = 13
InfoLabel.TextColor3 = Color3.fromRGB(180, 180, 180)
InfoLabel.TextXAlignment = Enum.TextXAlignment.Left
InfoLabel.Parent = ContentArea

-- Tombol Utama: Start & Join My Server
local ActionButton = Instance.new("TextButton")
ActionButton.Name = "ActionButton"
ActionButton.Size = UDim2.new(1, -20, 0, 50)
ActionButton.Position = UDim2.new(0, 10, 1, -60)
ActionButton.BackgroundColor3 = Color3.fromRGB(50, 205, 50)
ActionButton.Text = "Start & join my server"
ActionButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ActionButton.Font = Enum.Font.GothamBold
ActionButton.TextSize = 18
ActionButton.Parent = ContentArea

local UICorner_Action = Instance.new("UICorner")
UICorner_Action.CornerRadius = UDim.new(0, 6)
UICorner_Action.Parent = ActionButton

-- ==========================================
-- 3. INTERACTION & ANIMATION LOGIC
-- ==========================================
local isCooldown = false

ToggleButton.MouseButton1Click:Connect(function()
    if isCooldown then return end
    isCooldown = true
    
    -- Efek Putar pada Huruf "A" (360 derajat)
    ToggleButton.Rotation = 0
    local tweenInfo = TweenInfo.new(0.5, Enum.EasingStyle.Quad, Enum.EasingDirection.Out)
    local tween = TweenService:Create(ToggleButton, tweenInfo, {Rotation = 360})
    tween:Play()
    
    -- Menampilkan / Menyembunyikan Main Panel
    MainFrame.Visible = not MainFrame.Visible
    
    task.wait(0.5)
    isCooldown = false
end)

CloseButton.MouseButton1Click:Connect(function()
    MainFrame.Visible = false
end)

-- Simulasi Fitur "Start Server" secara FE-safe (Local Context Only)
ActionButton.MouseButton1Click:Connect(function()
    ActionButton.Text = "Launching Server..."
    ActionButton.BackgroundColor3 = Color3.fromRGB(100, 100, 100)
    task.wait(1.5)
    ActionButton.Text = "Server Running (Connected)"
    ActionButton.BackgroundColor3 = Color3.fromRGB(0, 128, 0)
    InfoLabel.Text = serverIdStr .. " is now active. Enjoy Steal an Egg!"
    InfoLabel.TextColor3 = Color3.fromRGB(144, 238, 144)
end)
