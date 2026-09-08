print("🚀 Iniciando Auto Farm Premium...")

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local hrp = character:WaitForChild("HumanoidRootPart")
local humanoid = character:WaitForChild("Humanoid")

-- ===== VARIÁVEIS =====
local coletando = false
local matando = false
local coletandoOrbes = false
local minimizado = false
local diamantes = 0
local kills = 0
local orbes = 0
local espada = nil
local alvo = nil
local raio = 150
local loopColeta = nil
local loopKill = nil
local loopOrbes = nil

-- ===== SOM =====
local function playSound()
    local sound = Instance.new("Sound")
    sound.SoundId = "rbxassetid://9120387848"
    sound.Volume = 0.3
    sound.Parent = game.Workspace
    sound:Play()
    game:GetService("Debris"):AddItem(sound, 1)
end

-- ============================================
-- INTERFACE BONITA
-- ============================================

local gui = Instance.new("ScreenGui")
gui.Name = "AutoFarmPremium"
gui.Parent = player.PlayerGui
gui.ResetOnSpawn = false

local bg = Instance.new("Frame")
bg.Size = UDim2.new(1, 0, 1, 0)
bg.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
bg.BackgroundTransparency = 0.6
bg.Parent = gui

local frame = Instance.new("Frame")
frame.Size = UDim2.new(0, 380, 0, 420)
frame.Position = UDim2.new(0.5, -190, 0.5, -210)
frame.BackgroundColor3 = Color3.fromRGB(20, 20, 45)
frame.BackgroundTransparency = 0.05
frame.BorderSizePixel = 0
frame.Parent = gui
frame.Draggable = true
frame.Active = true

local shadow = Instance.new("Frame")
shadow.Size = UDim2.new(1, 20, 1, 20)
shadow.Position = UDim2.new(0, -10, 0, -10)
shadow.BackgroundColor3 = Color3.fromRGB(0, 0, 0)
shadow.BackgroundTransparency = 0.7
shadow.BorderSizePixel = 0
shadow.Parent = frame

local shadowCorner = Instance.new("UICorner")
shadowCorner.CornerRadius = UDim.new(0, 16)
shadowCorner.Parent = shadow

local corner = Instance.new("UICorner")
corner.CornerRadius = UDim.new(0, 16)
corner.Parent = frame

local gradient = Instance.new("UIGradient")
gradient.Color = ColorSequence.new({
    ColorSequenceKeypoint.new(0, Color3.fromRGB(40, 40, 80)),
    ColorSequenceKeypoint.new(1, Color3.fromRGB(20, 20, 50))
})
gradient.Parent = frame

-- ===== CABEÇALHO =====
local header = Instance.new("Frame")
header.Size = UDim2.new(1, 0, 0, 50)
header.BackgroundColor3 = Color3.fromRGB(30, 30, 70)
header.BackgroundTransparency = 0.1
header.BorderSizePixel = 0
header.Parent = frame

local headerCorner = Instance.new("UICorner")
headerCorner.CornerRadius = UDim.new(0, 16)
headerCorner.Parent = header

local titulo = Instance.new("TextLabel")
titulo.Size = UDim2.new(1, -80, 1, 0)
titulo.Position = UDim2.new(0, 15, 0, 0)
titulo.BackgroundTransparency = 1
titulo.Text = "⚡ AUTO FARM PREMIUM ⚡"
titulo.TextColor3 = Color3.fromRGB(0, 200, 255)
titulo.TextScaled = true
titulo.Font = Enum.Font.GothamBold
titulo.TextXAlignment = Enum.TextXAlignment.Left
titulo.Parent = header

local titleGlow = Instance.new("TextLabel")
titleGlow.Size = UDim2.new(1, -80, 1, 0)
titleGlow.Position = UDim2.new(0, 16, 0, 1)
titleGlow.BackgroundTransparency = 1
titleGlow.Text = "⚡ AUTO FARM PREMIUM ⚡"
titleGlow.TextColor3 = Color3.fromRGB(0, 150, 200)
titleGlow.TextScaled = true
titleGlow.Font = Enum.Font.GothamBold
titleGlow.TextXAlignment = Enum.TextXAlignment.Left
titleGlow.TextTransparency = 0.5
titleGlow.Parent = header

