# T03: Serveis de transferència de fitxers - Activitat A: Servidor FTP

## Introducció
Aquesta guia documenta el procés complet de configuració d'un servidor FTP (File Transfer Protocol) a Ubuntu Server. FTP és un protocol estàndard per a la transferència d'arxius entre client i servidor a través d'una xarxa TCP. En aquesta activitat configurarem un servidor FTP bàsic que permetrà accés tant anònim com autenticat per usuaris locals.

## Fase 1: Preparació i Configuració del Sistema

### Configuració de la Màquina Virtual
Abans de començar la instal·lació del servei FTP, és necessari configurar correctament la màquina virtual per tenir accés tant a Internet com a la xarxa interna.

![Configuració del primer adaptador de xarxa - NAT](/tasca03/img_t03/captura1.png)

El primer adaptador de xarxa es configura en mode **NAT** per proporcionar accés a Internet al servidor. Això és essencial per poder descarregar els paquets necessaris durant la instal·lació. El tipus d'adaptador seleccionat és Intel PRO/1000 MT Desktop, compatible amb la majoria de sistemes.

![Configuració del segon adaptador de xarxa - Host-Only](/tasca03/img_t03/captura2.png)

El segon adaptador es configura en mode **Host-Only**, creant una xarxa privada entre la màquina host i la màquina virtual. Aquesta configuració permet connectar-nos des del nostre equip físic al servidor FTP sense necessitat de configuracions complexes de xarxa. La interfície utilitzada és "VirtualBox Host-Only Ethernet Adapter".

### Actualització del Sistema Operatiu
Abans de procedir amb qualsevol instal·lació, assegurar que el sistema estigui actualitzat és una bona pràctica de seguretat i estabilitat.

```bash
sudo apt update && sudo apt upgrade -y
```

![Actualització del sistema Ubuntu Server](/tasca03/img_t03/captura3.png)

Aquesta comanda realitza dues accions:
1. **`apt update`**: Actualitza la llista de repositoris i paquets disponibles
2. **`apt upgrade -y`**: Aplica totes les actualitzacions disponibles automàticament

L'opció `-y` contesta "yes" a totes les preguntes durant l'actualització, automatitzant el procés.

### Verificació de la Configuració de Xarxa
Després de configurar les interfícies de xarxa a VirtualBox, cal verificar que estiguin funcionant correctament dins del sistema operatiu.

```bash
ip a
```

![Verificació de les interfícies de xarxa amb ip a](/tasca03/img_t03/captura4.png)

La sortida mostra dues interfícies de xarxa activades:
1. **enp0s3**: Assignada a l'adaptador NAT amb IP 10.0.2.15
   - Proporciona accés a Internet
   - Connexions sortints sense necessitat de configuració addicional
2. **enp0s8**: Assignada a l'adaptador Host-Only amb IP 192.168.56.115
   - Xarxa privada per a connexions internes
   - IP accessible des de la màquina física

Aquesta configuració de doble xarxa és ideal per a entorns de laboratori, permetent tant l'accés a Internet per a instal·lacions com la connexió des de l'equip host per a proves.

## Fase 2: Instal·lació del Servidor FTP

### Instal·lació del Paquet vsftpd
vsftpd (Very Secure FTP Daemon) és un dels servidors FTP més populars i segurs per a sistemes Unix/Linux.

```bash
sudo apt install vsftpd -y
```

![Instal·lació del servidor vsftpd](/tasca03/img_t03/captura5.png)

La comanda `apt install` descarrega i instal·la el paquet vsftpd juntament amb les seves dependències. L'opció `-y` accepta automàticament la instal·lació sense demanar confirmació.

### Procés d'Instal·lació
![Sortida detallada de la instal·lació de vsftpd](/tasca03/img_t03/captura6.png)

Durant la instal·lació, s'observen els següents detalls:
- **Paquets instal·lats**: `vsftpd` i `ssl-cert`
- **Espai utilitzat**: 380KB addicionals
- **Versió instal·lada**: vsftpd 3.0.5-0ubuntu3.1
- **Configuració automàtica**: El sistema crea enllaços simbòlics per iniciar el servei automàticament

