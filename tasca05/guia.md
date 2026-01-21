# Tasca 05: Instal·lació del Domini Active Directory

En aquesta guia documentarem la instal·lació i configuració d'un domini Active Directory sobre Windows Server 2025, creant un nou bosc i promocionant el servidor com a controlador de domini.

## 1. Accés al Server Manager

### Dashboard inicial

![Dashboard del Server Manager](/tasca05/img_t05/captura1.png)

**Anàlisi**: Pantalla inicial del Server Manager després de la instal·lació de Windows Server 2025:
- **Roles instal·lats**: 3 rols detectats (probablement inclouen rols bàsics)
- **Server groups**: 1 grup creat automàticament
- **Servers totals**: 1 servidor (el local, DC01)
- Podem veure les opcions principals al menú esquerre: Local Server, All Servers, AD DS, DNS, etc.

## 2. Inici de l'Assistent d'Addició de Rols

### Pàgina inicial de l'assistent

![Assistent Add Roles and Features](/tasca05/img_t05/captura2.png)

**Anàlisi**: Pàgina inicial de l'Assistent per afegir rols i funcionalitats:
- **Requisits previs**: Ens recorda verificar:
  - Contrasenya forta per a l'administrador
  - Configuració de xarxa (adreces IP estàtiques)
  - Actualitzacions de seguretat instal·lades
- Podem saltar aquesta pàgina en futures execucions

### Selecció del tipus d'instal·lació