local minBtn = Instance.new("TextButton")
minBtn.Size = UDim2.new(0, 32, 0, 32)
minBtn.Position = UDim2.new(1, -70, 0, 9)
minBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 120)
minBtn.BackgroundTransparency = 0.2
minBtn.Text = "➖"
minBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
minBtn.TextSize = 18
minBtn.Font = Enum.Font.GothamBold
minBtn.BorderSizePixel = 0
minBtn.Parent = header

local minCorner = Instance.new("UICorner")
minCorner.CornerRadius = UDim.new(0, 8)
minCorner.Parent = minBtn

local closeBtn = Instance.new("TextButton")
closeBtn.Size = UDim2.new(0, 32, 0, 32)
closeBtn.Position = UDim2.new(1, -34, 0, 9)
closeBtn.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
closeBtn.BackgroundTransparency = 0.2
closeBtn.Text = "✕"
closeBtn.TextColor3 = Color3.fromRGB(255, 255, 255)
closeBtn.TextSize = 16
closeBtn.Font = Enum.Font.GothamBold
closeBtn.BorderSizePixel = 0
closeBtn.Parent = header

local closeCorner = Instance.new("UICorner")
closeCorner.CornerRadius = UDim.new(0, 8)
closeCorner.Parent = closeBtn

-- ===== CONTEÚDO =====
local content = Instance.new("Frame")
content.Size = UDim2.new(1, -30, 1, -60)
content.Position = UDim2.new(0, 15, 0, 55)
content.BackgroundTransparency = 1
content.Parent = frame

-- ===== STATUS =====
local statusFrame = Instance.new("Frame")
statusFrame.Size = UDim2.new(1, 0, 0, 30)
statusFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 60)
statusFrame.BackgroundTransparency = 0.3
statusFrame.BorderSizePixel = 0
statusFrame.Parent = content

local statusCorner = Instance.new("UICorner")
statusCorner.CornerRadius = UDim.new(0, 8)
statusCorner.Parent = statusFrame

local statusIcon = Instance.new("TextLabel")
statusIcon.Size = UDim2.new(0, 30, 1, 0)
statusIcon.BackgroundTransparency = 1
statusIcon.Text = "❤️"
statusIcon.TextColor3 = Color3.fromRGB(255, 50, 50)
statusIcon.TextSize = 18
statusIcon.Font = Enum.Font.GothamBold
statusIcon.Parent = statusFrame

local statusText = Instance.new("TextLabel")
statusText.Size = UDim2.new(1, -40, 1, 0)
statusText.Position = UDim2.new(0, 35, 0, 0)
statusText.BackgroundTransparency = 1
statusText.Text = "🟢 VIVO"
statusText.TextColor3 = Color3.fromRGB(0, 255, 100)
statusText.TextSize = 14
statusText.Font = Enum.Font.GothamBold
statusText.TextXAlignment = Enum.TextXAlignment.Left
statusText.Parent = statusFrame

-- ===== BOTÃO COLETA =====
local btnColeta = Instance.new("TextButton")
btnColeta.Size = UDim2.new(1, 0, 0, 40)
btnColeta.Position = UDim2.new(0, 0, 0, 40)
btnColeta.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
btnColeta.BackgroundTransparency = 0.15
btnColeta.Text = "💎 COLETA DIAMANTES: DESLIGADO"
btnColeta.TextColor3 = Color3.fromRGB(255, 255, 255)
btnColeta.TextSize = 13
btnColeta.Font = Enum.Font.GothamBold
btnColeta.BorderSizePixel = 0
btnColeta.Parent = content

local btnColetaCorner = Instance.new("UICorner")
btnColetaCorner.CornerRadius = UDim.new(0, 10)
btnColetaCorner.Parent = btnColeta

