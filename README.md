local Players = game:GetService("Players")
local player = Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoidRootPart = character:WaitForChild("HumanoidRootPart")

local flying = false
local speed = 50

-- Criar GUI
local gui = Instance.new("ScreenGui", player:WaitForChild("PlayerGui"))
gui.Name = "FlyMobileGUI"

local function createButton(name, position, text)
	local btn = Instance.new("TextButton")
	btn.Name = name
	btn.Size = UDim2.new(0, 80, 0, 40)
	btn.Position = position
	btn.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
	btn.TextColor3 = Color3.new(1, 1, 1)
	btn.Text = text
	btn.Parent = gui
	return btn
end

local flyToggle = createButton("FlyToggle", UDim2.new(0, 20, 0, 20), "Ativar FLY")
local upBtn = createButton("UpButton", UDim2.new(0, 20, 0, 70), "↑")
local downBtn = createButton("DownButton", UDim2.new(0, 20, 0, 120), "↓")

upBtn.Visible = false
downBtn.Visible = false

-- Corpo de controle
local bodyGyro = Instance.new("BodyGyro")
local bodyVelocity = Instance.new("BodyVelocity")

local function startFly()
	flying = true
	bodyGyro.Parent = humanoidRootPart
	bodyVelocity.Parent = humanoidRootPart
	bodyGyro.MaxTorque = Vector3.new(9e9, 9e9, 9e9)
	bodyGyro.P = 9e4
	bodyVelocity.MaxForce = Vector3.new(9e9, 9e9, 9e9)
	upBtn.Visible = true
	downBtn.Visible = true

	game:GetService("RunService").RenderStepped:Connect(function()
		if flying then
			bodyGyro.CFrame = workspace.CurrentCamera.CFrame
			bodyVelocity.Velocity = workspace.CurrentCamera.CFrame.LookVector * 0
		end
	end)
end

local function stopFly()
	flying = false
	upBtn.Visible = false
	downBtn.Visible = false
	bodyGyro:Destroy()
	bodyVelocity:Destroy()
end

-- Botões
flyToggle.MouseButton1Click:Connect(function()
	if flying then
		stopFly()
		flyToggle.Text = "Ativar FLY"
	else
		startFly()
		flyToggle.Text = "Desativar FLY"
	end
end)

upBtn.MouseButton1Click:Connect(function()
	if flying then
		bodyVelocity.Velocity = Vector3.new(0, speed, 0)
	end
end)

downBtn.MouseButton1Click:Connect(function()
	if flying then
		bodyVelocity.Velocity = Vector3.new(0, -speed, 0)
	end
end)
