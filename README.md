# Lua-script--- Script avançado para Blox Fruits
while true do
    wait(1) -- Espera 1 segundo entre cada ação
    local player = game.Players.LocalPlayer
    local character = player.Character
    local humanoid = character:FindFirstChildOfClass("Humanoid")

    if humanoid and humanoid.Health > 0 then
        -- Auto-farm
        local closestEnemy = nil
        local closestDistance = math.huge

        for _, enemy in pairs(game.Workspace.Enemies:GetChildren()) do
            if enemy:FindFirstChildOfClass("Humanoid") and enemy.Humanoid.Health > 0 then
                local distance = (character.HumanoidRootPart.Position - enemy.HumanoidRootPart.Position).magnitude
                if distance < closestDistance then
                    closestEnemy = enemy
                    closestDistance = distance
                end
            end
        end

        if closestEnemy then
            character.HumanoidRootPart.CFrame = closestEnemy.HumanoidRootPart.CFrame
            humanoid:MoveTo(closestEnemy.HumanoidRootPart.Position)
            wait(0.5)
            humanoid:MoveTo(closestEnemy.HumanoidRootPart.Position)
        end

        -- Auto raça V2, V3, V4
        -- Adicione aqui o código específico para evoluir as raças conforme necessário

        -- Teleporte para fruta (cor azul)
        for _, fruit in pairs(game.Workspace.Fruits:GetChildren()) do
            if fruit.Color == Color3.fromRGB(0, 0, 255) then
                character.HumanoidRootPart.CFrame = fruit.CFrame
                break
            end
        end

        -- Usar códigos e girar fruta se tiver dinheiro
        local codes = {"CODE1", "CODE2", "CODE3"} -- Substitua pelos códigos reais
        for _, code in pairs(codes) do
            game:GetService("ReplicatedStorage").Remotes.RedeemCode:InvokeServer(code)
        end

        if player.Data.Beli.Value >= 250000 then -- Verifica se tem dinheiro suficiente para girar fruta
            game:GetService("ReplicatedStorage").Remotes.SpinFruit:InvokeServer()
        end

        -- Auto haki e auto haki de observação
        if not player.Data.HakiActive.Value then
            game:GetService("ReplicatedStorage").Remotes.ToggleHaki:InvokeServer()
        end

        if not player.Data.ObservationHakiActive.Value then
            game:GetService("ReplicatedStorage").Remotes.ToggleObservationHaki:InvokeServer()
        end

        -- Armazenar frutas automaticamente
        for _, fruit in pairs(player.Backpack:GetChildren()) do
            if fruit:IsA("Tool") and fruit.Name:find("Fruit") then
                game:GetService("ReplicatedStorage").Remotes.StoreFruit:InvokeServer(fruit)
            end
        end
    end
end
