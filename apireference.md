## API reference

## Basic data types:
* size_t: Can be used as entity iterator (64 Bit)
* HostVersion: A 16-bit value containing a version number (high and low)
* SpriteHandle: A handle to a sprite (64 Bit)
* SoundHandle: A handle to a sound (64 Bit)
* FontHandle: A handle to a font (64 Bit)
* CVarHandle: A handle to a CVar (control/config variable, 64 Bit)

## Special variables
* ${COMMON} : Used for include directives to dynamically query the common path

## Enums:
### MovementDir:
* MOVE_FORWARD: Move in forward direction
* MOVE_BACKWARD: Move backwards in direction
* MOVE_LEFT: Move left
* MOVE_RIGHT: Move right
### FileSeekWay:
* SEEKW_BEGIN: Start from the begin of a file
* SEEKW_CURRENT: Start from current file offset
* SEEKW_END: Start from the end of a file
### HudInfoMessageColor:
* HUD_MSG_COLOR_DEFAULT: Default color
* HUD_MSG_COLOR_GREEN: Green color
* HUD_MSG_COLOR_RED: Red color
* HUD_MSG_COLOR_YELLOW: Yellow color
* HUD_MSG_COLOR_BLUE: Blue color
### CVarType
* CVAR_TYPE_BOOL: Boolean CVar
* CVAR_TYPE_INT: Integer CVar
* CVAR_TYPE_FLOAT: Float CVar
* CVAR_TYPE_STRING: String CVar
	
## Callbacks:
```angelscript
//Called for every file object in list
bool FuncFileListing(const string& in);
```
	
