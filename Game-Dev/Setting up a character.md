# Steps -
1. Setup the class structure
2. Setup Input binding using **gameplayTag** + make custom enhanced input component
3. Create animation BP and create **AnimInstances** needed
4. Add+setup the gameplay ability system for the character + define ur own ability activation policies.

## Step 1 (Setup classes)-
![[Pasted image 20240926061225.png]]
#### Steps - 
1. Tools -> create new C++ class -> GameModeBase -> rename + put in a folder gameModes + make it public -> create class
2. Click live coding button to compile the new class and u see a folder named C++ classes
3. In step 1, we created the GameMode, now we have to create the WarriorBaseCharacter.
4. In the public directory create new class WarriorBaseCharacter in a subfolder characters.
5. Now, dont press live coding, go into visual studio and reload all, then open project in visual studio code and delete unused functions from .h files
6. Now setup using this code subsequently

```cpp
#include "Characters/WarriorBaseCharacter.h"

// Sets default values
AWarriorBaseCharacter::AWarriorBaseCharacter()
{
 	// Set this character to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = false;
	PrimaryActorTick.bStartWithTickEnabled = false;

	GetMesh()->bReceivesDecals = false;
}
```
Initially basecharacter.cpp
Now, relaunch the editor and create a new derived class from the BaseCharacter.cpp for the Hero character. 
Then come back to the public folder and make another class name HeroController
make Blueprint classes out of the character and the controller, the gamemode.
Now, change the default gamemode in project settings to the BP_BaseGameMode just created
Now in world settings, set game mode override to be none to apply the BP as the default game mode

![[Pasted image 20240926153538.png]]

### Animation -
![[Pasted image 20240927054635.png]]

### Weapon
![[Pasted image 20240928053453.png]]
![[Pasted image 20240928053528.png]]
without GAS, we will have to have all sorts of checks to make sure that this happens correctly
![[Pasted image 20240928053639.png]]
![[Pasted image 20240928063031.png]]
