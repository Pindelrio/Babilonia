# Introducció

#### Moduls
#### Plugins
 - Al *.uplugin* cambiar el Type per "Editor"
 - Afegir el modul al *.uproject*
 - Registrar el plugin al *ProjectName.Build.cs*
# Construction Script

- Es un script que esta dins dels ==Actors== principalment s’utilitza per inicialitzar coses petites
* S’executa sempre l’editor. 
* S'executa sempre que es canvia una propietat
# On Construction

- Equivalent al constructionScript en c++
# PostEditChangeProperty

# Blutility
- #### GetSelectedAsset/s
- #### SetEditorProperty
- #### SaveLoadedAssets
- #### GetSelectedAsset/sData
- #### GetAssetRegistry -> GetDependencies / GetReferencers
- #### GetAsset / GetAllAssets
- #### GetAssetsByClass
- #### GetClass
- #### GetSoftClassTopLevelAssetPath
- #### GetTagValue
- #### Parse into Array
- ### Get Asset tools -> Export Asset
- ### Make Array
- 

# Rutas
![[Pasted image 20241022200423.png]]
#### Package Name
* On es troba l'asset i com es diu
#### Package Path
* On es troba el asset
#### Asset Name
- Nom de la clase
#### Asset Class Path
- ##### **/Script/Engine.StaticMesh**
- <span style="color:rgb(255, 255, 0)">Script</span> significa que la clase que ve a continuació es de c++ i l'<span style="color:rgb(255, 255, 0)">Engine</span> es el nom de ENGINE_API (Módul d'on esta la clase)

- A C++ Podem fer servir el UStaticMesh::StaticClass()->GetClassPathName()
#### Com identifica Unreal les entitats del motor
* Quan copiem una propitat ens dona aqueta ruta: **/Script/Engine.StaticMesh**'/Game/StarterContent/Props/MaterialSphere.MaterialSphere'

La CLASSE+ la RUTA DEL PAQUET. El ".MaterialSphere" es la propietat de la clase on realment es guarden les dades de l'StaticMesh
**/Script/Engine.StaticMesh**'/Game/StarterContent/Props/MaterialSphere.MaterialSphere'

## ==Asset Class Path + Package Path + AssetName . AssetName==

## Ruta dels moduls
* La ruta de la nostra classe ACPPToolTest seria: "/Script/ToolsPlugin.CPPToolTest" o sigui \[codi c++\] + \[nom del modul\] + \[nom de la Classe sense U o A\]

# Llibreries
FPaths::ProjectSavedDir()
FFileHelper::SaveStringToFile
 