#24/09/2024


 ![[Diagrama de conexiones Unreal.drawio.png]]

Game Instance-> Objecte que es crea persistent. l'utilitzarem per pasar info entre nivells 
Game Mode -> Les regles del joc, el que s'ha de fer
Game State -> L'estat de la nostra partida.
Player controller -> El jugador, ordenador 
Character / Pawn -> Qui realment fa l'acció propia 
Player State -> Estat de com esta el player
HUD -> Gestor del que es veu a la UI (Widgets). Sempre que hem de parlar amb el character ho farem a traves del hud

New Editor Window
Net Mode -> Play as Listen Server -> fa el machmaking a la mateixa partida

Projecte de Third person -> Importem content First
Cambiem al GameMode: Edit BP_ThirdPersonGameMode el DefaultPawnClass a BP_FirstPersonShooter

En el GameModel cambiem el GameModeBaseClass a FirstPersonShooter

En el BluePrint de First Person Shooter cambiem l'esqueletal mesh per SKM_Manny i l'animation tb ABP_Many

Com que el character esta tan a servidor com client surt 4 vegades
Nodes
* Is Server
* has Authority / Switch has Authority

GetPlayerController [0] -> Es sempre el de la propia maquina
Per verificar que estem en local Igualem el Get Controller == Get PlayerController![[Pasted image 20241008163611.png]]

Set visibility 
Set Owner No see
Replicate (Sincronizat)

>[!info] Lema dels Videojocs: Falseja tot el que puguis

# Replicació
#### Replicar Fàcilment
* En el BP -> ClassDefaults -> Details -> ==**Replicate**==. Indica que te alguna cosa a replicar (sense especificar)
* En els detalls del SM -> Details -> ==**Component Replicates**.== Especifiquem que és el cub el que hem de replicar

#### Replicació manual
* En el BP -> ClassDefaults -> Details -> ==**Replicate**==. Indica que te alguna cosa a replicar (sense especificar)
* En la Custom function que fem -> Replicates tenim: 
	Not Replicated -> Local
	Multicast -> Executa a tots els custom events tan Server com Client
* Les variables també s'ha de replicar. Details -> Replication (Apareixen 2 boletes al blueprint)
#25/09/2024
El que s'ha de fer sempre en local i donarà problemes sempre al replicar: 
	Hi han unes propietats al BP (Net frequency) que es poden regular modificar pero no acaba de funcionar
	Fisiques, Animacions, Fx, Interfaz
# Creació de un HUD
Creem un LVL, un GM, un PC y un HUD
En el GM canviem els default per als nostres PC i HUD, DefaultPawnClass = none
Utilitzar el GM creat al nostre nou Level
Crear un BP User Widget
En el HUD -> Create Widget -> crear var -> Set var -> Add to Viewport
![[Pasted image 20241024101408.png|475]]
#### Session

