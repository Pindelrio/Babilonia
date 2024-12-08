


*** 
#09/10/2024
Per passar un projecte de Blueprints a C ++, afegim un content en C++ o creem una classe.

# <span style="color:orange">Blueprints</span>

Podem heredar BP quan es creen (com a child) o be un cop creat a Details -> "Parent Object"
### ***Actor
* Representació física d'alguna cosa
* Els BP que hereden d'Actor es construeixen automàticament (al fer Spawn)
### ***Object
* Representació de la abstracció d'una classe. No te representació física, te molt poca cosa i en podem crear molts perquè no pesen. 
* Els BP que hereden de Object els construim amb en node "Construct Object From Class" 

>[!info] Level Blueprint
> No es recomenable utilitzar-lo. Es fa servir més aviat per fer proves. Es únic per nivell i te moltes restriccions

==Node **Create Event**== -> Crida functions i aïlla la funció
Set Timer By Event -> Loop
Set Timer By Name -> per casos aïllats, com quan no s'ha construït la funció 
### __Variables en Blueprints

Iconos  les Categories podem posar iconos amb Win + `
#### <span style="color:Aquamarine">Propietats </span>
==Instance Editable== -> Permet editar la variable fora del BP, en el world (Actors)
==Expose on Spawn== (Necessita que Instance Editable estigui activat) -> Exposa la variable com un Pin abans de que es crei al construct
==Call In Editor== -> S'ha d'activar en el sequencer
==Private== -> Per defecte totes les vars son publiques

Propietat **Variable Type** d'un Objecte
* Sempre que afegim un Soft Object hem de fer un "Load Asset" i un "cast to object"
![[Pasted image 20241010103055.png]]

#### <span style="color:Aquamarine">Structs i DataTables</span>
DataTables -> Son de lectura, sempre posar referencies toves (softLink)

	Nodes 
	* GetDataTableRow -> Retorna una fila en concret per key
	* GetDataTableRowNames -> Retorna la taula sensera

#### <span style="color:Aquamarine">Enums</span>
Llista finitia d'elements
Normalment per tipos
Podem fer Switch i ForEach
#### <span style="color:Aquamarine">Inputs</span>
Mapping context -> Mapejem les tecles per cada lloc anem, Cotxe, pistola, etc, 
Input actions -> Creem accions i les mapejem en un boto

#14/10/2024
# Funcions

Events -> No retornen res. Triggers. Es poden executar en local/Multicast/OnServer. Hi ha nodes que nomes estan a Events (Tot el que te veure amb events)
- Els events que venen de un BluePrint son Dispatchers. Ens podem subscriure (Bind) a un Dispatcher. 
Functions -> Pot tenir inputs, variables locals. La propietat Target es a qui pertany el Object.functionX()
- Pura: (Verd) No te linea de execució. S'executa cada vegada que la crides
- Impura: (Blau). Només d'executa un cop
Casos especials que només funcionen en el editor.
- Constructor script -> Salta cada vegada que modifiquem una propietat  o compilem.
- En els Widgets hi ha el PreConstruct que es el mateix que el "Construction Script"
Macros -> Seria un switch de linea de execucio. Semblats a les funciones sense variables locals. No poden ser pures. Tenen una opció de **Exe** que serveix per tenir varies lineas de temps. Normalment sera per comprovacions

### BP Interface
Es abstracte i s'executa com diguem a cada actor
10.00

Actor Component

#22/10/2024

1. ==Herencia==
* Tots els fills tenen la mateixa opció. Podem override. No es poden treure coses del pare
2. ==Components== (Petits blocs de codi)
- S'afageix a l'actor. comunicació semi abstracte (no hem de castejar). La logica es queda al component. Un component no pot tenir un component dins (Normalment no s'accedeix al pare)
3. ==Interface==
- La comunicació és abstracte. L'hem d'implementar a tots els Actors.

Base Interactable
* puerta
* door

# Sistema de danys
Event Any Damage 
 ![[Pasted image 20241022091815.png|300]]

Apply Damage (Exemple character) FYI: Pot estar diferent
- Owner -> Propietari, qui ha creat (PlayerController)
- Parent -> Pare (Character)
- Instigator -> Owner de l'objecte que ha fet la opcio, sempre controller 
- Causer -> ThirdPersonCharacter

Verificar sempre aquest nodes 
![[Pasted image 20241022092141.png|325]]

# Dispachers
Bengalas que es llençem desde una illa
![[Pasted image 20241022093724.png|500]]
![[Pasted image 20241022094120.png|400]]
#23/10/2024

# Widgets
En mode mon. En les imatges utlitzar la maxima size que pot tenir i reescalar. Evitem que ho comprimeixi per pixels
En mode pantalla.  si que s'ha de fer servir el Draw size 

PlayFab.com
yt1d.com -> Per baixar videos
see of thieves

9.40
Plugin ==Common UI== 

Redimensionar
Ctl dret + ratoli

Scale Box -> Mante el tamany del contingut que hi ha dins
- Fill, estira
- S
-Size box -> Especifiquel el tamany