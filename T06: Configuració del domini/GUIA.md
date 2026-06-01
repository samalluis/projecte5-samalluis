# Guia de Configuració d'Infraestructura: Active Directory i Servidor de Fitxers

---

## 1. Creació d’Estructura d’Unitats Organitzatives (OU) 

### Procediment Operatiu
1. Inicialitzar la consola de gestió **Active Directory Users and Computers** (`dsa.msc`).
2. Crear un objecte d'Unitat Organitzativa (OU) arrel amb l'identificador corporatiu (ex: `Empresa`).
3. Dins de l'OU principal, instanciar de forma jeràrquica tres sub-OUs per a la segmentació d'actius:
   * `Grups`
   * `Usuaris`
   * `Equips`

### Justificació Tècnica
L'arquitectura lògica basada en Unitats Organitzatives és la condició inherent per a la **delegació de controls administratius** (permetent la gestió granular d'identitats per departament) i per a la focalització de directives de configuració mitjançant **Objectes de Política de Grup (GPO)**. Addicionalment, actuar com a contenidors lògics ofereix protecció nativa a nivell d'objecte contra l'esborrament accidental del directori.

---

## 2. Creació i Anidament de Grups de Seguretat  

### Configuració de Rols i Tipologies
Dins de l'OU `Grups`, es requereix la creació de les següents entitats:
* `gestio`
* `magatzem`
* `gerencia`
* `personal`

### Especificacions de l'Àmbit
* **Tipus de grup:** Seguretat (*Security*)
* **Àmbit del grup:** Global (*Global*)

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20211440.png?raw=true)

### Pertinença i Anidament de Seguretat (Nested Groups)
1. Accedir a les propietats de l'objecte de grup `personal`.
2. Navegar fins a la pestanya **Members** i executar l'ordre d'addició.
3. Integrar com a membres d'aquesta entitat els grups departamentals `gestio`, `magatzem` i `gerencia`.

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20211615.png?raw=true)

> **Nota d'Enginyeria:** Aquesta topologia d'anidament garanteix que qualsevol compte d'usuari vinculat a un grup departamental hereti de manera indirecta la condició de membre del vector general `personal`.

---

## 3. Provisionament i Compartició de la Carpeta Home 

### Disseny de l'Emmagatzematge Físic
1. **Adscripció de maquinari:** Afegir un volum de disc dur virtual de 5 GB d'emmagatzematge secundari al servidor d'infraestructura.
2. **Inicialització del volum:** Des de la consola **Disk Management** (`diskmgmt.msc`), inicialitzar el nou dispositiu, estructurar una taula de particions i aplicar un format de sistema de fitxers amb l'etiqueta de volum `DATA` (assignat normalment sota la unitat lògica `E:`).

### Desplegament del Recurs Compartit (SMB Share)
1. Crear el directori físic d'arrel a l'emmagatzematge secundari: `E:\personal`.
2. Habilitar la compartició de xarxa de l'objecte mitjançant el protocol SMB.
3. Configurar la seguretat del canal d'accés (Share Permissions) atorgant el bit de control de l'operació **Control Total (Full Control)** al grup institucional **Domain Users**.

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20211736.png?raw=true)

---

## 4. Estructuració de Permisos NTFS i Seguretat Avançada 

### Procediment de Hardening de l'Estructura de Fitxers
1. Obrir les propietats del directori físic `E:\personal` i navegar a la pestanya `Security` -> `Advanced`.
2. **Ruptura de l'Herència:** Executar la directiva **Disable inheritance**. En el prompt del sistema, seleccionar l'opció **Convert inherited permissions into explicit permissions on this object** per desvincular les ACLs de l'arrel del volum sense perdre el control actual.

### Definició d'ACLs Granulars de NTFS

| Entitat (SID) | Tipus de Permís | Àmbit d'Aplicació (Apply to) | Funcionalitat |
| :--- | :--- | :--- | :--- |
| **Domain Users** | Especial (Creació de carpetes / escriptura de dades) | **This folder only** (Només aquest directori) | Permet la instanciació inicial del directori personal de l'usuari, limitant qualsevol accés lateral. |
| **CREATOR OWNER** | Control Total (*Full Control*) | **Subfolders and files only** (Només subcarpetes i fitxers) | Garanteix que l'usuari propietari del directori resultant de la variable rebi l'administració absoluta sobre els seus fitxers. |
| **SYSTEM** | Control Total (*Full Control*) | **This folder, subfolders and files** | Component crític no eliminable per garantir l'òptima indexació del sistema i els processos d'Auditoria de Windows. |

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20212001.png?raw=true)

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20212451.png?raw=true)

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20212506.png?raw=true)

