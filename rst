local Players = game:GetService("Players")
local player = Players.LocalPlayer

local function instantRespawn()
    local char = player.Character
    
    if char then
        -- 1. Destruimos tu personaje original instantáneamente sin esperas
        char:Destroy()
    end
    
    -- 2. Creamos un modelo falso (Dummy) para engañar al motor del juego
    local dummy = Instance.new("Model")
    dummy.Name = player.Name
    
    local hum = Instance.new("Humanoid")
    hum.Parent = dummy
    
    -- 3. Lo metemos al Workspace y le decimos al juego que este es tu nuevo personaje
    dummy.Parent = workspace
    player.Character = dummy
    
    -- 4. Matamos al falso instantáneamente
    hum.Health = 0
    
    -- 5. Limpiamos la basura casi al instante para forzar el LoadCharacter del servidor
    task.delay(0.05, function()
        if dummy then
            dummy:Destroy()
            player.Character = nil
        end
    end)
    
    print("[+] Respawn instantáneo forzado mediante Dummy Bypass.")
end

-- Ejecutar una vez
instantRespawn()
