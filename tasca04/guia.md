# Tasca 04: Instal·lació Windows Server 2025

En aquesta guia documentarem la instal·lació i configuració d'un servidor Windows Server 2025 dins d'un entorn virtualitzat, seguint els requisits especificats per TransLògic S.A.

## 1. Anàlisi dels requisits vs configuració real

### Requisits de Microsoft vs Configuració de la màquina virtual

Abans de començar la instal·lació, comparem els requisits mínims de Microsoft amb la nostra configuració:

| Component | Requisits Microsoft | Configuració VirtualBox | Coherent? |
|-----------|---------------------|-------------------------|-----------|
| **CPU** | Processador de 1,4 GHz de 64 bits | 4 processadors (CPUs) virtuals | ✅ Sí |
| **RAM** | 512 MB mínim (2 GB recomanat) | 8 GB (8192 MB) | ✅ Sí |
| **Disc dur** | 32 GB mínim | Disc principal: 32 GB<br>Disc secundari: 10 GB | ✅ Sí |
| **Xarxa** | Adaptador Ethernet | 2 interfícies:<br>1. Xarxa NAT<br>2. Host-Only | ✅ Sí |
| **Pantalla** | Monitor Super VGA (1024x768) | Comprovat durant instal·lació | ✅ Sí |

**Conclusió**: La nostra configuració supera amplament els requisits mínims de Microsoft i és coherent amb les bones pràctiques per a un servidor Windows Server 2025.

## 2. Configuració de la màquina virtual

### Creació de la màquina virtual bàsica

![Configuració bàsica de la VM](/tasca04/img_t04/captura1.png)

**Anàlisi**: Estem creant una màquina virtual anomenada "windows server" amb Windows Server 2025 Standard Evaluation. Observem que:
- S'utilitza una imatge ISO d'avaluació de Windows Server 2025
- El tipus és "Microsoft Windows" i la versió "Windows Server 2022 (64-bit)"
- S'ha seleccionat l'opció "Skip Unattended Installation" per fer una instal·lació manual

### Configuració de hardware

![Configuració de hardware](/tasca04/img_t04/captura2.png)

**Anàlisi**: Configurem els recursos hardware de la màquina virtual:
- **Memòria RAM**: 8192 MB (8 GB) - dins del rang recomanat
- **Processadors**: 4 CPUs virtuals - adequat per a un servidor
- L'opció "Enable EFI (special OSes only)" està marcada, necessària per a Windows Server 2025

## 3. Configuració d'emmagatzematge

### Disc principal de 32 GB

![Disc principal de 32 GB](/tasca04/img_t04/captura3.png)

**Anàlisi**: Configuració del disc principal del sistema:
- **Port SATA 0**: Disc principal del sistema operatiu
- **Mida virtual**: 32,00 GB (complint el requisit mínim de Microsoft)
- **Format**: VDI (VirtualBox Disk Image)
- **Ubicació**: `C:\Users\cfgm2smxa01\Documents\Windows server.vdi`

### Disc secundari de 10 GB

![Disc secundari de 10 GB](/tasca04/img_t04/captura4.png)

**Anàlisi**: Configuració del disc secundari:
- **Port SATA 2**: Disc secundari per dades
- **Mida virtual**: 10,00 GB
- **Tipus**: Normal (VDI)
- Opció "Dispositiu d'estat sòlid" marcada (SSD virtual)

## 4. Configuració de xarxa

### Primera interfície: Xarxa NAT

![Interfície NAT](/tasca04/img_t04/captura5.png)

**Anàlisi**: Configuració de l'Adaptador 1:
- **Connectat a**: Xarxa NAT
- **Tipus d'adaptador**: Intel PRO/1000 MT Desktop (82540EM)
- **Mode promiscu**: Denega
- **Cable connectat**: Sí
- Aquesta configuració permet connectivitat a Internet per a actualitzacions

### Segona interfície: Host-Only

![Interfície Host-Only](/tasca04/img_t04/captura6.png)

**Anàlisi**: Configuració de l'Adaptador 2:
- **Connectat a**: Adaptador de només l'amfitrió (Host-Only)
- **Nom**: VirtualBox Host-Only Ethernet Adapter
- Aquesta interfície permet comunicació directa entre l'amfitrió i la màquina virtual

## 5. Procés d'instal·lació del sistema operatiu

### Selecció d'idioma

