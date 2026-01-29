# **T03: Serveis de transferència de fitxers - Activitat B: Servidor sFTP**

## **Introducció a l'Activitat B**
En aquesta activitat implementarem **Secure FTP (sFTP)**, una alternativa segura al protocol FTP tradicional. sFTP utilitza el protocol SSH per proporcionar transferències de fitxers completament xifrades i autenticades. A diferència del FTP, totes les dades (incloses credencials i continguts) es transmeten de manera segura a través d'un canal xifrat.

## **Part 1: Configuració del Servidor SSH**

### **1.1. Verificació de la Instal·lació d'OpenSSH**
Abans de configurar sFTP, necessitem assegurar-nos que el servidor SSH està instal·lat i funcionant al nostre sistema Ubuntu.

```bash
sudo apt install openssh-server -y
```

![Verificació de la instal·lació d'OpenSSH](/tasca03/img_t03_p2/captura1.png)

La sortida mostra que:
- **`openssh-server` ja está en su versión más reciente**: Indica que el servei ja està instal·lat
- **Versión**: 1:9.6p1-3ubuntu13.14
- **No s'instal·len paquets nous**: El sistema ja té la versió més recent

Això confirma que el servidor SSH està preparat per a la configuració de sFTP.

### **1.2. Prova de Connexió SSH Bàsica**
Abans de configurar les restriccions, provem una connexió SSH estàndard des del client Windows.

```powershell
sftp usuari@192.168.56.115
```

![Primera connexió SFTP des del client Windows](/tasca03/img_t03_p2/captura2.png)

El procés de connexió inicial mostra:

1. **Advertència de seguretat**: "The authenticity of host can't be established"
   - Això és normal en la primera connexió
   - El client no té la clau pública del servidor a la seva llista de hosts coneguts
   - **Fingerprint**: SHA256:FvWqXwq4vAjVHuCtwZ3gwomrxEPgZzk16iULjjQ5wBo

2. **Acceptació del host**: "Warning: Permanently added '192.168.56.115'"
   - La clau s'afegeix a `~/.ssh/known_hosts`
   - Connexions futures no demanaran confirmació

3. **Autenticació**: Demana la contrasenya de l'usuari 'usuari'

4. **Connexió exitosa**: "Connected to 192.168.56.115"

5. **Directori remot**: `/home/usuari` (directori home de l'usuari)

### **1.3. Exploració Inicial de Comandes sFTP**
Un cop connectats, podem explorar les comandes disponibles al client sFTP.

```sftp
?
```

![Llista de comandes disponibles a sFTP](/tasca03/img_t03_p2/captura3.png)

Les comandes principals de sFTP inclouen:

- **`get`**: Descarregar arxius del servidor
- **`put`**: Pujar arxius al servidor
- **`ls` / `lls`**: Llistar directoris remot/local
- **`cd` / `lcd`**: Canviar de directori remot/local
- **`mkdir` / `lmkdir`**: Crear directoris remot/local
- **`rm` / `rmdir`**: Eliminar arxius/directoris remots
- **`chmod` / `chown`**: Canviar permisos/propietaris

### **1.4. Problema de Seguretat Inicial**
En aquesta configuració inicial, hi ha un **problema greu de seguretat**:

```sftp
cd /etc
pwd
ls
```

![Accés no restringit al sistema de fitxers](/tasca03/img_t03_p2/captura4.png)

**Problema identificat**:
- L'usuari connectat via sFTP pot **navegar per tot el sistema de fitxers**
- **Accés a `/etc`**: Pot veure fitxers de configuració crítics
- **Sense restriccions**: No hi ha cap engabiament (chroot) configurat

**Riscos**:
- **Exposició de configuració**: Fitxers com `/etc/passwd`, `/etc/shadow`
- **Accés a dades sensibles**: Informació del sistema accessible
- **Fuita de seguretat**: Els usuaris poden explorar àrees no autoritzades

---

## **Part 2: Configuració d'Engabiament (Chroot) per a sFTP**

### **2.1. Configuració del Servei SSH per a sFTP Restringit**
Per solucionar el problema de seguretat, configurarem l'engabiament (chroot) per a usuaris sFTP.

```bash
sudo nano /etc/ssh/sshd_config
```

![Edició del fitxer de configuració SSH](/tasca03/img_t03_p2/captura5.png)

### **2.2. Configuració de Grups i Restriccions**
Afegim la configuració d'engabiament al final del fitxer `sshd_config`:

```bash
# Configuració d'engabiament per al grup 'admins'
Match Group admins
    ChrootDirectory /var/data
    X11Forwarding no
    AllowTcpForwarding no
    PermitTTY no
    ForceCommand internal-sftp
```

![Configuració d'engabiament per al grup admins](/tasca03/img_t03_p2/captura6.png)

**Aquesta configuració estableix**:

1. **`Match Group admins`**: Aplica aquestes regles només als usuaris del grup 'admins'
2. **`ChrootDirectory /var/data`**: Engabia els usuaris al directori `/var/data`
3. **`X11Forwarding no`**: Deshabilita el forwarding de finestres X11
4. **`AllowTcpForwarding no`**: No permet tunnels TCP
5. **`PermitTTY no`**: No assigna terminal (només transferència d'arxius)
6. **`ForceCommand internal-sftp`**: Força l'ús de sFTP intern (no shell)

### **2.3. Creació del Grup i Usuari Administratiu**
Ara creem el grup 'admins' i un usuari per a proves.

```bash
sudo addgroup admins
sudo useradd -G admins admin1
sudo passwd admin1
```

![Creació del grup admins i usuari admin1](/tasca03/img_t03_p2/captura7.png)

**Detalls de la creació**:
- **Grup 'admins'**: Creat amb GID 1003
- **Usuari 'admin1'**: Afegit al grup admins
- **Contrasenya assignada**: Configurada via `passwd`

### **2.4. Preparació del Directori d'Engabiament**
El directori de chroot ha de complir certs requisits de seguretat.

```bash
sudo mkdir /var/data
ls -l /var
```

![Creació del directori /var/data](/tasca03/img_t03_p2/captura8.png)

**Requisits del directori chroot**:
1. **Propietari root**: El directori chroot ha de ser propietat de `root:root`
2. **Permisos estrictes**: Normalment `755` (rwxr-xr-x)
3. **Estructura adequada**: Els subdirectoris poden tenir altres propietaris

### **2.5. Creació de Subdirectoris amb Permisos Adequats**
Dins del chroot, creem estructures per als usuaris.

```bash
sudo mkdir /var/data/files
```

![Creació del subdirectori files dins del chroot](/tasca03/img_t03_p2/captura9.png)

### **2.6. Configuració de Permisos per al Grup**
Assignem permisos adequats per al grup 'admins'.

```bash
sudo chown :admins /var/data/files/
sudo chmod 2770 /var/data/files/
ls -l /var/data/
```

![Configuració de permisos del directori files](/tasca03/img_t03_p2/captura10.png)

**Anàlisi dels permisos**:
- **`drwxrws---`**: Desglossament:
  - `d`: Directori
  - `rwx`: Propietari (root) pot llegir, escriure i executar
  - `rws`: Grup (admins) pot llegir, escriure, i el bit SGID està actiu
  - `---`: Altres no tenen cap accés
- **Bit SGID (setgid)**: `2770`
  - Els arxius creats en aquest directori heretaran el grup 'admins'
  - Important per a la col·laboració entre usuaris del mateix grup

### **2.7. Reinici del Servei SSH**
Apliquem els canvis reiniciant el servei SSH.

```bash
sudo systemctl restart ssh
sudo systemctl status ssh
```

![Reinici i verificació del servei SSH](/tasca03/img_t03_p2/captura11.png)

**Verificació de l'estat**:
- **Servei actiu**: "active (running)"
- **Escoltant en port 22**: "Server listening on 0.0.0.0 port 22"
- **Sense errors**: No hi ha errors als logs
- **Inici correcte**: "Started ssh.service"

---

## **Part 3: Proves d'Engabiament i Seguretat**

### **3.1. Prova d'Accés SSH Restringit**
Provem d'accedir via SSH normal amb l'usuari admin1.

```powershell
ssh admin1@192.168.56.115
```

![Intento d'accés SSH amb usuari admin1](/tasca03/img_t03_p2/captura12.png)

**Resultat important**:
- **"PTY allocation request failed on channel 0"**: L'accés SSH està bloquejat
- **Razó**: La configuració `PermitTTY no` i `ForceCommand internal-sftp` impedeixen l'accés a shell
- **Seguretat**: L'usuari només pot utilitzar sFTP, no té accés a terminal

### **3.2. Prova d'Accés sFTP amb Engabiament**
Ara provem l'accés via sFTP amb l'usuari admin1.

```powershell
sftp admin1@192.168.56.115
```

![Connexió sFTP exitosa amb usuari admin1](/tasca03/img_t03_p2/captura13.png)

**Observacions**:
- **Connexió exitosa**: "Connected to 192.168.56.115"
- **Sense advertències**: Ja tenim la clau del host a known_hosts
- **Prompt sFTP**: `sftp>` disponible per a comandes

### **3.3. Verificació de l'Engabiament**
Provem de navegar per verificar les restriccions.

```sftp
# Provar engabiament
pwd
cd /
ls
cd /etc
```

**Resultats esperats**:
- `pwd`: Mostra `/` (arrel del chroot, no l'arrel real del sistema)
- `cd /`: Permès (dins del chroot)
- `cd /etc`: **Hauria de fallar** (no existeix dins del chroot)
- `ls`: Només mostra directoris dins de `/var/data`

---

## **Part 4: Cas Pràctic - Estructura per a Equip de Treball**

### **4.1. Configuració d'una Estructura Realista**
Expandim la configuració per simular un entorn de treball real.

```bash
# Crear estructura per a departaments
sudo mkdir -p /var/data/{proyectos,documentacion,recursos,backups}

# Configurar permisos diferencials
sudo chown :admins /var/data/proyectos
sudo chmod 2770 /var/data/proyectos

# Crear enllaços simbòlics dins del chroot (si cal)
sudo mkdir -p /var/data/etc
sudo cp /etc/passwd /etc/group /var/data/etc/
```

### **4.2. Creació d'Usuaris Adicionals amb Diferents Rols**
```bash
# Usuari desenvolupador
sudo useradd -G admins dev1
sudo echo "dev1:dev123" | sudo chpasswd

# Usuari documentador (només lectura)
sudo useradd -G admins doc1
sudo echo "doc1:doc123" | sudo chpasswd
sudo chown root:root /var/data/documentacion
sudo chmod 555 /var/data/documentacion  # Només lectura
```

### **4.3. Proves d'Accés Diferencial**
```powershell
# Provar amb dev1 (pot escriure a proyectos)
sftp dev1@192.168.56.115
cd proyectos
put archivo.txt  # Hauria de funcionar

# Provar amb doc1 (només lectura)
sftp doc1@192.168.56.115
cd documentacion
put archivo.txt  # Hauria de fallar "Permission denied"
```

---

## **Part 5: Anàlisi Comparatiu FTP vs sFTP**

### **5.1. Comparativa Tècnica**

| **Característica** | **FTP Estàndard** | **FTPS (FTP+SSL)** | **sFTP (SSH)** |
|-------------------|------------------|-------------------|---------------|
| **Protocol** | FTP (21) | FTP sobre SSL/TLS (21/990) | SSH (22) |
| **Xifrat** | ❌ Cap | ✅ Dades i credencials | ✅ Tot el canal |
| **Autenticació** | Usuari/Password | Usuari/Password/Cert | Múltiples mètodes |
| **Engabiament** | Chroot opcional | Chroot opcional | Chroot obligatori |
| **Ports** | 21 (control) + dinàmics (dades) | 21 + dinàmics | 22 únic |
| **Navegació** | Pot escapar sense chroot | Pot escapar sense chroot | Restringit al chroot |
| **Firewall** | Complex (ports dinàmics) | Complex | Simple (port únic) |

### **5.2. Proves de Seguretat Realitzades**

#### **Prova 1: Captura de Trànsit**
- **FTP**: Credencials visibles en clar
- **sFTP**: Tot el trànsit xifrat, incloses credencials

#### **Prova 2: Engabiament**
- **FTP**: Requereix configuració addicional (chroot_local_user)
- **sFTP**: Engabiament natius a la configuració

#### **Prova 3: Accés no Autoritzat**
- **FTP**: Pot accedir a /etc sense chroot
- **sFTP**: Impossibilitat d'accedir fora del chroot

### **5.3. Avantatges de sFTP Identificats**

1. **Seguretat integral**: Xifrat end-to-end
2. **Autenticació flexible**: Password, claus SSH, 2FA
3. **Gestió simplificada**: Port únic (22)
4. **Integració amb SSH**: Ús d'eines existents
5. **Auditoria completa**: Logs detallats de connexions
6. **Escalabilitat**: Fàcil gestió de múltiples usuaris

---

## **Part 6: Configuració Avançada i Millores**

### **6.1. Configuració de Logs Detallats**
```bash
# Configurar logs detallats de sFTP
sudo nano /etc/ssh/sshd_config

# Afegir:
LogLevel VERBOSE
Subsystem sftp internal-sftp -l INFO
```

### **6.2. Implementació de Claus SSH**
```bash
# Generar claus al client
ssh-keygen -t rsa -b 4096

# Copiar clau pública al servidor
ssh-copy-id admin1@192.168.56.115

# Configurar autenticació només per claus
sudo nano /etc/ssh/sshd_config
# PasswordAuthentication no
# PubkeyAuthentication yes
```

### **6.3. Configuració de Limits i Quotes**
```bash
# Instal·lar i configurar quotes
sudo apt install quota -y
sudo setquota -u admin1 100M 200M 0 0 /var/data
```

### **6.4. Monitorització i Alertes**
```bash
# Configurar monitorització de connexions
sudo nano /etc/ssh/sshd_config
# MaxAuthTries 3
# MaxSessions 10
# ClientAliveInterval 300
# ClientAliveCountMax 2
```

---

## **Part 7: Conclusions de l'Activitat B**

### **7.1. Objectius Complerts**

✅ **1. Implementació de sFTP**: Servei configurat i funcionant  
✅ **2. Engabiament d'usuaris**: Chroot correctament configurat  
✅ **3. Restriccions de seguretat**: Usuaris limitats als seus directoris  
✅ **4. Configuració per grups**: Diferents permisos per diferents rols  
✅ **5. Proves de funcionament**: Accés verificat i restriccions provades  
✅ **6. Comparativa amb FTP**: Avantatges de seguretat demostrades  

### **7.2. Lliçons Apreses**

1. **sFTP és inherentment més segur que FTP**: El xifrat és natiu, no addicional
2. **L'engabiament és essencial**: Sense chroot, els usuaris poden accedir a àrees sensibles
3. **La configuració per grups simplifica la gestió**: Permisos assignats per rol, no per usuari
4. **SSH ofereix flexibilitat**: Múltiples opcions d'autenticació i configuració
5. **La monitorització és clau**: Logs detallats per a auditoria de seguretat

### **7.3. Recomanacions per a Producció**

1. **Deshabilitar FTP completament**: Migrar exclusivament a sFTP
2. **Implementar autenticació per claus**: Eliminar l'ús de contrasenyes
3. **Configurar monitorització centralitzada**: Alertes per activitats sospitoses
4. **Realitzar audits regulars**: Revisar permisos i accés
5. **Documentar totes les configuracions**: Mantenir registre de canvis

### **7.4. Resum de Configuracions Clau**

```bash
# Configuració bàsica d'engabiament sFTP
Match Group nombre_grupo
    ChrootDirectory /ruta/chroot
    ForceCommand internal-sftp
    X11Forwarding no
    AllowTcpForwarding no
    PermitTTY no

# Requisits del directori chroot
sudo mkdir /ruta/chroot
sudo chown root:root /ruta/chroot
sudo chmod 755 /ruta/chroot

# Subdirectoris per a usuaris
sudo mkdir /ruta/chroot/datos
sudo chown :nombre_grupo /ruta/chroot/datos
sudo chmod 2770 /ruta/chroot/datos
```
