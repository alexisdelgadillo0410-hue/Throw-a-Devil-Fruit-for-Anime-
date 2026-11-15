-- ============================================
-- Alex Void Gang | Auto Farm + Perfect 20 + AFK
-- ============================================

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local VirtualUser = game:GetService("VirtualUser")
local UserInputService = game:GetService("UserInputService")
local playerGui = Players.LocalPlayer:WaitForChild("PlayerGui")
local player = Players.LocalPlayer

local CONFIG = {
    Cooldown = 4, -- Time to wait between throws
}

local autoFarmEnabled = false
local afkEnabled = false
local tapEnabled = false
local noclipConnection = nil

-- ============================================
-- 1. PERFECT 20 METER
-- ============================================
local function toggleTrueAutoTap(state)
    for _, obj in ipairs(player.PlayerScripts:GetDescendants()) do
        if obj:IsA("ModuleScript") and obj.Name == "ThrowMeter" then
            local ok, MeterModule = pcall(require, obj)
            if ok and MeterModule then
                MeterModule.TestAutoTap = state and 1.1 or nil
            end
            break
        end
    end
end

-- ============================================
-- 2. NOCLIP & WALK FARM
-- ============================================
local function setNoclip(enabled)
    if noclipConnection then noclipConnection:Disconnect() noclipConnection = nil end
    if not enabled then return end
    noclipConnection = RunService.Stepped:Connect(function()
        local char = player.Character
        if not char then return end
        for _, part in ipairs(char:GetDescendants()) do
            if part:IsA("BasePart") then 
                part.CanCollide = false 
                part.CanTouch = false 
            end
        end
    end)
end

local function runAutoFarm()
    while autoFarmEnabled do
        local char = player.Character
        local hum = char and char:FindFirstChildOfClass("Humanoid")
        local hrp = char and char:FindFirstChild("HumanoidRootPart")
        local starter = workspace:FindFirstChild("StarterArea")
        local line = starter and starter:FindFirstChild("CrossLine")
        
        if hum and hrp and line and hum.Health > 0 then
            if not hrp.Anchored then
                -- Teleport safely behind the line
                local spawnPos = line.Position + Vector3.new(0, 3, 15) 
                local plotIndex = player:GetAttribute("PlotIndex")
                
                if plotIndex and workspace:FindFirstChild("Plots") then
                    local myPlot = workspace.Plots:FindFirstChild(tostring(plotIndex))
                    if myPlot then
                        spawnPos = myPlot.Position + Vector3.new(0, 3, 0)
                    end
                end
                
                hrp.CFrame = CFrame.new(spawnPos)
                task.wait(0.5)
                
                -- Walk forward across the line
                local targetPos = line.Position + Vector3.new(0, 0, -5) 
                hum:MoveTo(targetPos)
                
                -- Wait for the walk to trigger the zone
                task.wait(1.5)
            end
            
            -- Wait for the game to finish the throw (unanchor)
            local timeout = os.clock() + 10
            while hrp.Anchored and os.clock() < timeout do
                task.wait(0.2)
            end
            
            task.wait(CONFIG.Cooldown) 
        else
            task.wait(1)
        end
    end
end

local function startAutoFarm()
    if autoFarmEnabled then return end
    autoFarmEnabled = true
    setNoclip(true)
    task.spawn(runAutoFarm)
end

local function stopAutoFarm()
    autoFarmEnabled = false
    setNoclip(false)
end

-- ============================================
-- 3. GUI CREATION
-- ============================================
local existing = playerGui:FindFirstChild("AlexVoidGangGui")
if existing then existing:Destroy() end

local gui = Instance.new("ScreenGui")
gui.Name = "AlexVoidGangGui"
gui.ResetOnSpawn = false
gui.IgnoreGuiInset = true
gui.Parent = playerGui