![Selecció d'idioma](/tasca04/img_t04/captura7.png)

**Anàlisi**: Configuració de l'idioma d'instal·lació:
- **Language to install**: English (United States)
- **Time and currency format**: Spanish (Spain, International Sort)
- Aquesta configuració permet mantenir l'anglès com a idioma del sistema però amb formats espanyols

### Configuració de teclat

![Configuració de teclat](/tasca04/img_t04/captura8.png)

**Anàlisi**: Selecció del teclat:
- **Keyboard or input method**: Spanish
- Important per a una correcta entrada de caràcters especials com la "ñ" o accents

### Opció d'instal·lació

![Opció d'instal·lació](/tasca04/img_t04/captura9.png)

**Anàlisi**: Selecció del tipus d'instal·lació:
- **Install Windows Server**: Instal·lació completa
- **I agree everything will be deleted...**: Confirma l'acceptació de l'esborrat de dades existents
- Aquesta és una instal·lació neta sobre un disc buit

### Selecció d'edició

![Selecció d'edició](/tasca04/img_t04/captura10.png)

**Anàlisi**: Selecció de l'edició de Windows Server:
- **Operating System**: Windows Server 2025 Standard Evaluation
- **Option**: Windows Server 2025 Standard Evaluation (Desktop Experience)
- S'ha triat la versió amb Desktop Experience per tenir una interfície gràfica completa

### Acceptació de termes de llicència

![Termes de llicència](/tasca04/img_t04/captura11.png)

**Anàlisi**: Acceptació dels termes de llicència:
- Important notar que és una versió d'avaluació (preview)
- S'hi indica que no s'aplica la garantia limitada
- L'acceptació és necessària per continuar la instal·lació

### Configuració de l'administrador

![Configuració d'administrador](/tasca04/img_t04/captura12.png)

**Anàlisi**: Creació del compte d'administrador:
- **User name**: Administrator
- **Password**: P@ssw0rd (com indicat a la captura 13)
- És important utilitzar una contrasenya segura en entorns de producció

### Accés inicial al sistema

![Accés inicial](/tasca04/img_t04/captura13.png)

**Anàlisi**: Primera inici de sessió al sistema:
- S'utilitza l'usuari "Administrator" amb la contrasenya "P@ssw0rd"
- Pantalla inicial de Windows Server 2025 amb l'usuari administratiu

## 6. Post-instal·lació i configuració inicial

### Server Manager

![Server Manager](/tasca04/img_t04/captura14.png)

**Anàlisi**: El Server Manager és la consola principal d'administració de Windows Server:
- **Roles**: Ja s'han instal·lat 3 rols (probablement inclouen AD DS i DNS)
- **Server groups**: 1 grup creat automàticament
- **Servers total**: 1 servidor (el local)

### Canvi del nom de l'equip

![Propietats del sistema](/tasca04/img_t04/captura15.png)

**Anàlisi**: Configuració del nom de l'equip:
- **Full computer name**: DC01.translogic01.test
- **Domain**: translogic01.test
- El nom s'ha canviat a DCxx (on xx és el número de llista), en aquest cas DC01

## 7. Anàlisi de la configuració final

### Resum de la configuració implementada:

1. **✅ Hardware virtual**:
   - 8 GB RAM (8192 MB)
   - 4 processadors virtuals
   - Disc principal de 32 GB (SATA 0)
   - Disc secundari de 10 GB (SATA 2)
   - 2 interfícies de xarxa (NAT + Host-Only)

2. **✅ Sistema operatiu**:
   - Windows Server 2025 Standard Evaluation
   - Mode GUI (Desktop Experience)
   - Idioma US amb configuració i teclat en espanyol

3. **✅ Configuració de sistema**:
   - Nom d'equip: DC01.translogic01.test
   - Domini: translogic01.test
   - Usuari administratiu configurat
   - Adaptador NAT funcionant amb DHCP

4. **✅ Compliment de requisits**:
   - Supera tots els requisits mínims de Microsoft
   - Configuració adequada per a un servidor d'avaluació
   - Preparada per a la implantació en entorns de producció

### Observacions importants:

- La màquina virtual està configurada amb EFI, necessari per a Windows Server 2025
- La partició de disc utilitza el format GPT (necessari per a sistemes EFI)
- S'ha implementat la configuració dual de xarxa sol·licitada
- El sistema està preparat per a la instal·lació de rols de servidor addicionals

## 8. Recomanacions per a entorns de producció

1. **Seguretat**:
   - Canviar la contrasenya d'administrador per una més segura
   - Configurar el firewall de Windows
   - Habilitar actualitzacions automàtiques

2. **Xarxa**:
   - Configurar adreces IP estàtiques en entorns de producció
   - Considerar la implementació de VLANs per aïllar trànsit

3. **Emmagatzematge**:
   - Considerar RAID per redundància en entorns de producció
   - Implementar polítiques de backup regulars

4. **Monitorització**:
   - Configurar alertes de rendiment
   - Implementar registre centralitzat (log aggregation)

---

## **RESUM FINAL DE LA TASCA 04**

### **Èxits aconseguits:**

1. **✅ Màquina virtual creada** amb especificacions exactes
2. **✅ Windows Server 2025 instal·lat** en mode GUI
3. **✅ Configuració d'idioma** correcta (US amb teclat espanyol)
4. **✅ Nom d'equip canviat** a DC01 segons especificacions
5. **✅ Configuració de xarxa dual** implementada (NAT + Host-Only)
6. **✅ Discos configurats** correctament (32GB + 10GB)
7. **✅ Requisits de Microsoft complerts** i superats

### **Elements clau del sistema:**

- **Nom de l'equip**: DC01.translogic01.test
- **Sistema operatiu**: Windows Server 2025 Standard Evaluation
- **RAM**: 8 GB
- **CPU**: 4 processadors virtuals
- **Discs**: 32 GB (sistema) + 10 GB (dades)
- **Xarxa**: 2 interfícies (NAT + Host-Only)

### **Verificació final:**

- Tots els components hardware virtuals configurats correctament
- Sistema operatiu instal·lat i funcionant
- Configuració de xarxa operativa
- Usuari administratiu creat i accessible
- Server Manager funcionant correctament

### **Conclusió:**

Hem implementat amb èxit un servidor Windows Server 2025 en un entorn virtualitzat, complint tots els requisits especificats. La configuració és robusta, escalable i preparada per a la instal·lació de roles de servidor addicionals. Aquesta instal·lació serveix com a base per al desplegament dels servidors de TransLògic S.A. i com a guia de referència per a futures implementacions.
