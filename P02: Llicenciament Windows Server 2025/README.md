## Breu descripció

Mentre inicieu la vostra aventura emprenedora, cal seguir pagant factures. Gràcies a la gran feina que vau desenvolupar a **EverPia**, els responsables de la consultora han decidit donar-vos suport, passant-vos encàrrecs de clients que ara mateix no estan en condicions d’acceptar.

---

## Introducció al client

L'empresa **TransLògic S.A.**, dedicada a la logística regional, vol renovar la seva infraestructura de servidors a **Windows Server 2025**.

Actualment disposen d'un servidor físic antic que ha quedat obsolet i volen **virtualitzar tota la seva càrrega de treball** per millorar la disponibilitat.

### Detalls de la infraestructura

#### 🖥 Servidor Físic (Host)

- 1 unitat
- 2 processadors (CPUs)
- 12 nuclis físics per processador  
- **Total: 24 nuclis**

#### 💻 Càrrega de treball (Màquines Virtuals - VMs)

- 1 VM – Controlador de Domini (Active Directory)
- 1 VM – Servidor de Fitxers
- 1 VM – Servidor d'Impressores i Gestió Documental
- 1 VM – SQL Server (Base de dades de l'ERP)
- 8 VMs – Aplicacions de logística i terminals de magatzem

**Total: 12 màquines virtuals amb Windows Server**

#### 👥 Usuaris i dispositius

- 45 empleats en total
  - 30 treballadors d’oficina (PC + portàtil propi)
  - 15 mossos de magatzem (comparteixen 5 tauletes robustes en 3 torns)

---

## Encàrrec del client

Com a consultors informàtics, haureu de:

1. Analitzar el model de **llicenciament per nucli (core)** de Windows Server 2025.
2. Calcular el **cost total** segons les dues opcions principals:
   - Windows Server **Standard**
   - Windows Server **Datacenter**
3. Determinar quin tipus de **CAL (User vs Device)** és més econòmic per a l'empresa.
4. Justificar la decisió final basant-se en:
   - Costos
   - Escalabilitat futura
   - Funcionalitats (Storage Spaces Direct, SDN, etc.)
5. **Presentació al client:**  
   Crear una presentació (5-10 minuts) explicant la solució de forma clara per a un perfil no tècnic (el gerent de TransLògic).

---

## Materials i links de suport

- **UD6.AA1. Introducció a Windows Server** — Moodle 0224 SOX  
- Microsoft — *Precios y licencias de Windows Server* [link] [link] [link] [preus]
