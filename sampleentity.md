## Sample entity implementation

Let us get more into coding by examining an example entity included within the builder template: The headcrab.

The headcrab is a fanmade re-creation of the headcrab of the popular Half-Life series. In our example it shall
walk around and attack the player when nearby. Otherwise it walks around to different directions.

So, let's assume we have an empty class implementing the IScriptedEntity interface. 
```angelscript
class CHeadcrabEntity : IScriptedEntity {
    //Interface implementation...
}
```

One of the first things we would like to to is to load the sprite file that represents the headcrab in the scene.
We can load the sprite using R_LoadSprite(). This shall be done in the OnSpawn() method. We do also want to create
a bounding box associated with a model which is used for collisions.
```angelscript
Vector m_vecPos;
Vector m_vecSize;
float m_fRotation;
SpriteHandle m_hSprite;
Model m_oModel;

CHeadcrabEntity()
{
    this.m_vecSize = Vector(32, 32);
    this.m_fRotation = 0.0f;
}

void OnSpawn(const Vector& in vec)
{
    this.m_vecPos = vec;
    this.m_hSprite = R_LoadSprite(GetPackagePath() + "gfx\\headcrab.png", 1, 32, 32, 1, true);
    BoundingBox bbox;
    bbox.Alloc();
    bbox.AddBBoxItem(Vector(0, 0), this.m_vecSize);
    this.m_oModel.Alloc();
    this.m_oModel.Initialize2(bbox, this.m_hSprite);
}
```

R_LoadSprite accepts the following parameters:
1. The full path to the sprite image file. We use GetPackagePath() to dynamically query the path of our game package.
2. The total number of frames the sprite contains in case this is a spritesheet. 
3. The width dimension
4. The height dimension
5. The amount of frames per line, since sprite sheets can have multiple lines
6. Indicator if the sprite shall be stretched to custom dimension instead of using the original file ones

R_LoadSprite returns a valid sprite handle (SpriteHandle) on success. Be sure to store the handle into a class member property.

We do also create a bounding box that holds the cubes of the model which is used for collision detection. You can add multiple cubes
to a bounding box which will all be taken into account for collision detection. The bounding box will be attached to a model which
holds the bounding box and the main sprite.

AddBBoxItem receives two arguments:
1. A vector containing the relative position to the current entity position 
2. A vector containing the dimension of this current cube

The model will then be initialized with the bounding box and the main sprite handle.
Note that you have allocate bounding boxes and models before using them. 

After that we now want to draw the entity sprite at the given position. We will need to calculate the screen position according to
the current world position
```angelscript
void OnDrawOnTop()
{
    if (!R_ShouldDraw(this.m_vecPos, this.m_vecSize))
        return;
        
    Vector vOut;
    R_GetDrawingPosition(this.m_vecPos, this.m_vecSize, vOut);

    R_DrawSprite(this.m_hSprite, vOut, 0, this.m_fRotation, Vector(-1, -1), 0.0, 0.0, false, Color(0, 0, 0, 0));
}
```

At first we determine if the entity is in screen range. If not, we abort the drawing process. 
Otherwise we let the engine calculate and provide us with the screen drawing position of the entity.
After that we draw the entity. 
R_DrawSprite receives the following arguments:
1. The handle to the sprite
2. A vector containing the screen position of where to draw
3. The index of the frame to draw if this sprite is a spritesheet containing multiple frames
4. The rotation of the sprite
5. A vector containing the rotation position
6. A scale factor for width dimension
7. A scale factor for height dimension
8. Indicator if a color shall be applied to the sprite
9. A color data struct object containing the color (RGBA)

Note you can also use the OnDraw() method, but it should preferably be used for things such as floors or decals, basically everything that represents
something on the ground.

