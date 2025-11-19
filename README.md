local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")

local LocalPlayer = Players.LocalPlayer

local AntiStunConnection = nil

local function EnableAntiStun()
    AntiStunConnection = RunService.RenderStepped:Connect(function()
        local Character = LocalPlayer.Character
        if not Character then return end

        local HRP = Character:FindFirstChild("HumanoidRootPart")
        local Humanoid = Character:FindFirstChildOfClass("Humanoid")

        if HRP and Humanoid then
            local Move = Humanoid.MoveDirection
            if Move.Magnitude > 0 then
                HRP.CFrame = HRP.CFrame + Vector3.new(Move.X, 0, Move.Z).Unit * 1.2
            end
        end
    end)
end

local function DisableAntiStun()
    if AntiStunConnection then
        AntiStunConnection:Disconnect()
        AntiStunConnection = nil
    end
end

local SpeedConnection = nil
local SpeedValue = 700

local function SetWalkSpeed(speed)
    local Char = LocalPlayer.Character
    if not Char then return end

    local Humanoid = Char:FindFirstChildOfClass("Humanoid")
    if Humanoid then
        Humanoid.WalkSpeed = speed
    end
end

local function EnableSpeed()
    SpeedConnection = RunService.RenderStepped:Connect(function()
        SetWalkSpeed(SpeedValue)
    end)
end

local function DisableSpeed()
    if SpeedConnection then
        SpeedConnection:Disconnect()
        SpeedConnection = nil
    end
    SetWalkSpeed(16)
end

local Activated = false

UserInputService.InputBegan:Connect(function(input, gpe)
    if gpe then return end

    if input.KeyCode == Enum.KeyCode.K then
        Activated = not Activated

        if Activated then
            EnableAntiStun()
            EnableSpeed()
        else
            DisableAntiStun()
            DisableSpeed()
        end
    end
end)
