# 🦠 REPTE: Malware i Programari Antimalware

## 👤 Alumne
**Nom i cognoms:** Guillem Barjuan  

---

## 📄 Introducció a la tasca

En aquesta pràctica realitzo un **repte complet sobre malware i sistemes antimalware** amb l’objectiu d’entendre, tant a nivell teòric com pràctic, com funcionen les amenaces informàtiques i quines eines tenim al nostre abast per prevenir-les i mitigar-les.

La tasca es duu a terme **exclusivament en màquines virtuals**, utilitzant instantànies (*snapshots*) per garantir que tots els experiments es poden revertir sense risc. En tot moment es comprova que els canvis de configuració realitzats produeixen l’efecte esperat mitjançant proves pràctiques.

Aquest repte em permet treballar amb **malware real i de prova**, entendre el comportament dels **ransomware** i verificar l’eficàcia de les proteccions que incorpora **Windows 11**.

---

## 🎯 Objectius de la pràctica

L’objectiu principal d’aquesta activitat és:

- Comprendre com funciona el malware.
- Verificar el funcionament real d’un programari **antimalware**.
- Analitzar les proteccions de seguretat integrades a **Windows 11**.
- Simular atacs de **ransomware** i comprovar la resposta del sistema.
- Analitzar un cas real de ransomware: **WannaCry**.

---

## 🧪 Activitat 1: Verificació del funcionament d’un antimalware

En aquesta primera part comprovo el funcionament de les proteccions antimalware del sistema operatiu i del navegador.

- Desactivo temporalment **SmartScreen** per poder realitzar proves controlades.
- Utilitzo el fitxer de prova **EICAR**, que simula malware sense ser perillós.
- Verifico si l’antimalware detecta el fitxer en diferents situacions.
- Provo a amagar el fitxer EICAR dins d’arxius comprimits en diversos formats:
  - `.zip`
  - `.tar`
  - `.7z`
- Comprovo en cada cas si l’antimalware detecta l’amenaça un cop es reactiva la protecció en temps real.

Aquesta activitat em permet entendre com funcionen els motors de detecció i fins a quin punt són efectius davant fitxers comprimts.

---

## 🛡️ Activitat 2: Sistemes de protecció de Windows 11

En aquesta secció analitzo les proteccions natives de Windows 11:

- Les opcions disponibles a **“Protección antivirus y contra amenazas”**.
- Les funcionalitats del **Control de aplicaciones y navegador**.
- Les opcions específiques de **protecció contra ransomware**, com ara:
  - Accés controlat a carpetes.
  - Protecció davant modificacions no autoritzades.
  - Alertes i bloquejos automàtics.

Aquesta part em permet entendre com Windows 11 pot actuar com a primera línia de defensa davant amenaces modernes.

---

## 🔐 Activitat 3: Prova pràctica de protecció contra Ransomware (PSRansom)

En aquesta activitat simulo el comportament d’un ransomware mitjançant un script de prova:

- Creo diversos arxius dins la carpeta **Documents**.
- Desactivo la protecció contra ransomware i executo el ransomware de prova.
- Comprovo com els fitxers són xifrats i com es genera el fitxer de recuperació.
- Desxifro els fitxers utilitzant la clau proporcionada.
- Torno a activar la protecció contra ransomware.
- Repeteixo l’atac i comprovo que:
  - Els fitxers **no són xifrats**.
  - El sistema genera una **alerta de seguretat**.

Aquesta prova demostra clarament l’eficàcia de les proteccions contra ransomware quan estan correctament activades.

---

## 🧨 Activitat 4: Anàlisi del Ransomware WannaCry

En aquesta part analitzo un **cas real** de ransomware molt conegut: **WannaCry**.

S’estudien els següents aspectes:

- Per què WannaCry es va propagar tan ràpidament.
- Quina vulnerabilitat explotava i el seu **CVE associat**.
- La gravetat de la vulnerabilitat.
- Si és recomanable pagar el rescat i per què.
- El paper de les empreses negociadores de rescats.
- Mesures de **prevenció** davant atacs de ransomware.
- Mesures a aplicar **després** de patir un atac si no s’ha prevenit correctament.

Aquesta part reforça la importància de la prevenció, les actualitzacions i les còpies de seguretat.

---

## ⚠️ Activitat 5: Prova pràctica amb WannaCry (entorn controlat)

⚠️ **Aquesta activitat es realitza exclusivament en una màquina virtual aïllada**, sense xarxa ni carpetes compartides.

- Creo diversos fitxers de diferents tipus a la carpeta Documents.
- Descarrego el malware real **WannaCry** des d’un repositori controlat.
- Analitzo el comportament de l’antimalware davant:
  - Fitxers comprimts amb contrasenya.
  - Execució directa del malware.
- Comprovo quins antivirus detecten l’amenaça utilitzant serveis en línia.
- Amb totes les proteccions desactivades, executo WannaCry i:
  - Documento el missatge de rescat.
  - Analitzo quins fitxers han estat xifrats i quins no.

Un cop documentada tota la prova, **restauro la màquina virtual a la instantània inicial** per eliminar qualsevol rastre del malware.

---

## ✅ Conclusions

Aquest repte m’ha permès:

- Entendre com funcionen els ransomware a nivell pràctic.
- Comprovar la importància real de les proteccions de Windows 11.
- Verificar que la prevenció és clau davant amenaces greus.
- Treballar de manera segura amb malware en entorns controlats.
- Reforçar la importància de les còpies de seguretat i les bones pràctiques de seguretat.

Aquesta pràctica em dona una visió realista i professional sobre la gestió d’incidents de seguretat en entorns Windows.

---

## 📚 Referències i recursos

- Apunts del mòdul (secció Malware)
- ISO Windows 11 (comuna)
- EICAR: https://www.eicar.org
- PSRansom: https://github.com/JoelGMSec/PSRansom
- WannaCry (theZoo): https://github.com/ytisf/theZoo
- VirusTotal: https://www.virustotal.com
- Kaspersky OpenTIP: https://opentip.kaspersky.com
- AVG – WannaCry: https://www.avg.com/es/signal/wannacry-ransomware-what-you-need-to-know
