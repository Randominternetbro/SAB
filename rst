local Players = game:GetService("Players")
local player = Players.LocalPlayer

local function voidRespawn()
    local char = player.Character
    local hrp = char and char:FindFirstChild("HumanoidRootPart")
    
    if hrp then
        -- Desanclamos el personaje por si acaso
        hrp.Anchored = false
        
        -- Nos teletransportamos instantáneamente a una profundidad extrema (-50,000 studs)
        -- Esto activa el disparador de muerte por vacío del servidor al instante
        hrp.CFrame = CFrame.new(hrp.Position.X, -50000, hrp.Position.Z)
        
        print("[+] Enviado al vacío. Forzando reaparición por límite de mapa.")
    end
end

voidRespawn()
