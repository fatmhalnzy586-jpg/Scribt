-- ملف فارم الصيد والخيارات المتقدمة لماب BlockSpin
local OrionLib = loadstring(game:HttpGet(('https://raw.githubusercontent.com/shlexware/Orion/main/source')))()
local Window = OrionLib:MakeWindow({Name = "ملف فارم BlockSpin 🔪", HidePremium = false, SaveConfig = true, ConfigFolder = "FarmConfig"})

local MainTab = Window:MakeTab({Name = "الأوتوفارم والحماية", Icon = "rbxassetid://4483345998", PremiumOnly = false})

-- 1. فارم الصيد تحت الأرض
MainTab:AddToggle({
    Name = "تشغيل فارم الصيد تحت الأرض",
    Default = false,
    Callback = function(Value)
        _G.UndergroundFarm = Value
        task.spawn(function()
            local player = game.Players.LocalPlayer
            while _G.UndergroundFarm do
                task.wait(0.1)
                if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    local pos = player.Character.HumanoidRootPart.Position
                    player.Character.HumanoidRootPart.CFrame = CFrame.new(pos.X, -25, pos.Z)
                    
                    local tool = player.Character:FindFirstChildOfClass("Tool")
                    if tool then tool:Activate() end
                end
            end
        end)
    end
})

-- 2. مضاد الموت
MainTab:AddToggle({
    Name = "مضاد الموت (Godmode)",
    Default = false,
    Callback = function(Value)
        _G.GodMode = Value
        task.spawn(function()
            while _G.GodMode do
                task.wait(0.1)
                local player = game.Players.LocalPlayer
                if player.Character and player.Character:FindFirstChild("Humanoid") then
                    player.Character.Humanoid.Health = player.Character.Humanoid.MaxHealth
                    player.Character.Humanoid:SetStateEnabled(Enum.HumanoidStateType.Dead, false)
                end
            end
        end)
    end
})

-- 3. اختراق الجدران والتحكم بالحركة
local MoveTab = Window:MakeTab({Name = "الحركة", Icon = "rbxassetid://4483345998", PremiumOnly = false})

MoveTab:AddToggle({
    Name = "اختراق الجدران (Noclip)",
    Default = false,
    Callback = function(Value)
        _G.Noclip = Value
        game:GetService("RunService").Stepped:Connect(function()
            if _G.Noclip and game.Players.LocalPlayer.Character then
                for _, part in pairs(game.Players.LocalPlayer.Character:GetChildren()) do
                    if part:IsA("BasePart") then part.CanCollide = false end
                end
            end
        end)
    end
})

MoveTab:AddSlider({
    Name = "السرعة", Min = 16, Max = 300, Default = 16, Increment = 1,
    Callback = function(v)
        if game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = v
        end
    end
})

MoveTab:AddSlider({
    Name = "القفز", Min = 50, Max = 500, Default = 50, Increment = 1,
    Callback = function(v)
        if game.Players.LocalPlayer.Character:FindFirstChild("Humanoid") then
            game.Players.LocalPlayer.Character.Humanoid.JumpPower = v
        end
    end
})

OrionLib:Init()