## Classes:
### Color (Used to define colors for sprites, boxes, strings, etc.):
```angelscript
Color() //Default constructor
Color(uint8 r, uint8 g, uint8 b, uint8 a) //Construct with default values
uint8 R() //Getter for red channel
uint8 G() //Getter for green channel
uint8 B() //Getter for blue channel
uint8 A() //Getter for alpha channel
```
### ConColor (Used to define colors when printing to the console):
```angelscript
ConColor() //Default constructor
ConColor(uint8 r, uint8 g, uint8 b) //Construct with default values
uint8 R() //Getter for red channel
uint8 G() //Getter for green channel
uint8 B() //Getter for blue channel
```
### Vector (Screen positions and resolutions are defined as 2D vectors):
```angelscript
Vector() //Default constructor
Vector(Vector &in) //Construct from a different Vector
Vector(int x, int y) //Construct from x and y coordinates
Vector[0/1] //Getter and setter for a specific dimension
Vector=Vector //Assign the values of a Vector to a different Vector
Vector==Vector //Compare two Vectors of equality
Vector[0/1]++ //Increment Vector dimension value
Vector[0/1]-* //Decrement Vector dimension value
Vector+Vector //Perform addition of two Vectors
Vector-Vector //Substract a Vector from another
int GetX() const //Get X dimension value
int GetY() const //Get Y dimension value
int Distance(const Vector &in) //Calculate the distance between two Vectors
void Zero() //Fill the Vector with zero
```
### SpriteInfo (Useful for obtaining information about a sprite before loading it):
```angelscript
SpriteInfo() //Default constructor
Vector& GetResolution() //Get resolution of the sprite
int GetDepth() //Get color depth of the sprite
int GetFormat() //Get format of the sprite
```
### BoundingBox (Used to define the bounding box of a model):
```angelscript
BoundingBox() //Default constructor
BoundingBox=BoundingBox //Basically used to copy the entire data to the other Model object instance
bool Alloc() //Allocate memory needed for using the object instance
void AddBBoxItem(const Vector& in, const Vector& in) //Add a bbox item. 
	First Vector is the relative position of the item and the second Vector 
	is the dimensions of the item
bool IsCollided(const Vector& in, const Vector& in, const BoundingBox& in) //Check for collision. 
	First Vector is the absolute screen position of the owner entity, second Vector is the absolute screen
	position of the reference entity. Last parameter is the BoundingBox object instance of the reference entity
bool IsInside(const Vector &in, const Vector &in) //Check if a Vector is inside the BoundingBox.
	First argument is the absolute position of the owner entity, second argument is the position to be checked.
bool IsEmpty() //Returns true if there are no items added to the BoundingBox
void Clear() //Remove all added items
```
### TempSprite (Useful for creating non-specific temporary sprites which do only have a visual relevance):
```angelscript
TempSprite() //Default constructor
TempSprite(const string &in, uint32 dwSwitchDelay, bool bInfinite, int iFrameCount, int iFrameWidth, int iFrameHeight, int iFramesPerLine, bool bForceCustomSize)
	//Instantiate with the given data. First argument is the sprite file name relative to the directory of the package. 
	Second argument defines the delay between each frame change. Third argument defines whether this entity shall last permanently.
	iFrameCount defines the amount of frames in the sprite sheet. iFrameWidth and iFrameHeight are for the dimensions of the sprite. 
	If the sprite sheet has multiple lines then you can define the amount of frames per each line with iFramesPerLine. bForceCustomSize is used to
	determine if the sprite shall be loaded with a custom size instead of the original size
bool Initialize(const string &in, uint32 dwSwitchDelay, bool bInfinite, int iFrameCount, int iFrameWidth, int iFrameHeight, int iFramesPerLine, bool bForceCustomSize)
	//Used if not constructed with the custom initialization constructor. For the arguments see the custom constructor definition above.
void Draw(const Vector &in, float fRotation) //Draw the sprite at the given position using the given rotation
void Release() //Release the temp sprite. Use this when you don't need the sprite anymore or when the game shuts down
```
### Model (Useful for associating a model with a scripted entity. Used by the game engine to process collisions):
```angelscript
Model() //Default constructor
Model(const string &in) //Construct with providing a relative file name to the model file
Model(const string &in, bool b) //Same as above, but you can define if a custom size shall be used
Model=Model //Basically used to copy the entire data to the other Model object instance
bool Initialize(const string&in szMdlFile, bool bForceCustomSize) //Initialize the model. Same as the second custom constructor above.
bool Initialize2(const BoundingBox& in, SpriteHandle hSprite) //This is to be used with an already existing sprite file and a Bounding Box.
void Release() //Release the Model data. Use this when shutting down or not using the Model anymore
bool IsCollided(const Vector& in mypos, const Vector& in refpos, const Model& in mdl) //Check if a reference model collides with this model using the given absolute coordinates
bool IsValid() //Returns true if the model has been initialized successfully and can be used
bool Alloc() //Allocate memory for the model. Call this before Initialize()/Initialize2()
void SetCenter(const Vector &in) //This can be used to define the center of a model
SpriteHandle Handle() //Returns the handle to the loaded sprite associated with the model
const Vector& GetCenter() const //Getter for the center of the model
BoundingBox& GetBBox() //Returns the associated bounding box
```
### Timer (Useful for processing stuff after a period of time):
```angelscript
Timer() //Default constructor
Timer(uint32 delay) //Construct with a given delay value. Also sets the timer in active state
void Reset() //Resets the internal timing values. Call this when IsElapsed() returns true in order to run the timer again
void Update() //Perform internal update calculations
void SetActive(bool bStatus) //Set the activation state of the timer
void SetDelay(uint32 delay) //Set the delay of the timer
bool IsActive() //Indicate activation state
uint32 GetDelay() //Get delay value
bool IsElapsed() //Indicates whether the time has elapsed
```
### FileReader (can be used to read from files, e.g. custom configs):
```angelscript
FileReader() //Default constructor
FileReader(const string& in) //Construct object and open the given file
	for reading. Note that you can only read files relative to the directory of your package
bool Open(const string &in) //Open a file for reading. 
bool IsOpen() //Indicate whether the file has been successfully opened 
bool Eof() //Returns true if the internal file pointer has reached the end of the file
void Seek(FileSeekWay from, int offset) //Jump "offset" bytes from the seek way inside the file
string GetLine() //Get line at current file pointer
string GetEntireContent(bool bSkipNLChar = false) //Read the entire file. The boolean argument
	specifies whether no newline char shall be appended to each read line
void Close() //Closes the file
```
### FileWriter (can be used to write to files):
```angelscript
FileWriter() //Default constructor
FileWriter(const string& in) //Construct and opens a file for writing. Using the constructor it defaults to append to the file
bool Open(const string &in, bool bAppend = true)
	//Opens a file for writing. The boolean parameter can be used to define whether to append to the file 
	if it already exists. Otherwise the file is overwritten.
bool Eof() //Returns true if the internal file pointer has reached the end of the file
void Seek(FileSeekWay from, int offset) //Jump "offset" bytes from the seek way inside the file
void Write(const string &in) //Write a string to the file
void WriteLine(const string &in) //Write a line to the file
void Close() //Close the file
```
### SaveGameWriter (used to write saved game states to disk)
```angelscript
SaveGameWriter() //Default constructor
bool BeginSaveGame() //Begin writing to save game file
bool WritePackage(const string &in szPackage) //Write package name to file
bool WriteFromPath(const string &in szFromPath) //Write frompath attribute to file
bool WriteMap(const string &in szMap) //Write map to file
bool WriteAttribute(const string &in szName, const string &in szValue) //Write any data key-pair value that belongs to a state to be saved
void EndSaveGame() //Finish file writing
```
### SaveGameReader (used to read saved game states from disk)
```angelscript
SaveGameReader() //Default constructor
bool OpenSaveGameFile(const string &in szFile) //Open a saved game file
void AcquireSaveGameData() //Read all key-pair values from saved game file
string GetDataItem(const string &in szIdent) //Query a key-pair value by identifier
void Close() //Finish file reading
```

