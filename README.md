local ScreenGui = Instance.new("ScreenGui")
local Frame = Instance.new("Frame")
local Title = Instance.new("TextLabel")
local Rights = Instance.new("TextLabel")
local UIListLayout = Instance.new("UIListLayout")

ScreenGui.Parent = game.CoreGui
Frame.Parent = ScreenGui
Frame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
Frame.Position = UDim2.new(0.3, 0, 0.2, 0)
Frame.Size = UDim2.new(0, 250, 0, 320)
Frame.Active = true
Frame.Draggable = true

Title.Parent = Frame
Title.Text = "قائمة الغش | ابو عابد"
Title.Size = UDim2.new(1, 0, 0, 40)
Title.TextColor3 = Color3.fromRGB(255, 255, 255)
Title.BackgroundColor3 = Color3.fromRGB(35, 35, 35)

UIListLayout.Parent = Frame
UIListLayout.SortOrder = Enum.SortOrder.LayoutOrder
UIListLayout.Padding = UDim.new(0, 5)

local function CreateToggle(name, callback)
    local btn = Instance.new("TextButton")
    btn.Parent = Frame
    btn.Size = UDim2.new(0.9, 0, 0, 40)
    btn.Position = UDim2.new(0.05, 0, 0, 0)
    btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
    btn.TextColor3 = Color3.fromRGB(255, 255, 255)
    btn.Text = name .. ": OFF"
    
    local enabled = false
    btn.MouseButton1Click:Connect(function()
        enabled = not enabled
        if enabled then
            btn.Text = name .. ": ON"
            btn.BackgroundColor3 = Color3.fromRGB(0, 170, 0)
        else
            btn.Text = name .. ": OFF"
            btn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
        end
        callback(enabled)
    end)
end

-- 1. ESP
CreateToggle("ESP", function(state)
    _G.ESP = state
    while _G.ESP do
        for _, p in pairs(game.Players:GetPlayers()) do
            if p ~= game.Players.LocalPlayer and p.Character and not p.Character:FindFirstChild("AbedHighlight") then
                local hl = Instance.new("Highlight")
                hl.Name = "AbedHighlight"
                hl.Parent = p.Character
                hl.FillColor = Color3.fromRGB(0, 255, 0)
            end
        end
        task.wait(1)
    end
    if not _G.ESP then
        for _, p in pairs(game.Players:GetPlayers()) do
            if p.Character and p.Character:FindFirstChild("AbedHighlight") then
                p.Character.AbedHighlight:Destroy()
            end
        end
    end
end)

-- 2. Silent Aim (محسن ومفعل مع FOV)
local FOVCircle = Drawing.new("Circle")
FOVCircle.Radius = 150
FOVCircle.Color = Color3.fromRGB(255, 0, 0)
FOVCircle.Thickness = 1
FOVCircle.Filled = false
FOVCircle.Visible = false

CreateToggle("Silent Aim", function(state)
    _G.SilentAim = state
    FOVCircle.Visible = state
    
    local Camera = workspace.CurrentCamera
    local LocalPlayer = game.Players.LocalPlayer
    
    game:GetService("RunService").RenderStepped:Connect(function()
        if _G.SilentAim then
            FOVCircle.Position = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
        end
    end)
    
    local function GetClosestPlayer()
        local Closest = nil
        local MaxDist = FOVCircle.Radius
        
        for _, v in pairs(game.Players:GetPlayers()) do
            if v ~= LocalPlayer and v.Character and v.Character:FindFirstChild("HumanoidRootPart") then
                local Pos, OnScreen = Camera:WorldToViewportPoint(v.Character.HumanoidRootPart.Position)
                if OnScreen then
                    local MousePos = Vector2.new(Camera.ViewportSize.X / 2, Camera.ViewportSize.Y / 2)
                    local Dist = (Vector2.new(Pos.X, Pos.Y) - MousePos).Magnitude
                    if Dist < MaxDist then
                        Closest = v.Character.Head
                        MaxDist = Dist
                    end
                end
            end
        end
        return Closest
    end
    
    local mt = getrawmetatable(game)
    local old = mt.__namecall
    setreadonly(mt, false)
    
    mt.__namecall = newcclosure(function(self, ...)
        local method = getnamecallmethod()
        if _G.SilentAim and (method == "FindPartOnRayWithIgnoreList" or method == "Raycast") then
            local Target = GetClosestPlayer()
            if Target then
                return Target, Target.Position
            end
        end
        return old(self, ...)
    end)
    setreadonly(mt, true)
end)

-- 3. Inf Stamina
CreateToggle("Inf Stamina", function(state)
    _G.InfStamina = state
    game:GetService("RunService").RenderStepped:Connect(function()
        if _G.InfStamina and game.Players.LocalPlayer.Character then
            local hum = game.Players.LocalPlayer.Character:FindFirstChildOfClass("Humanoid")
            if hum then hum.WalkSpeed = 30 end
        end
    end)
end)

-- 4. FPS Boost
CreateToggle("FPS Boost", function(state)
    if state then
        for _, v in pairs(game.Workspace:GetDescendants()) do
            if v:IsA("BasePart") then v.Material = Enum.Material.SmoothPlastic end
        end
    end
end)

Rights.Parent = Frame
Rights.Text = "سكربت بواسطة: ابو عابد"
Rights.Size = UDim2.new(1, 0, 0, 30)
Rights.TextColor3 = Color3.fromRGB(180, 180, 180)
Rights.BackgroundTransparency = 1
