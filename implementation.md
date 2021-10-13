## Implementation

The implementation of your game and entities is done via AngelScript coding. 
Basically the engine provides an environment which is handled as a world. A world
is defined as a map configuration file and specifies all objects/entities in the world
that are there from start. A minimum amount of entities consist of player and goal entity.
The player finishes the map by reaching the goal entity. The goal entity specifies the
next loaded map or, if the game is finished due this to be the last map, then it will mark so.
In a map there can be placed multiple types of entities that can interfact with whatever you want.
Normally you would create opponent entities, obstacles, and everything else. So, from a coding
point of view, you will just have to modify the behaviour of the player entity and create new
world entities. Actually everything is an entity, be it, for instance, a tank, an alien, a lava
pit, a weapon decal or whatever else entity you want to specify. 

<b>WARNING:</b> You need base programming knowledge in order to proceed! Please refer to the official
<a href="https://www.angelcode.com/angelscript/sdk/docs/manual/index.html">AngelScript documentation</a> in order to get to know how the base language works. 

An entity is implementing the IScriptedEntity interface, which looks like follows:
```angelscript
interface IScriptedEntity
{
	//Called when the entity gets spawned. The position in the map is passed as argument
	void OnSpawn(const Vector& in vec)

	//Called when the entity gets released
	void OnRelease()

	//Process entity stuff
	void OnProcess()

	//Entity can draw everything in default order here
	void OnDraw()

	//Draw on top
	void OnDrawOnTop()

	//Indicate whether this entity shall be removed by the game
	bool NeedsRemoval()

	//Indicate if entity can be collided
	bool IsCollidable()

	//Called when the entity recieves damage
	void OnDamage(uint32 damageValue)

	//Called for wall collisions
	void OnWallCollided()

	//Called for entity collisions
	void OnCollided(IScriptedEntity@ ref)

	//Called for recieving the model data for this entity. 
	Model& GetModel()

	//Called for recieving the current position. This is useful if the entity shall move.
	Vector& GetPosition()

	//Set position
	void SetPosition(const Vector &in vec)

	//Return the rotation.
	float GetRotation()

	//Set rotation
	void SetRotation(float fRot)

	//Return a name string here, e.g. the class name or instance name.
	string GetName()

	//This vector is used for drawing the selection box
	Vector& GetSize()

	//Return save game properties
	string GetSaveGameProperties()
}
```

So, when creating a new entity, you need to create a script for that. For instance, if you would like
to create a squid alien, then you could name it alien_squid.as and place it in the entities directory of your game package.
The script name 'alien_squid' will also be the identifier to be used when using ent_require and ent_spawn map commands.
In case you will want to spawn this entity from a map file, then you will also need to create an interface function for that:
```angelscript
class CSquidAlien : IScriptedEntity {
    //Implementation...
}

void CreateEntity(const Vector &in vecPos, float fRot, const string &in szIdent, const string &in szPath, const string &in szProps)
{
	CSquidAlien @alien = CSquidAlien();
	Ent_SpawnEntity(szIdent, @alien, vecPos);
}
```

However if you want to spawn an entity solely from other entities, you don't need to implement the interface function in that case.

There is also one special case of scripts: The player entity. A player entity must be defined in the script file called 'player.as' and
must implement the interfaces IPlayerEntity and ICollectingEntity. The first one is used for input management and scoring, where the latter
one is used for HUD management.

IPlayerEntity interface:
```angelscript
//Called for key pressing events
void OnKeyPress(int vKey, bool bDown)

//Called for mouse events
void OnMousePress(int key, bool bDown)

//Called for getting current cursor position
void OnUpdateCursor(const Vector &in pos)

//Add to player score
void AddPlayerScore(int amount)

//Used to return the current player score
int GetPlayerScore()
```

ICollectingEntity interface:
```angelscript
//Add health to entity
void AddHealth(uint health)

//Add ammo to entity
void AddAmmo(const string &in ident, uint amount)
```

A player entity must also provide the following interface functions:
```angelscript
//This function is called when a savegame is loaded. It is responsible for restoring the save game status by handling the passed properties
void RestoreState(const string &in szIdent, const string &in szValue)

//This function is called when the user presses the save key. It is responsible for saving the current game status
bool SaveGame()
```

In order to get to know how to actually implement entities, please have a look at the code provided with the templated created project.


[Next chapter](sampleentity.html)<br/>
[Back](index.html)
