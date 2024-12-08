#26/09/2024
# Bibliografia
* C++ Primer
* Game Programming Patterns
* Effective modern C++
# Introducció

>[!info] 
>La diferencia entre un int i un int32 es que int32 sabem que ocupa un espai de memoria concret, en canvi int la que hi hagui lliure a la memoria

A Unreal -> Desmarcar Live coding ![[Pasted image 20241007140422.png]]

Al crear una classe a Unreal posem la class type Public
Cada modul es un .build.cs
Unreal crea la solució de VS (em base al automation tools)
```c++
#pragma once  
  
#include "CoreMinimal.h"  
#include "GameFramework/Actor.h"  
#include "TestTemplateActor.generated.h"  
 
UCLASS()  
class UTHUB_CPP2024_API ATestTemplateActor : public AActor  
{  
    GENERATED_BODY()
public:   
    // Sets default values for this actor's properties  
    ATestTemplateActor();  
protected:  
    // Called when the game starts or when spawned  
    virtual void BeginPlay() override;  
public:   
    // Called every frame  
    virtual void Tick(float DeltaTime) override;  
};
```
Una Classe és la abstracció d'un concepte
Pragma Once -> impedeix que s'inclogui les llibreries 2 vegades. Un .h el que fa es copiar el codi
UCLASS() -> Indiquem que al sistema de reflexión de Unreal que aquesta classe pertany a un UObject. Correspondencia al sistema de reflexió de UE de la classe (És una etiqueta que s'utilitza per identificar les propietats d'un objecte per defecte)
Sistema de reflexión -> Un sistema que permet obtenir informació del propi llenguatge. C++ no te un sistema de reflexión
Les Macros son etiquetes que es canvien quan compilem. 
Quan veiem UTHUB_CPP20224_API, aquesta macro ens indica que es un módul que esta dins un altre módul
GENERATED_BODY() -> Alla on es copiara tot el propi .h
virtual -> modificador polimòrfic. Que pot ser sobreescrita




**Classe** -> Abstracció
**Objecte** -> La instancia de la classe
**UClass** -> Correspondencia al sistema de reflexió de UE de la classe (És una etiqueta que s'utilitza per identificar les propietats d'un objecte per defecte)
**UObject** -> Correspondencia al sistema de reflexió de UE a l'Objecte 
L' UObject és la entitat més bàsica que unreal utilitza per exposar a l'editor (son punters)
Actor
Tot en que comença per U és un object (el UClass es fill de UObject)

Function in line -> Funció que es crea i declara al mateix lloc
:: -> Scope operator, el que va després pertany a l'scope anterior

UFUNCTION() -> Unreal crea un UObject que representa la funció
	Especificadors
		BluePrintCallable
		CallInEditor
		...
UPROPERTY(EditAnywhere)

Si modifiquem .h hem de copilar

Filtrar les categories de Log
UE_LOG(LogTemp, Display, TEXT("El valor es : %d"), someValue)

### Cadenes de text

==FString== → Cadena de caracters que es pasen per Const reference, son *caras* ( Reserva un troç de memoria)
==FName== → (Fa un fstring de name ) Un String esta pensat per noms propis (noms que es reaprofiten al editor) → Crea una taula de punters
==FText== → Serveix per localització NSLOCTEXT(”Namespace”, “UnlocalizedText”,”Full text”); → Diccionari
# Templates y subclases

Un template és un prototip que permet crear funcions.
Tot el que comença en T son templates
TArray (equivalent a vector en c++)

#01/10/2024

```c++
template<typename T, typename T2>
T Sum(T paramA, T2 paramB)
{
	return paramA + paramB;
}
```
### Casteig
static_cast -> El més basic. Int -> float
reinterpred cast -> Tan es el que hi hagui ho interpreta com vol.
const cast -> Treu l'especificador de l'element

### Templates per crear classes

Modificador **Protected** -> Pot accedir ell i només els fills 
Un Struct i una Classe es el mateix pero en un Struct tot és públic
Les funcions templates normalemtns es posen inline dins el .h. Si es posen al cpp s'ha de declarar al .h
Quan creem una classe en C++ s'inicialitza el constructor per defecte.

```c++
//Podem arribar a inicialitzar les variables d'una classe així:
Attribute<T>::Attribute() : Value(0)
//També en la declaració
public: T Value = 0;
// O en el constructor
Attribute<T>::Attribute() { Value = 0; }
// O bé 
Attribute<T>::Attribute(T InValue=0)
```

Error de linker

### Procés d'execució

Preprocesat -> quan es llegiex el codi i es posa el .h, les directives, etc, 
Compilacio -> quan es creen els objectes (clases, funcions, etc) .obj
Linkad -> unir objectes amb els noms

Els templates no poden ser part del sistema de reflexió perque no estan registrats com a tal.

#04/10/2024

Dependències duras → Es carrega en memoria
Dependències toves → punter que encara no s’ha carregat en memòria

