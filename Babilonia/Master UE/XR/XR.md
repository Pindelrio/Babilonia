# VR Pawn

#9/10/2024

El Pawn Puede ser poseido
==Actor== → Tangible i en escena, qualsevol representacio fisica
de Object hereda actor i caracter

Desactivar ‘Enable live Coding’ perque sino compila tota l’estona
Necessitem un component per mando → Motion Controller (No necessita una representacio fisica)

- La Propietat de MotionController ens permet definir què traquejarem
- Rendering

El que fem servir ha d’estar “Rigejat” tenir moviments
Mantener tasa de FrameRate estable i elevable
No controlar la camapra del player
En el .h intentem no posar .h sino fer un “Forward declaration” → class nomllibreria;
Asignem el Core al RootComponent

```cpp
RootComponent = VRCore;
VRCamera->SetupAttachment(RootComponent);
```

Crear malla
Buscar cosas lowpoly

#16/10/2024 
# Teleport

#23/20/2024

# Inputs
Input Mapping context

# Interface

Crear Interface (UInterface)
Declarar metodes
```cpp
class MPROG_XR_API IInteract  
{  
    GENERATED_BODY()  
  
    // Add interface functions to this class. This is the class that will be inherited to implement this interface.  
public:  
  
    //BlueprintNativeEvent serveix per poder sobreescriure la funcio a Blueprints (Detecta si hi ha un metode creat)  
    UFUNCTION(BlueprintCallable, BlueprintNativeEvent, Category = "Interact")  
    void Interact();  
};
```

En la Clase on la volem fer servir:
En el .h
Afegir l'Include 
```cpp
 #include "Interact.h"
public: 
// nom de la funcio + _Implementation  
void Interact_Implementation();
```
en el cpp
``` cpp
void ADoor::Interact_Implementation()  
{  
    IInteract::Interact_Implementation();  

	//Do something

	//Comprobem que el destí te la interface creada
    if(GetClass()->ImplementsInterface(UInteract::StaticClass()))  
    {  
		//Executem el metode emb "Execute_NomMetode"
       IInteract::Execute_Interact(SomeActor)  
    }  
         
}
```

# Optimitzar VR

Project settings
	- Forward Shading -> True
	<span style="color:rgb(255, 0, 0)">Rendering - Default Settings</span>
	- Bloom - False
	- Ambient Occlusion -> False
	- Auto Exposure -> False
	- Motion Blur -> False
	- Lens Flares -> False
	- Anti-Alising Method -> MSAA
	<span style="color:rgb(255, 0, 0)">VR</span>
	- Instanced Stereo -> True
	- Mobile HDR -> False
	- Round Robin Occlusion Queries -> true
	<span style="color:rgb(255, 0, 0)">Postprocessing</span>
	Custom Depth-Stencil Pass -> Disabled
	Custom Depth with TemporalAA Jitter -> False
<span style="color:rgb(255, 0, 0)">Translucency</span>
	Separate Translucency -> False
World Settings
	VR
			Mono Culling Distance -> 2500
Materials
	Details
			Fully Rough-> True
Console Commands
		vr.PixelDensity 0.5
		r.ScreenPercentage 100