-- Script para voo simples no Brookhaven

local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local humanoid = character:WaitForChild("Humanoid")

-- Função para ativar voo
local flying = false
local bodyGyro, bodyVelocity

local function startFlying()
    if flying then return end
    flying = true
    
    -- Criar os objetos para voo
    bodyGyro = Instance.new("BodyGyro")
    bodyGyro.MaxTorque = Vector3.new(400000, 400000, 400000)
    bodyGyro.CFrame = character.HumanoidRootPart.CFrame
    bodyGyro.Parent = character.HumanoidRootPart
    
    bodyVelocity = Instance.new("BodyVelocity")
    bodyVelocity.MaxForce = Vector3.new(400000, 400000, 400000)
    bodyVelocity.Velocity = Vector3.new(0, 50, 0)
    bodyVelocity.Parent = character.HumanoidRootPart
    
    -- Loop de controle do voo
    game:GetService("RunService").Heartbeat:Connect(function()
        if flying then
            bodyGyro.CFrame = character.HumanoidRootPart.CFrame
            bodyVelocity.Velocity = Vector3.new(0, 50, 0) -- Ajuste da velocidade do voo
        end
    end)
end

-- Função para parar o voo
local function stopFlying()
    if not flying then return end
    flying = false
    if bodyGyro then bodyGyro:Destroy() end
    if bodyVelocity then bodyVelocity:Destroy() end
end

-- Tecla para alternar o voo
local UIS = game:GetService("UserInputService")
UIS.InputBegan:Connect(function(input)
    if input.KeyCode == Enum.KeyCode.Space then
        if flying then
            stopFlying()
        else
            startFlying()
        end
    end
end)
