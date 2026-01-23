# Guia Tècnica – Malware i Antimalware

Aquest document recull les captures i l'anàlisi realitzades durant la pràctica sobre malware i programari antimalware. S'expliquen els passos realitzats per comprovar el funcionament de les proteccions, la detecció de fitxers de prova com EICAR, l'execució controlada d'un ransomware de prova i l'anàlisi de comportaments de seguretat en Windows 11.

---

## 1. Configuració de proteccions del navegador i detecció del fitxer EICAR

En aquesta primera part es comprova el funcionament de les proteccions integrades del navegador (SmartScreen) i de Windows Defender amb el fitxer de prova EICAR.

### Configuració de seguretat del navegador

![Configuració del navegador](/T08/img/captura1.png)

En aquesta finestra es mostren diverses opcions de seguretat del navegador (probablement Microsoft Edge):
- Administració de certificats HTTPS/SSL.
- Protecció amb SmartScreen de Microsoft Defender.
- Correcció ortogràfica d'URL per evitar redireccions malignes.
- Opció per esborrar llistat de llocs permessos.
- HTTPS automàtic per a connexions més segures.
- Bloqueig de programari intimidatori i estafes mitjançant IA.

**Anàlisi:** Abans de poder descarregar el fitxer de prova EICAR, cal desactivar temporalment algunes d’aquestes proteccions (com SmartScreen) per permetre la baixada del fitxer, ja que el sistema el identificarà com a potencialment perillós.

---

### Pàgina web d’EICAR

![Pàgina web d’EICAR](/T08/img/captura2.png)

Es mostra la pàgina d’informació de l’organització EICAR, dedicada a la investigació en ciberseguretat i proves antivirus. S'ofereix contacte, descàrregues i enquestes sobre l’ús del fitxer de prova.

**Anàlisi:** D’aquesta web es baixarà el fitxer de prova EICAR, que serveix per verificar el correcte funcionament dels sistemes antimalware sense utilitzar malware real.

---

### Llistat de fitxers EICAR disponibles

![Fitxers EICAR disponibles](/T08/img/captura3.png)

Es mostren les diferents variants del fitxer de prova:
- `EICAR.COM` (68 bytes)
- `EICAR.COM.TXT` (68 bytes)
- `EICAR.COM.ZIP` (184 bytes)
- `EICAR.COM-2.ZIP` (308 bytes)

**Anàlisi:** Cada variant conté la mateixa signatura de prova, però empaquetada de formes diferents per comprovar si l’antivirus és capaç de detectar-la també dins d’arxius comprimits.

---

### Detecció del fitxer EICAR per part de Windows Defender

![Detecció d’EICAR per Windows Defender](/T08/img/captura4.png)

En intentar descarregar el fitxer `EICAR.COM.ZIP`, Windows Defender mostra una alerta indicant que s’ha detectat una amenaça i bloqueja la descàrrega.

**Anàlisi:** Això confirma que el sistema antimalware està actiu i funciona correctament amb el fitxer de prova estàndard.

---

### Panell de seguretat de Windows

![Panell de seguretat de Windows](/T08/img/captura5.png)

Es mostra el menú d’accions ràpides de “Seguretat de Windows”, amb opcions com:
- Executar examen ràpid
- Buscar actualitzacions de protecció
- Veure opcions de notificació
- Veure el panell de seguretat

**Anàlisi:** Des d’aquí es pot accedir a la configuració i estat de les proteccions del sistema.

---

### Vista general de la seguretat de Windows

![Vista general de la seguretat](/T08/img/captura6.png)

El panell indica que la protecció en temps real està **desactivada**, cosa que deixa el dispositiu vulnerable. També suggereix iniciar sessió amb un compte Microsoft per millorar la seguretat.

**Anàlisi:** Per poder descarregar el fitxer EICAR sense que sigui bloquejat, s’haurà de desactivar la protecció en temps real momentàniament.

---

### Configuració de l’antivirus i protecció contra amenaces

![Configuració d’antivirus](/T08/img/captura7.png)

