# Um Multicharacter support for okokSpawnSelector

- Requires qb-apartments

qb-apartments > client > main.lua 
search for
```apartments:client:setupSpawnUI```

__Replace:__

```lua
RegisterNetEvent('apartments:client:setupSpawnUI', function(cData)
    QBCore.Functions.TriggerCallback('apartments:GetOwnedApartment', function(result)
        if result then
            TriggerEvent('qb-spawn:client:setupSpawns', cData, false, nil)
            TriggerEvent('qb-spawn:client:openUI', true)
            TriggerEvent('apartments:client:SetHomeBlip', result.type)
        else
            if Apartments.Starting then
                TriggerEvent('qb-spawn:client:setupSpawns', cData, true, Apartments.Locations)
                TriggerEvent('qb-spawn:client:openUI', true)
            else
                TriggerEvent('qb-spawn:client:setupSpawns', cData, false, nil)
                TriggerEvent('qb-spawn:client:openUI', true)
            end
        end
    end, cData.citizenid)
end)
```

__With:__

```lua
RegisterNetEvent('apartments:client:setupSpawnUI', function(cData)
    QBCore.Functions.TriggerCallback('apartments:GetOwnedApartment', function(result)
        local coords = QBCore.Functions.GetPlayerData().position
        if result then
            TriggerEvent('okokSpawnSelector:spawnMenu', false, coords)
            TriggerEvent('apartments:client:SetHomeBlip', result.type)
        else
            if Apartments.Starting then
                TriggerEvent('okokSpawnSelector:spawnMenu', true)
            else
                TriggerEvent('okokSpawnSelector:spawnMenu', false, coords)
            end
        end
    end, cData.citizenid)
end)
```