local statusColeta = Instance.new("TextLabel")
statusColeta.Size = UDim2.new(1, 0, 0, 16)
statusColeta.Position = UDim2.new(0, 0, 0, 84)
statusColeta.BackgroundTransparency = 1
statusColeta.Text = "🔴 DESATIVADO"
statusColeta.TextColor3 = Color3.fromRGB(255, 100, 100)
statusColeta.TextSize = 11
statusColeta.Font = Enum.Font.Gotham
statusColeta.TextXAlignment = Enum.TextXAlignment.Center
statusColeta.Parent = content

-- ===== BOTÃO KILL =====
local btnKill = Instance.new("TextButton")
btnKill.Size = UDim2.new(1, 0, 0, 40)
btnKill.Position = UDim2.new(0, 0, 0, 108)
btnKill.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
btnKill.BackgroundTransparency = 0.15
btnKill.Text = "⚔️ AUTO KILL: DESLIGADO"
btnKill.TextColor3 = Color3.fromRGB(255, 255, 255)
btnKill.TextSize = 13
btnKill.Font = Enum.Font.GothamBold
btnKill.BorderSizePixel = 0
btnKill.Parent = content

local btnKillCorner = Instance.new("UICorner")
btnKillCorner.CornerRadius = UDim.new(0, 10)
btnKillCorner.Parent = btnKill

local statusKill = Instance.new("TextLabel")
statusKill.Size = UDim2.new(1, 0, 0, 16)
statusKill.Position = UDim2.new(0, 0, 0, 152)
statusKill.BackgroundTransparency = 1
statusKill.Text = "🔴 DESATIVADO"
statusKill.TextColor3 = Color3.fromRGB(255, 100, 100)
statusKill.TextSize = 11
statusKill.Font = Enum.Font.Gotham
statusKill.TextXAlignment = Enum.TextXAlignment.Center
statusKill.Parent = content

-- ===== BOTÃO ORBES =====
local btnOrbes = Instance.new("TextButton")
btnOrbes.Size = UDim2.new(1, 0, 0, 40)
btnOrbes.Position = UDim2.new(0, 0, 0, 176)
btnOrbes.BackgroundColor3 = Color3.fromRGB(255, 215, 0)
btnOrbes.BackgroundTransparency = 0.15
btnOrbes.Text = "⚪ COLETA ORBES: DESLIGADO"
btnOrbes.TextColor3 = Color3.fromRGB(255, 255, 255)
btnOrbes.TextSize = 13
btnOrbes.Font = Enum.Font.GothamBold
btnOrbes.BorderSizePixel = 0
btnOrbes.Parent = content

local btnOrbesCorner = Instance.new("UICorner")
btnOrbesCorner.CornerRadius = UDim.new(0, 10)
btnOrbesCorner.Parent = btnOrbes

local statusOrbes = Instance.new("TextLabel")
statusOrbes.Size = UDim2.new(1, 0, 0, 16)
statusOrbes.Position = UDim2.new(0, 0, 0, 220)
statusOrbes.BackgroundTransparency = 1
statusOrbes.Text = "🔴 DESATIVADO"
statusOrbes.TextColor3 = Color3.fromRGB(255, 100, 100)
statusOrbes.TextSize = 11
statusOrbes.Font = Enum.Font.Gotham
statusOrbes.TextXAlignment = Enum.TextXAlignment.Center
statusOrbes.Parent = content

-- ===== RAIO =====
local raioFrame = Instance.new("Frame")
raioFrame.Size = UDim2.new(1, 0, 0, 30)
raioFrame.Position = UDim2.new(0, 0, 0, 244)
raioFrame.BackgroundColor3 = Color3.fromRGB(30, 30, 60)
raioFrame.BackgroundTransparency = 0.3
raioFrame.BorderSizePixel = 0
raioFrame.Parent = content

local raioCorner = Instance.new("UICorner")
raioCorner.CornerRadius = UDim.new(0, 8)
raioCorner.Parent = raioFrame

local raioText = Instance.new("TextLabel")
raioText.Size = UDim2.new(0.4, 0, 1, 0)
raioText.Position = UDim2.new(0, 10, 0, 0)
raioText.BackgroundTransparency = 1
raioText.Text = "📏 RAIO: 150"
raioText.TextColor3 = Color3.fromRGB(200, 200, 200)
raioText.TextSize = 12
raioText.Font = Enum.Font.Gotham
raioText.TextXAlignment = Enum.TextXAlignment.Left
raioText.Parent = raioFrame

