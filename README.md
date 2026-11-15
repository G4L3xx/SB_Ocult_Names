local Players = game:GetService("Players")

local function ocultarGui(obj)
	if not obj:IsA("BillboardGui") then
		return
	end

	local nome = obj.Name:lower()

	if nome:find("name") or nome:find("tag") or nome:find("device") then
		obj.Enabled = false

		obj:GetPropertyChangedSignal("Enabled"):Connect(function()
			if obj.Enabled then
				obj.Enabled = false
			end
		end)
	end
end

local function ocultarPersonagem(character)
	local humanoid = character:WaitForChild("Humanoid", 10)

	if humanoid then
		humanoid.DisplayDistanceType = Enum.HumanoidDisplayDistanceType.None
		humanoid.NameDisplayDistance = 0
		humanoid.HealthDisplayDistance = 0
	end

	for _, obj in ipairs(character:GetDescendants()) do
		ocultarGui(obj)
	end

	character.DescendantAdded:Connect(ocultarGui)
end

local function configurarJogador(player)
	if player.Character then
		task.spawn(ocultarPersonagem, player.Character)
	end

	player.CharacterAdded:Connect(ocultarPersonagem)
end

for _, player in ipairs(Players:GetPlayers()) do
	configurarJogador(player)
end

Players.PlayerAdded:Connect(configurarJogador)
