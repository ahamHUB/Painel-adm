-- Painel ADM completo com key, minimizar e arrastar
local key = "pedro2025" -- sua key única
local UserInputService = game:GetService("UserInputService")

-- Criando ScreenGui
local screenGui = Instance.new("ScreenGui", game.CoreGui)
screenGui.Name = "PainelADM"

-- Frame principal
local painel = Instance.new("Frame", screenGui)
painel.Size = UDim2.new(0, 500, 0, 350)
painel.Position = UDim2.new(0.5, -250, 0.5, -175)
painel.BackgroundColor3 = Color3.fromRGB(25, 25, 25)
painel.Visible = false
painel.AnchorPoint = Vector2.new(0.5, 0.5)
Instance.new("UICorner", painel).CornerRadius = UDim.new(0, 15)

-- Título
local titulo = Instance.new("TextLabel", painel)
titulo.Size = UDim2.new(1, 0, 0, 40)
titulo.Text = "Painel Administrativo"
titulo.BackgroundTransparency = 1
titulo.TextColor3 = Color3.new(1,1,1)
titulo.Font = Enum.Font.GothamBold
titulo.TextSize = 20

-- Botão minimizar
local minimizar = Instance.new("TextButton", painel)
minimizar.Size = UDim2.new(0, 30, 0, 30)
minimizar.Position = UDim2.new(1, -35, 0, 5)
minimizar.Text = "-"
minimizar.BackgroundColor3 = Color3.fromRGB(80,80,80)
minimizar.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", minimizar).CornerRadius = UDim.new(0, 6)

-- KeyFrame
local keyFrame = Instance.new("Frame", screenGui)
keyFrame.Size = UDim2.new(0, 250, 0, 120)
keyFrame.Position = UDim2.new(0.5, -125, 0.5, -60)
keyFrame.BackgroundColor3 = Color3.fromRGB(30,30,30)
keyFrame.AnchorPoint = Vector2.new(0.5,0.5)
Instance.new("UICorner", keyFrame).CornerRadius = UDim.new(0, 12)

local keyBox = Instance.new("TextBox", keyFrame)
keyBox.Size = UDim2.new(0.8, 0, 0, 35)
keyBox.Position = UDim2.new(0.1, 0, 0.2, 0)
keyBox.PlaceholderText = "Digite sua key..."
keyBox.Text = ""
keyBox.BackgroundColor3 = Color3.fromRGB(45,45,45)
keyBox.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", keyBox).CornerRadius = UDim.new(0, 8)

local confirmar = Instance.new("TextButton", keyFrame)
confirmar.Size = UDim2.new(0.6, 0, 0, 35)
confirmar.Position = UDim2.new(0.2, 0, 0.65, 0)
confirmar.Text = "Entrar"
confirmar.BackgroundColor3 = Color3.fromRGB(0,170,0)
confirmar.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", confirmar).CornerRadius = UDim.new(0, 8)

-- Função da key
confirmar.MouseButton1Click:Connect(function()
    if keyBox.Text == key then
        keyFrame.Visible = false
        painel.Visible = true
    else
        keyBox.Text = "Key incorreta!"
        keyBox.TextColor3 = Color3.fromRGB(255,0,0)
        task.wait(1.2)
        keyBox.Text = ""
        keyBox.TextColor3 = Color3.new(1,1,1)
    end
end)

-- Função helper para criar botões
local function criarBotao(nome, posX, posY, cor, acao)
    local btn = Instance.new("TextButton", painel)
    btn.Size = UDim2.new(0, 140, 0, 40)
    btn.Position = UDim2.new(0, posX, 0, posY)
    btn.Text = nome
    btn.BackgroundColor3 = cor
    btn.TextColor3 = Color3.new(1,1,1)
    Instance.new("UICorner", btn).CornerRadius = UDim.new(0, 8)
    btn.MouseButton1Click:Connect(acao)
    return btn
end

