-- LocalScript ใน StarterGui > ScreenGui (GameUI)

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- ปุ่ม
local gui = script.Parent
local speedButton = gui:WaitForChild("SpeedButton")
local chopButton = gui:WaitForChild("ChopButton")
local lightButton = gui:WaitForChild("LightButton")

-- ========= Speed =========
local normalSpeed = 16
local boostSpeed = 32
local boosted = false

speedButton.Text = "⚡ Speed: OFF"
speedButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)

speedButton.Activated:Connect(function()
	boosted = not boosted
	if boosted then
		humanoid.WalkSpeed = boostSpeed
		speedButton.Text = "⚡ Speed: ON"
		speedButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
	else
		humanoid.WalkSpeed = normalSpeed
		speedButton.Text = "⚡ Speed: OFF"
		speedButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
	end
end)

-- ========= Auto Chop =========
local autoChop = false
local chopDistance = 6
local chopCooldown = 1

chopButton.Text = "🌳 Auto Chop: OFF"
chopButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)

chopButton.Activated:Connect(function()
	autoChop = not autoChop
	if autoChop then
		chopButton.Text = "🌳 Auto Chop: ON"
		chopButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
	else
		chopButton.Text = "🌳 Auto Chop: OFF"
		chopButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
	end
end)

-- ========= Light =========
local lightOn = false
local Lighting = game:GetService("Lighting")

lightButton.Text = "☀️ Light: OFF"
lightButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)

lightButton.Activated:Connect(function()
	lightOn = not lightOn
	if lightOn then
		Lighting.Brightness = 3
		Lighting.ClockTime = 14
		Lighting.Ambient = Color3.fromRGB(255, 255, 255)
		lightButton.Text = "☀️ Light: ON"
		lightButton.BackgroundColor3 = Color3.fromRGB(100, 255, 100)
	else
		Lighting.Brightness = 1
		Lighting.ClockTime = 0
		Lighting.Ambient = Color3.fromRGB(100, 100, 100)
		lightButton.Text = "☀️ Light: OFF"
		lightButton.BackgroundColor3 = Color3.fromRGB(255, 100, 100)
	end
end)

-- ========= ระบบ Auto Chop =========
task.spawn(function()
	while true do
		if autoChop and character and humanoid and humanoid.Health > 0 then
			local root = character:FindFirstChild("HumanoidRootPart")
			if root then
				for _, tree in pairs(workspace:GetChildren()) do
					if tree:IsA("Model") and tree:FindFirstChild("Trunk") then
						local dist = (root.Position - tree.Trunk.Position).Magnitude
						if dist < chopDistance then
							print("🪓 ตัดไม้: " .. tree.Name)
						end
					end
				end
			end
		end
		task.wait(chopCooldown)
	end
end)

-- ========= รีเซ็ตเมื่อ respawn =========
player.CharacterAdded:Connect(function(newChar)
	character = newChar
	humanoid = newChar:WaitForChild("Humanoid")
	humanoid.WalkSpeed = normalSpeed
end)
