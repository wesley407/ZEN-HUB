-- ========================= ZEN HUB GUI =========================
-- GUI: Minimalista, elegante, tons roxo/azul
local ScreenGui = Instance.new("ScreenGui")
local MainFrame = Instance.new("Frame")
local TabFrame = Instance.new("Frame")
local ContentFrame = Instance.new("Frame")
local UICorner = Instance.new("UICorner")
local UIGradient = Instance.new("UIGradient")

ScreenGui.Name = "ZEN_HUB"
ScreenGui.Parent = game.CoreGui

-- ========================= MAIN FRAME =========================
MainFrame.Name = "MainFrame"
MainFrame.Parent = ScreenGui
MainFrame.BackgroundColor3 = Color3.fromRGB(45, 45, 80)
MainFrame.Position = UDim2.new(0.3,0,0.2,0)
MainFrame.Size = UDim2.new(0,450,0,500)
UICorner.Parent = MainFrame

-- Gradient para vibe zen
UIGradient.Parent = MainFrame
UIGradient.Color = ColorSequence.new{ColorSequenceKeypoint.new(0, Color3.fromRGB(80,50,130)), ColorSequenceKeypoint.new(1, Color3.fromRGB(50,50,90))}

-- ========================= TAB FRAME =========================
TabFrame.Name = "TabFrame"
TabFrame.Parent = MainFrame
TabFrame.BackgroundColor3 = Color3.fromRGB(35,35,60)
TabFrame.Position = UDim2.new(0,0,0,0)
TabFrame.Size = UDim2.new(0,120,0,500)
UICorner:Clone().Parent = TabFrame

-- Abas
local tabs = {"Teleports","Cars","Player","Shops","Secrets"}
local buttons = {}

for i,tabName in ipairs(tabs) do
    local button = Instance.new("TextButton")
    button.Name = tabName.."Button"
    button.Parent = TabFrame
    button.Size = UDim2.new(1,0,0,50)
    button.Position = UDim2.new(0,0,0,(i-1)*50)
    button.BackgroundColor3 = Color3.fromRGB(60,40,120)
    button.Text = tabName
    button.Font = Enum.Font.GothamBold
    button.TextSize = 20
    button.TextColor3 = Color3.fromRGB(255,255,255)
    table.insert(buttons,button)
end

-- ========================= CONTENT FRAME =========================
ContentFrame.Name = "ContentFrame"
ContentFrame.Parent = MainFrame
ContentFrame.BackgroundColor3 = Color3.fromRGB(50,50,100)
ContentFrame.Position = UDim2.new(0,120,0,0)
ContentFrame.Size = UDim2.new(0,330,0,500)
UICorner:Clone().Parent = ContentFrame

