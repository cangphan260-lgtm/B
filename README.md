-- LightningWarning.client.lua
-- Client: phát hiện sét và thông báo

local Workspace = game:GetService("Workspace")
local StarterGui = game:GetService("StarterGui")

local LightningNames = {
	"Lightning",
	"LightningStrike",
	"Thunder",
	"Thunderbolt",
	"Storm",
	"Strike",
	"Set",
	"SetDanh",
	"SetLightning"
}

local lightningDetected = {}

local function notify(title, text)
	pcall(function()
		StarterGui:SetCore("SendNotification", {
			Title = title,
			Text = text,
			Duration = 5
		})
	end)
end

local function isLightningObject(object)
	local name = string.lower(object.Name)

	for _, keyword in ipairs(LightningNames) do
		if string.find(name, string.lower(keyword), 1, true) then
			return true
		end
	end

	return false
end

local function detectLightning(object)
	if lightningDetected[object] then
		return
	end

	lightningDetected[object] = true

	notify(
		"⚡ CẢNH BÁO SÉT!",
		"Phát hiện dấu hiệu sét: " .. object.Name
	)

	task.delay(5, function()
		lightningDetected[object] = nil
	end)
end

-- Phát hiện sét mới xuất hiện
Workspace.DescendantAdded:Connect(function(object)
	if isLightningObject(object) then
		detectLightning(object)
	end
end)

print("[Lightning Warning] Đã bật.")
