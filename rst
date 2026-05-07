local Players = game:GetService("Players")
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local player = Players.LocalPlayer

local function ultraInstantKillAndRespawn()
    local char = player.Character
    if not char then return end
    
    -- 1. MUERTE FÍSICA INSTANTÁNEA (Bypass de Animación)
    -- Rompemos el cuello directamente. Esto mata al personaje en 1 milisegundo.
    local head = char:FindFirstChild("Head")
    if head then
        local neck = head:FindFirstChild("Neck") or char:FindFirstChild("Neck", true)
        if neck then 
            neck:Destroy() 
        end
    end
    char:BreakJoints() -- Asegura la muerte en el servidor
    
    -- 2. BYPASS DE FÍSICA (Evita lag de caída)
    -- Mandamos el personaje al vacío absoluto para que el motor no calcule colisiones de muerte.
    pcall(function()
        char:SetPrimaryPartCFrame(CFrame.new(0, -99999, 0))
    end)
    
    -- Limpieza local inmediata
    task.wait(0.02)
    char:Destroy()
    player.Character = nil
    
    -- 3. BYPASS DE TIEMPO (Disparador de Remotos de Reaparición)
    -- Escaneamos el juego en busca de botones de "Spawn" o "Deploy" invisibles para activarlos.
    for _, v in pairs(ReplicatedStorage:GetDescendants()) do
        if v:IsA("RemoteEvent") then
            local name = v.Name:lower()
            if name:find("spawn") or name:find("respawn") or name:find("deploy") or name:find("loadchar") or name:find("play") then
                pcall(function()
                    v:FireServer()
                end)
            end
        end
    end
end

-- Ejecutar
ultraInstantKillAndRespawn()