El paquet `ssl-cert` s'instal·la automàticament com a dependència, proporcionant certificats SSL per a futures configuracions de FTP segur (FTPS).

### Verificació del Servei
Després de la instal·lació, és important verificar que el servei estigui actiu i funcionant correctament.

```bash
sudo systemctl status vsftpd
```

![Estat del servei vsftpd després de la instal·lació](/tasca03/img_t03/captura7.png)

La sortida mostra informació crítica:
- **Estat**: `active (running)` - El servei està en execució
- **Habilitat a l'inici**: `enabled` - S'iniciarà automàticament en reiniciar el sistema
- **PID**: 10307 - Process ID del dimoni vsftpd
- **Memòria utilitzada**: 708.0KB (pic de 1.4MB)
- **Arxiu de configuració**: `/etc/vsftpd.conf`

El servei s'ha iniciat correctament i està escoltant connexions FTP al port 21 per defecte.

## Fase 3: Preparació i Configuració Bàsica

### Creació de Còpies de Seguretat
Abans de modificar cap configuració, sempre és una bona pràctica crear còpies de seguretat dels fitxers originals.

```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.conf.original
```

![Creació de la còpia de seguretat original](/tasca03/img_t03/captura8.png)

Aquesta primera còpia preserva l'estat original del fitxer de configuració abans de cap modificació.

```bash
sudo cp /etc/vsftpd.conf /etc/vsftpd.bak
```

![Creació de la còpia de seguretat de treball](/tasca03/img_t03/captura9.png)

La segona còpia serveix com a punt de restauració ràpid si es comet algun error durant la configuració. Tenir múltiples còpies permet:
1. **Restaurar a l'estat original** després de proves
2. **Comparar configuracions** entre diferents versions
3. **Tornar enrere ràpidament** si alguna modificació causa problemes

### Edició del Fitxer de Configuració
El fitxer principal de configuració de vsftpd es troba a `/etc/vsftpd.conf`. Utilitzarem l'editor nano per modificar-lo.

```bash
sudo nano /etc/vsftpd.conf
```

![Obertura del fitxer de configuració vsftpd.conf amb nano](/tasca03/img_t03/captura10.png)

L'editor nano s'obre mostrant el contingut del fitxer de configuració. És un editor de text senzill però potent, ideal per a administradors de sistemes.

### Configuració Bàsica del Servei
![Contingut inicial del fitxer vsftpd.conf](/tasca03/img_t03/captura11.png)

Dins del fitxer de configuració, es poden veure les opcions principals que configurarem:
- **`listen_ipv6=YES`**: Habilita l'escolta en IPv6
- **`anonymous_enable=NO`**: Accés anònim deshabilitat per defecte (el canviarem a YES)
- **`local_enable=YES`**: Permet l'accés d'usuaris locals (descomentat)
- **`write_enable=YES`**: Permet operacions d'escriptura (descomentat)

Per configurar l'accés anònim, canviarem `anonymous_enable=NO` a `anonymous_enable=YES`. Això permetrà connexions FTP sense autenticació.

### Reinici del Servei
Després de modificar el fitxer de configuració, cal reiniciar el servei perquè els canvis tinguin efecte.

```bash
sudo systemctl restart vsftpd
```

![Reinici del servei vsftpd](/tasca03/img_t03/captura12.png)

La comanda `systemctl restart` atura i torna a iniciar el servei vsftpd, aplicant els canvis realitzats al fitxer de configuració.

### Verificació Post-Reinici
Sempre és important verificar que el servei ha reiniciat correctament després de canvis de configuració.

```bash
sudo systemctl status vsftpd
```

![Estat del servei vsftpd després del reinici](/tasca03/img_t03/captura13.png)

La sortida confirma que:
- El servei s'ha reiniciat correctament
- El nou PID és 10586 (indicant un nou procés)
- El servei continua actiu i en execució
- No hi ha errors en el registre del sistema