local raioMenos = Instance.new("TextButton")
raioMenos.Size = UDim2.new(0, 28, 0, 22)
raioMenos.Position = UDim2.new(1, -68, 0.5, -11)
raioMenos.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
raioMenos.BackgroundTransparency = 0.2
raioMenos.Text = "−"
raioMenos.TextColor3 = Color3.fromRGB(255, 255, 255)
raioMenos.TextSize = 16
raioMenos.Font = Enum.Font.GothamBold
raioMenos.BorderSizePixel = 0
raioMenos.Parent = raioFrame

local raioMenosCorner = Instance.new("UICorner")
raioMenosCorner.CornerRadius = UDim.new(0, 6)
raioMenosCorner.Parent = raioMenos

local raioValor = Instance.new("TextLabel")
raioValor.Size = UDim2.new(0, 30, 1, 0)
raioValor.Position = UDim2.new(1, -38, 0, 0)
raioValor.BackgroundTransparency = 1
raioValor.Text = raio
raioValor.TextColor3 = Color3.fromRGB(255, 255, 255)
raioValor.TextSize = 12
raioValor.Font = Enum.Font.GothamBold
raioValor.TextXAlignment = Enum.TextXAlignment.Center
raioValor.Parent = raioFrame

local raioMais = Instance.new("TextButton")
raioMais.Size = UDim2.new(0, 28, 0, 22)
raioMais.Position = UDim2.new(1, -34, 0.5, -11)
raioMais.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
raioMais.BackgroundTransparency = 0.2
raioMais.Text = "+"
raioMais.TextColor3 = Color3.fromRGB(255, 255, 255)
raioMais.TextSize = 16
raioMais.Font = Enum.Font.GothamBold
raioMais.BorderSizePixel = 0
raioMais.Parent = raioFrame

local raioMaisCorner = Instance.new("UICorner")
raioMaisCorner.CornerRadius = UDim.new(0, 6)
raioMaisCorner.Parent = raioMais

-- ===== CONTADORES =====
local contador = Instance.new("TextLabel")
contador.Size = UDim2.new(1, 0, 0, 20)
contador.Position = UDim2.new(0, 0, 0, 280)
contador.BackgroundTransparency = 1
contador.Text = "💎 0  |  ⚔️ 0  |  ⚪ 0"
contador.TextColor3 = Color3.fromRGB(200, 200, 200)
contador.TextSize = 13
contador.Font = Enum.Font.Gotham
contador.TextXAlignment = Enum.TextXAlignment.Center
contador.Parent = content

-- ===== DETECTOR ORBES =====
local orbesDetect = Instance.new("TextLabel")
orbesDetect.Size = UDim2.new(1, 0, 0, 16)
orbesDetect.Position = UDim2.new(0, 0, 0, 302)
orbesDetect.BackgroundTransparency = 1
orbesDetect.Text = "🔍 ORBES NO MAPA: 0"
orbesDetect.TextColor3 = Color3.fromRGB(200, 200, 200)
orbesDetect.TextSize = 11
orbesDetect.Font = Enum.Font.Gotham
orbesDetect.TextXAlignment = Enum.TextXAlignment.Center
orbesDetect.Parent = content

-- ===== ALVO =====
local alvoText = Instance.new("TextLabel")
alvoText.Size = UDim2.new(1, 0, 0, 16)
alvoText.Position = UDim2.new(0, 0, 0, 320)
alvoText.BackgroundTransparency = 1
alvoText.Text = "🎯 NENHUM ALVO"
alvoText.TextColor3 = Color3.fromRGB(200, 200, 200)
alvoText.TextSize = 11
alvoText.Font = Enum.Font.Gotham
alvoText.TextXAlignment = Enum.TextXAlignment.Center
alvoText.Parent = content

-- ============================================
-- FUNÇÕES
-- ============================================

function vivo()
    return character and character.Parent and humanoid and humanoid.Health > 0
end

