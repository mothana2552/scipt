local Player = game:GetService("Players")
local LocalPlayer = Player.LocalPlayer
local Char = LocalPlayer.Character
local Humanoid = Char.Humanoid
local VirtualInputManager = game:GetService("VirtualInputManager")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local GuiService = game:GetService("GuiService")
 
-- ฟังก์ชันสำหรับการจับปลา
equipitem = function(v)
    if LocalPlayer.Backpack:FindFirstChild(v) then
        local a = LocalPlayer.Backpack:FindFirstChild(v)
        Humanoid:EquipTool(a)
    end
end
 
-- สร้าง UI ใหม่
local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/xHeptc/Kavo-UI-Library/main/source.lua"))()
local Window = Library.CreateLib("Poi", "BloodTheme")
local MainTab = Window:NewTab("Fishing")
local AutoTab = Window:NewTab("Automation")
 
-- ** Fishing Section **
local MainSection = MainTab:NewSection("เครื่องมือตกปลา")
MainSection:NewLabel("จัดการเครื่องมือและการตั้งค่าการตกปลา")
MainSection:NewButton("ติดตั้งคันเบ็ด", "ติดตั้งคันเบ็ดตกปลาโดยอัตโนมัติ", function()
    for i, v in pairs(LocalPlayer.Backpack:GetChildren()) do
        if v:IsA("Tool") and v.Name:lower():find("rod") then
            equipitem(v.Name)
        end
    end
end)
 
-- ** Automation Section **
local AutoSection = AutoTab:NewSection("ฟังก์ชันอัตโนมัติ")
AutoSection:NewToggle("AutoCast", "ตกปลาโดยอัตโนมัติ", function(v)
    _G.AutoCast = v
    pcall(function()
        while _G.AutoCast do
            task.wait(0.5)
            local Rod = Char:FindFirstChildOfClass("Tool")
            if Rod and Rod:FindFirstChild("events") and Rod.events:FindFirstChild("cast") then
                Rod.events.cast:FireServer(100, 1)
            else
                warn("ไม่พบคันเบ็ดหรือเหตุการณ์ cast หายไป!")
            end
        end
    end)
end)
 
AutoSection:NewToggle("AutoReel", "หมุนสายเบ็ดโดยอัตโนมัติ", function(v)
    _G.AutoReel = v
    pcall(function()
        while _G.AutoReel do
            task.wait(0.2)
            for _, v in pairs(LocalPlayer.PlayerGui:GetChildren()) do
                if v:IsA "ScreenGui" and v.Name == "reel" then
                    if v:FindFirstChild("bar") then
                        task.wait(0.15)
                        ReplicatedStorage.events.reelfinished:FireServer(100, true)
                    else
                        warn("UI ของ Reel ขาด bar!")
                    end
                end
            end
        end
    end)
end)
 
AutoSection:NewToggle("AutoShake", "สั่น UI โดยอัตโนมัติเมื่อมี UI ปรากฏขึ้น", function(v)
    _G.AutoShake = v
    pcall(function()
        while _G.AutoShake do
            task.wait(0.01)
            local PlayerGUI = game:GetService("Players").LocalPlayer:WaitForChild("PlayerGui")
            local shakeUI = PlayerGUI:FindFirstChild("shakeui")
            if shakeUI and shakeUI.Enabled then
                local safezone = shakeUI:FindFirstChild("safezone")
                if safezone then
                    local button = safezone:FindFirstChild("button")
                    if button and button:IsA("ImageButton") and button.Visible then
                        GuiService.SelectedObject = button
                        VirtualInputManager:SendKeyEvent(true, Enum.KeyCode.Return, false, game)
                        VirtualInputManager:SendKeyEvent(false, Enum.KeyCode.Return, false, game)
                    end
                end
            end
        end
    end)
end)
 
-- ** ฟังก์ชันการวาร์ปไปยังเกาะ **
function teleportToIsland(islandName)
    local targetIsland = game.Workspace:FindFirstChild(islandName)
 
    if targetIsland then
        -- วาร์ปไปยังตำแหน่งของเกาะ
        HumanoidRootPart.CFrame = targetIsland.CFrame
    else
        print("ไม่พบเกาะที่ระบุ: " .. islandName)
    end
end
 
-- ตัวอย่างการเรียกใช้งาน
teleportToIsland("Island1")  -- เปลี่ยนชื่อเกาะที่ต้องการไป
