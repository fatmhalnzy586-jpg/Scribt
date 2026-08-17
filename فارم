-- BlockSpin Ultimate Script (Underground Farm & Cheats)
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "BlockSpin Ultimate Hub 🔪", HidePremium = false, SaveConfig = true, ConfigFolder = "BlockSpinConfig"})

-- 1. Tab: Underground Farm & Godmode
local FarmTab = Window:MakeTab({Name = "الصيد والمناعة", Icon = "rbxassetid://4483345998", PremiumOnly = false})

-- Underground Fishing Farm (فارم الصيد تحت الأرض)
FarmTab:AddToggle({
    Name = "فارم الصيد تحت الأرض (Underground Fishing)",
    Default = false,
    Callback = function(Value)
        _G.UndergroundFarm = Value
        
        task.spawn(function()
            local player = game.Players.LocalPlayer
            while _G.UndergroundFarm do
                task.wait(0.1)
                if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    -- النزول بمسافة آمنة تحت الأرض لتفادي الهجمات ورؤية نقاط الصيد
                    local currentPos = player.Character.HumanoidRootPart.Position
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(currentPos.X, -25, currentPos.Z)
                    
                    -- تفعيل أداة الصيد والضرب التلقائي
                    local tool = player.Character:FindFirstChildOfClass("Tool")
                    if tool then
                        tool:Activate()
                    end
                end
            end
        end)
    end
})

-- Anti-Death / Godmode (مضاد الموت)
FarmTab:AddToggle({
    Name = "مضاد الموت (Godmode / Anti-Death)",
    Default = false,
    Callback = function(Value)
        _G.GodMode = Value
        
        task.spawn(function()
            while _G.GodMode do
                task.wait(0.2)
                local player = game.Players.LocalPlayer
                if player.Character and player.Character:FindFirstChild("Humanoid") then
                    -- إلغاء أوامر الموت والحفاظ على الصحة كاملة
                    player.Character.Humanoid.Health = player.Character.Humanoid.MaxHealth
                    player.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
                end
            end
        end)
    end
})

-- 2. Tab: Movement & Movement Hacks
local MovementTab = Window:MakeTab({Name = "الحركة والعبور", Icon = "rbxassetid://4483345998", PremiumOnly = false})

-- Noclip (اختراق الجدران)
MovementTab:AddToggle({
    Name = "اختراق الجدران (Noclip)",
    Default = false,
    Callback = function(Value)
        _G.Noclip = Value
        
        game:GetService("RunService").Stepped:Connect(function()
            if _G.Noclip then
                local character = game.Players.LocalPlayer.Character
                if character then
                    for _, part in pairs(character:GetChildren()) do
                        if part:IsA("BasePart") then
                            part.CanCollide = false
                        end
                    end
                end
            end
        end)
    end
})

-- WalkSpeed Slider (السرعة)
MovementTab:AddSlider({
    Name = "السرعة (WalkSpeed)",
    Min = 16,
    Max = 300,
    Default = 16,
    Color = Color3.fromRGB(0, 255, 127),
    Increment = 1,
    ValueName = "Speed",
    Callback = function(Value)
        if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = Value
        end
    end    
})

-- JumpPower Slider (القفز)
MovementTab:AddSlider({
    Name = "قوة القفز (JumpPower)",
    Min = 50,
    Max = 500,
    Default = 50,
    Color = Color3.fromRGB(0, 191, 255),
    Increment = 1,
    ValueName = "Jump",
    Callback = function(Value)
        if game.Players.LocalPlayer.Character and game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid.JumpPower = Value
        end
    end    
})

OrionLib:Init()