Patró de composició -> Composar una llista d'atributs o propietats inicials que tindrà una entitat

[[Punters]]

Punter -> Una variable que emmagatzema una adreça de memòria
```c++
int variable = 10; // Reservem un espai de memoria de 4 bytes on hi haura 10
int* punter = &variable; // Reserva un espai en memoria on hi guarda l'adreça de memoria de la variable
```
Referencia -> Fa referencia a alguna cosa que ja existeix, te molt poc cost
```c++
int variable = 10; 
int& ref = variable; //Quan tenim & en el tipo, o sigui declarant la variable. És una referencia. Un Alias

Exemple:
int a = 10;
int& b = a;

b=20; 
a ?? es igual a 20
```
A Unreal sempre utlitzarem punters
Tot el que sigui mes gran de 8 bytes ho passarem com "Const Ref"
```c++
const VarType& VarName
```
La funció Emplace -> assigna i crea el constructor de la propietat

#07/10/2024

<aside> 💡

Raider → Ctl + t → Busca en tota la solució

</aside>

Una referencia sempre que hi ha alguna cosa, no pot ser mai null
En canvi si passem un punter podríem arribar a passar un null

Al passar referencies garantim que mai es passi un null.

Make Literal → crea un literal 

UPROPERTY() → Ja inicialitza a 0

*cap a 1h canvi inicialitzacio

### Gestió de memoria

Que és el ==Stack== (es la mes rapida) (Statica)→ És la pila on guardem totes les dades. El Stack necesita saber quanta memoria necessita. Al sortir desapareix 

==Heap== → Memoria dinàmica

int* pA = new int(10) → Creem un punter al Stack(statica). Al crear l'enter el creem al Heap (memoria dinamica). 
- Despres hem de fer un delete per alliberar el Heap i apuntar el punter a nullptr
-