-- Funções dos comandos
local function getTarget(nome)
    for _,plr in pairs(game.Players:GetPlayers()) do
        if string.lower(plr.Name):sub(1,#nome) == string.lower(nome) then
            return plr
        end
    end
end

-- Input para escolher player
local playerBox = Instance.new("TextBox", painel)
playerBox.Size = UDim2.new(0, 200, 0, 30)
playerBox.Position = UDim2.new(0, 20, 0, 50)
playerBox.PlaceholderText = "Nome do player..."
playerBox.Text = ""
playerBox.BackgroundColor3 = Color3.fromRGB(40,40,40)
playerBox.TextColor3 = Color3.new(1,1,1)
Instance.new("UICorner", playerBox).CornerRadius = UDim.new(0, 6)

-- Botões
criarBotao("Kill", 20, 100, Color3.fromRGB(200,50,50), function()
    local target = getTarget(playerBox.Text)
    if target and target.Character and target.Character:FindFirstChild("Humanoid") then
        target.Character.Humanoid.Health = 0
    end
end)

criarBotao("Freeze", 180, 100, Color3.fromRGB(50,50,200), function()
    local target = getTarget(playerBox.Text)
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        target.Character.HumanoidRootPart.Anchored = true
    end
end)

criarBotao("Unfreeze", 340, 100, Color3.fromRGB(50,200,50), function()
    local target = getTarget(playerBox.Text)
    if target and target.Character and target.Character:FindFirstChild("HumanoidRootPart") then
        target.Character.HumanoidRootPart.Anchored = false
    end
end)

criarBotao("Jail", 20, 160, Color3.fromRGB(150,0,150), function()
    local target = getTarget(playerBox.Text)
    if target and target.Character then
        target.Character:MoveTo(Vector3.new(0,-500,0))
    end
end)

criarBotao("UnJail", 180, 160, Color3.fromRGB(150,0,80), function()
    local target = getTarget(playerBox.Text)
    if target and target.Character then
        target.Character:MoveTo(workspace.SpawnLocation.Position)
    end
end)

criarBotao("TP", 340, 160, Color3.fromRGB(255,128,0), function()
    local target = getTarget(playerBox.Text)
    local plr = game.Players.LocalPlayer
    if target and plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
        plr.Character.HumanoidRootPart.CFrame = target.Character.HumanoidRootPart.CFrame
    end
end)

criarBotao("God", 20, 220, Color3.fromRGB(0,200,200), function()
    local plr = game.Players.LocalPlayer
    if plr.Character and plr.Character:FindFirstChild("Humanoid") then
        plr.Character.Humanoid.MaxHealth = math.huge
        plr.Character.Humanoid.Health = math.huge
    end
end)

criarBotao("UnGod", 180, 220, Color3.fromRGB(200,200,0), function()
    local plr = game.Players.LocalPlayer
    if plr.Character and plr.Character:FindFirstChild("Humanoid") then
        plr.Character.Humanoid.MaxHealth = 100
        plr.Character.Humanoid.Health = 100
    end
end)

criarBotao("Char", 340, 220, Color3.fromRGB(100,100,255), function()
    local plr = game.Players.LocalPlayer
    local target = getTarget(playerBox.Text)
    if plr.Character and target and target.Character then
        plr.Character:Destroy()
        plr.Character = target.Character:Clone()
    end
end)

-- Tecla para abrir/fechar painel
UserInputService.InputBegan:Connect(function(input, gpe)
    if not gpe and input.KeyCode == Enum.KeyCode.P then
        painel.Visible = not painel.Visible
    end
end)

------------------------------------------------
-- FUNÇÃO DE MINIMIZAR
------------------------------------------------
local conteudo = {}
for _,obj in ipairs(painel:GetChildren()) do
    if obj ~= titulo and obj ~= minimizar then
        table.insert(conteudo, obj)
    end
end

local minimizado = false
minimizar.MouseButton1Click:Connect(function()
    minimizado = not minimizado
    for _,obj in ipairs(conteudo) do
        obj.Visible = not minimizado
    end
    if minimizado then
        painel.Size = UDim2.new(0, 500, 0, 50)
        minimizar.Text = "+"
    else
        painel.Size = UDim2.new(0, 500, 0, 350)
        minimizar.Text = "-"
    end
end)

------------------------------------------------
-- FUNÇÃO DE ARRASTAR (Drag & Drop)
------------------------------------------------
local dragging
local dragInput
local dragStart
local startPos

local function update(input)
    local delta = input.Position - dragStart
    painel.Position = UDim2.new(
        startPos.X.Scale,
        startPos.X.Offset + delta.X,
        startPos.Y.Scale,
        startPos.Y.Offset + delta.Y
    )
end

painel.InputBegan:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseButton1 or input.UserInputType == Enum.UserInputType.Touch then
        dragging = true
        dragStart = input.Position
        startPos = painel.Position

        input.Changed:Connect(function()
            if input.UserInputState == Enum.UserInputState.End then
                dragging = false
            end
        end)
    end
end)

painel.InputChanged:Connect(function(input)
    if input.UserInputType == Enum.UserInputType.MouseMovement or input.UserInputType == Enum.UserInputType.Touch then
        dragInput = input
    end
end)

UserInputService.InputChanged:Connect(function(input)
    if input == dragInput and dragging then
        update(input)
    end
end)