## Fase 4: Configuració de l'Accés Anònim

### Verificació del Directori FTP
Per defecte, vsftpd utilitza el directori `/srv/ftp` per a l'accés anònim. Cal verificar si existeix i quins permisos té.

```bash
sudo ls -la /srv/
```

![Llistat del contingut del directori /srv](/tasca03/img_t03/captura14.png)

La sortida mostra:
- **Directori `/srv/ftp`** ja existeix (creat automàticament durant la instal·lació)
- **Propietari**: `root`
- **Grup**: `ftp`
- **Permisos**: `drwxr-xr-x` (755) - El propietari té tots els permisos, altres poden llegir i executar

Si el directori no existís, caldria crear-lo manualment:

```bash
sudo mkdir -p /srv/ftp
```

![Creació del directori /srv/ftp](/tasca03/img_t03/captura15.png)

L'opció `-p` assegura que es creïn tots els directoris necessaris en la ruta.

### Assignació de Permisos Correctes
Per a l'accés anònim, el directori FTP ha de tenir els permisos adequats per assegurar-ne la seguretat.

```bash
sudo chown nobody:nogroup /srv/ftp
```

![Canvi de propietari del directori /srv/ftp](/tasca03/img_t03/captura16.png)

Aquesta comanda canvia el propietari i grup del directori:
- **Propietari**: `nobody` - Usuari amb mínims privilegis
- **Grup**: `nogroup` - Grup sense membres

Aquesta configuració assegura que l'accés anònim no pugui modificar fitxers del sistema ni accedir a àrees restringides.

### Creació de l'Estructura de Directoris
Per organitzar millor els fitxers disponibles via FTP, crearem una estructura de directoris dins de `/srv/ftp`.

```bash
sudo mkdir -p /srv/ftp/pub
sudo mkdir -p /srv/ftp/pub/files
sudo mkdir -p /srv/ftp/pub/music
sudo mkdir -p /srv/ftp/pub/pics
```

