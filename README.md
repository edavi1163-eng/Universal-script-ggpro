```lua
local player = game.Players.LocalPlayer
local camera = game.Workspace.CurrentCamera
local espEnabled = false
local espLinhaEnabled = false
local espCaixaEnabled = false
local aimbotEnabled = true
local aimkillEnabled = false
local speedEnabled = false
local speedValue = 16
local target = nil

local function onShoot()
    if target then
        local enemyHead = target.Character:FindFirstChild("Head")
        local ray = Ray.new(camera.CFrame.Position, (enemyHead.Position - camera.CFrame.Position).Unit * 1000)
        local hit, position = game.Workspace:FindPartOnRay(ray, player.Character)
        if hit and hit.Parent == target.Character then
            target.Character.Humanoid.Health = 0
        end
    end
end

local function findTarget()
    local players = game.Players:GetPlayers()
    for _, p in pairs(players) do
        if p ~= player and p.Character and p.Character:FindFirstChild("Humanoid") and p.Character.Humanoid.Health > 0 then
            target = p
            return
        end
    end
    target = nil
end

local function esp()
    while espEnabled do
        for _, p in pairs(game.Players:GetPlayers()) do
            if p ~= player and p.Character and p.Character:FindFirstChild("Humanoid") then
                local enemyHead = p.Character:FindFirstChild("Head")
                local screenPosition = camera:WorldToScreenPoint(enemyHead.Position)
                if screenPosition.Z > 0 then
                    if espLinhaEnabled then
                        local linha = Drawing.new("Line")
                        linha.From = Vector2.new(camera.ViewportSize.X / 2, camera.ViewportSize.Y / 2)
                        linha.To = Vector2.new(screenPosition.X, screenPosition.Y)
                        linha.Color = Color3.new(1, 0, 0)
                        linha.Thickness = 1
                        linha.Transparency = 0.5
                        linha.Visible = true
                        wait(0.1)
                        linha:Remove()
                    end
                    if espCaixaEnabled then
                        local espBox = Drawing.new("Square")
                        espBox.Position = Vector2.new(screenPosition.X, screenPosition.Y)
                        espBox.Size = Vector2.new(10, 10)
                        espBox.Color = Color3.new(1, 0, 0)
                        espBox.Thickness = 2
                        espBox.Transparency = 0.5
                        espBox.Visible = true
                        wait(0.1)
                        espBox:Remove()
                    end
                end
            end
        end
        wait(0.1)
    end
end

local function aimkill()
    for _, p in pairs(game.Players:GetPlayers()) do
        if p ~= player and p.Character and p.Character:FindFirstChild("Humanoid") then
            p.Character.Humanoid.Health = 0
        end
    end
end

local function speedHack()
    if speedEnabled then
        player.Character.Humanoid.WalkSpeed = speedValue
    else
        player.Character.Humanoid.WalkSpeed = 16
    end
end

local painel = Instance.new("ScreenGui")
painel.Name = "Painel de Headshot"
painel.Parent = game.CoreGui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0.2, 0, 0.7, 0)
frame.Position = UDim2.new(0.8, 0, 0.3, 0)
frame.BackgroundColor3 = Color3.new(1, 0, 0)
frame.BorderSizePixel = 1
frame.BorderColor3 = Color3.new(1, 1, 1)
frame.Parent = painel

local aimbotButton = Instance.new("TextButton")
aimbotButton.Size = UDim2.new(1, 0, 0.2, 0)
aimbotButton.Position = UDim2.new(0, 0, 0, 0)
aimbotButton.Text = "Aimbot: Ativado"
aimbotButton.BackgroundColor3 = Color3.new(0, 1, 0)
aimbotButton.Parent = frame

local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(1, 0, 0.2, 0)
espButton.Position = UDim2.new(0, 0, 0.2, 0)
espButton.Text = "ESP: Desativado"
espButton.BackgroundColor3 = Color3.new(1, 0, 0)
espButton.Parent = frame

local espLinhaButton = Instance.new("TextButton")
espLinhaButton.Size = UDim2.new(1, 0, 0.2, 0)
espLinhaButton.Position = UDim2.new(0, 0, 0.4, 0)
espLinhaButton.Text = "ESP Linha: Desativado"
espLinhaButton.BackgroundColor3 = Color3.new(1, 0, 0)
espLinhaButton.Parent = frame

local espCaixaButton = Instance.new("TextButton")
espCaixaButton.Size = UDim2.new(1, 0, 0.2, 0)
espCaixaButton.Position = UDim2.new(0, 0, 0.6, 0)
espCaixaButton.Text = "ESP Caixa: Desativado"
espCaixaButton.BackgroundColor3 = Color3.new(1, 0, 0)
espCaixaButton.Parent = frame

local aimkillButton = Instance.new("TextButton")
aimkillButton.Size = UDim2.new(1, 0, 0.2, 0)
aimkillButton.Position = UDim2.new(0, 0, 0.8, 0)
aimkillButton.Text = "Aimkill: Desativado"
aimkillButton.BackgroundColor3 = Color3.new(1, 0, 0)
aimkillButton.Parent = frame

local speedButton = Instance.new("TextButton")
speedButton.Size = UDim2.new(1, 0, 0.2, 0)
speedButton.Position = UDim2.new(0, 0, 1, 0)
speedButton.Text = "Speed: Desativado"
speedButton.BackgroundColor3 = Color3.new(1, 0, 0)
speedButton.Parent = frame

local speedTextBox = Instance.new("TextBox")
speedTextBox.Size = UDim2.new(1, 0, 0.2, 0)
speedTextBox.Position = UDim2.new(0, 0, 1.2, 0)
speedTextBox.Text = "16"
speedTextBox.Parent = frame

aimbotButton.MouseButton1Click:Connect(function()
    aimbotEnabled = not aimbotEnabled
    if aimbotEnabled then
        aimbotButton.Text = "Aimbot: Ativado"
        aimbotButton.BackgroundColor3 = Color3.new(0, 1, 0)
    else
        aimbotButton.Text = "Aimbot: Desativado"
        aimbotButton.BackgroundColor3 = Color3.new(1, 0, 0)
    end
end)

espButton.MouseButton1Click:Connect(function()
    espEnabled = not espEnabled
    if espEnabled then
        espButton.Text = "ESP: Ativado"
        espButton.BackgroundColor3 = Color3.new(0, 1, 0)
        esp()
    else
        espButton.Text = "ESP: Desativado"
        espButton.BackgroundColor3 = Color3.new(1, 0, 0)
    end
end)

espLinhaButton.MouseButton1Click:Connect(function()
    espLinhaEnabled = not espLinhaEnabled
    if espLinhaEnabled then
        espLinhaButton.Text = "ESP Linha: Ativado"
        espLinhaButton.BackgroundColor3 = Color3.new(0, 1, 0)
    else
        espLinhaButton.Text = "ESP Linha: Desativado"
        espLinhaButton.BackgroundColor3 = Color3.new(1, 0, 0)
    end
end)

espCaixaButton.MouseButton1Click:Connect(function()
    espCaixaEnabled = not espCaixaEnabled
    if espCaixaEnabled then
        espCaixaButton.Text = "ESP Caixa: Ativado"
        espCaixaButton.BackgroundColor3 = Color3.new(0, 1, 0)
    else
        espCaixaButton.Text = "ESP Caixa: Desativado"
        espCaixaButton.BackgroundColor3 = Color3.new(1, 0, 0)
    end
end)

aimkillButton.MouseButton1Click:Connect(function()
    aimkillEnabled = not aimkillEnabled
    if aimkillEnabled then
        aimkillButton.Text = "Aimkill: Ativado"
        aimkillButton.BackgroundColor3 = Color3.new(0, 1, 0)
        aimkill()
    else
        aimkillButton.Text = "Aimkill: Desativado"
        aimkillButton.BackgroundColor3 = Color3.new(1, 0, 0)
    end
end)

speedButton.MouseButton1Click:Connect(function()
    speedEnabled = not speedEnabled
    if speedEnabled then
        speedButton.Text = "Speed: Ativado"
        speedButton.BackgroundColor3 = Color3.new(0, 1, 0)
        speedValue = tonumber(speedTextBox.Text)# Universal-script-ggpro