S’indica de nou que la protecció en temps real està desactivada. També es mostra que les actualitzacions de definicions de virus estan al dia (13/01/2026).

**Anàlisi:** Aquesta finestra permet activar/desactivar la protecció en temps real i gestionar les actualitzacions de protecció.

---

### Descàrrega bloqueada per Windows Defender

![Descàrrega bloqueada](/T08/img/captura8.png)

Es veu que el fitxer `eicar_com.zip` no s’ha pogut descarregar perquè s’ha detectat un virus.

**Anàlisi:** Windows Defender actua en temps real (si està activat) i impedeix la baixada de fitxers amb signatures malicioses conegudes.

---

## 2. Compressió del fitxer EICAR en diferents formats

Després de desactivar temporalment la protecció en temps real, es descarrega el fitxer EICAR i es comprimeix en diferents formats per comprovar si l’antivirus el detecta dins dels arxius comprimits.

### Descàrrega de WinRAR

![Descàrrega de WinRAR](/T08/img/captura9.png)

Per comprimir el fitxer en diferents formats, es descarrega l’eina WinRAR 7.13 per a Windows x64.

**Anàlisi:** Cal una eina de compressió per crear arxius ZIP, 7Z, TAR, etc., i provar si l’antivirus escaneja el contingut intern.

---

### Configuració de la instal·lació de WinRAR

![Instal·lació de WinRAR](/T08/img/captura10.png)

Durant la instal·lació es poden escollir els tipus d’arxius a associar amb WinRAR i les opcions d’integració amb l’explorador de Windows.

**Anàlisi:** Es recomana deixar les opcions per defecte per tenir accés ràpid a les funcions de compressió i descompressió des del menú contextual.

---

### Carpeta de descàrregues amb els fitxers EICAR

![Carpeta de descàrregues](/T08/img/captura11.png)

Es veuen els fitxers descarregats i creats:
- `eicar` (fitxer original)
- `eicar_com` (carpeta)
- `winrar-x64-713es.exe` (instal·lador de WinRAR)

**Anàlisi:** El fitxer original `eicar` és el fitxer de prova en format COM.

---

### Arxius comprimits creats amb el fitxer EICAR

![Arxius comprimits amb EICAR](/T08/img/captura12.png)

S’han creat tres arxius comprimits:
- `eicar_en_zip` (ZIP)
- `eicar_en_7zip.7z` (7Z)
- `eicar_en_tar.tar` (TAR)

**Anàlisi:** L’objectiu és verificar si l’antivirus detecta el codi EICAR dins d’aquests arxius comprimits quan s’activa la protecció en temps real.

---

## 3. Prova de ransomware de prova (PSRansom)

Aquesta part simula un atac de ransomware mitjançant un script de PowerShell per comprovar les proteccions integrades de Windows 11.

### Carpeta Documents amb fitxers de prova

![Carpeta Documents](/T08/img/captura13.png)

Abans d’executar el ransomware de prova, es creen alguns fitxers TXT a la carpeta Documents (`document1`, `fortnite`, `hola`).

**Anàlisi:** Són fitxers innocents que serviran com a víctimes del xifratge simulat.

---

### Carpeta de descàrregues amb el script PSRansom

![Descàrregues amb PSRansom](/T08/img/captura14.png)

Es veu el fitxer `PSRansom.ps1` entre les descàrregues, juntament amb els arxius comprimits d’EICAR.

**Anàlisi:** El script PSRansom permet simular un atac de ransomware per provar les proteccions del sistema.

---

### Canvi de política d'execució de PowerShell

