local Players = game:GetService("Players")
local player = Players.LocalPlayer

local function softRespawn()
    local char = player.Character
    if not char then return end
    
    local hrp = char:FindFirstChild("HumanoidRootPart")
    local humanoid = char:FindFirstChildOfClass("Humanoid")
    
    if hrp and humanoid then
        -- 1. Buscamos un SpawnLocation válido en el mapa
        local spawnPoint = nil
        for _, obj in pairs(workspace:GetDescendants()) do
            if obj:IsA("SpawnLocation") and obj.Enabled then
                spawnPoint = obj
                break
            end
        end
        
        -- 2. Si lo encontramos, nos teletransportamos ahí arriba
        if spawnPoint then
            -- Detenemos por completo la física del personaje para que no salga volando
            hrp.AssemblyLinearVelocity = Vector3.new(0, 0, 0)
            hrp.AssemblyAngularVelocity = Vector3.new(0, 0, 0)
            
            -- Teletransporte instantáneo usando PivotTo (método moderno de Roblox)
            char:PivotTo(spawnPoint.CFrame + Vector3.new(0, 4, 0))
            
            -- Intentamos curar al personaje localmente (puede ser visual según el juego)
            humanoid.Health = humanoid.MaxHealth
            
            -- Resetear estados físicos por si estabas cayendo o tropezando
            humanoid:ChangeState(Enum.HumanoidStateType.GettingUp)
            
            print("[+] Falso Respawn completado. Teletransportado al Spawn sin morir.")
        else
            warn("[-] No se encontró un SpawnLocation en el juego.")
        end
    end
end

softRespawn()
