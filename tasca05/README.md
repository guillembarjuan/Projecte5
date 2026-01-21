# 🏢 T05: Instal·lació del Domini (Active Directory)

## 📄 Descripció de la Tasca

Aquesta tasca és la continuació directa de la instal·lació de **Windows Server 2025** realitzada anteriorment. En aquest cas, l’objectiu és **desplegar un Directori Actiu (Active Directory)** sobre la màquina virtual creada, simulant un entorn real del client **TransLògic S.A.**

El procés serveix com a **prova de concepte (PoC)** per demostrar als responsables del client que la infraestructura proposada és viable i funcional, i també per ajustar futures configuracions segons les seves necessitats reals abans del desplegament definitiu.

---

## 🎯 Objectius de la Tasca

- Instal·lar els **rols necessaris** per al funcionament del Directori Actiu.
- Crear un **domini nou dins d’un bosc nou**.
- Configurar el **nivell funcional del domini a 2025**.
- Promocionar el servidor com a **Controlador de Domini (DC)**.
- Generar i desar un **script PowerShell** que permeti automatitzar tot el procés.
- Incorporar aquest script al repositori del projecte com a part de la documentació tècnica.

---

## 🌐 Creació del Domini

Durant la realització de la tasca:

- Creo un **bosc nou** amb un domini anomenat:

translogicXX.test

on `XX` correspon al meu número de llista.

- Configuro el **nivell funcional del domini i del bosc a Windows Server 2025**, assegurant compatibilitat amb les versions més recents i bones pràctiques actuals.

Aquest domini servirà com a base per a la gestió centralitzada d’usuaris, equips i polítiques de seguretat.

---

## 🖥️ Promoció del Servidor a Controlador de Domini

Un cop instal·lats els rols necessaris, promociono el servidor com a **Controlador de Domini**.

Durant aquest procés:

- Reviso i documento detalladament la **pantalla de resum**, on es mostren totes les configuracions aplicades abans de finalitzar la promoció.
- Verifico que el servidor passa correctament a actuar com a DC després del reinici.

Aquesta part és clau dins la PoC, ja que demostra que el servidor queda plenament operatiu com a element central del domini.

---

## 📜 Automatització amb PowerShell

Per millorar l’eficiència i garantir la replicabilitat del procés:

- Gravo en un **fitxer PowerShell** tot l’script necessari per automatitzar:
- La instal·lació dels rols.
- La creació del domini.
- La promoció del servidor a controlador de domini.

Aquest script permet repetir el desplegament de manera ràpida i segura en altres entorns.

---

## 📂 Incorporació de l’Script al Repositori

Un cop finalitzat tot el procediment:

- Copio l’script PowerShell generat a la carpeta del **repositori del projecte**.
- Per fer-ho, faig servir un dels mecanismes disponibles (USB, serveis d’Internet o còpia segura).

D’aquesta manera, l’script queda integrat com a part de la documentació i pot ser reutilitzat en futures implantacions.

---

## 📸 Documentació

La documentació de la tasca inclou:

- Captures de pantalla dels passos més rellevants:
- Instal·lació de rols
- Creació del domini
- Pantalla resum de la promoció a DC
- Observacions personals sobre el procés.
- L’script PowerShell utilitzat per automatitzar la instal·lació.

Tot el contingut està redactat en **format Markdown**, seguint un estil clar i professional.

---

## ✅ Resultat Final

Amb aquesta tasca he aconseguit:

- Desplegar correctament un **domini Active Directory** funcional.
- Promocionar el servidor a **Controlador de Domini** amb èxit.
- Disposar d’una **prova de concepte sòlida** per al client TransLògic.
- Crear una base tècnica reutilitzable gràcies a l’script PowerShell.
- Generar documentació clara per a futurs desplegaments reals.

Aquesta tasca deixa el sistema preparat per a la gestió centralitzada d’usuaris, equips i polítiques en entorns empresarials.

---

## 📚 Materials i Recursos

- **Guia UD6.AA3 – Instal·lació DC**  
Moodle SOX

