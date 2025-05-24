-- Forsaken GUI Script
local plr = game.Players.LocalPlayer
local char = plr.Character or plr.CharacterAdded:Wait()
local hum = char:WaitForChild("Humanoid")

-- Infinite Stamina
if getgenv().InfStamina then
    game:GetService("RunService").RenderStepped:Connect(function()
        local st = hum:FindFirstChild("Stamina")
        if st then st.Value = 100 end
    end)
end

-- Speed Boost
if getgenv().Speed then
    hum.WalkSpeed = getgenv().Speed
    hum:GetPropertyChangedSignal("WalkSpeed"):Connect(function()
        hum.WalkSpeed = getgenv().Speed
    end)
end

-- Auto Generate when sitting at machine
if getgenv().AutoGen then
    for _,v in pairs(workspace:GetDescendants()) do
        if v.Name == "Generator" and v:FindFirstChild("Interact") then
            v.Interact:FireServer()
        end
    end
end

-- Simple draggable UI
local ScreenGui = Instance.new("ScreenGui", plr:WaitForChild("PlayerGui"))
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 150, 0, 60)
Frame.Position = UDim2.new(0.5, -75, 0.3, 0)
Frame.BackgroundColor3 = Color3.new(0.1,0.1,0.1)
Frame.BorderSizePixel = 0

local dragging, offset
Frame.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        offset = input.Position - Frame.Position
    end
end)
UIS.InputEnded:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = false
    end
end)
UIS.InputChanged:Connect(function(input)
    if dragging and input.UserInputType == Enum.UserInputType.MouseMovement then
        Frame.Position = UDim2.new(0, input.Position.X - offset.X.Offset, 0, input.Position.Y - offset.Y.Offset)
    end
end)