---

## 5. Disseny de Plantilles d'Usuari (User Templates) 

### Nomenclatura i Seguretat d'Objectes
Crear tres objectes de tipus usuari per actuar com a models de configuració base dins de l'OU `Usuaris`:
* `_gestio`
* `_magatzem`
* `_gerencia`

### Directives de Seguretat Obligatòries
* **Prefix nominal:** L'ús del guió baix (`_`) s'estableix com a patró de disseny per forçar l'ordenació alfanumèrica dels perfils a l'inici del llistat d'AD.
* **Estat del compte:** Configurar obligatòriament el paràmetre **Account is disabled** com a mecanisme de mitigació davant de vectors d'atac basats en l'ús il·lícit de comptes genèrics.

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20212716.png?raw=true)

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20212824.png?raw=true)

---

## 6. Automatització de Paràmetres en les Plantilles 

### Assignació Departamental
A les propietats de cada compte de plantilla (`_gestio`, `_magatzem`, `_gerencia`), navegar a la pestanya **Member Of** i adscriure el compte al seu grup de seguretat corresponent.

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20212854.png?raw=true)

### Automatització del Mapeig de la Carpeta Personal (Home Folder)
1. Accedir a la pestanya **Profile**, secció **Home folder**.
2. Seleccionar el botó de ràdio **Connect**.
3. Determinar el caràcter d'identificació de la unitat de xarxa virtual (`Z:`).
4. Configurar la ruta UNC absoluta utilitzant el següent patró basat en el directori d'infraestructura: **\DC\personal%username%**

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20213214.png?raw=true)

> **Directiva Tècnica Crítica:** L'ús de la variable d'entorn `%username%` és d'obligat compliment. El directori actiu resol dinàmicament aquesta cadena en el moment d'executar l'acció de duplicat, forçant la creació automàtica del directori amb el nom propi de la identitat final de l'usuari i inyectant les llistes d'accés de forma automatitzada.

---

## 7. Preaprovisionament d'Usuaris de Prova i Equips 

### Generació d'Identitats per Duplicitat
1. Executar l'acció **Copy** sobre cadascuna de les plantilles administratives configurades en la fase anterior (`_gestio`, `_magatzem`, `_gerencia`).
2. Completar els camps relatius al nom propi, cognoms i credencial d'accés (User Logon Name).
3. Establir una contrasenya robusta que compleixi les polítiques de domini i commutar l'estat del compte cap a **Account is enabled**.

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20213505.png?raw=true)

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20213538.png?raw=true)

### Preaprovisionament d'Actius de Programari
Dins de l'OU `Equips`, realitzar l'alta d'un nou objecte de tipus **Computer** amb l'identificador nominal d'host `PC1`. Aquesta fase de preaprovisionament garanteix la traçabilitat de la màquina abans de la seva annexió física al canal.

![](https://github.com/samalluis/projecte5-samalluis/blob/main/T06:%20Configuraci%C3%B3%20del%20domini/pics/Captura%20de%20pantalla%202026-06-01%20213620.png?raw=true)

---

## 8. Annexió al Domini i Auditoria de Funcionament 

> [NOTA!]
> No he sigut capaz de trobar una ISO de windows 11 que hem funcioni be al VirtualBox que tinc a casa, he probat amb les ISO oficials de win pero no podia instalar-les

### Procediment d'Unió d'Estació de Treball Client (Windows 11)
1. Accedir a la configuració d'interfície de xarxa en l'equip client i establir com a **DNS primari** de forma estàtica l'adreça IP exacta del controlador de domini (DC).
2. Modificar l'identificador de l'equip (*Computer Name*) a `PC1`.
3. Iniciar el procés de canvi d'entorn de grup de treball a Domini introduint el vector `foodlogistic.test` (o el domini arrel configurat).
4. Autenticar l'operació d'unió mitjançant les credencials del perfil Administrador del domini.
5. Executar el reinici forçat de l'estació de treball.

### Matriu de Validació i Auditoria Final
* **Autenticació local de xarxa:** Iniciar la sessió d'usuari a `PC1` utilitzant els perfils de prova creats en la fase de preaprovisionament.
* **Auditoria al Servidor:** Verificar que a la ruta física `E:\personal` del servidor s'han desplegat automàticament els directoris individuals vinculats als noms reals de cada usuari de prova.
* **Auditoria al Client:** Validar que, en obrir l'Explorador de fitxers de l'estació de treball, l'usuari té un accés operatiu transparent d'escriptura i lectura mitjançant el muntatge de la unitat de xarxa virtual de lletra `Z:`.
