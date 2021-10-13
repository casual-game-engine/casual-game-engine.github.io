## Player specific implementations

The player entity will need to handle the movement of itself according to key and mouse input.
Therefore you can use the interface functions from IPlayerEntity interface. In the template sample
it checks these with the current bindings and then sets bit flags accordingly which will then be
used for movement and invalidated after each execution frame.

You will also want to access the player HUD using the HUD_* engine functions. These are useful to display
items on screen but also to manage current ammo or collected items. 

Also as a player you will need to informa the engine about your current score which will be shown at the
end of each map.

At last the player entity script file needs to provide the methods to save current game state or load an
existing game state by implementing the SaveGame() and RestoreGameState functions.

Please see the player.as script file from the builder template in order how it is done.

[Next chapter](mapping.html)<br/>
[Back](index.html)
