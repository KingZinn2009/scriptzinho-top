local players = game:GetService("Players")
local runService = game:GetService("RunService")
local localPlayer = players.LocalPlayer

-- Configurações do ESP
local espColor = Color3.fromRGB(255, 0, 0) -- Vermelho
local espTransparency = 0.7
local espSize = 3

local function createESP(player)
    if player == localPlayer then return end

    local highlight = Drawing.new("Square")
    highlight.Visible = false
    highlight.Color = espColor
    highlight.Thickness = 1
    highlight.Transparency = espTransparency
    highlight.Filled = false

    local function updateESP()
        if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
            local rootPart = player.Character.HumanoidRootPart
            local vector, onScreen = game.Workspace.CurrentCamera:WorldToViewportPoint(rootPart.Position)

            if onScreen then
                highlight.Visible = true
                highlight.Position = Vector2.new(vector.X, vector.Y)
                highlight.Size = Vector2.new(espSize, espSize * 2)
            else
                highlight.Visible = false
            end
        else
            highlight.Visible = false
        end
    end

    runService.RenderStepped:Connect(updateESP)
end

for _, player in pairs(players:GetPlayers()) do
    createESP(player)
end

players.PlayerAdded:Connect(createESP)