local toggleBtn = Instance.new("TextButton")
toggleBtn.Size = UDim2.fromOffset(44, 44)
toggleBtn.Position = UDim2.new(0, 15, 0, 70)
toggleBtn.BackgroundColor3 = Color3.fromRGB(160, 60, 255)
toggleBtn.Text = "A"
toggleBtn.TextColor3 = Color3.new(1,1,1)
toggleBtn.Font = Enum.Font.GothamBold
toggleBtn.TextSize = 20
toggleBtn.Parent = gui
Instance.new("UICorner", toggleBtn).CornerRadius = UDim.new(1, 0)

local main = Instance.new("Frame")
main.Size = UDim2.fromOffset(260, 260)
main.Position = UDim2.new(0.5, -130, 0.5, -130)
main.BackgroundColor3 = Color3.fromRGB(20, 10, 35)
main.Visible = true
main.Parent = gui
Instance.new("UICorner", main).CornerRadius = UDim.new(0, 12)
local stroke = Instance.new("UIStroke", main)
stroke.Color = Color3.fromRGB(160, 60, 255)
stroke.Thickness = 2

local dragging, dragInput, dragStart, startPos
main.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseButton1 then
        dragging = true
        dragStart = input.Position
        startPos = main.Position
    end
end)
main.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.Touch or input.UserInputType == Enum.UserInputType.MouseMovement then
        dragInput = input
    end
end)
UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        local delta = input.Position - dragStart
        main.Position = UDim2.new(startPos.X.Scale, startPos.X.Offset + delta.X, startPos.Y.Scale, startPos.Y.Offset + delta.Y)
    end
end)

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1, 0, 0, 40)
title.BackgroundColor3 = Color3.fromRGB(80, 20, 140)
title.Text = "Alex Void Gang"
title.TextColor3 = Color3.new(1,1,1)
title.Font = Enum.Font.GothamBold
title.Parent = main
Instance.new("UICorner", title).CornerRadius = UDim.new(0, 12)

toggleBtn.MouseButton1Click:Connect(function() main.Visible = not main.Visible end)

local function makeBtn(text, y)
    local b = Instance.new("TextButton")
    b.Size = UDim2.new(1, -20, 0, 38)
    b.Position = UDim2.fromOffset(10, y)
    b.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    b.Text = text
    b.TextColor3 = Color3.fromRGB(120, 120, 120)
    b.Font = Enum.Font.GothamBold
    b.TextSize = 14
    b.Parent = main
    Instance.new("UICorner", b).CornerRadius = UDim.new(0, 8)
    return b
end

local farmBtn = makeBtn("AUTO FARM: OFF", 50)
local afkBtn = makeBtn("AFK: OFF", 95)
local tapBtn = makeBtn("PERFECT 20: ON", 140)

farmBtn.MouseButton1Click:Connect(function()
    if autoFarmEnabled then stopAutoFarm() farmBtn.Text = "AUTO FARM: OFF" farmBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
    else startAutoFarm() farmBtn.Text = "AUTO FARM: ON" farmBtn.BackgroundColor3 = Color3.fromRGB(80, 200, 120) end
end)

afkBtn.MouseButton1Click:Connect(function()
    afkEnabled = not afkEnabled
    afkBtn.Text = afkEnabled and "AFK: ON" or "AFK: OFF"
    afkBtn.BackgroundColor3 = afkEnabled and Color3.fromRGB(80, 200, 120) or Color3.fromRGB(60, 60, 60)
end)

tapBtn.MouseButton1Click:Connect(function()
    tapEnabled = not tapEnabled
    tapBtn.Text = tapEnabled and "PERFECT 20: ON" or "PERFECT 20: OFF"
    tapBtn.BackgroundColor3 = tapEnabled and Color3.fromRGB(80, 200, 120) or Color3.fromRGB(60, 60, 60)
    toggleTrueAutoTap(tapEnabled)
end)

player.Idled:Connect(function()
    if afkEnabled then
        VirtualUser:CaptureController()
        VirtualUser:ClickButton2(Vector2.new())
    end
end)# paste-something
A place to paste and store code or notes.
