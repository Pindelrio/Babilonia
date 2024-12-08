#23/09/2024

# Introducció

Unreal Engine esta constituit per Moduls
Podem utilitzar Plugins-> Coses que tenen a veure amb l'editor o Moduls-> Coses propies
#### Creem un plugin nou
![[Pasted image 20241015160424.png|750]]

La documentació del motor en realitat es el codi parsejat a la web

Carpetes projecte C++
==Engine== hi ha els móduls del motor
- per exemple la carpeta ThirdPart gairabè la podem esborrar tota
==Games== -> Source
	- Els arxius **Nom.Build.cs** -> Diu les regles de compilació del módul
	- El sistema previ de compilat "Unreal Build Tool (UBT)" mira les regles de compilació
	- Nom.Target.cs -> Creara a l'hora de compilar un target nou
	- NomEditor.Target.cs
- En la configuració del VS/Raider tenim 
	- Debug Game
	- Debug Game Editor
	- Development
	- Development Editor
	- Shipping
==Plugins== -> Aqui tindrem el nostre plugin creat
En el **Nomplugin.uplugin** equivalent al *.uproject*  hem de canviar el Type a "Editor"
```c++
"Modules": [  
    {       "Name": "ToolsPlugin",  
       "Type": "Editor",  
       "LoadingPhase": "Default"  
    }  
]
```
Afegim el mòdul al .uProject per dir quan volem incloure el mòdul al compilar (el TargeAllowList) en aquest cas es en "Editor". 
``` C++
"Plugins": [
...
{  
    "Name": "ToolsPlugin",  
    "Enabled": true,  
    "TargetAllowList": [  
       "Editor"  
    ]  
}]
```


Per indicarli a unreal a quin modul ha de linkar afegim al fitxer Games -> Source -> NomProject -> NomProject.Build.cs el nom del plugin
``` C++
PublicDependencyModuleNames.AddRange(new string[] { "Core", "CoreUObject", "Engine", "InputCore", "EnhancedInput", "ToolsPlugin" });
```

>[!tip] Unreal Tip
> A l'Editor *preferences* deshabilitar Live Coding
> - General - Live Coding
> 	- Automatically Compile Newly Added C++ Classes
> - General - Miscellanius
> 	- Automatically Compile Newly Added C++ Classes
> ![[Pasted image 20241015184051.png|400]]
 

#30/09/2024 

# Blueprint Editor Utilities

### Construction Script

Es un script que esta dins els ==Actors== (Objete que te transform, fisicas y rendering) que principalment s’utilitza per inicialitzar coses petites
S’executa sempre fora del joc, fora del runtime, en temps d’editor. 
Sempre que canviem una propietat d'un actor s'executa

> [!tip] Unreal Tip
> #### Si fem servir un Actor per provar coses d'editor, no cuinar-lo
> A les propietats de Actor → Class defaults → Details → Marcar “Is Editor only Actor”. Això fa que no es cuini al publicar el joc
> 

PIE World → Play in editor (Quan els actor només existeixen en el mode editor. Si juguem desapareixen. Te molt a veure amb el Word Context Object)
El Construction Script s'executa en mode Editor. Per això no podem fer Spawn desde le Construction Script perque fariem un loop infinit

### Els actors es composen de components
Si que podem fer un “AddComponent”

>[!info]
Si tenim error en “Spawn Actor” perque no tenim el transform inicialitzat podem fer split i s’arregla

>[!info] 
>Per anar directament a un actor al Content Browser→ Seleccionem l’Actor i premem **Ctl + B**
 Anem directament a la Edició del Asset → Seleccionar Actor i premem **Ctl + E**


==Static Mesh== es una peça de geometria
==Static Mesh Actor== → És un "StaticMesh" que te un Component que s’encarrega/gestiona el rendering, la fisica i el transform. Si treiem el Tick de l’actor, que es el que gestiona el component treuriem totes aquestes coses
Un ==Asset== nomes es un static mesh → el Component i l’Actor son Objectes abstractes

>[!Info]
>Al crear una variable marcar el check “Instance Editable” perque es vegi a l’editor.

Al posar un Static Mesh habilitar a Configuració “Show Engine Content” per veure les Mesh del motor

#### Vector 
Cantitat/(modul, magnitud) i Direccio
#### Un vector unitari
Un vector que te de valor 1

#### Prioritats d'Objectes
* El que es modifica al Level te mes prioritat
- Despres en el CDO

