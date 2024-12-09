-- Configurações iniciais
local player = game.Players.LocalPlayer
local mouse = player:GetMouse()
local camera = workspace.CurrentCamera
local runService = game:GetService("RunService")
local enemies = {} -- Lista de inimigos na cena

-- Função para detectar inimigos
local function detectEnemies()
    local target
    local mouseRay = mouse.UnitRay
    local raycastParams = RaycastParams.new()
    raycastParams.FilterType = Enum.RaycastFilterType.Blacklist
    raycastParams.FilterDescendantsInstances = {player.Character}

    local raycastResult = workspace:Raycast(mouseRay.Origin, mouseRay.Direction * 500, raycastParams)
    if raycastResult then
        local hitPart = raycastResult.Instance
        if hitPart and hitPart.Parent and hitPart.Parent:FindFirstChild("Humanoid") then
            target = hitPart.Parent
        end
    end
    return target
end

-- Função para destacar inimigos
local function highlightEnemy(enemy)
    if enemy and not enemies[enemy] then
        local highlight = Instance.new("SelectionBox")
        highlight.Adornee = enemy
        highlight.Parent = enemy
        enemies[enemy] = highlight
    end
end

-- Loop para detectar e destacar inimigos
runService.RenderStepped:Connect(function()
    local target = detectEnemies()
    if target then
        highlightEnemy(target)
    end
end)

-- Função para criar a funcionalidade de tiro
local function shoot()
    local target = detectEnemies()
    if target then
        -- Lógica de tiro aqui (reduzir vida do inimigo, etc.)
        print("Inimigo atingido: " .. target.Name)
    end
end

mouse.Button1Down:Connect(shoot)
