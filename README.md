# snippets

# Wasabi Ambulance qb-progressbar for bandages version 1.10.8+

Go to wasabi_ambulance/game/client/client.lua
 search for useBandage

replace with

```lua
RegisterNetEvent('wasabi_ambulance:useBandage', function()
    local HasItem = lib.callback.await('wasabi_ambulance:itemCheck', 500, Config.Bandages.item)
    if not HasItem or HasItem < 1 then return end
    TriggerServerEvent('wasabi_ambulance:useBandage', cache.serverId)
    QBCore.Functions.Progressbar('use_bandage', ("Healing Wounds..."), 4000, false, true, {
        disableMovement = false,
        disableCarMovement = false,
        disableMouse = false,
        disableCombat = true,
    }, {
        animDict = 'anim@amb@business@weed@weed_inspecting_high_dry@',
        anim = 'weed_inspecting_high_base_inspector',
        flags = 49,
    }, {}, {}, function() -- Done
        StopAnimTask(ped, 'anim@amb@business@weed@weed_inspecting_high_dry@', 'weed_inspecting_high_base_inspector', 1.0)
        TriggerServerEvent('hospital:server:removeBandage')
        TriggerEvent('inventory:client:ItemBox', QBCore.Shared.Items['bandage'], 'remove')
        StopAnimTask(ped, 'anim@amb@business@weed@weed_inspecting_high_dry@', 'weed_inspecting_high_base_inspector', 1.0)
        local health = GetEntityHealth(cache.ped)
        health = (Config.Bandages.hpRegen * 2) + health
        if health > 200 then health = 200 end
        SetEntityHealth(PlayerPedId(), health + 0.0)
        if Config.Bandages.healBleed and next(PlayerInjury) then
            ClearPatientSymptoms()
        end
    end, function() -- Cancel
        StopAnimTask(ped, 'anim@amb@business@weed@weed_inspecting_high_dry@', 'weed_inspecting_high_base_inspector', 1.0)
        TriggerEvent('wasabi_bridge:notify', Strings.action_cancelled, Strings.action_cancelled_desc, 'error')
    end)
end)
```