Tots els actors pertanyen al nivell

# Exercici.
Crea un BP_ConstructionExample que  crei X static mesh a una idstancia i amb un offset
![[Pasted image 20241126213950.png]]

#### Backear
Al fer Tools-> Merge podem crear una malla estatica a partir del BP dinamic que hem estat creant 

Si afegim functions al Construction Script podem configurar les propietats per exemple per fer Call in editor
![[Pasted image 20241126223745.png]]
#03/10/2024

![[Pasted image 20241126165653.png]]

Un Componente -> Un Objecte subeditat/vinculat a un actor (Patro composició )
- Es poden crear al constructor (`Component = CreateDefaultSubobject<USceneComponent>(TEXT("Root"));`)
- O en qualsevol altre lloc (`UStaticMeshComponent* NewComp = NewObject<UStaticMeshComponent>(this);`)          


#### Garbage collector
El sistema de reflexió de UE te un sistema de garbage collector.
Cada x temps escaneja tots els objectes i si l'estan referenciant el deixa sino el borra

Per evitar que el GC no elimini la variable li posem UPROPERTY() això li diu a UE que la variable esta referenciada
>[!info] Documentació
>Pagina de documentació de tots els especificadors de Unreal
>https://benui.ca/

// EditInstanceOnly -> Nomes es pot editar si esta creada al nivell

```c++
UPROPERTY(VisibleAnywhere, BlueprintReadOnly, meta = (AllowPrivateAccess = "true"))  
UStaticMeshComponent* Body;
```

#### Estructura
AStaticMeshActor -> Asset
- UStaticMeshComponent -> Component (Regles de Renderitzar, de físiques i afegir el Transform)
	- UStaticMesh -> Asset. La malla estàtica

UActorComponent
- USceneComponent -> Afageix el transform
	- UPrimitiveComponent
		- UStaticMeshComponent

USceneComponent -> El Component més basic que podem tenir, només posa el Transform
```cpp
RootComponent = CreateDefaultSubobject<USceneComponent>(TEXT("Root"));
```

#### ==OnConstruction== 
* Es l'equivalen a c++ del ConstructionScript
```cpp
void ACPPToolTest::OnConstruction(const FTransform& Transform)  
{  
    // if (NumberOfElements == ComponentList.Num())  
    //     return;  
    if (NumberOfElements != ComponentList.Num())  
    {       for (UStaticMeshComponent* Comp : ComponentList)  
		   {      Comp->UnregisterComponent();  
		          Comp->MarkAsGarbage();  
	       }      
	       ComponentList.Empty();  
  
       for (int32 i = 0; i < NumberOfElements; i++)  
       {          
	       //AIXÓ NOMÉS ES POT EXECUTAR AL CONSTRUCTOR -> UStaticMeshComponent* NewComp = CreateDefaultSubobject<UStaticMeshComponent>;  
  
          //FORA DEL CONSTRUCTOR LA FORMA CORRECTA ES COM A NEW OBJECT          
          //Amb el THIS li diem a qui pertany l'objecte          
          UStaticMeshComponent* NewComp = NewObject<UStaticMeshComponent>(this);  
  
          NewComp->SetupAttachment(Body);  
  
          //REGISTREM EL COMPONENT A L'ACTOR  
          NewComp->RegisterComponent();  
  
          if (Visualizer) //Si un punter esta buit val 0  
          {  
             NewComp->SetStaticMesh(Visualizer);  
          }          
          ComponentList.Add(NewComp);  
       }    
	}
}
```

#07/10/2024 

# PostEditChangeProperty


```cpp
#if WITH_EDITOR  
void ACPPToolTest::PostEditChangeProperty(struct FPropertyChangedEvent& PropertyChangedEvent)  {}
#endif
```

>[!Info] Macro per obtenir nom
>```cpp
>GET_MEMBER_NAME_CHECKED(ACPPToolTest, NumberOfElements)

# Static
==static== FName GenericName -> Si es estàtica vol dir que es global. (per exemple FColor)
- Es una variable que tots els objectes comparteixen. Només n'hi ha una per clase. No pertany a la propia clase no a les instancies

```cpp
  
class Entity  
{  
public:  
    Entity()  
    {       EntityCount++;  
    }private:  
    static uint32 EntityCount;  
};
```

>[!Info] Declaració
>Les variables estàtiques les declarem al .h i la definim al .cpp
>```cpp
>//Al .h
>static const FName GenericName
>//Al cpp
>const FName ACPPTooltest::GenericName(TEXT("SomeText"));
>

