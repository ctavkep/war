-- Create GUI
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local ScreenGui = Instance.new("ScreenGui", LocalPlayer:WaitForChild("PlayerGui"))
local Frame = Instance.new("Frame", ScreenGui)
local TitleLabel = Instance.new("TextLabel", Frame)
local AimbotButton = Instance.new("TextButton", Frame)
local RotationButton = Instance.new("TextButton", Frame)
local SpeedInput = Instance.new("TextBox", Frame)
local ESPButton = Instance.new("TextButton", Frame)

-- Frame properties
Frame.Size = UDim2.new(0.6, 0, 0.4, 0)
Frame.Position = UDim2.new(0.2, 0, 0.3, 0)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 70)  -- Dark blended color with blue
Frame.BorderSizePixel = 0

-- TitleLabel properties
TitleLabel.Size = UDim2.new(1, 0, 0.2, 0)
TitleLabel.Position = UDim2.new(0, 0, 0, 0)
TitleLabel.Text = "Neverlose.cc"
TitleLabel.TextColor3 = Color3.fromRGB(255, 255, 255)
TitleLabel.BackgroundTransparency = 1
TitleLabel.TextScaled = true

-- AimbotButton properties
AimbotButton.Size = UDim2.new(0.4, 0, 0.2, 0)
AimbotButton.Position = UDim2.new(0.05, 0, 0.25, 0)
AimbotButton.Text = "aimbats (OFF)"
AimbotButton.TextColor3 = Color3.fromRGB(255, 255, 255)
AimbotButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
AimbotButton.BorderSizePixel = 0
AimbotButton.TextScaled = true

-- RotationButton properties
RotationButton.Size = UDim2.new(0.4, 0, 0.2, 0)
RotationButton.Position = UDim2.new(0.05, 0, 0.5, 0)
RotationButton.Text = "Вращение (OFF)"
RotationButton.TextColor3 = Color3.fromRGB(255, 255, 255)
RotationButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
RotationButton.BorderSizePixel = 0
RotationButton.TextScaled = true

-- SpeedInput properties
SpeedInput.Size = UDim2.new(0.4, 0, 0.2, 0)
SpeedInput.Position = UDim2.new(0.05, 0, 0.75, 0)
SpeedInput.PlaceholderText = "Введите скорость"
SpeedInput.TextColor3 = Color3.fromRGB(255, 255, 255)
SpeedInput.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
SpeedInput.BorderSizePixel = 0
SpeedInput.TextScaled = true

-- ESP Button properties
ESPButton.Size = UDim2.new(0.25, 0, 0.15, 0)
ESPButton.Position = UDim2.new(0.75, 0, 0.05, 0)
ESPButton.Text = "ESP (OFF)"
ESPButton.TextColor3 = Color3.fromRGB(255, 255, 255)
ESPButton.BackgroundColor3 = Color3.fromRGB(80, 80, 80)
ESPButton.BorderSizePixel = 0
ESPButton.TextScaled = true

-- Aimbot Variables
local Camera = workspace.CurrentCamera
local UserInputService = game:GetService("UserInputService")
local RunService = game:GetService("RunService")
local TweenService = game:GetService("TweenService")

local aimbotEnabled = false
_G.AimbotEnabled = false
_G.TeamCheck = false
_G.AimPart = "Head"
_G.Sensitivity = 0
_G.CircleSides = 64
_G.CircleColor = Color3.fromRGB(255, 255, 255)
_G.CircleTransparency = 0.7
_G.CircleRadius = 80
_G.CircleFilled = false
_G.CircleVisible = true
_G.CircleThickness = 1 -- Added thickness for the circle

local FOVCircle = Drawing.new("Circle")
FOVCircle.Color = _G.CircleColor
FOVCircle.Filled = _G.CircleFilled
FOVCircle.Transparency = _G.CircleTransparency
FOVCircle.Radius = _G.CircleRadius
FOVCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)

local Holding = false

local function GetClosestPlayer()
    local MaximumDistance = _G.CircleRadius
    local Target = nil
    
    for _, v in next, Players:GetPlayers() do
        if v.Name ~= LocalPlayer.Name then
            if _G.TeamCheck == true then
                if v.Team ~= LocalPlayer.Team then
                    if v.Character and v.Character:FindFirstChild("HumanoidRootPart") and v.Character:FindFirstChild("Humanoid").Health > 0 then
                        local ScreenPoint = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                        local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
                        
                        if VectorDistance < MaximumDistance then
                            Target = v
                        end
                    end
                end
            else
                if v.Character and v.Character:FindFirstChild("HumanoidRootPart") and v.Character:FindFirstChild("Humanoid").Health > 0 then
                    local ScreenPoint = Camera:WorldToScreenPoint(v.Character.HumanoidRootPart.Position)
                    local VectorDistance = (Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y) - Vector2.new(ScreenPoint.X, ScreenPoint.Y)).Magnitude
                    
                    if VectorDistance < MaximumDistance then
                        Target = v
                    end
                end
            end
        end
    end
    
    return Target
end

UserInputService.InputBegan:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton2 then
        Holding = true
    end
end)

UserInputService.InputEnded:Connect(function(Input)
    if Input.UserInputType == Enum.UserInputType.MouseButton2 then
        Holding = false
    end
end)

