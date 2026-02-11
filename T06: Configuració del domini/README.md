## Breu descripció

### Introducció

Un cop tenim ja el nostre domini creat, el següent pas és **desplegar el domini**, és a dir, crear els diferents objectes que el formen: **grups, usuaris i màquines**.  

Aquí veurem la utilitat d’organitzar els objectes amb **Unitats Organitzatives (OU)**.

---

## Procediment pràctic

### 1️⃣ Estructura d’Unitats Organitzatives (OU)

- Crear una estructura d’unitats organitzatives coherent.
- Justificar la decisió presa en l’organització.

### 2️⃣ Estructura de grups

Definir la següent estructura de grups:

- `gestio`
- `magatzem`
- `gerencia`
- `personal`  
  - *Tots els grups anteriors han de ser membres d’aquest grup.*

### 3️⃣ Plantilles d’usuari

Crear una plantilla d’usuari per cadascun dels grups:

- Gestio  
- Magatzem  
- Gerencia  

Cada plantilla ha de tenir:

- La **pertinença al grup** definida.
- La **creació de la carpeta personal** configurada.

### 4️⃣ Usuaris de prova

- Definir un usuari de prova per cadascuna de les plantilles creades.

### 5️⃣ Equip client

- Aprovisionar un equip anomenat `PC1` dins la OU `equips`.
- Crear una **màquina virtual amb Windows 11** amb:
  - 4 GB de RAM
  - Disc amb espai suficient
  - Xarxa configurada en **NAT**
- Un cop creat l’equip, afegir-lo al **domini**.

### 6️⃣ Comprovació

- Comprovar el correcte funcionament iniciant sessió a l’equip client amb els **tres usuaris de prova**.

---

## Materials i links de suport

- **UD6.AA3 Desplegament** — Moodle 0224 SOX