# Blutility

Activar el plugin "Editor Scripting Utilities" 
![[Pasted image 20241007131208.png]]

El Blutility podem fer tasques per Assets o per Actions

>[!info] Patró Command
>Consisteix en crear un objecte que representi una ordre

Asset -> Qualsevol cosa que poso al projecte, un mesh, png, BluePrint
Actor -> Te component. Tot el que podem posar el mon. Character, Pawn
BluePrint-> No es un actor, es un asset (object) que te les regles per representar un Actor (encara que el poguem posar el mon). Al compilar els BP desapareixen, nomes es queden els actors 
- Eventgraph
- Un ConstructionStcript
- Functions
- Macros
- CDO

![[Pasted image 20241021192646.png]]
![[Pasted image 20241021192748.png]]

En el Asset Utility tenim un event "Run" que serveix per llençar l'utility desde les propietats de Unreal
![[Pasted image 20241127133227.png]]![[Pasted image 20241127133300.png|103]]
- A Class Defaults -> Suported Classes -> Han de ser Clases de Assets
Podrem utilitzar la acció en el Asset, bto dret -> eleccionar "Scripted Assets Action"

#14/10/2024 

UKismetSystemLibrary
MipMapping
![[Pasted image 20241127172109.png|275]]
Log String
Print String
### ==Get Selected Assets== -> Carrega els assets en memòria
### Modificar una propietat ==Set Editor Properties==
* O fem servir el SetEditorProperty
- També podem fer "Cast to" i fer un "Set"
- ![[Pasted image 20241127174531.png]]
### ==Save Loaded Asset== 

> [!info]
> * Les funcions pures recalculen les variables cada vegada que es criden 
> * Les funcions impures no les variables es mantenen
> 

Creem una llibreria igual com si fos una clase de c++
![[Pasted image 20241127174949.png]]

Si fem funcions en c++ per blueprint: Tot en blueprint va per punters (*) Si en canvi es un & es pensa que es una sortida 

#18/10/2024

Funció estàtica -> Se puede acceder desde qualsevol lloc

Nodes Blaus -> Objectes carregats en memoria 
Node Blanc -> Interface

==Get Selected Assets== -> Només quan necessitem carregar el element
==Get Selected Asset Data== -> Ens dona els punters, no carrega el contingut en memoria

# Asset Registry
Asset Registry -> Subsitema de UE que gestiona tots els assets del motor

Tarea. - > Crear sistema que llisti totes les referencies del projecte

==GetAssetRegistry== (Node blanc -> Interface)  
- ==Get All Assets== (Ja retorna Asset Data)
- ==Get Asset By Class ==

# Rutas
![[Pasted image 20241022200423.png]]

#### Com identifica Unreal les entitats del motor
La classe + la Ruta del paquet. El .MaterialSphere es la propietat on es guarden
**/Script/Engine.StaticMesh**'/Game/StarterContent/Props/MaterialSphere.MaterialSphere'

#### **/Script/Engine.StaticMesh**
**Script** significa que la clase que ve a continuació es de c++ i l'**Engine** es el nom de ENGINE_API (Modul d'on es la clase)

#### '/Game/StarterContent/Props/MaterialSphere.MaterialSphere'
==Asset Class Path + Package Path + AssetName . AssetName==

![[Pasted image 20241022204422.png]]

Una UClass en realitat hereda de UObject

La ruta de la nostra classe ACPPToolTest seria: 
/Script/ToolsPlugin.CPPToolTest
(es un codi c++) + ( nom del modul) + (nom de la Classe sense U o A)

*****

Si utilitzem el Soft Class per poder utilitzar el nom millor perque així no carreguem la classe en memoria
![[Pasted image 20241018120456.png]]



#21/10/2024 

En la configuració del modul podriem inicialitzar la variable (mapa) de ClassToPrefix
Afegir aquesta macro al cpp per 

`IMPLEMENT_MODULE(FPinEditorUtilities2024Module, PinEditorUtilities2024)`
![[Pasted image 20241203153914.png|800]]


