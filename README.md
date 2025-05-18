-- Natan DEAD REAILS - HUB para DEAD RAILS (Android compatível)

local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local LocalPlayer = Players.LocalPlayer
local Camera = workspace.CurrentCamera

-- UI Setup
local ScreenGui = Instance.new("ScreenGui", game.CoreGui)
ScreenGui.Name = "NatanDeadReails"

local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 200, 0, 300)
Frame.Position = UDim2.new(0, 20, 0.5, -150)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.BorderSizePixel = 0
Frame.Active = true
Frame.Draggable = true

local function createButton(name, posY, callback)
	local button = Instance.new("TextButton", Frame)
	button.Size = UDim2.new(1, -20, 0, 30)
	button.Position = UDim2.new(0, 10, 0, posY)
	button.Text = name
	button.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
	button.TextColor3 = Color3.new(1, 1, 1)
	button.MouseButton1Click:Connect(callback)
	return button
end

-- Primeira Pessoa
local inFirstPerson = false
createButton("Primeira Pessoa", 10, function()
	inFirstPerson = not inFirstPerson
	if inFirstPerson then
		LocalPlayer.CameraMode = Enum.CameraMode.LockFirstPerson
	else
		LocalPlayer.CameraMode = Enum.CameraMode.Classic
	end
end)

-- Noclip
local noclip = false
createButton("Noclip", 50, function()
	noclip = not noclip
end)

RunService.Stepped:Connect(function()
	if noclip and LocalPlayer.Character then
		for _, v in pairs(LocalPlayer.Character:GetDescendants()) do
			if v:IsA("BasePart") and v.CanCollide == true then
				v.CanCollide = false
			end
		end
	end
end)

-- Kill Aura
local killAura = false
createButton("Kill Aura", 90, function()
	killAura = not killAura
end)

RunService.RenderStepped:Connect(function()
	if killAura and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
		for _, player in pairs(Players:GetPlayers()) do
			if player ~= LocalPlayer and player.Character and player.Character:FindFirstChild("Humanoid") and player.Character:FindFirstChild("HumanoidRootPart") then
				local distance = (player.Character.HumanoidRootPart.Position - LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
				if distance < 10 then
					player.Character.Humanoid:TakeDamage(5)
				end
			end
		end
	end
end)

-- ESP
local espEnabled = false
createButton("ESP", 130, function()
	espEnabled = not espEnabled
	for _, player in pairs(Players:GetPlayers()) do
		if player ~= LocalPlayer then
			if player.Character and player.Character:FindFirstChild("Head") then
				if espEnabled then
					local billboard = Instance.new("BillboardGui", player.Character.Head)
					billboard.Name = "ESP"
					billboard.Size = UDim2.new(0, 100, 0, 20)
					billboard.AlwaysOnTop = true
					local label = Instance.new("TextLabel", billboard)
					label.Size = UDim2.new(1, 0, 1, 0)
					label.Text = player.Name
					label.TextColor3 = Color3.new(1, 0, 0)
					label.BackgroundTransparency = 1
				else
					if player.Character.Head:FindFirstChild("ESP") then
						player.Character.Head.ESP:Destroy()
					end
				end
			end
		end
	end
end)

-- Auto Farm
local autoFarm = false
createButton("Auto Farm", 170, function()
	autoFarm = not autoFarm
end)

RunService.RenderStepped:Connect(function()
	if autoFarm and LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
		for _, obj in pairs(workspace:GetDescendants()) do
			if obj:IsA("Model") and obj:FindFirstChild("Humanoid") and obj ~= LocalPlayer.Character then
				local enemyRoot = obj:FindFirstChild("HumanoidRootPart")
				if enemyRoot then
					LocalPlayer.Character.HumanoidRootPart.CFrame = enemyRoot.CFrame + Vector3.new(0, 0, 2)
					break
				end
			end
		end
	end
end)

-- Desligar Script
createButton("Desligar", 210, function()
	ScreenGui:Destroy()
	noclip = false
	killAura = false
	autoFarm = false
	espEnabled = false
end)

-- Créditos
local credit = Instance.new("TextLabel", Frame)
credit.Size = UDim2.new(1, -20, 0, 30)
credit.Position = UDim2.new(0, 10, 1, -35)
credit.Text = "Natan DEAD REAILS"
credit.TextColor3 = Color3.new(1, 1, 1)
credit.BackgroundTransparency = 1
credit.TextScaled = true
