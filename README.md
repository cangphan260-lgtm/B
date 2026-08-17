--// Lightning Detector + Toggle Menu
--// Dùng cho game Roblox của bạn

local Players = game:GetService("Players")
local Workspace = game:GetService("Workspace")
local StarterGui = game:GetService("StarterGui")

local player = Players.LocalPlayer

--==================================================
-- SETTINGS
--==================================================

local LightningEnabled = true

local LightningKeywords = {
    "lightning",
    "thunder",
    "thunderbolt",
    "lightningstrike",
    "storm",
    "strike",
    "sét",
    "set",
}

--==================================================
-- GUI
--==================================================

local gui = Instance.new("ScreenGui")
gui.Name = "LightningProtectionMenu"
gui.ResetOnSpawn = false
gui.Parent = player:WaitForChild("PlayerGui")

local frame = Instance.new("Frame")
frame.Size = UDim2.fromOffset(270, 150)
frame.Position = UDim2.new(0.5, -135, 0.5, -75)
frame.BackgroundColor3 = Color3.fromRGB(25,25,25)
frame.BorderSizePixel = 0
frame.Parent = gui

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0,12)
corner.Parent = frame

local title = Instance.new("TextLabel")
title.Size = UDim2.new(1,0,0,45)
title.BackgroundTransparency = 1
title.Text = "⚡ Lightning Protection"
title.TextColor3 = Color3.new(1,1,1)
title.TextSize = 19
title.Font = Enum.Font.GothamBold
title.Parent = frame

local toggle = Instance.new("TextButton")
toggle.Size = UDim2.new(1,-30,0,50)
toggle.Position = UDim2.fromOffset(15,55)
toggle.TextSize = 17
toggle.Font = Enum.Font.GothamBold
toggle.TextColor3 = Color3.new(1,1,1)
toggle.Parent = frame

local toggleCorner = Instance.new("UICorner")
toggleCorner.CornerRadius = UDim.new(0,10)
toggleCorner.Parent = toggle

local status = Instance.new("TextLabel")
status.Size = UDim2.new(1,-30,0,30)
status.Position = UDim2.fromOffset(15,110)
status.BackgroundTransparency = 1
status.TextSize = 13
status.Font = Enum.Font.Gotham
status.TextColor3 = Color3.fromRGB(180,180,180)
status.Parent = frame

local function updateMenu()
    if LightningEnabled then
        toggle.Text = "⚡ CHỐNG SÉT: BẬT"
        status.Text = "Đang theo dõi Workspace"
    else
        toggle.Text = "⚡ CHỐNG SÉT: TẮT"
        status.Text = "Đã tắt trình phát hiện"
    end
end

updateMenu()

toggle.MouseButton1Click:Connect(function()
    LightningEnabled = not LightningEnabled
    updateMenu()
end)

--==================================================
-- NOTIFICATION
--==================================================

local function notify(text)
    pcall(function()
        StarterGui:SetCore("SendNotification", {
            Title = "⚡ CẢNH BÁO SÉT",
            Text = text,
            Duration = 5
        })
    end)
end

--==================================================
-- DETECTOR
--==================================================

local detected = {}

local function containsLightningName(object)
    local name = string.lower(object.Name)

    for _, keyword in ipairs(LightningKeywords) do
        if string.find(name, keyword, 1, true) then
            return true
        end
    end

    return false
end

local function detect(object)
    if not LightningEnabled then
        return
    end

    if detected[object] then
        return
    end

    detected[object] = true

    notify(
        "Phát hiện dấu hiệu sét: "
        .. object.Name
    )

    task.delay(5,function()
        detected[object] = nil
    end)
end

-- Theo dõi object mới
Workspace.DescendantAdded:Connect(function(object)

    if containsLightningName(object) then
        detect(object)
    end

end)

-- Quét object đã tồn tại
for _, object in ipairs(Workspace:GetDescendants()) do

    if containsLightningName(object) then
        detect(object)
    end

end