#### Biblioteca FModuleManager::LoadModule(TEXT("ToolsPlugin"))  
- Ens permet buscar un modul`

#### Exercici. 
En el Event Run recorrerem tot el projecte

Per saber si un Asset es del tipo X podem fer 2 opcions:
- 1 - Mirar si el "Asset Class Path" es == al packageName
- 2 - Amb l'AssetRegistry podem buscar by Class posant el text per ex. "/Script/Engine.Texture2D"
- 3 - Per obtenir la clase
![[Pasted image 20241203165413.png]]
#### Get Tag Value 
Per obtenir la metadata de una propietat concreta fem servir GetTagValue (Vigilar que el nom de la propietat es la interna)

![[Pasted image 20241202174949.png|775]]



#22/10/2024 

### Llibreries per exportar
Rutas absolutas
FPaths::ProjectSavedDir()
FFileHelper::SaveStringToFile

Variables globals que podem fer servir: Començen per GI
![[Pasted image 20241202203402.png]]


## Get Asset Tools
- Llibreria per exportar Assets
- "Export Assets "
![[Pasted image 20241202204814.png|500]]

### Get Dependencies / Referencers
![[Pasted image 20241202205004.png]]

#### <span style="color:rgb(255, 0, 0)">Get Assets</span> te moltes maneres de buscar 
![[Pasted image 20241202205221.png]]



#28/10/2024 

Exercici 1

#### L'operador '/' concatena i afageix una / a 2 Fstrings
return FPaths::ProjectSavedDir() / TEXT("Export");

# Per modificar la propietat d'un Asset
- SetEditorProperty
 - O un Castej al actor
# CDO Class default object
BP -> CDO -> Actor
![[Pasted image 20241204163713.png|550]]

Als actors hi ha una funció que es la 
### PostInitProperties()
On es poden inicialitzar les variables de UProperty

#11/11/2024

Podem obtenir el Bluprint d'un Asset desde BP amb ==GetBluprintAsset== 

### Per obtenir el CDO del blueprint en c++
- En els Blueprints hi ha una GeneratedClass que es l'entitat nova que creem (per ex BP_ThirdPersonChar)
```cpp
//UBluprint asset      //Generated class     //CDO  
return InBlueprintAsset->GeneratedClass->GetDefaultObject<UObject>();
```
![[Pasted image 20241111105903.png|500]]

# meta = DeterminesOutputType
* permet fer una mena de template que sap diferenciar quina mena de retorn te
UFUNCTION(BlueprintPure, category="Pin Utils", meta=(DeterminesOutputType=NomVariable))

Per entendre que volem aquest exemple:  K2Node_AllMoveTo. Son programacions que s'han de fer en un mode editor. Per definir el comportament especific de un node de bp

#### Com modificar variables del CDO
![[Pasted image 20241111110933.png]]

# AssetRegistry desde c++
```cpp
//Obliga a carregar el modul (Som un Single Lazy)  
const auto& AssetRegistryModule = FModuleManager::Get().LoadModuleChecked<FAssetRegistryModule>(TEXT("AssetRegistry"));  
IAssetRegistry& AssetRegistry = AssetRegistryModule.Get();   
//Això potser no esta carregat  
//IAssetRegistry* AssetRegistry = IAssetRegistry::Get();
```

#### Show message
```cpp
//L'execució s'espera a la resposta  
TEnumAsByte<EAppReturnType::Type> Result = UEditorDialogLibrary::ShowMessage(  
    FText::FromString(TEXT("Rename")),  
    FText::FromString(FString::Printf(TEXT("Rename \n %s to \n %s"),*SourceName, *DestName)),  
    EAppMsgType::YesNo      
);
```


# GET ASSETS - Filters
![[Pasted image 20241204193655.png|500]]

en cpp
```cpp
FARFilter ARFilter;  
ARFilter.ClassPaths.Add(InClass->GetClassPathName());  
ARFilter.PackagePaths.Add(TEXT("Game/"));  
  