-- Função para criar botões dentro do content frame
local function CreateContentButton(parent,text,callback)
    local btn = Instance.new("TextButton")
    btn.Size = UDim2.new(0,300,0,40)
    btn.Position = UDim2.new(0,15,0,#parent:GetChildren()*45)
    btn.BackgroundColor3 = Color3.fromRGB(70,50,140)
    btn.Text = text
    btn.Font = Enum.Font.GothamBold
    btn.TextSize = 18
    btn.TextColor3 = Color3.fromRGB(255,255,255)
    btn.Parent = parent
    btn.MouseButton1Click:Connect(callback)
end

-- ========================= TELEPORTS =========================
local teleports = {
    ["Casa Inicial"] = Vector3.new(69,5,120),
    ["Casa Luxo"] = Vector3.new(200,5,300),
    ["Casa Modern"] = Vector3.new(350,5,250),
    ["Casa Vermelha"] = Vector3.new(500,5,400),
    ["Casa Secreta"] = Vector3.new(750,5,100),
    ["Escola"] = Vector3.new(150,5,200),
    ["Igreja"] = Vector3.new(250,5,180),
    ["Hospital"] = Vector3.new(400,5,150),
    ["Supermercado"] = Vector3.new(100,5,400),
    ["Estação de Polícia"] = Vector3.new(600,5,200),
    ["Parque"] = Vector3.new(300,5,500),
    ["Praia"] = Vector3.new(700,5,600),
    ["Parque Secreto"] = Vector3.new(850,5,350)
}

-- ========================= CARROS =========================
local cars = {
    "CarroEsportivo","CarroSUV","CarroMoto","CarroCamper",
    "CarroFuturo","CarroPolicia","CarroAmbulancia"
}

-- ========================= LOJAS =========================
local shops = {
    ["Loja de Roupas"] = Vector3.new(600,5,500),
    ["Loja de Brinquedos"] = Vector3.new(700,5,450),
    ["Loja de Comida"] = Vector3.new(750,5,600)
}

-- ========================= SEGREDOS =========================
local secrets = {
    ["Entrada Secreta"] = Vector3.new(900,5,200),
    ["Sala Oculta"] = Vector3.new(850,5,700),
    ["Teto da Escola"] = Vector3.new(150,50,200)
}

-- ========================= PLAYER SETTINGS =========================
local PlayerSettings = {
    Speed = 16,
    JumpPower = 50
}

-- Função Teleport
local function teleportTo(pos)
    local plr = game.Players.LocalPlayer
    if plr.Character and plr.Character:FindFirstChild("HumanoidRootPart") then
        plr.Character.HumanoidRootPart.CFrame = CFrame.new(pos)
    end
end

-- Função criar aba
local function LoadTab(tabName)
    -- Limpa o content frame
    for _,v in pairs(ContentFrame:GetChildren()) do
        if v:IsA("TextButton") then v:Destroy() end
    end

    if tabName == "Teleports" then
        for name,pos in pairs(teleports) do
            CreateContentButton(ContentFrame,name,function()
                teleportTo(pos)
            end)
        end
    elseif tabName == "Cars" then
        for _,carName in pairs(cars) do
            CreateContentButton(ContentFrame,carName,function()
                local car = workspace:FindFirstChild(carName)
                if car and car.PrimaryPart then
                    teleportTo(car.PrimaryPart.Position + Vector3.new(0,5,0))
                end
            end)
        end
    elseif tabName == "Shops" then
        for name,pos in pairs(shops) do
            CreateContentButton(ContentFrame,name,function()
                teleportTo(pos)
            end)
        end
    elseif tabName == "Secrets" then
        for name,pos in pairs(secrets) do
            CreateContentButton(ContentFrame,name,function()
                teleportTo(pos)
            end)
        end
    elseif tabName == "Player" then
        -- Speed
        local btnSpeed = Instance.new("TextButton")
        btnSpeed.Size = UDim2.new(0,300,0,40)
        btnSpeed.Position = UDim2.new(0,15,0,0)
        btnSpeed.Text = "Set Speed (16-500)"
        btnSpeed.Parent = ContentFrame
        btnSpeed.MouseButton1Click:Connect(function()
            local plr = game.Players.LocalPlayer
            plr.Character.Humanoid.WalkSpeed = PlayerSettings.Speed
        end)

        -- Jump
        local btnJump = Instance.new("TextButton")
        btnJump.Size = UDim2.new(0,300,0,40)
        btnJump.Position = UDim2.new(0,15,0,50)
        btnJump.Text = "Set JumpPower (50-500)"
        btnJump.Parent = ContentFrame
        btnJump.MouseButton1Click:Connect(function()
            local plr = game.Players.LocalPlayer
            plr.Character.Humanoid.JumpPower = PlayerSettings.JumpPower
        end)

        -- Reset
        local btnReset = Instance.new("TextButton")
        btnReset.Size = UDim2.new(0,300,0,40)
        btnReset.Position = UDim2.new(0,15,0,100)
        btnReset.Text = "Reset Speed/Jump"
        btnReset.Parent = ContentFrame
        btnReset.MouseButton1Click:Connect(function()
            local plr = game.Players.LocalPlayer
            plr.Character.Humanoid.WalkSpeed = 16
            plr.Character.Humanoid.JumpPower = 50
        end)
    end
