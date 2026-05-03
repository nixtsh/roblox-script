forcebuy.lua
local Players = game:GetService("Players")
local TweenService = game:GetService("TweenService")
local UserInputService = game:GetService("UserInputService")
local MarketplaceService = game:GetService("MarketplaceService")

local player = Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")

if playerGui:FindFirstChild("ForceBuy") then
    playerGui.ForceBuy:Destroy()
end

local screenGui = Instance.new("ScreenGui")
screenGui.Name = "ForceBuy"
screenGui.ResetOnSpawn = false
screenGui.ZIndexBehavior = Enum.ZIndexBehavior.Sibling
screenGui.IgnoreGuiInset = true
screenGui.Parent = playerGui

local panel = Instance.new("Frame")
panel.Name = "Panel"
panel.Size = UDim2.new(0.5, 0, 0.6, 0)
panel.Position = UDim2.new(0.25, 0, 0.2, 0)
panel.BackgroundColor3 = Color3.fromRGB(25, 25, 30)
panel.BorderSizePixel = 0
panel.Parent = screenGui

local label = Instance.new("TextLabel")
label.Size = UDim2.new(1, 0, 0.1, 0)
label.Position = UDim2.new(0, 0, 0, 0)
label.BackgroundTransparency = 1
label.Text = "Interface de Compra Forçada"
label.TextColor3 = Color3.fromRGB(255, 255, 255)
label.TextSize = 16
label.Font = Enum.Font.SourceSansBold
label.Parent = panel

local idField = Instance.new("TextBox")
idField.Size = UDim2.new(0.8, 0, 0.1, 0)
idField.Position = UDim2.new(0.1, 0, 0.2, 0)
idField.BackgroundColor3 = Color3.fromRGB(40, 40, 50)
idField.Text = "Digite o ID"
idField.TextColor3 = Color3.fromRGB(255, 255, 255)
idField.TextSize = 14
idField.Font = Enum.Font.SourceSans
idField.Parent = panel

local buyButton = Instance.new("TextButton")
buyButton.Size = UDim2.new(0.8, 0, 0.1, 0)
buyButton.Position = UDim2.new(0.1, 0, 0.4, 0)
buyButton.BackgroundColor3 = Color3.fromRGB(50, 150, 50)
buyButton.Text = "Comprar Forçado"
buyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
buyButton.TextSize = 14
buyButton.Font = Enum.Font.SourceSansBold
buyButton.Parent = panel

buyButton.MouseButton1Click:Connect(function()
    local id = tonumber(idField.Text)
    if id then
        MarketplaceService:PromptProductPurchaseFinished(player.UserId, id, true)
    end
end)

local autoBuyButton = Instance.new("TextButton")
autoBuyButton.Size = UDim2.new(0.8, 0, 0.1, 0)
autoBuyButton.Position = UDim2.new(0.1, 0, 0.6, 0)
autoBuyButton.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
autoBuyButton.Text = "Comprar Automaticamente"
autoBuyButton.TextColor3 = Color3.fromRGB(255, 255, 255)
autoBuyButton.TextSize = 14
autoBuyButton.Font = Enum.Font.SourceSansBold
autoBuyButton.Parent = panel

autoBuyButton.MouseButton1Click:Connect(function()
    local id = tonumber(idField.Text)
    if id then
        local running = true
        while running do
            task.delay(0.5, function()
                if running then
                    MarketplaceService:PromptProductPurchaseFinished(player.UserId, id, true)
                end
            end)
        end
    end
end)

local stopButton = Instance.new("TextButton")
stopButton.Size = UDim2.new(0.8, 0, 0.1, 0)
stopButton.Position = UDim2.new(0.1, 0, 0.8, 0)
stopButton.BackgroundColor3 = Color3.fromRGB(50, 50, 150)
stopButton.Text = "Parar"
stopButton.TextColor3 = Color3.fromRGB(255, 255, 255)
stopButton.TextSize = 14
stopButton.Font = Enum.Font.SourceSansBold
stopButton.Parent = panel

stopButton.MouseButton1Click:Connect(function()
    autoBuyButton.Text = "Comprar Automaticamente"
    autoBuyButton.BackgroundColor3 = Color3.fromRGB(150, 50, 50)
end)

-- Botão de fechar
local closeButton = Instance.new("TextButton")
closeButton.Size = UDim2.new(0.1, 0, 0.1, 0)
closeButton.Position = UDim2.new(0.9, 0, 0, 0)
closeButton.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeButton.Text = "X"
closeButton.TextColor3 = Color3.fromRGB(255, 255, 255)
closeButton.TextSize = 14
closeButton.Font = Enum.Font.SourceSansBold
closeButton.Parent = panel

closeButton.MouseButton1Click:Connect(function()
    screenGui:Destroy()
end)
