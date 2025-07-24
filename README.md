# script-noclip






-- Interface Básica
local ScreenGui = Instance.new("ScreenGui", game.Players.LocalPlayer:WaitForChild("PlayerGui"))
local Frame = Instance.new("Frame", ScreenGui)
Frame.Size = UDim2.new(0, 150, 0, 200)
Frame.Position = UDim2.new(0, 10, 0, 10)
Frame.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
Frame.Active = true
Frame.Draggable = true

local function createButton(text, posY)
    local btn = Instance.new("TextButton", Frame)
    btn.Size = UDim2.new(1, 0, 0, 40)
    btn.Position = UDim2.new(0, 0, 0, posY)
    btn.Text = text
    btn.BackgroundColor3 = Color3.fromRGB(50, 50, 50)
    btn.TextColor3 = Color3.new(1, 1, 1)
    return btn
end

-- Estados
local espOn = false
local aimbotOn = false
local noclipOn = false
local espObjects = {}

-- ESP
local function toggleESP()
    espOn = not espOn
    if not espOn then
        for _, drawing in pairs(espObjects) do
            drawing:Remove()
        end
        espObjects = {}
        return
    end

    game:GetService("RunService").RenderStepped:Connect(function()
        if not espOn then return end
        for _, player in pairs(game.Players:GetPlayers()) do
            if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                local pos, onScreen = workspace.CurrentCamera:WorldToViewportPoint(player.Character.HumanoidRootPart.Position)
                if onScreen then
                    if not espObjects[player] then
                        local esp = Drawing.new("Text")
                        espObjects[player] = esp
                        esp.Size = 14
                        esp.Color = Color3.fromRGB(255, 0, 0)
                        esp.Center = true
                        esp.Outline = true
                    end
                    local esp = espObjects[player]
                    esp.Text = player.Name
                    esp.Position = Vector2.new(pos.X, pos.Y - 25)
                    esp.Visible = true
                elseif espObjects[player] then
                    espObjects[player].Visible = false
                end
            end
        end
    end)
end

-- Aimbot
local function toggleAimbot()
    aimbotOn = not aimbotOn
    if aimbotOn then
        game:GetService("RunService").RenderStepped:Connect(function()
            if not aimbotOn then return end
            local closest = nil
            local closestDist = math.huge
            for _, player in pairs(game.Players:GetPlayers()) do
                if player ~= game.Players.LocalPlayer and player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
                    local dist = (player.Character.HumanoidRootPart.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
                    if dist < closestDist then
                        closestDist = dist
                        closest = player
                    end
                end
            end
            if closest and closest.Character then
                workspace.CurrentCamera.CFrame = CFrame.new(
                    workspace.CurrentCamera.CFrame.Position,
                    closest.Character.HumanoidRootPart.Position
                )
            end
        end)
    end
end

-- NoClip
local function toggleNoclip()
    noclipOn = not noclipOn
    game:GetService('RunService').Stepped:Connect(function()
        if noclipOn then
            for _, part in pairs(game.Players.LocalPlayer.Character:GetDescendants()) do
                if part:IsA("BasePart") then
                    part.CanCollide = false
                end
            end
        end
    end)
end

-- Botões
local espBtn = createButton("ESP", 0)
espBtn.MouseButton1Click:Connect(toggleESP)

local aimbotBtn = createButton("Aimbot", 50)
aimbotBtn.MouseButton1Click:Connect(toggleAimbot)

local noclipBtn = createButton("No Clip", 100)
noclipBtn.MouseButton1Click:Connect(toggleNoclip)

    end
end
        
    