1. Create -> Public conections 4
2. Join
3. Find ( Posar max results)
![Example de sessió|600](https://interfaceingame.com/wp-content/uploads/ark-survival-evolved/ark-survival-evolved-session-list-1920x1080.jpg)
Posar "Instance Editable" i "Expose in Spawn"
Open level -> Options "listen" -> Per poder unir altres jugadors![[Pasted image 20241010121311.png|306]]
>[!Important] Al probar el HUD Al **Net Mode** Hem d'executar en mode **StandAlone**
### Unirse a sessió
En el GM del Joc buscar la funció (a override) handle Starting New Player i fer SpawnActor del nostre Character, li diem que el posseixi
![[Pasted image 20241010151159.png|500]]

Fer funcions de referencia
![[Pasted image 20241010172435.png]]

#30/09/2024

* Per extendre un override, cridem tambe al pare

Alinear els nodes 
![[Pasted image 20241010181227.png]]

En les funcions li podem dir que ho faci només al client 
![[Pasted image 20241011085919.png]]
#### Widgets als Actors
* Podem afegir un Mesh de Widget i posar la class que es. 
* Propietat Space. World a l'objecte, Screen sempre visible a pantalla encara que no tinguis el focus

### Fluxe de comunicació amb el HUD

<svg version="1.1" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 495.59999084472656 98" width="495.59999084472656" height="98">
  <!-- svg-source:excalidraw -->
  
  <defs>
    <style class="style-fonts">
      @font-face {
        font-family: Excalifont;
        src: url(data:font/woff2;base64,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);
          }
    </style>
    
  </defs>
  <rect x="0" y="0" width="495.59999084472656" height="98" fill="#ffffff"></rect><g transform="translate(10 38) rotate(0 72.23035430908203 12)"><text x="0" y="16.9152" font-family="Excalifont, Segoe UI Emoji" font-size="19.2px" fill="#f08c00" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="alphabetic">Widget -&gt; HUD </text></g><g transform="translate(161 37) rotate(0 104.07994079589844 12.5)"><text x="0" y="17.619999999999997" font-family="Excalifont, Segoe UI Emoji" font-size="20px" fill="#1971c2" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="alphabetic">&lt;-&gt; Player Controller </text></g><g stroke-linecap="round"><g transform="translate(389 42) rotate(0 21.5 -8.5)"><path d="M-0.16 -0.11 C7.07 -3.02, 35.98 -14.48, 43.25 -17.37 M0.76 -0.64 C7.95 -3.49, 35.85 -14.12, 42.95 -16.86" stroke="#e03131" stroke-width="2" fill="none"></path></g><g transform="translate(389 42) rotate(0 21.5 -8.5)"><path d="M25.5 -1.7 C30.11 -5.4, 34.45 -9.19, 42.95 -16.86 M25.5 -1.7 C29.02 -4.83, 33.31 -8.71, 42.95 -16.86" stroke="#e03131" stroke-width="2" fill="none"></path></g><g transform="translate(389 42) rotate(0 21.5 -8.5)"><path d="M19.84 -16.47 C26.02 -16.08, 31.92 -15.8, 42.95 -16.86 M19.84 -16.47 C24.61 -16.33, 30.13 -16.97, 42.95 -16.86" stroke="#e03131" stroke-width="2" fill="none"></path></g></g><mask></mask><g stroke-linecap="round"><g transform="translate(395 57) rotate(0 22 8.5)"><path d="M-0.51 0.42 C6.94 3.34, 36.73 14.52, 44.23 17.31 M0.23 0.16 C7.67 2.94, 36.56 13.56, 43.92 16.5" stroke="#e03131" stroke-width="2" fill="none"></path></g><g transform="translate(395 57) rotate(0 22 8.5)"><path d="M20.34 16.14 C26.11 16.76, 30.91 17.29, 43.92 16.5 M20.34 16.14 C27.84 15.98, 35.46 16.51, 43.92 16.5" stroke="#e03131" stroke-width="2" fill="none"></path></g><g transform="translate(395 57) rotate(0 22 8.5)"><path d="M26.09 1.07 C30.61 4.85, 34.2 8.59, 43.92 16.5 M26.09 1.07 C31.79 5.78, 37.55 11.19, 43.92 16.5" stroke="#e03131" stroke-width="2" fill="none"></path></g></g><mask></mask><g transform="translate(454 10) rotate(0 15.799995422363281 12.5)"><text x="0" y="17.619999999999997" font-family="Excalifont, Segoe UI Emoji" font-size="20px" fill="#e03131" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="alphabetic">GM</text></g><g transform="translate(454 63) rotate(0 14.799995422363281 12.5)"><text x="0" y="17.619999999999997" font-family="Excalifont, Segoe UI Emoji" font-size="20px" fill="#e03131" text-anchor="start" style="white-space: pre;" direction="ltr" dominant-baseline="alphabetic">GS</text></g></svg>
>[!info] El player controller es crida abans de que el inicializar el HUD

Els Events
1.39

Node ==Set Actor Tick Enabled== Activa o desactiva el Event Tick de l'actor actual

Default Pawn, Si es replica???

#### Posar Mouse al Joc
Al PC_Main Menu
![[Pasted image 20241013143148.png]]
I en el PC Game
![[Pasted image 20241013150939.png]]
En Flush input -> reseteja els inputs de les pulsacions actives

Per eliminar un Widget -> El Garbage colector ho fa quan vol. Per assegurarnos que ho faci ho setejem a null
![[Pasted image 20241013153153.png]]

#14/102024

Los inputs son en el PC (Enhanced Input local)
Multicast -> Envia la info a tots els clients a la vegada menys al Server

Get Control Rotation -> Del character agafa la rotació del Rotation

9.25
9.52
10.09

#25/10/2024

# Master-Slave mode (Jocs Competitius)
Calcula totes les coses al servidor

Per exemple si fem una colisió en el client la treiem
![[Pasted image 20241025111003.png|525]]

# Joc es cooperatiu 
Es calcula al reves fa al reves. Els calculs al client 
![[Pasted image 20241029110536.png|176]]

Damage

#29/10/2024

Barra de vida

#30/10/2024