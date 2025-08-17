-- Blox Fruits GUI – Compatível com KRNL
-- Criado por Fredsxx

-- Proteção para CoreGui
local Success, CoreGui = pcall(function() return game:GetService("CoreGui") end)
if not Success then CoreGui = game.Players.LocalPlayer:WaitForChild("PlayerGui") end

-- Serviços
local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local Workspace = game:GetService("Workspace")
local LocalPlayer = Players.LocalPlayer

-- Criar GUI
local screenGui = Instance.new("ScreenGui", CoreGui)
screenGui.Name = "Fredsxx_BloxFruits_GUI"
screenGui.ResetOnSpawn = false

-- Botão flutuante
local floatBtn = Instance.new("TextButton", screenGui)
floatBtn.Size = UDim2.new(0, 100, 0, 35)
floatBtn.Position = UDim2.new(0, 10, 0, 10)
floatBtn.Text = "Abrir GUI"
floatBtn.BackgroundColor3 = Color3.fromRGB(40, 40, 40)
floatBtn.TextColor3 = Color3.new(1, 1, 1)
floatBtn.Font = Enum.Font.Gotham
floatBtn.TextSize = 14

-- Janela principal
local mainFrame = Instance.new("Frame", screenGui)
mainFrame.Size = UDim2.new(0, 310, 0, 260)
mainFrame.Position = UDim2.new(0, 10, 0, 60)
mainFrame.BackgroundColor3 = Color3.fromRGB(20, 20, 20)
mainFrame.BorderSizePixel = 0
mainFrame.Visible = false

-- Abas
local tabMain = Instance.new("TextButton", mainFrame)
tabMain.Text = "Main / Farm"
tabMain.Size = UDim2.new(0, 155, 0, 30)
tabMain.Position = UDim2.new(0, 0, 0, 0)
tabMain.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
tabMain.TextColor3 = Color3.new(1,1,1)

local tabFruits = Instance.new("TextButton", mainFrame)
tabFruits.Text = "Frutas"
tabFruits.Size = UDim2.new(0, 155, 0, 30)
tabFruits.Position = UDim2.new(0, 155, 0, 0)
tabFruits.BackgroundColor3 = Color3.fromRGB(30, 30, 30)
tabFruits.TextColor3 = Color3.new(1,1,1)

-- Páginas
local pageFarm = Instance.new("Frame", mainFrame)
pageFarm.Size = UDim2.new(1, 0, 1, -30)
pageFarm.Position = UDim2.new(0, 0, 0, 30)
pageFarm.BackgroundTransparency = 1
pageFarm.Visible = true

local pageFruits = Instance.new("Frame", mainFrame)
pageFruits.Size = pageFarm.Size
pageFruits.Position = pageFarm.Position
pageFruits.BackgroundTransparency = 1
pageFruits.Visible = false

-- Alternar páginas
tabMain.MouseButton1Click:Connect(function()
    pageFarm.Visible = true
    pageFruits.Visible = false
end)

tabFruits.MouseButton1Click:Connect(function()
    pageFarm.Visible = false
    pageFruits.Visible = true
end)

-- Alternar GUI
floatBtn.MouseButton1Click:Connect(function()
    mainFrame.Visible = not mainFrame.Visible
    floatBtn.Text = mainFrame.Visible and "Fechar GUI" or "Abrir GUI"
end)

-- Auto Farm e Auto Click
local autoFarm = false
local autoClick = false

spawn(function()
    while wait(0.5) do
        if autoFarm then
            local enemies = Workspace.Enemies:GetChildren()
            if #enemies > 0 and LocalPlayer.Character then
                local enemy = enemies[1]
                if enemy:FindFirstChild("HumanoidRootPart") then
                    LocalPlayer.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame * CFrame.new(0, 5, 0)
                    ReplicatedStorage.Remotes.Combat:FireServer(enemy)
                end
            end
        end
    end
end)

spawn(function()
    while wait(0.1) do
        if autoClick then
            ReplicatedStorage.Remotes.Combat:FireServer("Click")
        end
    end
end)

-- Botões - Main/Farm
local btnAFarm = Instance.new("TextButton", pageFarm)
btnAFarm.Text = "Auto Farm [OFF]"
btnAFarm.Size = UDim2.new(0, 280, 0, 30)
btnAFarm.Position = UDim2.new(0, 15, 0, 10)
btnAFarm.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
btnAFarm.TextColor3 = Color3.new(1,1,1)
btnAFarm.MouseButton1Click:Connect(function()
    autoFarm = not autoFarm
    btnAFarm.Text = autoFarm and "Auto Farm [ON]" or "Auto Farm [OFF]"
end)

local btnAClick = Instance.new("TextButton", pageFarm)
btnAClick.Text = "Auto Click [OFF]"
btnAClick.Size = btnAFarm.Size
btnAClick.Position = UDim2.new(0, 15, 0, 50)
btnAClick.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
btnAClick.TextColor3 = Color3.new(1,1,1)
btnAClick.MouseButton1Click:Connect(function()
    autoClick = not autoClick
    btnAClick.Text = autoClick and "Auto Click [ON]" or "Auto Click [OFF]"
end)

local btnAttack = Instance.new("TextButton", pageFarm)
btnAttack.Text = "Atacar com Espada / Fruta / Soco"
btnAttack.Size = btnAFarm.Size
btnAttack.Position = UDim2.new(0, 15, 0, 90)
btnAttack.BackgroundColor3 = Color3.fromRGB(60, 60, 60)
btnAttack.TextColor3 = Color3.new(1,1,1)
btnAttack.MouseButton1Click:Connect(function()
    local enemies = Workspace.Enemies:GetChildren()
    if #enemies > 0 then
        local enemy = enemies[1]
        if enemy:FindFirstChild("HumanoidRootPart") then
            LocalPlayer.Character.HumanoidRootPart.CFrame = enemy.HumanoidRootPart.CFrame
            ReplicatedStorage.Remotes.Combat:FireServer("Sword")
            ReplicatedStorage.Remotes.Combat:FireServer("Punch")
            ReplicatedStorage.Remotes.Com