## Interfaces:
### IScriptedEntity:
* Used to implement game entities
```angelscript
//Called when the entity gets spawned. The position on the screen is passed as argument
void OnSpawn(const Vector& in vec)
//Called when the entity gets released
void OnRelease()
//Process entity stuff
void OnProcess()
//Entity can draw everything in default order here
void OnDraw()
//Entity can draw on-top stuff here
void OnDrawOnTop()
//Called when collided with a wall
void OnWallCollided()
//Indicate whether this entity is collidable
bool IsCollidable()
//Called when the entity is collided with another entity
void OnCollided(IScriptedEntity@)
//Return the associated model of this entity
Model& GetModel()
//Return the position of the entity
Vector& GetPosition()
//Set new position of the entity
void SetPosition(const Vector &in)
//Get the size of the entity
Vector& GetSize()
//Get entity rotation
float GetRotation()
//Set new entity rotation
void SetRotation(float)
//Called when the entity shall get damaged
void OnDamage(uint32)
//Indicate whether this entity shall be removed by the game
bool NeedsRemoval()
//Return a name string here, e.g. the class name or instance name. Used to distinguish the entity class from others
string GetName()
//Return a string that contains all save game properties for this entity
string GetSaveGameProperties()
```
### IPlayerEntity:
* Used to implement player specific behaviors
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

### ICollectingEntity
* Used to implement methods for entities that can collect things
```angelscript
//Add health to entity
void AddHealth(uint health)
//Add ammo to entity
void AddAmmo(const string &in ident, uint amount)
```

## Script callbacks
```angelscript
//Used to spawn the entity from within a map definition file. The entity needs to get spawned there via Ent_SpawnEntity
void CreateEntity(const Vector &in vecPos, float fRot, const string &in szIdent, const string &in szPath, const string &in szProps)
//Used to restore a game state. This is used in the context of loading a saved game
void RestoreState(const string &in szIdent, const string &in szValue)
//Used to save the current game state to disk
bool SaveGame()
```

