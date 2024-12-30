-- Referências
local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local head = character:WaitForChild("Head")

-- Posição do "mim" (você pode alterar a posição, ou seja, onde você está)
local myPosition = Vector3.new(0, 10, 0) -- Posição onde a linha será desenhada até
-- (exemplo, você pode usar a posição do personagem ou qualquer outro lugar)

-- Função para desenhar linha entre dois pontos
local function drawLine(fromPos, toPos)
    local line = Instance.new("Part")
    line.Size = Vector3.new(0.2, 0.2, (fromPos - toPos).magnitude)
    line.Anchored = true
    line.CanCollide = false
    line.Color = Color3.fromRGB(255, 0, 0)  -- Cor vermelha
    line.Position = (fromPos + toPos) / 2
    line.CFrame = CFrame.new(fromPos, toPos)  -- Alinha a linha entre os dois pontos
    line.Parent = workspace
end

-- Função para criar o nome acima da cabeça do jogador
local function createNameTag(player)
    local nameTag = Instance.new("BillboardGui")
    nameTag.Adornee = player.Character.Head
    nameTag.Size = UDim2.new(0, 200, 0, 50)
    nameTag.StudsOffset = Vector3.new(0, 3, 0)  -- Ajuste a altura da tag, se necessário
    
    local textLabel = Instance.new("TextLabel")
    textLabel.Text = player.Name
    textLabel.TextColor3 = Color3.fromRGB(255, 0, 0)  -- Cor vermelha
    textLabel.TextSize = 18
    textLabel.BackgroundTransparency = 1
    textLabel.Size = UDim2.new(1, 0, 1, 0)
    textLabel.Parent = nameTag
    
    nameTag.Parent = workspace
end

-- Conectar a linha e criar o nome acima de cada jogador
for _, otherPlayer in ipairs(game.Players:GetPlayers()) do
    if otherPlayer ~= player then
        local otherCharacter = otherPlayer.Character
        if otherCharacter and otherCharacter:FindFirstChild("Head") then
            local otherHead = otherCharacter:FindFirstChild("Head")
            local otherPosition = otherHead.Position
            
            -- Desenha a linha entre o jogador e você
            drawLine(otherPosition, myPosition)
            
            -- Cria o nome do jogador
            createNameTag(otherPlayer)
        end
    end
end
