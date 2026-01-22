# 📁 T03: Serveis de Transferència de Fitxers

## 📄 Descripció de la Tasca

Després d’haver configurat serveis clau com **DHCP**, **DNS** i sistemes d’**accés remot** durant la meva experiència a EverPia, en aquesta tasca faig un pas més endavant per aprofundir en un altre pilar fonamental de les xarxes: la **transferència de fitxers**.

Tot i que avui dia els serveis de *cloud* com Google Drive o Dropbox són molt habituals, com a futur tècnic vull **dominar els protocols clàssics** que fan possible la transferència de dades entre sistemes. Per aquest motiu, m’inscric en aquesta formació amb l’objectiu de convertir-me en un expert en serveis de transferència de fitxers.

---

## 🎯 Objectius de la Tasca

En finalitzar aquesta formació, he de ser capaç de demostrar, amb configuracions reals, que entenc i domino els següents conceptes:

- Com funciona el **protocol FTP**.
- Quines diferències hi ha entre el **mode actiu i el mode passiu**.
- Com implementar un **servidor FTP segur**.
- Ús del **protocol sFTP** com a alternativa segura a l’FTP tradicional.
- **Engabiament (chroot)** d’usuaris en connexions sFTP.
- Coneixement d’**altres mètodes alternatius** per a la transferència de fitxers.

---

## 🧠 Pla de Treball

La formació es divideix en dues parts molt clares: una part teòrica i una part pràctica, pensada per treballar en entorns reals.

### 1️⃣ Fonaments Teòrics i Seguretat

En aquesta fase estudio els protocols **FTP i sFTP**, posant especial atenció a:

- El funcionament intern de cada protocol.
- Les diferències entre **FTP actiu i FTP passiu**.
- Els riscos de seguretat associats a l’FTP.
- L’ús de **xifrat i autenticació** en sFTP.
- L’engabiament d’usuaris per evitar accessos no autoritzats al sistema.

Aquesta base teòrica és imprescindible per entendre les decisions preses durant el laboratori pràctic.

---

### 2️⃣ Laboratori Pràctic – *The Real World*

En aquesta part passo a l’acció i treballo amb **màquines virtuals** per simular entorns reals. El laboratori es divideix en dues activitats principals:

#### 🔹 Activitat A – Servidor FTP

Configuro un **servidor FTP tradicional**, on:

- Creo usuaris específics.
- Aplico engabiament (*chroot*).
- Configuro permisos de lectura i escriptura.
- Comprovo com les dades es transfereixen **sense xifrar**, viatjant en clar per la xarxa.

Aquesta activitat em permet entendre clarament els riscos del FTP clàssic.

#### 🔹 Activitat B – Servidor sFTP (Secure FTP)

Implemento un **servidor sFTP** utilitzant SSH, on:

- Utilitzo connexions xifrades.
- Engabio els usuaris per limitar l’accés al sistema.
- Verifico que la transferència de fitxers és segura i controlada.

Aquesta part demostra per què sFTP és l’opció recomanada en entorns professionals.

---

## 🔐 Consideracions de Seguretat

Com a administrador de sistemes, tinc clar que **la seguretat no és una opció, és una obligació**. Aquesta tasca em permet entendre de manera pràctica les diferències reals entre FTP i sFTP i per què l’ús de protocols segurs és clau en qualsevol entorn empresarial.

---

## 📝 Sistema d’Avaluació

Per superar aquesta tasca he de demostrar els meus coneixements de dues maneres:

- **Prova escrita (40%)**  
  Preguntes sobre protocols, ports, modes de connexió i conceptes de seguretat.

- **Examen pràctic (60%)**  
  Repte cronometrat on haig de:
  - Configurar des de zero un servidor FTP.
  - Configurar un servidor sFTP.
  - Crear usuaris amb permisos específics.
  - Verificar la connexió des d’un client extern.

---

## ✅ Resultat Final

Amb aquesta tasca consolido els meus coneixements sobre:

- Protocols de transferència de fitxers.
- Diferències pràctiques entre FTP i sFTP.
- Importància de la seguretat i el xifrat.
- Configuració real de servidors en entorns Linux.

Aquesta formació em prepara per afrontar situacions reals en entorns professionals on la transferència segura de dades és crítica.

---

## 📚 Materials i Recursos

- **Materials de l’assignatura**  
  Moodle – Serveis de Xarxa