int* a; → Es un punter
* /*a -> Lo que apunta a "a"
int& Ref ; → referencia (homer simpson) / Alias

int b;

&b; → direccio de l’element

int* C

“*c “ → retornam “De referenciar” → Retorna el bloc de memoria


#16/10/2024

>[!info] Perque una classe surti a unreal 
>UCLASS(Blueprintable, BlueprintType)

Si fem servir el new després hem de fer el delete
Manera tipica de reservar memoria en c++ 
``` c++
SomeStruct* structTest = new SomStruct;
delete structTest;
```
En Unreal no cal perque farem servir el garbage colector
``` c++
UObject* SomeObj = NewObject<UObject>();
```
# Contenedores

Coleciones de un mismo tipo.
Los contenedores funcionan como un TArray.
Un TArray es una classe que reserva espai a la memoria dinàmica. Te dos parametres el tipo de dada i la cantitat d'espai (per defecte guarda mes espai de memoria del que necessita (la mitad mes del que te). Quan esta tot ple i volem crear mes elements esborrar i torna a crear tot l'array (delete, new).
```c++
TArray<int> SomeIntCollection;
// Sabem amb seguretat que necessitem 20 elements
SomeIntCollection.Reserve(20);
```

>[!important] Punter & Referencia
>int* X-> <span style="color:rgb(255, 255, 0)">PUNTER</span>
>int& X -> <span style="color:rgb(255, 255, 0)">REFERENCIA</span>
>\*X -> <span style="color:rgb(255, 255, 0)">VALOR</span> del que hi ha a X (De referència)
>&X -> <span style="color:rgb(255, 255, 0)">ADREÇA</span> de memoria de X

Exemple
![[Pasted image 20241016100150.png]]

int* X -> PUNTER
 int& X -> REFERENCIA
 \*X -> VALOR
 &X -> ADREÇA
 
#17/10/2024

Les variables amb static es construeixen un únic cop i després no es tornen a executar.

Move operator -> && agafam 

### Sobrecarga de operadores
```c++
//En la clase declrem funcions de sobrecarrega per operadors
T& operator Simbol (params)
```


#21/10/2024 
![[Pasted image 20241021111440.png|950]]

10.36 canarias

#28/10/2024
# UClass

Quan tenim un objecte tenim una clase, que es una etiqueta que ens diu de quina clase es un objecte


UObject{
	UClass* ObjectClass;
}
![[Pasted image 20241028084517.png|325]]


## TSubClassof
- L'utilitzarem nomes per vincular a l'editor
- Permet filtrar les clases per un tipo de clase concreta
- Es equivalent a un punter a UClass
- L'utilitzarem en dos casos, quan creem una funció o un selector per spawnear coses

#31/10/2024

# CDO
Las propietats del constructor no es poden utilitzar perque despres ve el CDO i les matchaca

#### PostInitProperty
- funcio per modificar una propietat al constructor

Tot el que es UPROPERTY() s'inicialitza a 0;

#07/11/2024

# Subsistemas

6 - Singleton

Els singleton acostumen a utilitzarse como a sistema
Hem de tenir clar les regles d'accés.

Tipos Static / LazyLoading

Const garantitza que this no canvia

#08/11/2024

Un UObject ja crea una instancia de CDO per tant no pot ser Singleton perque crearia una nova instancia

#11/11/2024

Quan treballar amb punters doncs quan treballem amb clases Polimorfisme

UPROPERTY()
* Creea la variable com a referencia dura. A 0 i nullptr
* Al destroyActor actualiza les referencies a aquesta variable a null

IsValid() -> Mira si la variable apunta  a alguna cosa i s'utilitza

# Delegate
Es un objecte/instancia que apunta a un punter d'una funció que es pot executar quan vulgui

(9:27)

Observer Capitol 4 pagina 33

```cpp
//En el .h
DECLARE_DYNAMIC_MULTICAST_DELEGATE(FOnAttributeOwnerRegistered);

public:  
    FOnAttributeOwnerRegistered OnAttributeOwnerRegistered;

//En el cpp
OnAttributeOwnerRegistered.Broadcast();
```
Delegate Single -> Solo admite una función. Importa lo que esta al otro lado 
Delegate Multi -> Es un array de delegate single. Ens es igual el que executa

#12/11/2024

UCLASS(Blueprintable, BlueprintType)

UFUNCTION(BlueprintNativeEvent)  
void DoAction();  
  
UFUNCTION(BlueprintImplementableEvent)  
void DoAction_Event();

#18/11/2024

El constructor Nomes per inicialitzar cosese 
Els bindeos millor posarlos al BeginPlay

# PlayerCamaraManager
Gestiona tot el relacionat amb la camara al nivell

Amb OtherActor.DisableInput() Desactiva tots els inputs 

FTimeHandler Temp
GetWorld()->GetTimerManagert().SetTimer(temp., callback, segons, loop)

# Timer

Son delegates
Tenim el Timer manager
FTimeManaget -> Permet fer  timers
FTimeManager::SetTimer()

#19/11/2024

INDEX_NONE -> Macro/enum que ens dona -1

Per ampliar els exercicis
* Importar dades iniciale en DataTable **
https://dev.epicgames.com/community/learning/tutorials/Gp9j/working-with-data-in-unreal-engine-data-tables-data-assets-uproperty-specifiers-and-more

# Subsistemas
Gestor subsistema
Accions
components

Patron Comando

# BlueprintImplementableEvent & BlueprintNativeEvent

BlueprintNativeEvent -> Es pot sobreescriure desde cpp y despres a Blueprint
* DoSoemthing()
* DoSomething_Implementation()

UFUNTION(BlueprintImplentableEvent, meta="BeginPlay")
BlueprintImplementableEvent -> Implentacio de blueprint
Noramlemnt posem el sufix "Recieve"
![[Pasted image 20241119091636.png]]

2 casos de us del patro comando
1. Cas Immutable (CDO) -> DoAction(Instigator)
	* `TArray<TSubclassOf<UCustomActionBase>> Actions;`
2. Cas Mutable -> Crear newObject 
	* TArray<UCustomActionBase* > 



Sistema de percepcion
Sistema de movimiento

#25/11/2024

En un delegate que sempre torni el propi actor, per saber a qui es el responsable

#28/11/2024

Els events de Blueprint son funcions que tenen un delegate single
Al vincular el delagate per exemple al "Set timer" en realitat fa un Add a la funcio on enganxem

![[Pasted image 20241128110032.png|500]]


Sistema de percepcio
AIPerceptionSystem.h

#2/12/2024

Temps per treballar amb la practica 2
Teoria hora 8:32h
# Predicados

Las funcions son un bloc de memoria. Punter a memoria
Quan cridem a una funció el que fem es referencia un punter, per tant gastem memoria.
Les funcions Inline subtitueixen les dereferencia i posa el codi al executable. Per tant millorem la memoria pero augmentem l'executable.
Guardar una funció a un punter: 
tipo_retorn (*Funptr)(ParametreEntrada) = SomeFunction
```cpp
void (*Functor) (int) = Func1;  
int (*Functor)()= Func2;  
void (*Functor) (int* ,float)= Func3;  
AActor (*Functor) (FName) = Func4;
```

# Lambdas
Objecte que es poden cridar. Son especials perque es crea en memoria amb las particularitats dels parametres que es demanan
S'executen per coses simples, per pasar 
\[](int A) -> int
{

}

**TFunction** es un template que ens permet encapsular qualsesvol tipo de funcio callable (que tingui el ( ) )

#### Funcions
- Les funcions son punters a memoria
Lambda
* Objectes que es poden cridar
TFunction
* Callable
![[Pasted image 20241203110822.png]]

#### Typedef / Using
- Es fan per posar altres noms 
![[Pasted image 20241203112919.png]]

Exemple de lambda
Això seria un Delay
![[Pasted image 20241203113845.png]]