function acharEspada()
    if character then
        for _, obj in pairs(character:GetChildren()) do
            if obj:IsA("Tool") then
                local nome = obj.Name:lower()
                if nome:find("sword") or nome:find("espada") or nome:find("katana") or nome:find("blade") then
                    espada = obj
                    return espada
                end
            end
        end
    end
    local backpack = player:FindFirstChild("Backpack")
    if backpack then
        for _, obj in pairs(backpack:GetChildren()) do
            if obj:IsA("Tool") then
                local nome = obj.Name:lower()
                if nome:find("sword") or nome:find("espada") or nome:find("katana") or nome:find("blade") then
                    espada = obj
                    return espada
                end
            end
        end
    end
    return nil
end

function acharAlvo()
    if not vivo() then return nil end
    local pos = hrp.Position
    local melhor = nil
    local distMax = raio + 50
    for _, jogador in pairs(game.Players:GetPlayers()) do
        if jogador ~= player then
            local c = jogador.Character
            if c and c:FindFirstChild("Humanoid") and c.Humanoid.Health > 0 then
                local hrp2 = c:FindFirstChild("HumanoidRootPart")
                if hrp2 then
                    local d = (hrp2.Position - pos).Magnitude
                    if d < distMax then
                        distMax = d
                        melhor = jogador
                    end
                end
            end
        end
    end
    return melhor
end

function contarOrbes()
    local count = 0
    local pos = hrp.Position
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            local nome = obj.Name:lower()
            if (nome:find("orb") or nome:find("esfera") or nome:find("bola")) and obj.BrickColor and obj.BrickColor.Name:lower():find("white") then
                local d = (obj.Position - pos).Magnitude
                if d < raio + 100 then
                    count = count + 1
                end
            end
        end
    end
    return count
end

function coletarDiamantes()
    if not vivo() then return end
    local pos = hrp.Position
    local count = 0
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            local nome = obj.Name:lower()
            if nome:find("diamond") or nome:find("diamante") or nome:find("gem") or nome:find("crystal") or nome:find("drop") then
                local d = (obj.Position - pos).Magnitude
                if d < raio then
                    local isDrop = not obj:FindFirstChild("Handle")
                    if isDrop then
                        pcall(function()
                            obj.CFrame = hrp.CFrame + Vector3.new(0, 1.5, 0)
                            game:GetService("Debris"):AddItem(obj, 0.1)
                            count = count + 1
                        end)
                    end
                end
            end
        end
    end
    if count > 0 then
        diamantes = diamantes + count
        contador.Text = "💎 "..diamantes.."  |  ⚔️ "..kills.."  |  ⚪ "..orbes
    end
end

function coletarOrbes()
    if not vivo() then return end
    local pos = hrp.Position
    local count = 0
    for _, obj in pairs(workspace:GetDescendants()) do
        if obj:IsA("BasePart") then
            local nome = obj.Name:lower()
            if (nome:find("orb") or nome:find("esfera") or nome:find("bola")) and obj.BrickColor and obj.BrickColor.Name:lower():find("white") then
                local d = (obj.Position - pos).Magnitude
                if d < raio then
                    pcall(function()
                        obj.CFrame = hrp.CFrame + Vector3.new(0, 1.5, 0)
                        game:GetService("Debris"):AddItem(obj, 0.1)
                        count = count + 1
                    end)
                end
            end
        end
    end
    if count > 0 then
        orbes = orbes + count
        contador.Text = "💎 "..diamantes.."  |  ⚔️ "..kills.."  |  ⚪ "..orbes
    end
    local total = contarOrbes()
    if total > 0 then
        orbesDetect.Text = "🟢 ORBES NO MAPA: "..total
        orbesDetect.TextColor3 = Color3.fromRGB(0, 255, 100)
    else
        orbesDetect.Text = "🔴 ORBES NO MAPA: 0"
        orbesDetect.TextColor3 = Color3.fromRGB(255, 100, 100)
    end
end

