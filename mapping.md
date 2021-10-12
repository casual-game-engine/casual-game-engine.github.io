## Mapping

Maps are configuration files that describe the placement of entities and objects in the world. 
Right now there is not map editor yet, so you have to edit the map files manually. A map editor
is planned for future.

Maps are located in the games maps directory, e.g. /packages/game/maps. A map should contain a goal entity
which the player must reach in order to finish the map. A goal entity can link to another following map or
mark the game as finished.

There are various configuration commands available for maps.
1. map_name [ name ] : Describes the name of the map
2. map_background [ image file ] : Sets the background image file of the map. Will look either in the games gfx or in .common gfx folder
3. ent_require [ entity script ] : Will include an entity script file which can then be used by other commands to use them
3. env_solidsprite [ file ] [ x ] [ y ] [ width ] [ height ] [ rotation ] [ repeat ] [ dir ] [ is_wall ]: Creates a solid sprite in the world
    file: The sprite image file to be used, looks either in the games gfx or .common gfx folder
    x: The X coordinate in the world
    y: The Y coordinate in the world
    width: Used width in the world
    height: Used height in the world
    rotation: Rotation of the sprite
    repeat: How often the sprite shall be placed from the start position
    dir: 0 = repeat horizontally, 1 = repeat vertically
    is_wall: If set to true then the sprite will be a wall. Collidable entities will not pass walls. If you don't want it to be a wall then set it to false
4. env_goal [ x ] [ y ] [ next_map ] : Creates the goal entity which the player must reach to finish the map
    x: The X coordinate in the world
    y: The Y coordinate in the world
    next_map: Specify the name of the next map to be loaded after this one. Use #finished if this is the last map
5. ent_spawn [ entity ] [ x ] [ y ] [ rotation ] [ properties ] : Spawn an entity in the world
    entity: Name of the entity script that manages the entity. Must be included with ent_require before
    x: The X coordinate in the world
    y: The Y coordinate in the world
    rotation: The rotation of the entity
    properties: A list of key-value tokens which can be handled by the entity entrypoint function 

The following is an example map file:
```
map_name "A test map"
map_background "grass.png"

ent_require "weapon_laser"
ent_require "weapon_gun"
ent_require "weapon_grenade"
ent_require "weapon_bolt"
ent_require "decal"
ent_require "blooddecal"
ent_require "explosion"
ent_require "item_health"
ent_require "item_ammo_rifle"
ent_require "item_ammo_shotgun"
ent_require "item_ammo_grenade"
ent_require "item_coin"
ent_require "headcrab"
ent_require "tank"
ent_require "teslatower"
ent_require "world_wavepoint"
ent_require "world_wavemonitor"

env_solidsprite "floor.png" 64 32 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 96 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 160 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 222 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 260 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 308 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 356 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 404 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 452 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 500 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 548 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 596 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 644 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 692 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 740 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 786 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 836 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 900 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 964 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 644 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 692 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1028 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1092 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1156 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1220 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1284 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1348 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1412 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1476 64 64 0.0 20 0 false
env_solidsprite "floor.png" 64 1520 64 64 0.0 20 0 false
env_solidsprite "wall.png" 32 -13 32 32 0.0 40 0 true
env_solidsprite "wall.png" 16 0 40 32 0.0 49 1 true
env_solidsprite "wall.png" 1295 0 40 32 0.0 49 1 true
env_solidsprite "wall.png" 32 1561 32 32 0.0 40 0 true
env_solidsprite "bricks.png" 300 300 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 268 300 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 460 300 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 300 429 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 300 750 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 268 750 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 460 750 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 300 879 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 300 1250 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 268 1250 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 460 1250 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 300 1379 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 900 300 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 868 300 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 1060 300 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 900 429 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 900 750 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 868 750 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 1060 750 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 900 879 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 900 1250 32 32 0.0 5 0 true
env_solidsprite "bricks.png" 868 1250 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 1060 1250 32 32 0.0 5 1 true
env_solidsprite "bricks.png" 900 1379 32 32 0.0 5 0 true

env_goal 1230 1500 "map2"

ent_spawn "player" 79 50 5.1
ent_spawn "plasma_ball" 312 580 0.0
ent_spawn "plasma_ball" 412 580 0.0
ent_spawn "plasma_ball" 912 1100 0.0
ent_spawn "plasma_ball" 990 1100 0.0
ent_spawn "item_ammo_grenade" 312 580 0.0
ent_spawn "item_ammo_rifle" 412 580 0.0
ent_spawn "item_ammo_grenade" 912 1100 0.0
ent_spawn "item_ammo_shotgun" 990 1100 0.0
ent_spawn "item_coin" 912 580 0.0
ent_spawn "item_coin" 965 580 0.0
ent_spawn "item_coin" 1020 580 0.0
ent_spawn "item_coin" 312 1080 0.0
ent_spawn "item_coin" 365 1080 0.0
ent_spawn "item_coin" 401 1080 0.0
ent_spawn "item_health" 630 815 0.0
ent_spawn "item_health" 690 815 0.0
ent_spawn "world_wavepoint" 550 1000 0.0 "target:headcrab;entcount:5;wavecount:5;wavedelay:20000;"
ent_spawn "world_wavepoint" 750 1200 0.0 "target:headcrab;entcount:5;wavecount:5;wavedelay:20000;"
ent_spawn "world_wavepoint" 200 1450 0.0 "target:tank;entcount:1;wavecount:5;wavedelay:30000;"
ent_spawn "world_wavepoint" 290 1450 0.0 "target:tank;entcount:1;wavecount:5;wavedelay:30000;"
ent_spawn "world_wavepoint" 1000 1450 0.0 "target:tank;entcount:1;wavecount:5;wavedelay:30000;"
ent_spawn "world_wavepoint" 1090 1450 0.0 "target:tank;entcount:1;wavecount:5;wavedelay:30000;"
ent_spawn "world_wavepoint" 1230 1400 0.0 "target:teslatower;entcount:1;wavecount:5;wavedelay:20000;"
ent_spawn "world_wavepoint" 1145 1425 0.0 "target:teslatower;entcount:1;wavecount:5;wavedelay:20000;"
ent_spawn "world_wavepoint" 1090 1500 0.0 "target:teslatower;entcount:1;wavecount:5;wavedelay:20000;"
ent_spawn "world_wavemonitor" 0 0 0.0
```

[Next chapter](localization.html)<br/>
[Back](index.html)
