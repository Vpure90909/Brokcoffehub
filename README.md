-- Coffe Hub Script para Roblox com ESP profissional

-- Variáveis principais
local player = game.Players.LocalPlayer
local playerGui = player:WaitForChild("PlayerGui")
local screenGui = Instance.new("ScreenGui")
screenGui.Name = "CoffeHub"
screenGui.Parent = playerGui

-- Função para criar o botão de abertura/fechamento
local hubButton = Instance.new("TextButton")
hubButton.Size = UDim2.new(0, 200, 0, 50)
hubButton.Position = UDim2.new(0, 20, 0, 20)
hubButton.Text = "Abrir Coffe Hub"
hubButton.Parent = screenGui

-- Função para a interface do Coffe Hub
local hubFrame = Instance.new("Frame")
hubFrame.Size = UDim2.new(0, 300, 0, 400)
hubFrame.Position = UDim2.new(0.5, -150, 0.5, -200)
hubFrame.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
hubFrame.Visible = false
hubFrame.Parent = screenGui

-- Título do Coffe Hub
local hubTitle = Instance.new("TextLabel")
hubTitle.Size = UDim2.new(1, 0, 0, 50)
hubTitle.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
hubTitle.Text = "Coffe Hub"
hubTitle.TextColor3 = Color3.fromRGB(255, 255, 255)
hubTitle.TextScaled = true
hubTitle.Parent = hubFrame

-- Função para esconder e mostrar a interface
local function toggleHub()
    hubFrame.Visible = not hubFrame.Visible
    hubButton.Text = hubFrame.Visible and "Fechar Coffe Hub" or "Abrir Coffe Hub"
end

-- Conectar o botão de abrir/fechar
hubButton.MouseButton1Click:Connect(toggleHub)

-- Função para dar dinheiro ao jogador
local function giveMoney(amount)
    local leaderstats = player:FindFirstChild("leaderstats")
    if leaderstats then
        local money = leaderstats:FindFirstChild("Cash")
        if money then
            money.Value = money.Value + amount
        end
    end
end

-- Botão de dar dinheiro
local giveMoneyButton = Instance.new("TextButton")
giveMoneyButton.Size = UDim2.new(0, 200, 0, 50)
giveMoneyButton.Position = UDim2.new(0, 50, 0, 100)
giveMoneyButton.Text = "Dar 1000 Dinheiro"
giveMoneyButton.Parent = hubFrame

giveMoneyButton.MouseButton1Click:Connect(function()
    giveMoney(1000)
end)

-- Função para teletransportar para um lugar (exemplo: casa do jogador)
local function teleportToHouse()
    -- Exemplo de teleporte (ajuste conforme necessário)
    local houseLocation = workspace:FindFirstChild("HouseLocation") -- Troque pelo nome do local correto
    if houseLocation then
        player.Character:SetPrimaryPartCFrame(houseLocation.CFrame)
    end
end

-- Botão de teleporte
local teleportButton = Instance.new("TextButton")
teleportButton.Size = UDim2.new(0, 200, 0, 50)
teleportButton.Position = UDim2.new(0, 50, 0, 200)
teleportButton.Text = "Teletransportar para Casa"
teleportButton.Parent = hubFrame

teleportButton.MouseButton1Click:Connect(teleportToHouse)

-- Função para desenhar linhas de ESP
local function drawESP(target)
    -- Cria uma linha entre a cabeça do jogador e o alvo
    local line = Instance.new("Line")
    line.Parent = screenGui
    line.Color = Color3.fromRGB(255, 0, 0)  -- Cor da linha (vermelha)
    line.Thickness = 2  -- Espessura da linha
    line.Transparency = 0.5  -- Transparência
    line.ZIndex = 5  -- Garantir que a linha apareça na frente da interface do jogo

    -- Atualiza a linha para ficar visível sempre
    local function updateLine()
        while target and target.Parent do
            -- Pega a posição da cabeça do jogador
            local playerHeadPosition = player.Character:WaitForChild("Head").Position
            -- Pega a posição do alvo (outro jogador)
            local targetPosition = target:WaitForChild("Head").Position

            -- Calcula a posição na tela para desenhar a linha
            local screenPosPlayer = workspace.CurrentCamera:WorldToScreenPoint(playerHeadPosition)
            local screenPosTarget = workspace.CurrentCamera:WorldToScreenPoint(targetPosition)

            -- Atualiza a linha com as novas posições
            line.From = Vector2.new(screenPosPlayer.X, screenPosPlayer.Y)
            line.To = Vector2.new(screenPosTarget.X, screenPosTarget.Y)

            wait(0.1)
        end
        -- Caso o alvo não exista mais, deleta a linha
        line:Destroy()
    end

    -- Inicia a atualização da linha
    updateLine()
end

-- Função de ESP para jogadores próximos
local function enableESP()
    for _, otherPlayer in pairs(game.Players:GetPlayers()) do
        if otherPlayer ~= player and otherPlayer.Character and otherPlayer.Character:FindFirstChild("Head") then
            drawESP(otherPlayer.Character)
        end
    end
end

-- Botão de ativação de ESP
local espButton = Instance.new("TextButton")
espButton.Size = UDim2.new(0, 200, 0, 50)
espButton.Position = UDim2.new(0, 50, 0, 300)
espButton.Text = "Ativar ESP"
espButton.Parent = hubFrame

espButton.MouseButton1Click:Connect(function()
    enableESP()
end)

-- Fim do script