RunService.RenderStepped:Connect(function()
    FOVCircle.Position = Vector2.new(UserInputService:GetMouseLocation().X, UserInputService:GetMouseLocation().Y)
    FOVCircle.Radius = _G.CircleRadius
    FOVCircle.Filled = _G.CircleFilled
    FOVCircle.Color = _G.CircleColor
    FOVCircle.Visible = _G.CircleVisible
    FOVCircle.Transparency = _G.CircleTransparency
    FOVCircle.Thickness = _G.CircleThickness -- Set thickness of the circle
    
    if aimbotEnabled then -- Check aimbot status regardless of GUI visibility
        if UserInputService:IsMouseButtonPressed(Enum.UserInputType.MouseButton2) then -- Activate Aimbot on right mouse button press
            local targetPlayer = GetClosestPlayer()
            if targetPlayer then
                TweenService:Create(Camera, TweenInfo.new(_G.Sensitivity, Enum.EasingStyle.Sine, Enum.EasingDirection.Out), {CFrame = CFrame.new(Camera.CFrame.Position, targetPlayer.Character[_G.AimPart].Position)}):Play()
            end
        end
    end
end)

-- Button click event to toggle Aimbot
AimbotButton.MouseButton1Click:Connect(function()
    aimbotEnabled = not aimbotEnabled
    _G.AimbotEnabled = aimbotEnabled
    AimbotButton.Text = aimbotEnabled and "aimbats (ON)" or "aimbats (OFF)"
end)

-- Spinbot functionality
local spinbotEnabled = false

local function setupSpinbot(speed)
    local plr = game:GetService("Players").LocalPlayer
    repeat task.wait() until plr.Character
    local humRoot = plr.Character:WaitForChild("HumanoidRootPart")
    plr.Character:WaitForChild("Humanoid").AutoRotate = false

    local velocity = Instance.new("AngularVelocity")
    velocity.Attachment0 = humRoot:WaitForChild("RootAttachment")
    velocity.MaxTorque = math.huge
    velocity.AngularVelocity = Vector3.new(0, speed, 0)
    velocity.Parent = humRoot
    velocity.Name = "Spinbot"
end

-- Connect the function to Player's CharacterAdded event
local playerAddedConnection

local function onCharacterAdded(character)
    task.wait(1) -- Wait for a second to ensure the character loads
    if spinbotEnabled then 
        setupSpinbot(50) -- Default speed at character spawn if enabled
    end
end

playerAddedConnection = LocalPlayer.CharacterAdded:Connect(onCharacterAdded)

-- Button click event to toggle Spinbot with custom speed
RotationButton.MouseButton1Click:Connect(function()
    spinbotEnabled = not spinbotEnabled
    RotationButton.Text = spinbotEnabled and "Вращение (ON)" or "Вращение (OFF)"
    
    if spinbotEnabled then
        local speedValue = tonumber(SpeedInput.Text) or 50 -- Default to 50 if input is invalid
        setupSpinbot(speedValue)
    else
        local velocity = LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart"):FindFirstChild("Spinbot")
        if velocity then velocity:Destroy() end -- Remove Spinbot when turned off
    end
end)

-- ESP functionality
local espEnabled = false

local function ApplyESP(v)
    if v.Character and v.Character:FindFirstChildOfClass('Humanoid') then
        v.Character.Humanoid.NameDisplayDistance = 9e9
        v.Character.Humanoid.NameOcclusion = "NoOcclusion"
        v.Character.Humanoid.HealthDisplayDistance = 9e9
        v.Character.Humanoid.HealthDisplayType = "AlwaysOn"
        v.Character.Humanoid.Health = v.Character.Humanoid.Health -- triggers changed
    end
end

for i,v in pairs(Players:GetPlayers()) do
    ApplyESP(v)
    v.CharacterAdded:Connect(function()
        task.wait(0.33)
        ApplyESP(v)
    end)
end

Players.PlayerAdded:Connect(function(v)
    ApplyESP(v)
    v.CharacterAdded:Connect(function()
        task.wait(0.33)
        ApplyESP(v)
    end)
end)

-- ESP Button click event to toggle ESP functionality
ESPButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    ESPButton.Text = espEnabled and "ESP (ON)" or "ESP (OFF)"
    
    if espEnabled then
        for _, player in pairs(Players:GetPlayers()) do
            ApplyESP(player)
        end
    else
        for _, player in pairs(Players:GetPlayers()) do
            if player.Character and player.Character:FindFirstChildOfClass('Humanoid') then
                player.Character.Humanoid.NameDisplayDistance = 200
                player.Character.Humanoid.NameOcclusion = "Occlusion"
                player.Character.Humanoid.HealthDisplayDistance = 200
                player.Character.Humanoid.HealthDisplayType = "Normal"
                player.Character.Humanoid.Health = player.Character.Humanoid.Health -- triggers changed
            end
        end
    end
end)

-- Toggle GUI visibility using the Insert key on the keyboard
UserInputService.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Insert then
        Frame.Visible = not Frame.Visible -- Toggle visibility of the GUI frame
    end
end)

-- Keep GUI state after death and Aimbot working when GUI is hidden
LocalPlayer.CharacterRemoving:Connect(function()
    Frame.Visible = true -- Keep GUI visible after death
end)
