local Players = game:GetService("Players")
local player = Players.LocalPlayer

local function resetAndFastRespawn()
    -- Obtenemos el personaje actual
    local character = player.Character or player.CharacterAdded:Wait()
    local humanoid = character:FindFirstChildOfClass("Humanoid")

    if humanoid then
        -- 1. Forzamos el reinicio (muerte)
        humanoid.Health = 0

        -- 2. Destruimos el modelo del personaje con un ligero retraso
        -- Esto engaña a la lógica de reaparición automática de muchos juegos
        -- para que carguen el personaje inmediatamente.
        task.delay(0.15, function()
            if character and character.Parent then
                character:Destroy()
                -- Opcional: Limpiar la referencia local para acelerar la carga
                player.Character = nil 
            end
        end)
        
        print("[+] Personaje reiniciado. Bypass de reaparición rápida ejecutado.")
    else
        warn("[-] No se encontró un Humanoid válido para reiniciar.")
    end
end

-- Llamamos a la función una sola vez
resetAndFastRespawn()
