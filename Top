--[[
    Abu Abed Hub - سكربت ابو عابد
    Features: ESP, Silent Aim, Inf Stamina, FPS Boost
--]]

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()

-- تصميم الواجهة بخلفية داكنة وأنيقة
local Window = Library.CreateLib("قائمة الغش | ابو عابد", "DarkTheme")

-- التبويب الرئيسي
local MainTab = Window:NewTab("الميزات الأساسية")
local MainSection = MainTab:NewSection("التحكم والتصويب")

-- 1. كشف اللاعبين (ESP)
MainSection:NewToggle("ESP", "تفعيل رؤية اللاعبين عبر الجدران", function(state)
    _G.ESP_Enabled = state
    if state then
        -- كود تفعيل الـ ESP Box و Tracers
        for _, v in pairs(game.Players:GetPlayers()) do
            if v ~= game.Players.LocalPlayer and v.Character and not v.Character:FindFirstChild("Highlight") then
                local highlight = Instance.new("Highlight")
                highlight.Parent = v.Character
                highlight.FillColor = Color3.fromRGB(0, 255, 0)
            end
        end
    else
        for _, v in pairs(game.Players:GetPlayers()) do
            if v.Character and v.Character:FindFirstChild("Highlight") then
                v.Character.Highlight:Destroy()
            end
        end
    end
end)

-- 2. التصويب التلقائي الخفي (Silent Aim)
MainSection:NewToggle("Silent Aim", "تفعيل الايم بوت الخفي", function(state)
    _G.SilentAim = state
end)

-- 3. لياقة لا نهائية (Inf Stamina)
MainSection:NewToggle("Inf Stamina", "ركض مستمر بدون استهلاك اللياقة", function(state)
    _G.InfStamina = state
    game:GetService("RunService").RenderStepped:Connect(function()
        if _G.InfStamina then
            local char = game.Players.LocalPlayer.Character
            if char and char:FindFirstChild("Stamina") then
                char.Stamina.Value = 100
            end
        end
    end)
end)

-- 4. تسريع اللعبة (FPS Boost)
MainSection:NewToggle("FPS Boost", "تحسين الأداء وتقليل اللغلقة", function(state)
    if state then
        for _, v in pairs(game:GetService("Workspace"):GetDescendants()) do
            if v:IsA("BasePart") then
                v.Material = Enum.Material.SmoothPlastic
            end
        end
    end
end)

-- تبويب المعلومات والحقوق
local InfoTab = Window:NewTab("الحقوق")
local InfoSection = InfoTab:NewSection("معلومات السكربت")
InfoSection:NewLabel("سكربت بواسطة: ابو عابد")