function loopKill()
    if not matando then return end

    if not vivo() then
        matando = false
        btnKill.Text = "⚔️ AUTO KILL: DESLIGADO"
        btnKill.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        statusKill.Text = "🔴 DESATIVADO"
        statusKill.TextColor3 = Color3.fromRGB(255, 100, 100)
        alvoText.Text = "🎯 MORTO"
        alvoText.TextColor3 = Color3.fromRGB(200, 50, 50)
        if loopKill then loopKill:Disconnect() loopKill = nil end
        return
    end

    if not espada or not espada.Parent then
        acharEspada()
        if not espada then
            statusKill.Text = "⚠️ SEM ESPADA!"
            statusKill.TextColor3 = Color3.fromRGB(255, 200, 0)
            wait(2)
            return
        end
    end

    if not alvo or not alvo.Character or alvo.Character.Humanoid.Health <= 0 then
        alvo = acharAlvo()
        if alvo then
            alvoText.Text = "🎯 "..alvo.Name
            alvoText.TextColor3 = Color3.fromRGB(255, 50, 50)
        else
            alvoText.Text = "🎯 NENHUM ALVO"
            alvoText.TextColor3 = Color3.fromRGB(200, 200, 200)
            statusKill.Text = "🔍 PROCURANDO..."
            statusKill.TextColor3 = Color3.fromRGB(255, 200, 0)
            wait(1)
            return
        end
    end

    if alvo and alvo.Character and alvo.Character.Humanoid.Health > 0 then
        local hrp2 = alvo.Character:FindFirstChild("HumanoidRootPart")
        if hrp2 then
            local posAlvo = hrp2.Position
            local dist = (posAlvo - hrp.Position).Magnitude

            local cf = hrp2.CFrame
            local costas = cf.LookVector * -1
            local posCostas = posAlvo + (costas * 3)

            if dist > 20 then
                local dir = (posCostas - hrp.Position).Unit
                hrp.CFrame = hrp.CFrame + (dir * 8)
                statusKill.Text = "🏃 CORRENDO..."
                statusKill.TextColor3 = Color3.fromRGB(255, 200, 0)
                return
            end

            if dist > 5 then
                local dirCostas = (posCostas - hrp.Position).Unit
                hrp.CFrame = hrp.CFrame + (dirCostas * 6)
                statusKill.Text = "🎯 FLANQUEANDO..."
                statusKill.TextColor3 = Color3.fromRGB(255, 200, 0)
                return
            end

            local olhar = CFrame.lookAt(hrp.Position, posAlvo)
            hrp.CFrame = CFrame.new(hrp.Position, olhar.LookVector + hrp.Position)

            pcall(function()
                espada:Activate()
                wait(0.1)
                espada:Activate()
            end)

            kills = kills + 1
            contador.Text = "💎 "..diamantes.."  |  ⚔️ "..kills.."  |  ⚪ "..orbes
            statusKill.Text = "⚔️ ATACANDO!"
            statusKill.TextColor3 = Color3.fromRGB(255, 0, 0)
            alvoText.Text = "🎯 "..alvo.Name.." 💀"
            alvoText.TextColor3 = Color3.fromRGB(255, 0, 0)
        end
    end
end

-- ============================================
-- BOTÕES COM SOM E ANIMAÇÃO
-- ============================================

local function animateButton(button)
    button.BackgroundTransparency = 0.5
    button:TweenSize(UDim2.new(1, 0, 0, 38), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.1, true)
    wait(0.1)
    button:TweenSize(UDim2.new(1, 0, 0, 40), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.1, true)
    button.BackgroundTransparency = 0.15
end

-- BOTÃO COLETA
btnColeta.MouseButton1Click:Connect(function()
    playSound()
    animateButton(btnColeta)
    
    if not vivo() then
        statusColeta.Text = "⚠️ MORTO!"
        statusColeta.TextColor3 = Color3.fromRGB(255, 200, 0)
        wait(1)
        statusColeta.Text = "🔴 DESATIVADO"
        statusColeta.TextColor3 = Color3.fromRGB(255, 100, 100)
        return
    end
    
    coletando = not coletando
    if coletando then
        btnColeta.Text = "💎 COLETA DIAMANTES: LIGADO"
        btnColeta.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
        statusColeta.Text = "🟢 COLETANDO..."
        statusColeta.TextColor3 = Color3.fromRGB(0, 255, 100)
        if loopColeta then loopColeta:Disconnect() end
        loopColeta = game:GetService("RunService").Heartbeat:Connect(function()
            if coletando and vivo() then coletarDiamantes() end
        end)
    else
        btnColeta.Text = "💎 COLETA DIAMANTES: DESLIGADO"
        btnColeta.BackgroundColor3 = Color3.fromRGB(0, 150, 255)
        statusColeta.Text = "🔴 DESATIVADO"
        statusColeta.TextColor3 = Color3.fromRGB(255, 100, 100)
        if loopColeta then loopColeta:Disconnect() loopColeta = nil end
    end
end)