So, now it doesn't do much. We want it to walk around. So we need a timer and calculate the movement when the timer is elapsed.
```angelscript
//Member declaration
Timer m_oMovement;

//In OnSpawn()
this.m_oMovement.SetDelay(100);
this.m_oMovement.Reset();
this.m_oMovement.SetActive(true);

//In OnProcess()
this.m_oMovement.Update();
if (this.m_oMovement.IsElapsed()) {
    this.m_oMovement.Reset();
    
    Ent_Move(this, this.m_fSpeed, MOVE_FORWARD);
}
```

The entity will move whenever the timer is elasped. Since we want the entity to continuously walk, we will reset the timer whenever it is elapsed.
We use the Ent_Move() engine function in order to move our entity. The engine will actually calculate our new world position according to our rotation,
speed and direction. Ent_Move accepts the following arguments:
1. A handle to the entity that shall get moved
2. The movement speed
3. The direction: Either MOVE_FORWARD, MOVE_BACKWARD, MOVE_LEFT or MOVE_RIGHT

If the entity would hit a wall then it will not move forward, but remain at the current position
In order to apply the new position and provide the engine with our data, we need to implement four methods:
```angelscript
//Called for recieving the current position. This is useful if the entity shall move.
Vector& GetPosition()
{
    return this.m_vecPos;
}

//Set new position
void SetPosition(const Vector &in vecPos)
{
    this.m_vecPos = vecPos;
}

//Return the rotation.
float GetRotation()
{
    return this.m_fRotation;
}

//Set new rotation
void SetRotation(float fRot)
{
    this.m_fRotation = fRot;
}
```

Now our entity will move forward. Now we do also want to change its direction randomly and also play a walking sound:
```angelscript
Timer m_oDirChange;
Timer m_oWalkSound;
SoundHandle m_hWalkSound;

//In OnSpawn():
this.m_oDirChange.SetDelay(5000);
this.m_oDirChange.Reset();
this.m_oDirChange.SetActive(true);
this.m_oWalkSound.SetDelay(1000 + Util_Random(1, 2000));
this.m_oWalkSound.Reset();
this.m_oWalkSound.SetActive(true);
this.m_hWalkSound = S_QuerySound(GetPackagePath() + "sound\\hc_walk.wav");

//In OnProcess():
this.m_oDirChange.Update();
if (this.m_oDirChange.IsElapsed()) {
    this.m_oDirChange.Reset();
    if (!this.m_bGotEnemy)
        this.m_fRotation = float(Util_Random(1, 360));
}

this.m_oWalkSound.Update();
if (this.m_oWalkSound.IsElapsed()) {
    this.m_oWalkSound.Reset();
    S_PlaySound(this.m_hWalkSound, S_GetCurrentVolume());
}
```

We will set the direction change timer to a random delay value. You can generate random numbers bewtween a range via Util_Random().
Sounds can be loaded via S_QuerySound(). This will load a sound into memory if it has not yet been loaded. You provide the full path to
the file as the only argument. A sound can then be played via S_PlaySound(). It accepts the handle to the sound as first argument and
the volume (0 - 10) as last argument. You can query the current game volume via S_GetCurrentVolume().