## Available API functions:
```angelscript
//Return current package name
string GetPackageName()
//Return current map name
string GetCurrentMap()
//Return the package path of the current loaded game or mod
string GetPackagePath()
//Return path to the common directory where to include common assets and scripts
string GetCommonPath()
//Print a text to the console
void Print(const string& in)
//Print text to the console with the given color
void PrintClr(const string& in, const ConColor &in)
//Get the virtual key code of a key binding identifier
int GetKeyBinding(const string &in)
//Load a font
FontHandle R_LoadFont(const string& in, uint8 ucFontSizeW, uint8 ucFontSizeH)
//Get information about a sprite file. Path is relative to the directory of the package
bool R_GetSpriteInfo(const string &in, SpriteInfo &out)\
//Load a sprite into memory. First argument is the sprite file name relative to the directory of the package. 
		iFrameCount defines the amount of frames in the sprite sheet. iFrameWidth and iFrameHeight are for the dimensions of the sprite. 
		If the sprite sheet has multiple lines then you can define the amount of frames per each line with iFramesPerLine. bForceCustomSize is used to
		determine if the sprite shall be loaded with a custom size instead of the original size.
		The szFile path is relative to the directory of the package
SpriteHandle R_LoadSprite(const string& in szFile, int iFrameCount, int iFrameWidth, int iFrameHeight, int iFramesPerLine, bool bForceCustomSize)
//Release a sprite if not used anymore
bool R_FreeSprite(SpriteHandle hSprite)
//Draw a box on the screen. It is not filled. You can define the line thickness of the box with iThickness.
bool R_DrawBox(const Vector& in pos, const Vector&in size, int iThickness, const Color&in color)
//Draw a filled box on the screen.
bool R_DrawFilledBox(const Vector&in pos, const Vector&in size, const Color&in color)
//Draw a line on the screen.
bool R_DrawLine(const Vector&in start, const Vector&in end, const Color&in color)
//Draw a loaded sprite on the screen. iFrame specifies the frame of the sprite sheet if the sprite is loaded from a sheet.
	You can define a vertical and horizontal scaling value. Also you can define a rotation vector which is used for
	rotating the sprite from. Also you can define a custom color which the sprite shall be rendered with.
bool R_DrawSprite(const SpriteHandle hSprite, const Vector&in pos, int iFrame, float fRotation, const Vector &in vRotPos, float fScale1, float fScale2, bool bUseCustomColorMask, const Color&in color)
//Draw a string on the screen.
bool R_DrawString(const FontHandle font, const string&in szText, const Vector&in pos, const Color&in color)
//Check if the required position is inside the view in order to be drawn
bool R_ShouldDraw(const Vector &in vMyPos, const Vector &in vMySize)
//Get the relative drawing positions of absolute world positions according to the view
void R_GetDrawingPosition(const Vector &in vMyPos, const Vector &in vMySize, Vector &out)
//Get the handle to the default loaded game engine font
FontHandle R_GetDefaultFont()
//Query a sound file located on the disk. Path is relative to the directory of the package
SoundHandle S_QuerySound(const string&in szSoundFile)
//Play a sound with the given volume (1-10). If bLoop is set to true, the sound will be looped
bool S_PlaySound(SoundHandle hSound, int32 lVolume, bool bLoop = false)
//Stop a currently played sound
bool S_StopSound(SoundHandle hSound)
//Get current game volume
int S_GetCurrentVolume()
//Get the center of the width of the screen
int Wnd_GetWindowCenterX()
//Get the center of the height of the screen
int Wnd_GetWindowCenterY()
//Spawn a scripted entity. Every IScriptedEntity class instance must be registered with the game engine using this method
bool Ent_SpawnEntity(const string &in, IScriptedEntity @obj, const Vector& in, bool bSpawn = true)
//Get the amount of all existing entities.
size_t Ent_GetEntityCount()
//Get the amount of existing entities with the given name
size_t Ent_GetEntityNameCount(const string &in szName)
//Get a handle to a given entity using it's ID.
IScriptedEntity@+ Ent_GetEntityHandle(size_t uiEntityId)
//Get a handle to the player entity
IScriptedEntity@+ Ent_GetPlayerEntity()
//Perform a trace line calculation from one point to another. The first found entity inside the trace line is returned.
	You can specify an entity that shall be skipped by the search
IScriptedEntity@+ Ent_TraceLine(const Vector&in vStart, const Vector&in vEnd, IScriptedEntity@+ pIgnoredEnt)
//Check if an entity is still valid. This is useful for validating if an entity has not yet been disposed
bool Ent_IsValid(IScriptedEntity@ pEntity)
//Get the ID of an entity by the entity handle
size_t Ent_GetId(IScriptedEntity@ pEntity)
//Move the specified entity in the required direction with the given speed
void Ent_Move(IScriptedEntity@ pThis, float fSpeed, MovementDir dir)
//Set activation status of the goal entity
void Ent_SetGoalActivationStatus(bool bStatus)
//List all sprites of a directory relative to the directory of the package.
bool Util_ListSprites(const string& in, FuncFileListing @cb)
//List all sounds of a directory relative to the directory of the package
bool Util_ListSounds(const string& in, FuncFileListing @cb)
//Return a random number between the given values
int Util_Random(int start, int end)
//Replace a token inside a string with a new token
string Util_StrReplace(const string& in szSource, const string &in szTarget, const string &in szNew)
//Create a property token with ident and value
string Props_CreateProperty(const string &in ident, const string &in value)
//Extract a value by ident from a property string
string Props_ExtractValue(const string &in properties, const string &in ident)
//Unlock an achievement
void Steam_SetAchievement(const string &in szName)
//Set stat value (int)
void Steam_SetStat(const string &in szName, int iValue)
//Set stat value (float)
void Steam_SetStat(const string &in szName, float fValue)
//Check if achievement is already unlocked
bool Steam_IsAchievementUnlocked(const string &in szName)
//Get stat value (int)
int Steam_GetStatInt(const string &in szName)
//Get stat value (float)
float Steam_GetStatFloat(const string &in szName)
//Register a CVar of a given type
CVarHandle CVar_Register(const string &in szName, CVarType eType, const string &in szInitial)
//Get boolean value of CVar
bool CVar_GetBool(const string &in szName, bool fallback)
//Get integer value of CVar
int CVar_GetInt(const string &in szName, int fallback)
//Get float value of CVar
float CVar_GetFloat(const string &in szName, float fallback)
//Get string value of CVar
string CVar_GetString(const string &in szName, const string &in fallback)
//Set CVar boolean value
void CVar_SetBool(const string &in szName, bool value)
//Set CVar integer value
void CVar_SetInt(const string &in szName, int value)
//Set CVar float value
void CVar_SetFloat(const string &in szName, float value)
//Set CVar string value
void CVar_SetString(const string &in szName, const string &in value)
//Execute a configuration script file
bool ExecConfig(const string &in szFile)
//Enable or disable drawing of HUD
void HUD_SetEnableStatus(bool value)
//Update HUD health value
void HUD_UpdateHealth(size_t value)
//Add new HUD ammo item
void HUD_AddAmmoItem(const string &in szIdent, const string &in szSprite)
//Add new collectable item
void HUD_AddCollectable(const string &in szIdent, const string &in szSprite, bool bDrawAlways)
//Update specific HUD ammo item
void HUD_UpdateAmmoItem(const string &in szIdent, size_t uiCurAmmo, size_t uiMaxAmmo)
//Update specific collectable item
void HUD_UpdateCollectable(const string &in szIdent, size_t uiCurCount)
//Get current ammo amount of item
size_t HUD_GetAmmoItemCurrent(const string &in szIdent)
//Get max ammo amount of item
size_t HUD_GetAmmoItemMax(const string &in szIdent)
//Get current amount of items of a collectable
size_t HUD_GetCollectableCount(const string &in szIdent)
//Specify ammo item to display
void HUD_SetAmmoDisplayItem(const string &in szIdent)
//Check whether HUD is currently enabled or not
bool HUD_IsEnabled()
//Add a HUD info message
void HUD_AddMessage(const string &in msg, HudInfoMessageColor color, int duration = 3000)
//Query a language phrase of a language file of current locale. Alternatively you can use '_' as shortcut
string Lang_QueryPhrase(const string &in szIdent, const string &in szDefault = "")
//Trigger saving the current game state
void TriggerGameSave()
```

## AngelScript internals:
You can use all things from
* AngelScript std string
* AngelScript script array
* AngelScript script math

[Back](index.html)