-- BOTÃO KILL
btnKill.MouseButton1Click:Connect(function()
    playSound()
    animateButton(btnKill)
    
    if not vivo() then
        statusKill.Text = "⚠️ MORTO!"
        statusKill.TextColor3 = Color3.fromRGB(255, 200, 0)
        wait(1)
        statusKill.Text = "🔴 DESATIVADO"
        statusKill.TextColor3 = Color3.fromRGB(255, 100, 100)
        return
    end
    
    matando = not matando
    if matando then
        acharEspada()
        if not espada then
            statusKill.Text = "⚠️ SEM ESPADA!"
            statusKill.TextColor3 = Color3.fromRGB(255, 200, 0)
            matando = false
            wait(2)
            statusKill.Text = "🔴 DESATIVADO"
            statusKill.TextColor3 = Color3.fromRGB(255, 100, 100)
            return
        end
        btnKill.Text = "⚔️ AUTO KILL: LIGADO"
        btnKill.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        statusKill.Text = "🟢 ATIVO"
        statusKill.TextColor3 = Color3.fromRGB(0, 255, 100)
        if loopKill then loopKill:Disconnect() end
        loopKill = game:GetService("RunService").Heartbeat:Connect(function()
            if matando then loopKill() wait(0.05) end
        end)
    else
        btnKill.Text = "⚔️ AUTO KILL: DESLIGADO"
        btnKill.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        statusKill.Text = "🔴 DESATIVADO"
        statusKill.TextColor3 = Color3.fromRGB(255, 100, 100)
        alvo = nil
        alvoText.Text = "🎯 NENHUM ALVO"
        alvoText.TextColor3 = Color3.fromRGB(200, 200, 200)
        if loopKill then loopKill:Disconnect() loopKill = nil end
    end
end)

-- BOTÃO ORBES
btnOrbes.MouseButton1Click:Connect(function()
    playSound()
    animateButton(btnOrbes)
    
    if not vivo() then
        statusOrbes.Text = "⚠️ MORTO!"
        statusOrbes.TextColor3 = Color3.fromRGB(255, 200, 0)
        wait(1)
        statusOrbes.Text = "🔴 DESATIVADO"
        statusOrbes.TextColor3 = Color3.fromRGB(255, 100, 100)
        return
    end
    
    coletandoOrbes = not coletandoOrbes
    if coletandoOrbes then
        btnOrbes.Text = "⚪ COLETA ORBES: LIGADO"
        btnOrbes.BackgroundColor3 = Color3.fromRGB(50, 200, 50)
        statusOrbes.Text = "🟢 COLETANDO..."
        statusOrbes.TextColor3 = Color3.fromRGB(0, 255, 100)
        if loopOrbes then loopOrbes:Disconnect() end
        loopOrbes = game:GetService("RunService").Heartbeat:Connect(function()
            if coletandoOrbes and vivo() then coletarOrbes() end
        end)
    else
        btnOrbes.Text = "⚪ COLETA ORBES: DESLIGADO"
        btnOrbes.BackgroundColor3 = Color3.fromRGB(255, 215, 0)
        statusOrbes.Text = "🔴 DESATIVADO"
        statusOrbes.TextColor3 = Color3.fromRGB(255, 100, 100)
        if loopOrbes then loopOrbes:Disconnect() loopOrbes = nil end
    end
end)

-- RAIO
raioMais.MouseButton1Click:Connect(function()
    playSound()
    if raio < 300 then
        raio = raio + 10
        raioText.Text = "📏 RAIO: "..raio
        raioValor.Text = raio
    end
end)

