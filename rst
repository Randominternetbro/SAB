local Players = game:GetService("Players")
local player = Players.LocalPlayer

local function ultraFastRespawn()
    local character = player.Character
    if not character then return end

    -- 1. Eliminamos el personaje actual del mundo físico de inmediato
    -- Esto evita animaciones de muerte y espera de física
    character:SetPrimaryPartCFrame(CFrame.new(0, -500, 0)) -- Lo enviamos al vacío
    
    task.wait() -- Espera mínima de un frame

    -- 2. Bypass de estado: Forzamos al servidor a pensar que el personaje ya no existe
    character:Destroy()
    player.Character = nil

    -- 3. Solicitud de carga forzada (Solo funciona si el juego permite LoadCharacter manual 
    -- o si el exploit tiene permisos de elevación sobre el RemoteEvent de Spawn)
    -- Si el juego es estándar, esto activará el ciclo de spawn más rápido posible.
    
    -- Intentamos llamar al evento de carga nativo si está expuesto
    pcall(function()
        player:LoadCharacter()
    end)

    print("[!] Intento de reaparición ultra rápida completado.")
end

-- Ejecución
ultraFastRespawn()
