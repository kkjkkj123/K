-- jp dos scripthub - mobile version with chat toggle

local player = game.Players.LocalPlayer
local garden = workspace:WaitForChild("Garden")
local fruitShop = workspace:WaitForChild("FruitShop")
local gearShop = workspace:WaitForChild("GearShop")

local scriptActive = true -- Starts active

-- Toggle via chat
player.Chatted:Connect(function(msg)
    msg = msg:lower()
    if msg == "/openjp" then
        scriptActive = true
        print("jp dos scripthub script activated!")
    elseif msg == "/closejp" then
        scriptActive = false
        print("jp dos scripthub script deactivated!")
    end
end)

function autoCollectPlants()
    for _, plant in pairs(garden:GetChildren()) do
        if plant:IsA("Part") and plant:FindFirstChild("Collectible") then
            firetouchinterest(player.Character.HumanoidRootPart, plant, 0)
            firetouchinterest(player.Character.HumanoidRootPart, plant, 1)
        end
    end
end

function autoBuyFruits()
    for _, fruit in pairs(fruitShop.Stock:GetChildren()) do
        if fruit:IsA("Part") then
            local args = {
                [1] = fruit.Name,
                [2] = 1
            }
            game.ReplicatedStorage.BuyFruit:InvokeServer(unpack(args))
        end
    end
end

function autoBuyGears()
    for _, gear in pairs(gearShop.Stock:GetChildren()) do
        if gear:IsA("Part") then
            local args = {
                [1] = gear.Name,
                [2] = 1
            }
            game.ReplicatedStorage.BuyGear:InvokeServer(unpack(args))
        end
    end
end

while true do
    if scriptActive then
        pcall(autoCollectPlants)
        pcall(autoBuyFruits)
        pcall(autoBuyGears)
    end
    wait(5)
end