![Selecció del tipus d'instal·lació](/tasca05/img_t05/captura3.png)

**Anàlisi**: Selecció del tipus d'instal·lació:
- **Role-based or feature-based installation**: Per configurar un sol servidor
- **Remote Desktop Services installation**: Per infraestructura VDI
- Triem l'opció "Role-based or feature-based installation" per instal·lar AD DS

### Selecció del servidor destí

![Selecció del servidor destí](/tasca05/img_t05/captura4.png)

**Anàlisi**: Selecció del servidor on instal·larem els rols:
- **Server Pool**: Mostra tots els servidors gestionats
- **DC01**: El nostre servidor local amb IP 10.0.2.17
- **Operating System**: Windows Server 2025 Standard Evaluation
- Seleccionem DC01 com a servidor destí

### Selecció de rols del servidor

![Selecció de rols del servidor](/tasca05/img_t05/captura5.png)

**Anàlisi**: Llista completa de rols disponibles per a Windows Server 2025:
- **Active Directory Domain Services**: El rol que necessitem
- **DNS Server**: Important per a AD DS
- **DHCP Server**: Opcional per a gestió d'IPs
- **File and Storage Services**: Ja té 1 de 12 instal·lats
- Triem "Active Directory Domain Services"

## 3. Instal·lació d'AD DS

### Funcionalitats necessàries

![Funcionalitats necessàries per AD DS](/tasca05/img_t05/captura6.png)

**Anàlisi**: L'assistent detecta que AD DS requereix funcionalitats addicionals:
- **Group Policy Management**: Per a la gestió de polítiques
- **Remote Server Administration Tools**: Eines d'administració remota
- **AD DS Tools**: Eines específiques d'Active Directory
- L'assistent ofereix afegir-les automàticament. Acceptem "Add Features"

### Informació sobre AD DS

![Informació sobre AD DS](/tasca05/img_t05/captura7.png)

**Anàlisi**: Pàgina informativa sobre Active Directory Domain Services:
- **Propòsit**: Emmagatzemar informació d'usuaris, ordinadors i dispositius
- **Recomanacions**:
  - Instal·lar mínim 2 controladors de domini per redundància
  - AD DS requereix un servidor DNS
- Ens informa sobre Azure Active Directory com a alternativa

### Confirmació de les seleccions

![Confirmació de les seleccions](/tasca05/img_t05/captura8.png)

**Anàlisi**: Pàgina de confirmació abans de la instal·lació:
- **Active Directory Domain Services**: Seleccionat
- **Eines d'administració**: Incloses
- **Opció**: "Restart the destination server automatically if required"
- Podem exportar la configuració a un script PowerShell

### Progrés de la instal·lació

![Progrés de la instal·lació](/tasca05/img_t05/captura9.png)

**Anàlisi**: Instal·lació en curs:
- **Estat**: "Configuration required. Installation succeeded on DC01"
- **Nota**: "Additional steps are required to make this machine a domain controller"
- Necessitem promocionar el servidor a controlador de domini
- L'assistent ens ofereix fer-ho immediatament

## 4. Configuració del Controlador de Domini

### Notificació de configuració requerida

![Notificació de configuració requerida](/tasca05/img_t05/captura10.png)

**Anàlisi**: Després de la instal·lació, el Server Manager mostra una notificació:
- **Configuració requerida per AD DS**: Ens indica que cal configurar el servidor com a DC
- Podem fer clic a "Promote this server to a domain controller" per iniciar el procés

### Configuració del desplegament

![Configuració del desplegament](/tasca05/img_t05/captura11.png)

**Anàlisi**: Configuració inicial del desplegament d'AD DS:
- **Opció seleccionada**: "Add a new forest" (afegir un nou bosc)
- **Root domain name**: `translogic01.test` (on "01" és el número de llista)
- Estem creant un domini completament nou

### Opcions del Controlador de Domini

![Opcions del Controlador de Domini](/tasca05/img_t05/captura12.png)

**Anàlisi**: Configuració de les opcions del controlador de domini:
- **Forest functional level**: Windows Server 2025
- **Domain functional level**: Windows Server 2025
- **Capacitats**:
  - ✅ Domain Name System (DNS) server
  - ✅ Global Catalog (GC)
  - □ Read only domain controller (RODC)
- **DSRM password**: Configurem la contrasenya de restauració

### Opcions de DNS

![Opcions de DNS](/tasca05/img_t05/captura13.png)

**Anàlisi**: Configuració de DNS:
- **Advertència**: "A delegation for this DNS server cannot be created..."
- Aquesta advertència és normal quan creem un domini nou sense una zona pare existent
- No és crític per a un entorn de prova

### Opcions Addicionals

![Opcions Addicionals](/tasca05/img_t05/captura14.png)

**Anàlisi**: Configuració del nom NetBIOS:
- **NetBIOS domain name**: `TRANSLOGIC01`
- El nom NetBIOS es genera automàticament a partir del nom del domini
- Podem canviar-lo si és necessari, però deixem el valor per defecte

### Rutes dels arxius d'AD DS

![Rutes dels arxius d'AD DS](/tasca05/img_t05/captura15.png)

**Anàlisi**: Configuració de les rutes dels arxius d'Active Directory:
- **Database folder**: `C:\WINDOWS\NTDS`
- **Log files folder**: `C:\WINDOWS\NTDS`
- **SYSVOL folder**: `C:\WINDOWS\SYSVOL`
- Aquestes són les rutes per defecte recomanades

## 5. Resum i Verificació

### Revisió de les opcions

![Revisió de les opcions](/tasca05/img_t05/captura16.png)

**Anàlisi**: Pàgina de revisió final abans de la instal·lació:
- **Resum de la configuració**:
  - Primer DC en un nou bosc
  - Nom del domini: `translogic01.test`
  - Nom NetBIOS: `TRANSLOGIC01`
  - Nivells funcionals: Windows Server 2025
  - Global Catalog: Sí
  - DNS Server: Sí
- **Script PowerShell**: Podem veure i exportar el script d'automatització

### Verificació de pre-requisits

![Verificació de pre-requisits](/tasca05/img_t05/captura17.png)

**Anàlisi**: Resultat de la verificació de pre-requisits:
- ✅ **"All prerequisite checks passed successfully"**
- **Advertències**:
  1. L'adaptador de xarxa no té adreça IP estàtica
  2. No es pot crear delegació DNS (esperat per a un nou bosc)
  3. El servidor es reiniciarà automàticament
- Podem procedir amb la instal·lació

### Resultats de la instal·lació

![Resultats de la instal·lació](/tasca05/img_t05/captura18.png)

**Anàlisi**: Instal·lació completada amb èxit:
- ✅ **"This server was successfully configured as a domain controller"**
- El servidor es reiniciarà automàticament
- Després del reinici, el servidor serà un controlador de domini funcionant

## 6. Script PowerShell per a Automatització

### Script generat automàticament

```powershell
#
# Windows PowerShell script for AD DS Deployment
#

Import-Module ADDSDeployment
Install-ADDSForest `
-CreateDnsDelegation:$false `
-DatabasePath "C:\WINDOWS\NTDS" `
-DomainMode "Win2025" `
-DomainName "translogic01.test" `
-DomainNetbiosName "TRANSLOGIC01" `
-ForestMode "Win2025" `
-InstallDns:$true `
-LogPath "C:\WINDOWS\NTDS" `
-NoRebootOnCompletion:$false `
-SysvolPath "C:\WINDOWS\SYSVOL" `
-Force:$true
```

### Com utilitzar el script

1. **Guardar el script**: Desar com `Install-ADDSForest.ps1`
2. **Executar com a administrador**:
   ```powershell
   PowerShell.exe -ExecutionPolicy Bypass -File "C:\ruta\Install-ADDSForest.ps1"
   ```
3. **Proporcionar contrasenyes**:
   - Contrasenya de mode de restauració (DSRM)
   - La contrasenya de l'administrador es demanarà durant l'execució

## 7. Mètodes per copiar el script al repositori

### Opció 1: USB (mètode directe)

1. Connectar una unitat USB al servidor
2. Copiar l'script a la unitat USB
3. Transferir-lo a l'ordinador de desenvolupament

### Opció 2: Servei SSH (recomanat)

1. **Instal·lar OpenSSH Server a Windows Server**:
   ```powershell
   Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
   Start-Service sshd
   Set-Service -Name sshd -StartupType 'Automatic'
   ```

2. **Copiar l'script des del client Linux**:
   ```bash
   scp administrador@10.0.2.17:"C:\ruta\Install-ADDSForest.ps1" /tasca05/
   ```

### Opció 3: Compartició de fitxers

1. Habilitar compartició al servidor Windows
2. Crear una carpeta compartida
3. Accedir des del client mitjançant SMB

### **Conclusió:**

Hem instal·lat i configurat amb èxit un domini Active Directory complet en Windows Server 2025. El servidor ara funciona com a controlador de domini amb DNS integrat, preparat per a la gestió centralitzada d'usuaris, ordinadors i polítiques de grup. El script PowerShell generat permetrà replicar aquesta configuració de manera consistent en futurs desplegaments per a TransLògic S.A.
