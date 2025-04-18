local player = game.Players.LocalPlayer
local character = player.Character or player.CharacterAdded:Wait()
local forcePower = 100 -- Ajuste a força do lançamento

local function onTouch(otherPart)
    local otherCharacter = otherPart.Parent
    local otherHumanoid = otherCharacter and otherCharacter:FindFirstChildOfClass("Humanoid")

    if otherHumanoid and otherCharacter ~= character then
        local bodyVelocity = Instance.new("BodyVelocity")
        bodyVelocity.Velocity = character.PrimaryPart.CFrame.LookVector * forcePower
        bodyVelocity.MaxForce = Vector3.new(10000, 10000, 10000)
        bodyVelocity.Parent = otherCharacter.PrimaryPart
        
        -- Remover a força após um curto período
        task.delay(0.5, function()
            bodyVelocity:Destroy()
        end)
    end
end

local function startSpin()
    local spinSpeed = 10 -- Ajuste a velocidade da rotação
    while true do
        character:SetPrimaryPartCFrame(character.PrimaryPart.CFrame * CFrame.Angles(0, math.ra(spinSpeed), 0))
        task.wait(0.05)
    end
end

-- Iniciar o giro
task.spawn(startSpin)

-- Conectar detecção de toque
character.PrimaryPart.Touched:Connect(onTouch)