![Creació de l'estructura de directoris per FTP](/tasca03/img_t03/captura17.png)

L'estructura creada és:
- **`/srv/ftp/pub`** - Directori principal públic
- **`/srv/ftp/pub/files`** - Per a documents i arxius generals
- **`/srv/ftp/pub/music`** - Per a arxius d'àudio
- **`/srv/ftp/pub/pics`** - Per a imatges i gràfics

L'opció `-p` crea els directoris pares si no existeixen.

### Creació d'Arxius de Prova
Per poder provar el funcionament del servidor FTP, crearem alguns arxius de prova.

```bash
sudo echo "Informació del servidor FTP" | sudo tee /srv/ftp/pub/info.txt
```

![Creació de l'arxiu info.txt](/tasca03/img_t03/captura18.png)

Aquesta comanda utilitza `tee` per:
1. Crear l'arxiu `/srv/ftp/pub/info.txt`
2. Escriure-hi el text "Informació del servidor FTP"
3. Mostrar el contingut per pantalla (com a confirmació)

La utilització de `tee` amb sudo assegura que l'arxiu es crea amb permisos adequats.

### Creació d'Arxius Addicionals
```bash
sudo touch /srv/ftp/pub/files/document.txt
sudo touch /srv/ftp/pub/music/song.mp3
```

![Creació d'arxius addicionals per FTP](/tasca03/img_t03/captura19.png)

La comanda `touch` crea arxius buits amb els noms especificats:
- **`document.txt`** - Arxiu de text a la carpeta files
- **`song.mp3`** - Arxiu d'àudio a la carpeta music

Aquests arxius buits serviran com a lloc per a futurs continguts o per verificar que la navegació per directoris funciona.

### Creació d'una Imatge de Prova
Per simular un arxiu més gran (com una imatge), utilitzarem `dd` per crear un fitxer de 1MB.

```bash
sudo dd if=/dev/zero of=/srv/ftp/pub/pics/sun.jpg bs=1M count=1
```

![Creació d'un arxiu d'imatge de prova](/tasca03/img_t03/captura20.png)

La comanda `dd` copia dades del dispositiu d'entrada al de sortida:
- **`if=/dev/zero`** - Font de dades (bytes zeros)
- **`of=/srv/ftp/pub/pics/sun.jpg`** - Destinació i nom de l'arxiu
- **`bs=1M`** - Block size de 1 Megabyte
- **`count=1`** - Nombre de blocs a copiar (1MB total)

La sortida mostra:
- **Records in**: 1+0 (un registre completat)
- **Records out**: 1+0 (un registre escrit)
- **Bytes copiats**: 1,048,576 bytes (1MB)
- **Velocitat**: 1.1 GB/s (velocitat de memòria)

---

## Fase 4: Configuració de l'Accés Anònim (Continuació)

### Verificació dels Permisos del Directori FTP
Després de crear l'estructura de directoris i arxius de prova, cal verificar que tots els permisos siguin correctes.

```bash
sudo ls -la /srv/ftp/
```

![Verificació dels permisos del directori /srv/ftp](/tasca03/img_t03/captura21.png)

La sortida mostra:
- **Directori actual (.)**: Propietari `nobody:nogroup` amb permisos `drwxr-xr-x` (755)
- **Directori pare (..)**: Propietari `root:root`
- **Directori pub**: Propietari `root:root` amb permisos `drwxr-xr-x` (755)

L'estructura està preparada correctament:
- L'accés anònim es limita al directori `/srv/ftp`
- Els arxius dins de `pub` són accessibles en mode lectura
- Els permisos eviten que usuaris anònims puguin modificar arxius

## Fase 5: Configuració d'Accés Autenticat per Usuaris Locals

### Creació d'Usuaris de Prova
Per permetre l'accés FTP autenticat, necessitem crear usuaris locals al sistema.

```bash
sudo adduser prova1 --gecos "" --disabled-password
```

![Creació de l'usuari prova1](/tasca03/img_t03/captura22.png)

La comanda `adduser` crea un nou usuari amb les opcions següents:
- **`--gecos ""`**: No afegeix informació addicional (nom complet, telèfon, etc.)
- **`--disabled-password`**: Crea l'usuari sense contrasenya (la configurarem després)

La sortida mostra el procés de creació:
- **UID/GID**: Assignats automàticament del rang 1000-59999
- **Grup**: S'ha creat el grup `prova1` (1001)
- **Directori home**: `/home/prova1` creat automàticament
- **Fitxers de configuració**: Copiats des de `/etc/skel`

```bash
sudo adduser prova2 --gecos "" --disabled-password
```

![Creació de l'usuari prova2](/tasca03/img_t03/captura23.png)

Seguim el mateix procés per crear el segon usuari de prova:
- **Grup**: `prova2` (1002)
- **Directori home**: `/home/prova2`

### Assignació de Contrasenyes als Usuaris
Després de crear els usuaris, cal assignar-los contrasenyes per poder autenticar-se.

```bash
echo "prova1:password1" | sudo chpasswd
echo "prova2:password2" | sudo chpasswd
```

*Aquestes comandes no apareixen a les captures però són necessàries. La comanda `chpasswd` permet assignar contrasenyes des de línia de comandes.*

### Verificació de la Creació d'Usuaris
Per confirmar que els usuaris s'han creat correctament, podem revisar el fitxer `/etc/passwd`.

```bash
tail -2 /etc/passwd
```

![Verificació dels usuaris creats al fitxer /etc/passwd](/tasca03/img_t03/captura24.png)

La sortida mostra les darreres dues línies del fitxer `/etc/passwd`:
- **`prova1:x:1001:1001:,,,:/home/prova1:/bin/bash`**
  - `x`: La contrasenya xifrada està a `/etc/shadow`
  - `1001`: User ID (UID)
  - `1001`: Group ID (GID)
  - `,,,`: Camp GECOS buit
  - `/home/prova1`: Directori home
  - `/bin/bash`: Shell per defecte
- **`prova2:x:1002:1002:,,,:/home/prova2:/bin/bash`**

### Configuració del Servei per a Usuaris Locals
Ara cal editar el fitxer de configuració de vsftpd per habilitar l'accés d'usuaris locals.

```bash
sudo nano /etc/vsftpd.conf
```

![Edició del fitxer vsftpd.conf per configurar usuaris locals](/tasca03/img_t03/captura25.png)

### Configuració Específica per Usuaris Locals
![Configuració del fitxer vsftpd.conf per usuaris locals](/tasca03/img_t03/captura26.png)

Dins del fitxer de configuració, verifiquem que aquestes opcions estiguin correctament configurades:

1. **`anonymous_enable=NO`**: Hem canviat l'accés anònim a NO
2. **`local_enable=YES`**: Permet l'accés d'usuaris locals (descomentat)
3. **`write_enable=YES`**: Permet operacions d'escriptura (descomentat)

Aquesta configuració permet:
- **Només usuaris autenticats** poden accedir al FTP
- Els usuaris poden **pujar i baixar arxius**
- Cada usuari accedeix al seu **directori home per defecte**

### Reinici del Servei amb Nova Configuració
Després de modificar la configuració, cal reiniciar el servei per aplicar els canvis.

```bash
sudo systemctl restart vsftpd
```

![Reinici del servei vsftpd després de canvis de configuració](/tasca03/img_t03/captura27.png)

La comanda reinicia el servei vsftpd, aplicant els canvis realitzats al fitxer de configuració per habilitar l'accés només per usuaris locals.

## Fase 6: Proves d'Accés Autenticat

### Prova de Connexió Local
Podem provar la connexió FTP des del mateix servidor utilitzant el client FTP de línia de comandes.

```bash
ftp localhost
```

![Prova de connexió FTP local amb usuari prova1](/tasca03/img_t03/captura28.png)

El procés de connexió mostra:

1. **Connexió establerta**: "Connected to localhost"
2. **Banner del servidor**: "220 (vsFTPd 3.0.5)"
3. **Autenticació**:
   - Usuari: `prova1`
   - Contrasenya: `password1` (no es mostra per seguretat)
   - Resposta: "230 Login successful"
4. **Comprovacions**:
   - `pwd`: Mostra "/home/prova1" (directori home de l'usuari)
   - `put document.txt`: Puja un arxiu local al servidor

La transferència mostra:
- **Mode passiu**: "Entering Extended Passive Mode"
- **Transferència correcta**: "226 Transfer complete"
- **Dades transferides**: 29 bytes
- **Velocitat**: 1.25 MiB/s

### Configuració d'Engabiat (Chroot)
Per seguretat, és important restringir els usuaris FTP als seus propis directoris home, impedint-los navegar per tot el sistema de fitxers.

```bash
sudo nano /etc/vsftpd.conf
```

![Edició del fitxer vsftpd.conf per configurar chroot](/tasca03/img_t03/captura29.png)

### Configuració de Chroot
![Configuració de chroot al fitxer vsftpd.conf](/tasca03/img_t03/captura30.png)

Afegim o descomentem aquestes línies al fitxer de configuració:

```bash
chroot_local_user=YES
allow_writeable_chroot=YES
```

Aquesta configuració ofereix:
- **`chroot_local_user=YES`**: Tots els usuaris locals queden restringits al seu directori home
- **`allow_writeable_chroot=YES`**: Permet l'escriptura dins del directori chroot

**Important**: Sense `allow_writeable_chroot=YES`, el directori home no pot tenir permisos d'escriptura quan està chrooted, cosa que causaria errors.

### Aplicació de la Configuració Chroot
```bash
sudo systemctl restart vsftpd
```

![Reinici del servei vsftpd amb configuració chroot](/tasca03/img_t03/captura31.png)

El servei es reinicia per aplicar la configuració de chroot. Aquest canvi és crític per a la seguretat del servidor FTP.

### Prova del Funcionament de Chroot
Després de configurar el chroot, provem que l'usuari no pot sortir del seu directori home.

```bash
ftp localhost
```

![Prova del funcionament del chroot amb usuari prova1](/tasca03/img_t03/captura32.png)

La prova demostra el funcionament del chroot:

1. **Autenticació correcta**: "230 Login successful"
2. **Llistat del directori**: `ls` mostra només arxius del directori home
3. **Intent de canvi a /etc**: `cd /etc` retorna "550 Failed to change directory"
4. **Arxiu pujat anteriorment**: `document.txt` està present al directori

**Resultat**: L'usuari `prova1` està completament aïllat dins del seu directori `/home/prova1` i no pot accedir a altres parts del sistema.

### Verificació de l'Estructura de Directoris
Podem visualitzar l'estructura completa dels directoris FTP utilitzant l'eina `tree`.

```bash
sudo tree /srv/
```

![Visualització de l'estructura de directoris FTP amb tree](/tasca03/img_t03/captura33.png)

La sortida mostra l'estructura jeràrquica completa:
```
/srv/
└── ftp
    └── pub
        ├── files
        │   ├── document.txt
        │   └── info.txt
        ├── music
        │   └── song.mp3
        └── pics
            └── sun.jpg
```

**Resum**:
- **6 directoris** en total
- **4 arxius** de prova
- Estructura organitzada per tipus de contingut
- Tots els arxius accessibles via FTP anònim (quan està habilitat)

## Fase 7: Configuració del Client Windows

### Descàrrega de FileZilla Client
Per connectar-nos des d'un client Windows al servidor FTP, utilitzarem FileZilla, un client FTP gratuït i de codi obert.

![Pàgina de descàrrega de FileZilla Client](/tasca03/img_t03/captura34.png)

FileZilla ofereix:
- **Suport per múltiples protocols**: FTP, SFTP, FTPS
- **Interfície gràfica intuïtiva**
- **Funcionalitats avançades**: Transferències en paral·lel, gestió de cues
- **Multiplataforma**: Disponible per Windows, macOS i Linux

### Selecció de la Versió Adequada
![Selecció de la versió de FileZilla per Windows 64-bit](/tasca03/img_t03/captura35.png)

A la pàgina de descàrrega, seleccionem:
- **Versió**: 3.69.5 (estable més recent)
- **Plataforma**: Windows (64bit x86)
- **Instal·lador**: Inclou ofertes addicionals (cal desmarcar durant la instal·lació si no es desitgen)

### Configuració de la Connexió a FileZilla
Després d'instal·lar FileZilla, configurar una connexió ràpida al servidor FTP.

![Configuració inicial de FileZilla per connectar al servidor FTP](/tasca03/img_t03/captura36.png)

A la connexió ràpida de FileZilla, introduïm:
- **Servidor**: `192.168.56.115` (IP del servidor en xarxa Host-Only)
- **Nom d'usuari**: (en blanc per a connexió anònima)
- **Contrasenya**: (en blanc)
- **Port**: 21 (per defecte)

**Observacions de la connexió**:
- "El servidor no permet caràcters no ASCII": Advertència sobre codificació de caràcters
- "Registrat en": Connexió anònima establerta
- "Recuperant el listado del directori": Navegació per l'estructura de directoris

### Navegació per l'Estructura FTP
![Navegació per l'estructura de directoris FTP des de FileZilla](/tasca03/img_t03/captura37.png)

FileZilla mostra:
- **Sitio local** (esquerra): Directoris de l'equip Windows client
- **Sitio remoto** (dreta): Estructura del servidor FTP
- **Directori arrel FTP**: `/` (només conté el directori `pub`)
- **Estructura completa**: `/pub/files/`, `/pub/music/`, `/pub/pics/`

La interfície permet:
- **Arrossegar i soltar** arxius entre local i remot
- **Visualització detallada**: nom, mida, tipus, data modificació
- **Navegació jeràrquica**: Doble clic per entrar en directoris

### Accés als Arxius de Prova
![Accés als arxius dins de /pub/files des de FileZilla](/tasca03/img_t03/captura38.png)

Dins del directori `/pub/files`, podem veure els arxius de prova:
- **`document.txt`**: Arxiu de text creat anteriorment
- **`info.txt`**: Arxiu amb informació del servidor

**Observacions de la connexió**:
- "Servidor no segur, no soporta FTP sobre TLS": Advertència de seguretat
- "Conexión establecida": Connexió FTP estàndard (no xifrada)
- "Recuperando el listado del directorio": Operació completada amb èxit

## Fase 8: Instal·lació d'Eines de Diagnòstic

### Instal·lació de Wireshark
Per analitzar el trànsit de xarxa i comprovar que les dades FTP viatgen en clar (sense xifrar), instal·lem Wireshark.

```bash
sudo apt install wireshark
```

![Instal·lació de Wireshark al servidor Ubuntu](/tasca03/img_t03/captura39.png)

Wireshark és un analitzador de protocol de xarxa que permet:
- **Capturar paquets** en temps real
- **Analitzar protocols** de xarxa
- **Depurar connexions** i problemes de xarxa
- **Verificar seguretat**: Comprovar que les dades estiguin xifrades

**Nota**: Durant la instal·lació, es demana si els usuaris no privilegiats poden capturar paquets. És recomanable respondre "Sí" per a més facilitat d'ús.

### Instal·lació del Client FTP (Opció Alternativa)
També podem instal·lar el client FTP de línia de comandes per a proves alternatives.

```bash
sudo apt install ftp -y
```

![Instal·lació del client FTP de línia de comandes](/tasca03/img_t03/captura40.png)

Aquest client permet:
- **Connexions FTP** des de terminal
- **Scripting** i automatització
- **Proves bàsiques** de connexió i transferència
- **Compatibilitat** amb múltiples sistemes operatius

---

## Fase 8: Instal·lació d'Eines de Diagnòstic (Continuació)

### Instal·lació del Client FTP (Confirmació)
Comprovem que el client FTP ja està instal·lat al sistema.

```bash
sudo apt install ftp -y
```

![Confirmació que el client FTP ja està instal·lat](/tasca03/img_t03/captura41.png)

La sortida mostra que:
- **`ftp` ja está en su versión más reciente**: El paquet ja està instal·lat
- **Versión**: 20210827-4build1
- **Estado**: "fijado ftp como instalado manualmente"
- **Paquets obsolets**: Es suggereix eliminar alguns paquets ja no necessaris

Aquesta confirmació ens assegura que tenim el client FTP de línia de comandes disponible per a realitzar connexions de prova tant locals com remotes.

## Fase 9: Proves de Connexió Remota

### Connexió FTP des del Client
Ara provem de connectar-nos des d'un client remot (probablement una altra màquina virtual) al servidor FTP.

```bash
ftp 192.168.56.115
```

![Connexió FTP remota des del client al servidor](/tasca03/img_t03/captura42.png)

El procés de connexió mostra:

1. **Connexió establerta**: "Connected to 192.168.56.115"
2. **Banner del servidor**: "220 (vsFTPd 3.0.5)" - Mostra la versió del servidor
3. **Demanda d'autenticació**:
   - "Name (192.168.56.115:usuari): prova1" - Sol·licita nom d'usuari
   - "331 Please specify the password." - Demana contrasenya
   - "230 Login successful." - Autenticació correcta
4. **Informació del sistema remot**: "Remote system type is UNIX"
5. **Mode de transferència**: "Using binary mode to transfer files"
6. **Prompt FTP**: `ftp>` - Connexió preparada per acceptar comandes

**Aspectes destacats**:
- **IP del servidor**: 192.168.56.115 (configuració Host-Only)
- **Port utilitzat**: 21 (per defecte FTP)
- **Usuari autenticat**: prova1
- **Connexió no xifrada**: Totes les dades viatgen en clar

### Comandes disponibles en sessió FTP:
```bash
# Després de connectar, pots utilitzar:
ls          # Llistar contingut del directori remot
pwd         # Mostrar directori actual remot
cd <dir>    # Canviar de directori remot
get <arxiu> # Descarregar arxiu del servidor
put <arxiu> # Pujar arxiu al servidor
quit        # Sortir de la sessió FTP
```

## Fase 10: Anàlisi de Seguretat amb Wireshark

### Captura de Trànsit FTP
La captura de Wireshark mostra el procés complet d'una connexió FTP, revelant la falta de seguretat del protocol.

![Anàlisi del trànsit FTP amb Wireshark](/tasca03/img_t03/captura43.png)

### Anàlisi Detallada del Trànsit FTP:

#### **1. Establiment de la Connexió TCP (Three-Way Handshake)**
```
Frame 3: Client → Server: SYN (Synchronize)
Frame 4: Server → Client: SYN-ACK (Synchronize-Acknowledge)
Frame 5: Client → Server: ACK (Acknowledge)
```
- **Port origen client**: 60884 (port efímer)
- **Port destí servidor**: 21 (FTP)
- **Seq=0**: Número de seqüència inicial
- **Win=65535**: Mida de finestra (buffer)

#### **2. Resposta del Servidor FTP**
```
Frame 6: Server → Client: "220 (vsFTPd 3.0.5)"
```
- **Codi 220**: Service ready for new user
- **Banner**: Inclou informació del servidor (versió)
- **Vulnerabilitat**: Els atacants poden usar aquesta informació per atacs específics

#### **3. Autenticació (En clar - NO XIFRADA)**
```
Frame 8: Client → Server: "USER prova1"
```
- **Comanda USER**: Envia nom d'usuari en text clar
- **Visible a qualsevol**: Qualsevol a la xarxa pot veure l'usuari

```
Frame 10: Server → Client: "331 Please specify the password."
```
- **Codi 331**: Username OK, need password

```
Frame 12: Client → Server: "PASS usuari"
```
- **COM CRÍTICA**: La contrasenya es transmet en text clar
- **Contrasenya**: "usuari" (visible a la captura)
- **Vulnerabilitat greu**: Qualsevol sniffer de xarxa pot capturar les credencials

#### **4. Autenticació Exitosa**
```
Frame 13: Server → Client: "230 Login successful."
```
- **Codi 230**: User logged in, proceed
- **Sessió activa**: Client autenticat correctament

---

## **Resum Final de l'Activitat A: Servidor FTP**

### **Objectius Complerts**:
✅ **1. Instal·lació del servidor FTP**
   - vsftpd instal·lat i funcionant
   - Servei configurat per iniciar automàticament

✅ **2. Configuració d'accés anònim**
   - Directori `/srv/ftp` configurat amb permisos adequats
   - Estructura de directoris creada (`files/`, `music/`, `pics/`)
   - Arxius de prova disponibles

✅ **3. Configuració d'usuaris locals**
   - Usuaris `prova1` i `prova2` creats
   - Contrasenyes assignades
   - Accés autenticat funcionant

✅ **4. Implementació de seguretat bàsica**
   - **Chroot** configurat per restringir usuaris als seus homes
   - Permisos adequats per a directoris i arxius
   - Separació d'accés anònim/autenticat

✅ **5. Configuració de xarxa**
   - Adaptador NAT per a accés a Internet
   - Adaptador Host-Only per a connexions internes
   - IP 192.168.56.115 accessible des de l'host

✅ **6. Proves de funcionament**
   - Connexions locals des del servidor
   - Connexions remotes des de client
   - Transferències d'arxius funcionant

✅ **7. Anàlisi de seguretat**
   - Captura de trànsit amb Wireshark
   - Identificació de vulnerabilitats
   - Comprensió dels riscos de FTP no xifrat

**Conclusió**: Hem implementat amb èxit un servidor FTP funcional però hem identificat les seves limitacions de seguretat. Aquesta comprensió és fonamental per a l'Activitat B, on implementarem una solució més segura amb sFTP.