AssetRegistry.GetAssets(ARFilter, OutAssetData);
```
#14/11/2024

Creen un subsitema d'editor (UEditorSubsystem)
#### Llibreria FEditorDelegates::
Tenim uns Delegats propis de Editor, com 
Ens podem enganxar als delegats per Hooks
BeginPIE -> Al entrar en mode editor
RefreshAllBrowsers -> refrescar els buscadors
SelectedProps -> Manipular propietats de l'actor 

#### Tipos de Delegates

**Single** -> Nomes podem enganxar una funció (Bind)
**Multicast** -> Podem enganxar n funcions (Add)

Els *Delegates* poden pertanyer al Sistema de reflexió (Dinàmics) o no

![[Pasted image 20241206101332.png|500]]

Normalment farem servir en els no dinamics
- AddUObject si es Multicast 
- o bé BindUObject si es Single

Exemple:
No dinamics (Normals)
`FEditorDelegates::SelectedProps.AddUObject(this, &ThisClass::FuncioACrear);`

En els Dinàmics
`GetCapsule()->OnComponentBeginOverlap.AddDynamic(this,&ThisClass::FuncioACrear);`
# Actor Action Utility

Menu contextual sobre los actores

#### GetSelectionSet
* Equivalent al *Get Selected Assets*

# Transactions
- UndoHistory
* Begin Transaction (en la part critica)
* End Transaction
* Transact Object (per propietats d'un objecte en concret)

#15/11/2024 
Exercicics 3 i 6, 7

#18/11/2024 

La Clase dels Blueprints es posa la ruta amb "_C" al final

UCLASS(Config=Editor)

UPROPERTY(Editor)

/Script/[Modulo].[NomClasse]

[/Script/ToolsPlugin.ToolsPluginEditorSubsystem]
+NonDuplicateActors=


Tarea 2
 Ej 1 Recursividad
 ej 2 - 
 Numero arbitrario de 
 Commando , condiciones especificas 

Asset Utility

#21/11/2024
# Widget Utility Blueprints

Opcions
- StackBox - Com el Vertical/Horizontal Box pero de forma lliure
- Grid Panel / **Uniform Grid Panel**

Es creen en el menú Tool -> Editor Utility Widget (previament s'ha de fer un *run editor utility widget*)
![[Pasted image 20241121105114.png]]

Tenim 2 tipos d'objectes 
		Panells -> podem posar coses dins -> tenen Slots
		Widgets -> 
Els controls *editors* ja tenen l'aspecte/tema de l'editor pre carregat.

### Slot
Quan posem algo a un panel es crea un *Slot* -> Com el panell distribuirà el que hi ha al widget
- El Row i Column  ajuden a diposar les coses dins com si fos Horizontal/Vertical box
- **Size** -> El Fill (invalida el size de Aparence) determina el pes/porcentatge que te aquell widget respecte la resta
- Padding -> Marge interior 

#### Event Pre Construct -> Es dispara al compilar. 
- **Is Design Time** -> Permet previsualitzar el widget en disseny
#### Event Construct -> Llança l'execució

![[Pasted image 20241121111011.png|525]]

#### Event Run
- El podem fer servir perquè s'executi mentre tenim el widget obert. Al tancar el widget es destrueix
##### Mes Eines
- Scroll Box
- Tree View
- ComboBox
- TextBox

#25/11/2024

Mirar MVVM Plugin (UMG ViewModel)

Al utilitzar un delegat hem de pasarli la referencia a ell mateix. Ho podem fer en el bind amb el CustomEvent
![[Pasted image 20241206213745.png|500]]
En el propi control del widget posar la referencia
![[Pasted image 20241208111954.png]]

>[!tip]
>Intentem que les interficies nomes tinguin components, així queda més net
>![[Pasted image 20241208105457.png]]

En Bluprints treballem amb User Widgets en c++ nomes en Widgets
Per les transactions: Com a cosa interesant mirar en el motor que fan el ctl +z / ctl +y

#### SafeZone
- el control safe zone ens permet embolicar el widget i podem modificar els padding 
 ![[Pasted image 20241208123524.png]]


#26/11/2024

Fer exercicis
BoundingBox -> 

2, 4
Ejemplo 2: Listar en un EditorUtilityWidĀet todas las Texturas del proyecto. Además, a su derecha, indicar en ÿorma de tabla la resolución que tienen por deÿecto. Marcar en rojo aquellas que no tenian un tamaño potencia de 2.

Ejemplo 4: Crear un Editor Utility WidĀet que añada desde el editor un preview del boundinĀ box de todos los elementos de la escena. Para esto, incluir un botón que Active/Desactive esta previsualización


Engine Programmer
* Sistemas

Exercici de agafar les propietats de dimension
![[Pasted image 20241126151903.png]]

Exercici 4
![[Pasted image 20241126152009.png]]

# Details / Property View

#### Single property view

En Blueprint fer servir el "Construct Object" en comptes de "CreateWidget"

