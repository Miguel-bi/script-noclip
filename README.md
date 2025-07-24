# script-noclip






-- Inicio do script

print ("Script Iniciado")


-- Função de saudação

function saudacao(Hello)
    print("Olá, " .. Jogador .. "! Bem-vindo ao meu Script!")
end

saudacao("Player Aleatório")
print("Script Finalizado")
end


local function CreateESP(player)
	if player == LocalPlayer or ESPTable[player] then return end

	local text = Drawing.new("Text")
	text.Size = 14
	text.Center = true
	text.Outline = true
	text.Font = 2
	text.Color = Color3.fromRGB(255, 255, 255)
	text.Visible = false

	local line = Drawing.new("Line")
	line.Thickness = 1.5
	line.Color = Color3.fromRGB(249, 214, 46)
	line.Visible = false

	ESPTable[player] = {Text = text, Line = line}
end



local function getClosestPlayer()
    local closestDistance = math.huge
    local closestPlayer = nil
    for _, player in pairs(game.Players:GetPlayers()) do
        if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local distance = (player.Character.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).magnitude
            if distance < closestDistance then
                closestDistance = distance
                closestPlayer = player
            end
        end
    end
    return closestPlayer
end

-- Travar a mira
local target = getClosestPlayer()
if target then
    game.Workspace.CurrentCamera.CFrame = CFrame.new(game.Workspace.CurrentCamera.CFrame.Position, target.Character.HumanoidRootPart.Position)
end




noclip = true
game:GetService("RunService").Stepped:Connect(function()
    if noclip then
        for _, v in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
            if part:IsA("BasePart") then
                part .CanCollide = false
            end
        end
    end
end
        
    
