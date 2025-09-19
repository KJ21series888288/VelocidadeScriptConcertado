--[[
Delta Executor Script: Speed GUI
Cria uma interface para alterar a velocidade do jogador no Roblox.
Feita para Delta Executor. GUI branca, arrastável, com botão de fechar.
]]

-- Função para tornar a GUI arrastável
local function makeDraggable(gui, dragHandle)
    local dragging
    local dragInput
    local dragStart
    local startPos

    dragHandle.InputBegan:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseButton1 then
            dragging = true
            dragStart = input.Position
            startPos = gui.Position
            input.Changed:Connect(function()
                if input.UserInputState == Enum.UserInputState.End then
                    dragging = false
                end
            end)
        end
    end)

    dragHandle.InputChanged:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.MouseMovement then
            dragInput = input
        end
    end)

    game:GetService("UserInputService").InputChanged:Connect(function(input)
        if input == dragInput and dragging then
            local delta = input.Position - dragStart
            gui.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X,
                                     startPos.Y.Scale, startPos.Y.Offset + delta.Y)
        end
    end)
end

-- GUI Creation
local ScreenGui = Instance.new("ScreenGui")
ScreenGui.Name = "SpeedGui"
ScreenGui.Parent = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")

local MainFrame = Instance.new("Frame")
MainFrame.Size = UDim2.new(0, 300, 0, 180)
MainFrame.Position = UDim2.new(0.5, -150, 0.4, -90)
MainFrame.BackgroundColor3 = Color3.fromRGB(255,255,255)
MainFrame.BorderSizePixel = 0
MainFrame.Parent = ScreenGui

local DragBar = Instance.new("Frame")
DragBar.Size = UDim2.new(1, 0, 0, 25)
DragBar.Position = UDim2.new(0, 0, 0, 0)
DragBar.BackgroundColor3 = Color3.fromRGB(220,220,220)
DragBar.Parent = MainFrame

local Title = Instance.new("TextLabel")
Title.Text = "Speed GUI"
Title.Size = UDim2.new(1, -30, 0, 25)
Title.Position = UDim2.new(0, 10, 0, 0)
Title.BackgroundTransparency = 1
Title.TextColor3 = Color3.fromRGB(0,0,0)
Title.Font = Enum.Font.SourceSansSemibold
Title.TextSize = 18
Title.Parent = DragBar

local CloseButton = Instance.new("TextButton")
CloseButton.Text = "X"
CloseButton.Size = UDim2.new(0, 25, 0, 25)
CloseButton.Position = UDim2.new(1, -25, 0, 0)
CloseButton.BackgroundColor3 = Color3.fromRGB(255,255,255)
CloseButton.TextColor3 = Color3.fromRGB(0,0,0)
CloseButton.Font = Enum.Font.SourceSansBold
CloseButton.TextSize = 18
CloseButton.Parent = DragBar

CloseButton.MouseButton1Click:Connect(function()
    ScreenGui:Destroy()
end)

makeDraggable(MainFrame, DragBar)

local SpeedLabel = Instance.new("TextLabel")
SpeedLabel.Text = "Velocidade:"
SpeedLabel.Position = UDim2.new(0, 20, 0, 40)
SpeedLabel.Size = UDim2.new(0, 100, 0, 30)
SpeedLabel.BackgroundTransparency = 1
SpeedLabel.TextColor3 = Color3.fromRGB(0,0,0)
SpeedLabel.Font = Enum.Font.SourceSans
SpeedLabel.TextSize = 16
SpeedLabel.Parent = MainFrame

local SpeedBox = Instance.new("TextBox")
SpeedBox.Text = "32"
SpeedBox.Position = UDim2.new(0, 130, 0, 40)
SpeedBox.Size = UDim2.new(0, 60, 0, 30)
SpeedBox.BackgroundColor3 = Color3.fromRGB(240,240,240)
SpeedBox.TextColor3 = Color3.fromRGB(0,0,0)
SpeedBox.Font = Enum.Font.SourceSans
SpeedBox.TextSize = 16
SpeedBox.Parent = MainFrame

local ApplyButton = Instance.new("TextButton")
ApplyButton.Text = "Aplicar velocidade"
ApplyButton.Position = UDim2.new(0.5, -70, 0, 90)
ApplyButton.Size = UDim2.new(0, 140, 0, 40)
ApplyButton.BackgroundColor3 = Color3.fromRGB(220,220,220)
ApplyButton.TextColor3 = Color3.fromRGB(0,0,0)
ApplyButton.Font = Enum.Font.SourceSansBold
ApplyButton.TextSize = 16
ApplyButton.Parent = MainFrame

-- Função para aplicar velocidade
ApplyButton.MouseButton1Click:Connect(function()
    local speed = tonumber(SpeedBox.Text)
    if speed and game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = speed
    end
end)

-- Garantir que ao fechar, a velocidade volte ao padrão (opcional)
ScreenGui.AncestryChanged:Connect(function()
    if not ScreenGui.Parent then
        local humanoid = game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid")
        if humanoid then
            humanoid.WalkSpeed = 16 -- Valor padrão do Roblox
        end
    end
end)