raioMenos.MouseButton1Click:Connect(function()
    playSound()
    if raio > 30 then
        raio = raio - 10
        raioText.Text = "📏 RAIO: "..raio
        raioValor.Text = raio
    end
end)

-- MINIMIZAR
minBtn.MouseButton1Click:Connect(function()
    playSound()
    minimizado = not minimizado
    if minimizado then
        frame:TweenSize(UDim2.new(0, 380, 0, 50), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3, true)
        frame.Position = UDim2.new(0.5, -190, 0.5, -25)
        content.Visible = false
        minBtn.Text = "➕"
        minBtn.BackgroundColor3 = Color3.fromRGB(0, 200, 100)
    else
        frame:TweenSize(UDim2.new(0, 380, 0, 420), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3, true)
        frame.Position = UDim2.new(0.5, -190, 0.5, -210)
        content.Visible = true
        minBtn.Text = "➖"
        minBtn.BackgroundColor3 = Color3.fromRGB(60, 60, 120)
    end
end)

-- FECHAR
closeBtn.MouseButton1Click:Connect(function()
    playSound()
    closeBtn.BackgroundColor3 = Color3.fromRGB(255, 0, 0)
    frame:TweenSize(UDim2.new(0, 0, 0, 0), Enum.EasingDirection.Out, Enum.EasingStyle.Quad, 0.3, true)
    wait(0.3)
    if loopColeta then loopColeta:Disconnect() end
    if loopKill then loopKill:Disconnect() end
    if loopOrbes then loopOrbes:Disconnect() end
    gui:Destroy()
    print("✅ Script finalizado!")
end)

-- ============================================
-- RESPAWN
-- ============================================

player.CharacterAdded:Connect(function(novoChar)
    character = novoChar
    hrp = character:WaitForChild("HumanoidRootPart")
    humanoid = character:WaitForChild("Humanoid")
    wait(2)
    acharEspada()
    if matando then
        matando = false
        btnKill.Text = "⚔️ AUTO KILL: DESLIGADO"
        btnKill.BackgroundColor3 = Color3.fromRGB(200, 50, 50)
        statusKill.Text = "🔄 REVIVEU!"
        statusKill.TextColor3 = Color3.fromRGB(255, 200, 0)
        alvoText.Text = "🎯 REVIVEU"
        alvoText.TextColor3 = Color3.fromRGB(0, 255, 100)
        if loopKill then loopKill:Disconnect() loopKill = nil end
        print("🔄 Reviveu! Ative o Kill manualmente.")
    end
end)

-- ============================================
-- LOOP STATUS
-- ============================================

game:GetService("RunService").Heartbeat:Connect(function()
    if vivo() then
        statusText.Text = "🟢 VIVO"
        statusText.TextColor3 = Color3.fromRGB(0, 255, 100)
        statusIcon.Text = "❤️"
        statusIcon.TextColor3 = Color3.fromRGB(255, 50, 50)
        if not coletandoOrbes then
            local total = contarOrbes()
            if total > 0 then
                orbesDetect.Text = "🟢 ORBES NO MAPA: "..total
                orbesDetect.TextColor3 = Color3.fromRGB(0, 255, 100)
            else
                orbesDetect.Text = "🔴 ORBES NO MAPA: 0"
                orbesDetect.TextColor3 = Color3.fromRGB(255, 100, 100)
            end
        end
    else
        statusText.Text = "🔴 MORTO"
        statusText.TextColor3 = Color3.fromRGB(255, 50, 50)
        statusIcon.Text = "💀"
        statusIcon.TextColor3 = Color3.fromRGB(150, 150, 150)
    end
end)

-- ============================================
-- MENSAGEM FINAL
-- ============================================

print("✅ AUTO FARM PREMIUM CARREGADO!")
print("💎 Clique no botão AZUL = Coleta Diamantes")
print("⚔️ Clique no botão VERMELHO = Auto Kill")
print("⚪ Clique no botão AMARELO = Coleta Orbes")
print("📏 + e - = Ajustar raio")
print("➖ = Minimizar")
print("✕ = Fechar")
print("🔊 Sons ativados!")
print("✨ Interface com animações!")