![Canvi d'execution policy](/T08/img/captura15.png)

Per poder executar scripts de PowerShell, cal canviar la política d'execució amb l'ordre:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted
```

**Anàlisi:** Això permet l'execució de scripts no signats, però cal tenir cura, ja que disminueix la seguretat del sistema.

---

### Execució del ransomware de prova (xifratge)

![Execució del ransomware](/T08/img/captura16.png)

S'executa el script amb l'opció d'encryption:

```powershell
.\PSRansom.ps1
```

El script simula el xifratge dels fitxers de la carpeta Documents, genera una clau de recuperació i guarda un fitxer `readme.txt`.

**Anàlisi:** El script no es connecta a un servidor de comanda i control real (ja que està caigut), però genera una clau AES de 256 bits per xifrar els fitxers localment.

---

### Carpeta Documents després del xifratge

![Documents xifrats](/T08/img/captura17.png)

Després de l'execució, els fitxers de la carpeta Documents han canviat l'extensió a `.psr` i el seu contingut està xifrat.

**Anàlisi:** Això simula el comportament d'un ransomware real, que xifra els fitxers i canvia l'extensió per exigir un rescat.

---

### Contingut del fitxer readme.txt amb la clau

![Fitxer readme.txt](/T08/img/captura18.png)

El fitxer `readme.txt` conté la clau de desxifrat (en format codificat) generada durant l'atac simulat.

**Anàlisi:** En un ransomware real aquesta clau s'enviaria a un servidor remot, però en aquesta prova es guarda localment per permetre la recuperació.

---

### Desxifrat dels fitxers amb el script PSRansom

![Desxifrat dels fitxers](/T08/img/captura19.png)

Utilitzant la clau guardada al `readme.txt`, s'executa el script en mode desxifrat:

```powershell
.\PSRansom.ps1 -d C:\Users\client\Documents -k GZRzy0AmjsdSboNuT27hr4KM
```

Els fitxers es recuperen correctament.

**Anàlisi:** Aquest pas demostra que, amb la clau adequada, es podrien recuperar els fitxers afectats per un ransomware.

---

### Carpeta Documents després del desxifrat

![Documents recuperats](/T08/img/captura20.png)

Un cop desxifrats, els fitxers tornen a ser accessibles i es troben a la carpeta Documents.

**Anàlisi:** Es comprova que el procés de xifrat i desxifrat amb el script PSRansom funciona com s'esperava.

---

Continuació de la pràctica sobre malware i antimalware, ara centrada en la prova amb el ransomware WannaCry, l'anàlisi amb eines en línia i les mesures de protecció.

---

## 4. Preparació per a la prova amb WannaCry

Abans d'executar el malware real, es preparen fitxers de prova i es comprimeixen amb contrasenya per evitar la detecció immediata per part de l'antivirus.

### Carpeta Documents amb arxius de prova reals

![Carpeta Documents amb arxius de prova](/T08/img/captura21.png)

S'han creat diversos arxius reals a la carpeta Documents per simular un entorn real:
- `arxiu de text.txt` (document de text)
- `foto fortnite.jpg` (imatge)
- `REGLAMENTO DEL BALONCESTO.docx` (document de Word)
- `messi.png` (imatge)
- `GRATUIT 50 EXERCICES.pdf` (PDF)

**Anàlisi:** Aquests arxius serviran com a "víctimes" per comprovar l'efecte del ransomware WannaCry. Inclouen diferents formats comuns per observar si el malware afecta a tots o només a alguns tipus.

---

### Vista de la carpeta Documents amb arxius comprimits

![Vista de Documents amb ZIP](/T08/img/captura22.png)

Es mostra la carpeta Documents amb els arxius anteriors i també un fitxer comprimit `fitxercomprimit.zip`.

**Anàlisi:** Es prepara un fitxer ZIP sense contrasenya per després analitzar-lo amb VirusTotal i comprovar la seva detecció.

---

### Creació d'un fitxer ZIP protegit amb contrasenya

![Creació de ZIP amb contrasenya](/T08/img/captura23.png)

Utilitzant WinRAR, es crea un fitxer comprimit anomenat `amb contrasenya.zip` que inclou tots els arxius de prova i està protegit amb contrasenya.

**Anàlisi:** La contrasenya impedeix que l'antivirus pugui escanejar el contingut intern del fitxer comprimit, una tècnica que utilitzen els atacants per evadir la detecció.

---

### Contingut del fitxer ZIP protegit

![Contingut del ZIP protegit](/T08/img/captura24.png)

Dins del fitxer `amb contrasenya.zip` es veuen tots els arxius de prova, cadascun amb la columna "Protegido..." marcat com "Sí".

**Anàlisi:** Aquest fitxer servirà per provar si l'antivirus pot detectar malware quan està dins d'un arxiu comprimit amb contrasenya.

---

### Error en intentar descomprimir el fitxer protegit

![Error de descompressió](/T08/img/captura25.png)

En intentar descomprimir el fitxer `amb contrasenya.zip`, Windows mostra errors perquè alguns arxius no es poden crear correctament.

**Anàlisi:** Aquests errors poden ser deguts a permisos o a interferències de l'antivirus. Importantment, la contrasenya protegeix el contingut fins i tot durant la descompressió.

---

## 5. Descàrrega del malware WannaCry

### Pàgina de GitHub del projecte theZoo

![Pàgina de theZoo a GitHub](/T08/img/captura26.png)

S'accedeix al repositori `ytisf/theZoo` a GitHub, que conté una col·lecció de malware real per a finalitats d'investigació. Es navega fins a la carpeta `Ransomware.WannaCry`.

**Anàlisi:** Aquest tipus de repositoris són útils per a investigació en seguretat, però cal utilitzar-los amb extrema precaució i només en entorns controlats i aïllats.

---

### Carpeta de descàrregues amb el fitxer WannaCry

![Descàrregues amb WannaCry](/T08/img/captura27.png)

El fitxer `Ransomware.WannaCry.zip` ha estat descarregat i apareix a la carpeta de Descàrregues, amb una mida de 3.400 KB.

**Anàlisi:** El fitxer està comprimit i, com es veurà, protegit amb contrasenya ("infected") per evitar que es descarregui o executi accidentalment.

---

### Sol·licitud de contrasenya per descomprimir WannaCry

![Sol·licitud de contrasenya](/T08/img/captura28.png)

En intentar obrir el fitxer `Ransomware.WannaCry.zip`, es demana una contrasenya. La contrasenya és "infected", com s'indica a les instruccions de la pràctica.

**Anàlisi:** Aquesta protecció per contrasenya evita que els sistemes antimalware analitzin el contingut del fitxer comprimit, ja que no poden descomprimir-lo sense la contrasenya.

---

### Alerta de Windows Defender en intentar descomprimir

![Alerta de Windows Defender](/T08/img/captura29.png)

Quan s'intenta descomprimir el fitxer, Windows Defender mostra una alerta indicant que s'ha detectat un "Arxiu malintencionat" i recomana no executar-lo.

**Anàlisi:** Tot i que el fitxer està protegit amb contrasenya, Windows Defender pot haver detectat el malware basant-se en metadades o en comportaments sospitosos durant l'intent de descompressió.

---

### Detecció de ransomware per part de Windows Defender

![Detecció de ransomware](/T08/img/captura30.png)

Windows Defender identifica específicament el contingut com a "Ransomware" i bloqueja l'accés al fitxer.

**Anàlisi:** Això demostra l'efectivitat de les proteccions integrades de Windows 11 contra amenaces conegudes com WannaCry.

---

## 6. Execució de WannaCry i efectes observats

### Carpeta Documents després de l'execució de WannaCry

![Documents infectats](/T08/img/captura31.png)

Després d'executar WannaCry (amb les proteccions desactivades), els arxius de la carpeta Documents han estat xifrats i se'ls ha canviat l'extensió a `.WNCRY`. Addicionalment, s'han creat fitxers com `@Please_Read_Me@.txt` i `@WanaDecryptor@.exe`.

**Anàlisi:** Aquests són els signes característics d'una infecció per WannaCry: xifrat de fitxers, canvi d'extensió i creació d'arxius de rescat i desxifrat.

---

### Pàgina principal de VirusTotal

![VirusTotal principal](/T08/img/captura32.png)

VirusTotal és un servei en línia que analitza fitxers sospitosos utilitzant múltiples motors antivirus. S'utilitzarà per analitzar el fitxer `arxiu de text.zip`.

**Anàlisi:** Eines com VirusTotal permeten comprovar la detecció d'un fitxer per part de més de 70 antivirus diferents, proporcionant una visió amplia de la seva perillositat.

---

### Pujada d'un fitxer a VirusTotal

![Pujada a VirusTotal](/T08/img/captura33.png)

S'ha pujat el fitxer `arxiu de text.zip` (que conté un arxiu de text simple) per analitzar-lo. Aquest fitxer no conté malware, però serveix com a prova del funcionament de VirusTotal.

**Anàlisi:** En aquest cas, el fitxer és net, però el procés és el mateix per a qualsevol fitxer sospitós.

---

### Resultats de l'anàlisi a VirusTotal

![Resultats VirusTotal](/T08/img/captura34.png)

Els resultats mostren que diversos antivirus (Acronis, BitDefender, Microsoft, Symantec, etc.) marquen el fitxer com "Undetected" (no detectat).

**Anàlisi:** Això és normal, ja que el fitxer pujat és un ZIP amb contingut innocent. Si es pugés el fitxer de WannaCry, la majoria d'antivirus el detectarien.

---

### Anàlisi del fitxer WannaCry a Kaspersky Threat Intelligence Portal

![Kaspersky Threat Intelligence](/T08/img/captura36.png)

S'analitza el fitxer `@WanaDecryptor@.exe` (part de WannaCry) al portal de Kaspersky. Es mostra informació tècnica com hashs (MD5, SHA1), mida, data de primera detecció (12 maig 2017), etc.

**Anàlisi:** Les plataformes de Threat Intelligence ofereixen informació detallada sobre amenaces conegudes, incloent indicadors de compromís (IOCs) que poden utilitzar-se per a la detecció i resposta.

---

### Desconnexió de xarxa per a proves aïllades

![Configuració de xarxa de la VM](/T08/img/captura37.png)

Per a les proves més perilloses amb WannaCry, es desconnecta l'adaptador de xarxa de la màquina virtual per aïllar-la completament.

**Anàlisi:** Aquesta és una mesura de seguretat crítica quan es treballa amb malware real. Evita que el malware es propagui per la xarxa o intenti connectar-se a servidors de comandament i control.

---

### Error de SmartScreen per falta de connexió

![Error de SmartScreen](/T08/img/captura38.png)

Sense connexió a Internet, SmartScreen no pot verificar la reputació de l'aplicació i mostra un missatge d'error.

**Anàlisi:** SmartScreen depèn de serveis en línia per verificar la reputació de fitxers i aplicacions. Sense connexió, aquesta capa de protecció queda desactivada.

---

### Detecció persistent de Windows Defender

![Detecció persistent](/T08/img/captura39.png)

Tot i la falta de connexió, Windows Defender continua detectant el fitxer de WannaCry com a ransomware basant-se en les seves signatures locals.

**Anàlisi:** Això demostra la importància de tenir les definicions de virus actualitzades localment, ja que permeten la detecció encara que no hi hagi connexió a Internet.

---

### Error en obrir arxius xifrats

![Error en obrir arxius xifrats](/T08/img/captura40.png)

Després de la infecció, quan s'intenta obrir un arxiu xifrat (com `foto fortnite.jpg`), el sistema no pot llegir-lo i mostra errors.

**Anàlisi:** Aquests errors confirmen que el xifrat ha estat efectiu. Els arxius són inutilitzables sense la clau de desxifrat, que en el cas de WannaCry real només s'obtindria pagant el rescat (no recomanat).

---

Darrera part de la pràctica, que inclou l'anàlisi dels efectes del ransomware, la restauració del sistema i les conclusions finals.

---

## 7. Efectes del ransomware i restauració del sistema

### Error en obrir arxius xifrats

![Error en obrir arxius](/T08/img/captura41.png)

Quan s'intenta obrir un arxiu que ha estat xifrat per WannaCry, el sistema no pot llegir-lo i mostra un missatge d'error genèric: "No podemos abrir este archivo. Se ha producido un error."

**Anàlisi:** Aquest error confirma que el xifratge ha estat efectiu. Els arxius originals són ara indesxifrables sense la clau adequada, il·lustrant l'impacte destructiu d'un atac de ransomware real.

---

### Contingut d'un arxiu xifrat visualitzat en editor de text

![Contingut xifrat](/T08/img/captura42.png)

En obrir un arxiu xifrat (com `foto fortnite.jpg.WNCRY`) amb un editor de text com el Bloc de notes, es mostra contingut binari incomprensible, amb caràcters estranys i símbols.

**Anàlisi:** Aquest és el resultat del xifratge simètric: el contingut original ha estat codificat i és irrecognoscible. Els caràcters estranys són la representació textual de dades binàries xifrades.

---

### Restauració de la instantània anterior al virus

![Restauració de la instantània](/T08/img/captura43.png)

Per revertir tots els canvis i eliminar el malware, es restaura la instantània "Abans del virus" des de l'administrador de la màquina virtual.

**Anàlisi:** Aquesta és una pràctica essencial quan es treballa amb malware en entorns virtuals: permet experimentar sense riscos i tornar ràpidament a un estat conegut i net.

---

### Escriptori net després de la restauració

![Escriptori net](/T08/img/captura44.png)

Després de restaurar la instantània, el sistema torna a estar net. Es pot veure l'escriptori de Windows 11 Enterprise Evaluation sense rastre de la infecció.

**Anàlisi:** La restauració d'instantànies és la manera més eficaç de recuperar-se d'una infecció per malware en un entorn de proves, tot i que en sistemes reals caldrien altres mètriques (com còpies de seguretat).

---

## 8. Respostes a les preguntes teòriques de la tasca

### Pregunta 1: Quines proteccions incorpora Windows 11 a "Protecció antivirus i contra amenaces"?

Windows 11 inclou múltiples capes de protecció integrades a Windows Defender (ara anomenat Microsoft Defender Antivirus):

- **Protecció en temps real:** Escaneig continu de fitxers i processos.
- **Protecció basada en el núvol:** Anàlisi ràpida mitjançant intel·ligència col·lectiva.
- **Protecció contra amenaces manipulatives:** Detecció de tècniques d'evasió.
- **Exclusions personalitzades:** Possibilitat d'afegir carpetes o processos exclosos.
- **Actualitzacions automàtiques:** Base de dades de signatures sempre actualitzada.

### Pregunta 2: Quines opcions tenim a "Control d'aplicacions i navegador"?

Aquesta secció inclou:

- **SmartScreen per a aplicacions i fitxers:** Verifica la reputació de programes baixats.
- **SmartScreen per a Microsoft Edge:** Bloqueja llocs web maliciosos.
- **Protecció contra exploits:** Mitiga vulnerabilitats en aplicacions.
- **Aïllament de memòria:** Separa processos crítics per evitar atacs.

### Pregunta 3: Opcions específiques per a la protecció contra ransomware a Windows 11

Windows 11 inclou **Access Controlled Folder**, una funcionalitat que:

- Protegeix carpetes crítiques (Documents, Imatges, etc.) contra modificacions no autoritzades.
- Permet afegir carpetes personalitzades a la llista protegida.
- Mostra alertes quan una aplicació no autoritzada intenta modificar fitxers protegits.
- Es pot gestionar des de "Configuració de ransomware" dins de Windows Security.

### Pregunta 4: Factors que fan que WannaCry es propagui ràpidament

WannaCry es va propagar ràpidament gràcies a:

- **Explotació de la vulnerabilitat EternalBlue:** Una fallada crítica en el protocol SMBv1 de Windows.
- **Propagació automàtica en xarxa:** Es movia lateralment entre equips de la mateixa xarxa sense necessitat d'interacció de l'usuari.
- **Component de worm (cuc):** Capacitat d'auto-replicar-se i buscar nous hosts.
- **Xarxes no segmentades:** Moltes organitzacions tenien xarxes planes sense divisions adequades.

### Pregunta 5: Vulnerabilitat i CVE associat

WannaCry explotava la vulnerabilitat coneguda com **EternalBlue**, que afecta el protocol Server Message Block (SMB) de Microsoft.

- **CVE-2017-0144:** Assignat a aquesta vulnerabilitat.
- **Gravetat:** CVSS 8.1 (Alta) – permet execució remota de codi sense autenticació.
- **Parx:** Microsoft va llançar l'actualització MS17-010 al març de 2017, però molts sistemes no estaven actualitzats quan WannaCry va atacar al maig de 2017.

### Pregunta 6: S'ha de pagar el rescat?

**No es recomana pagar el rescat** per diverses raons:

- No hi ha garantia que els atacants proporcionin la clau de desxifrat.
- Pagar finança activitats criminals i incentiva nous atacs.
- Hi ha eines de desxifrat gratuïtes disponibles per a WannaCry (com WannaKey, WanaKiwi) gràcies a errors en la implementació del malware.
- Empreses de negociació de rescats existeixen, però són controvertides i poden ser il·legals en algunes jurisdiccions.

### Pregunta 7: Mesures de prevenció contra ransomware

- **Mantenir sistemes actualitzats:** Aplicar tots els parxos de seguretat.
- **Fer còpies de seguretat regulars:** Emmagatzemades fora de línia o en serveis amb versionat.
- **Utilitzar programari antimalware actualitzat.**
- **Activar proteccions contra ransomware** com Access Controlled Folder.
- **Segmentar xarxes:** Limitar la propagació lateral.
- **Formar usuaris:** Evitar obrir attachments sospitosos o clicar en enllaços desconeguts.
- **Desactivar protocols insegurs:** Com SMBv1 si no és necessari.

### Pregunta 8: Mesures si ja hem sofert un atac

Si ja hem estat víctimes de WannaCry:

1. **Aïllar l'equip afectat:** Desconnectar de la xarxa immediatament.
2. **Identificar l'abast:** Determinar quins sistemes i dades han estat compromesos.
3. **No pagar el rescat.**
4. **Buscar eines de desxifrat:** Comprovar si existeixen desxifradors gratuïts per a la variant específica.
5. **Restaurar des de còpies de seguretat.**
6. **Reinstalar sistemes afectats** des de zero per assegurar-se que el malware ha estat completament eliminat.
7. **Canviar totes les contrasenyes** que poguessin haver estat exposades.
8. **Analitzar la causa de l'entrada** per evitar futures infeccions.

---

## 9. Conclusions i recomanacions finals

Aquesta pràctica ha permès comprovar de primera mà:

1. **L'efectivitat de les proteccions integrades** de Windows 11 contra amenaces conegudes com EICAR i WannaCry.
2. **La importància de mantenir les proteccions activades** i actualitzades.
3. **Com els atacants utilitzen tècniques d'evasió** com la compressió amb contrasenya per evitar la detecció.
4. **L'impacte devastador d'un ransomware real** sobre les dades i la continuïtat operativa.
5. **La utilitat de les eines d'anàlisi en línia** com VirusTotal per a la investigació de fitxers sospitosos.
6. **La necessitat de protocols de recuperació**, com còpies de seguretat i instantànies, per minimitzar el temps de recuperació.

### Recomanacions per a entorns reals:

- **Implementar una estratègia de còpies de seguretat 3-2-1:** 3 còpies, en 2 tipus de mitjans diferents, 1 d'elles fora de les instal·lacions.
- **Actualitzar sistemes regularment:** Establir processos de gestió de parxos.
- **Utilitzar autenticació multifactor** per dificultar l'accés no autoritzat.
- **Segmentar xarxes** per limitar la propagació de malware.
- **Realitzar simulacions d'atac** per avaluar la preparació de l'organització.
- **Educar contínuament als usuaris** sobre amenaces de phishing i enginyeria social.