Next thing is that we want to attack the player if the headcrab is nearby. Therefore we check repeatedly if the player is nearby and 
then run towards the player. If the headcrab is close enough we will damage the player entity every elpased timespan. For that we need
the following two methods:
```angelscript
void LookAt(const Vector &in vPos)
{
    //Look at position
    float flAngle = atan2(float(vPos[1] - this.m_vecPos[1]), float(vPos[0] - this.m_vecPos[0]));
    this.m_fRotation = flAngle + 6.30 / 4;
}

void CheckForEnemiesInRange()
{
    //Check for enemies in close range and act accordingly
    
    this.m_bGotEnemy = false;
    IScriptedEntity@ pEntity = null;
    
    for (size_t i = 0; i < Ent_GetEntityCount(); i++) {
        @pEntity = @Ent_GetEntityHandle(i);
        if ((@pEntity != null) && (pEntity.GetName() == "player")) {
            if (this.m_vecPos.Distance(pEntity.GetPosition()) <= C_HEADCRAB_REACT_RANGE) {
                this.m_bGotEnemy = true;
                break;
            }
        }
    }
    
    if (this.m_bGotEnemy) {
        if (this.m_fSpeed == C_HEADCRAB_DEFAULT_SPEED)
            this.m_fSpeed *= 2;
            
        if (pEntity.GetName().length() > 0) {
            this.LookAt(pEntity.GetPosition());
        }

        if (this.m_vecPos.Distance(pEntity.GetPosition()) <= C_HEADGRAB_ATTACK_RANGE) {
            this.m_oAttack.Update();
            if (this.m_oAttack.IsElapsed()) {
                pEntity.OnDamage(C_HEADGRAB_DAMAGE_VALUE);
                S_PlaySound(this.m_hAttackSound, S_GetCurrentVolume());
                this.m_oAttack.Reset();
            }
        }
    } else {
        if (this.m_fSpeed != C_HEADCRAB_DEFAULT_SPEED)
            this.m_fSpeed = C_HEADCRAB_DEFAULT_SPEED;
    }
}
```

The first method sets our rotation accordingly to look at the target entity by receiving its position.
The second method does the following:
1. Loops through all entities and checks if it is a player and if so checks if it is in close attack range and saves the player entity handle.
    Of course we could just use Ent_GetPlayerEntity(), but you may want to extend the behavior so that the headcrab will also attack other entities
    (in Half-Life it was so that alien entities did attack either the player or military human entities)
2. If the headcrab has an opponent then it doubles its current walking speed, else it reduces its walking speed if it had doubled it before
3. It also then looks at the opponent
4. It also checks for current distance and if it is in close attack range then it handles the attack timing. 
5. If attack timing is elapsed it damages the opponent entity by calling its OnDamage() method.

In order to let the headcrab take damage and be defeated we will now also implement this behaviour:
```angelscript
//Amon other member variables:
uint32 m_uiHealth;

//In OnProcess():
if (this.m_tmrFlicker.IsActive()) {
    this.m_tmrFlicker.Update();
    if (this.m_tmrFlicker.IsElapsed()) {
        this.m_tmrFlicker.Reset();
        
        this.m_uiFlickerCount++;
        if (this.m_uiFlickerCount >= 6) {
            this.m_tmrFlicker.SetActive(false);
            this.m_uiFlickerCount = 0;
        }
    }
}

//In OnDrawOnTop():
Color sDrawingColor = (this.m_tmrFlicker.IsActive()) ? Color(255, 0, 0, 150) : Color(0, 0, 0, 0);
bool bCustomColor = (this.m_tmrFlicker.IsActive()) && (this.m_uiFlickerCount % 2 == 0);
R_DrawSprite(this.m_hSprite, vOut, 0, this.m_fRotation, Vector(-1, -1), 0.0, 0.0, bCustomColor, sDrawingColor);

//Called when entity gets damaged
void OnDamage(uint32 damageValue)
{
    if (this.m_uiHealth < damageValue) {
        this.m_uiHealth = 0;
    } else {
        this.m_uiHealth -= damageValue;
    }
    
    this.m_tmrFlicker.Reset();
    this.m_tmrFlicker.SetActive(true);
    this.m_uiFlickerCount = 0;
}

bool NeedsRemoval()
{
    return this.m_uiHealth == 0;
}
```

You should now be able to undertand what the code is doing.

Also do not forget to provide your entity instance with a unit name and also indicate to the engine that this entity is collidable:
```angelscript
//Indicate whether this entity is collidable
bool IsCollidable()
{
    return true;
}

//Return a name string here, e.g. the class name or instance name.
string GetName()
{
    return "headcrab";
}
```


[Next chapter](playerspecifics.html)<br/>
[Back](index.html)
