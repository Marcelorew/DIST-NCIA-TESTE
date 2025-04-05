local tool = Instance.new("Tool")
tool.Name = "TeleGun"
tool.RequiresHandle = false

local function onActivate()
	local character = tool.Parent
	local player = game.Players:GetPlayerFromCharacter(character)
	if not player then return end

	local mouse = player:GetMouse()
	local origin = character:FindFirstChild("Head").Position
	local direction = (mouse.Hit.p - origin).Unit * 100

	local ray = Ray.new(origin, direction)
	local part, position = workspace:FindPartOnRay(ray, character)

	if part and part:IsDescendantOf(workspace) and not part:IsDescendantOf(character) then
		local bodyVelocity = Instance.new("BodyVelocity")
		bodyVelocity.Velocity = (character.HumanoidRootPart.Position - part.Position).Unit * 100
		bodyVelocity.MaxForce = Vector3.new(1e5, 1e5, 1e5)
		bodyVelocity.Parent = part
		game.Debris:AddItem(bodyVelocity, 0.2)

		local sound = Instance.new("Sound", character.HumanoidRootPart)
		sound.SoundId = "rbxassetid://2920959" -- som de tiro
		sound:Play()
		game.Debris:AddItem(sound, 1)
	end
end

tool.Activated:Connect(onActivate)
tool.Parent = game.StarterPack
