# Broken Guardians

---

**Nume:** Borozan George  
**Grupa:** 341  
**Data examenului:** 2025-2026  
**Titlul jocului:** Broken Guardians  

---

<br><br><br><br><br><br><br><br><br><br>

**Dezvoltarea Jocurilor 3D in Unreal Engine 5**

---

## Cuprins

1. [Descrierea Detaliata a Jocului](#1-descrierea-detaliata-a-jocului)
2. [Specificatii Tehnice](#2-specificatii-tehnice)
3. [Specificatii Tematice](#3-specificatii-tematice)
4. [Actiuni Disponibile si Control](#4-actiuni-disponibile-si-control)
5. [Clasele Proprii](#5-clasele-proprii)
6. [Algoritmi](#6-algoritmi)
7. [Unelte de Inteligenta Artificiala din Unreal](#7-unelte-de-inteligenta-artificiala-din-unreal)
8. [Lista de Taskuri Realizate](#8-lista-de-taskuri-realizate)
9. [Lista de Utilitare Folosite](#9-lista-de-utilitare-folosite)
10. [Lista de Pachete Externe Folosite](#10-lista-de-pachete-externe-folosite)
11. [Bibliografie](#11-bibliografie)

---

## 1. Descrierea Detaliata a Jocului

### Concept si Poveste

**Broken Guardians** ("Gardienii Franti") este un joc de tip Action-RPG in genul Souls-like Boss Rush, dezvoltat in Unreal Engine 5. Jocul prezinta o poveste dark fantasy despre coruptie, sacrificiu si rascumparare.

**Povestea:** Gardienii Franti — fosti frati de arme, acum umbre ale datoriei. Ei pazesc o relicva: o poarta deschisa ce aduce demoni in lumea noastra. Puterea ei i-a corupt, transformandu-i in monstri loiali. Tu ai fost unul dintre ei. Ai refuzat blestemul si ai fugit. Dar ai luat coruptia cu tine. Demonul tau interior castiga controlul. Acum te intorci sa distrugi sursa. O ultima datorie, de data asta reala. Ei iti stau in cale — pentru a te opri, sunt gata sa isi sacrifice si ultima farama de vointa, oferindu-se complet puterii care ii devoreaza.

### Ce s-a Implementat

Jocul se afla intr-o stare functionala de prototip, cu accent pe sistemele fundamentale de lupta si pe un framework scalabil pentru crearea de boss fights.

**Sistemul de Lupta:**
- **Doua arme functionale:** sabie si arc, cu atacuri distincte si animatii dedicate
- **Target Locking:** sistem de blocare pe tinte (orice Pawn din scena) cu rotirea camerei si ajustarea directiei proiectilelor / animatiilor
- **Rolling/Dodge:** sistem de ferire pentru evitarea atacurilor inamice
- **Sistem de Damage si Health Bars:** bara de viata vizibila pentru jucator si inamici

**Inamici in Scena:**
- Un inamic de baza (pentru testare si demonstratie)
- Un boss principal cu atacuri variate (implementate prin behavior trees)

**Algoritmi si Sisteme Complexe:**
- Sistem avansat de functionare a arcului cu fizica realista a proiectilelor
- Sistem de coliziuni optimizat pentru combat
- **Generare procedurala** pentru capela din arena: numar variabil de stalpi si pereti, dimensiuni configurabile
- **Implementare complexa de Spline:** zone de viteza, viraje aleatorii, utilizat pentru traiectorii de atac si testing

**Scenografie Tematica:**
- Arena de lupta stilizata ca un munte corupt
- Skybox potrivit atmosferei dark fantasy
- Capela generata procedural, integrata in ambient

![Arena de lupta cu boss-ul si capela generata procedural](arena_scenery.png)

![Exemplu de gameplay cu target lock activ](target_lock_ex.png)

![Vedere generala a scenei de joc](random_ok_image.png)

---

## 2. Specificatii Tehnice

- **Sistem de operare:** macOS (dezvoltat pe MacBook Pro M4 Pro), compatibil Windows 10/11
- **Spatiu pe disc:** ~50 GB (proiect complet cu assets)
- **Memorie RAM necesara:** minimum 16 GB, recomandat 24 GB (dezvoltarea a fost realizata pe 24gb de ram)
- **IDE/Build Tools:** Xcode (pentru macOS)
- **Pachete aditionale:** Niciuna
- **Conexiune la retea:** Nu este necesara (joc single-player offline)
- **Date salvate:** Jocul nu implementeaza inca un sistem de save; datele de configurare sunt stocate in folderul `Saved/Config/`
- **Versiune Unreal Engine:** 5.4.4

---

## 3. Specificatii Tematice

- **Categorii:** Action-RPG, Adventure, Souls-like, Boss Rush
- **Motivatia temei:** Pasiunea pentru jocurile FromSoftware si dorinta de a recrea experienta intensa si cinematica a luptelor din acestea
- **Taguri:** Souls-like, Boss Rush, Dark Fantasy, Action-RPG, Challenging, Skill-Based, Horror, Single Player
- **Mod de joc:** Single Player
- **Timp estimat de joc:** 1-3 ore pentru completare (variabil in functie de skill)
- **Limbi disponibile:** Engleza

---

## 4. Actiuni Disponibile si Control

Jocul utilizeaza sistemul **Enhanced Input** din Unreal Engine 5 pentru gestionarea inputurilor. Controalele sunt gandite pentru tastatura si mouse.

### Schema de Control

| Actiune | Tasta/Buton | Descriere |
|---------|-------------|-----------|
| **Miscare** | W, A, S, D | Deplasarea caracterului in cele 4 directii |
| **Rotire Camera** | Mouse | Controlul camerei pentru orientare |
| **Atac** | Left Click | Atac cu arma curenta; pentru sabie, click-uri succesive activeaza combo-uri |
| **Roll/Dodge** | Shift | Rostogolire pentru evitarea atacurilor inamice |
| **Target Lock** | Q | Blocheaza camera pe cel mai apropiat inamic |
| **Schimbare Arma** | Tab | Alternare intre sabie si arc |

### Input Actions (Enhanced Input System)

Sistemul de input este implementat prin urmatoarele actiuni definite in Unreal:

- `IA_Move` - Miscare pe axele X si Y
- `IA_Look` - Rotirea camerei
- `IA_Attack` - Declansare atac
- `IA_Dodge` - Actiune de rostogolire
- `IA_Lock` - Activare/dezactivare target lock
- `IA_SwitchWeapon` - Schimbarea armei active
- `IA_GenerateSplinePath` - Actiune de debug pentru generarea traiectoriilor spline

### Note

- Nu exista optiuni de remapare a tastelor in versiunea curenta
- Actiunea `IA_GenerateSplinePath` este utilizata doar pentru debugging si testare

---

## 5. Clasele Proprii

### 5.1 Componenta de Combat (BPC_Combat)

**Componenta de Combat** este nucleul sistemului de lupta, gestionand blocarea pe tinte, atacurile si interactiunea cu inamicii. Aceasta mentine starea curenta a luptei: tinta blocata, arma echipata, daca jucatorul este in mijlocul unui atac sau unei feriri. Componenta foloseste sistemul de input pentru a raspunde la comanda de blocare pe tinta, implementand un sistem toggle: daca exista deja o tinta blocata, aceasta este eliberata si miscarea caracterului revine la normal; daca nu exista tinta, se efectueaza o detectie automata a inamicilor.

**Detectia tintelor** se realizeaza prin **Sphere Trace** — un algoritm de collision detection care proiecteaza o sfera invizibila in directia camerei. Trace-ul porneste de la pozitia camerei jucatorului si se extinde inainte, cautand inamici. Cand o tinta valida este detectata, aceasta este stocata, iar la fiecare frame componenta calculeaza rotatia necesara pentru a mentine camera orientata catre inamic. Aceasta abordare ofera un balans optim intre performanta si acuratete pentru sistemul de target lock.

**Sistemul de atac cu arcul** integreaza **Animation Montages** pentru a sincroniza spawn-ul proiectilelor cu animatia. Cand se initiaza un atac, se porneste animatia corespunzatoare, iar la un moment specific din aceasta (definit printr-un **Animation Notify**), se obtine pozitia armei si se spawn-eaza proiectilul. Daca exista o tinta blocata, aceasta este transmisa proiectilului pentru a activa comportamentul de urmarire automata (homing).

### 5.2 Generator Procedural de Capela (BP_ChapelGenerator)

**Generatorul de Capela** este un sistem de **procedural generation** care construieste structuri circulare modulare din componente individuale: stalpi, pereti si cupola. Generatorul este parametrizabil prin variabile expuse in editor: numarul de pereti si usi, raza incintei, si inaltimile fiecarei componente. Sistemul este proiectat pentru a permite regenerarea rapida in editor, curatand intai constructia existenta inainte de a genera una noua.

**Algoritmul de constructie** foloseste un loop care itereaza prin numarul specificat de pereti. Pentru fiecare iteratie, se calculeaza pozitia curenta si urmatoarea pe un cerc folosind **functii trigonometrice** (cosinus si sinus) aplicate la unghiul curent inmultit cu raza. Astfel, fiecare stalp este pozitionat pe circumferinta cercului, orientat catre centru pentru a obtine efectul de cupola convergenta. Intre doi stalpi consecutivi se spawn-eaza un perete, scalat si rotit corespunzator pentru a acoperi distanta dintre punctele calculate.

**Sistemul de management al actorilor** mentine o lista a tuturor actorilor generati, permitand curatarea completa inainte de o regenerare. La finalizarea loop-ului principal, se spawn-eaza cupola centrala cu dimensiunile calculate din parametrii structurii. Aceasta abordare modulara permite ajustarea rapida a aspectului capelei direct din editor, fara a necesita modificari in logica blueprint-ului.

### 5.3 Componenta de Statistici (BPC_Stats)

**Componenta de Statistici** gestioneaza atributele numerice ale unui actor, precum viata curenta si viata maxima. Aceasta poate fi atasata atat jucatorului cat si inamicilor, oferind un sistem unificat de management al starii de sanatate. Componenta include o referinta optionala catre un widget de tip bara de progres pentru afisarea vizuala a starii de viata in interfata utilizatorului.

**Functia de modificare a valorilor** primeste ca parametri tipul statisticii (printr-o enumeratie) si valoarea de modificat. Folosind o structura de tip switch, sistemul directioneaza modificarea catre statistica corecta. Pentru sanatate, valoarea noua este calculata adunand modificarea la valoarea curenta, apoi rezultatul este **clamped** (limitat) intre zero si valoarea maxima pentru a preveni depasirea limitelor. Dupa actualizarea valorii, daca bara de progres este valida, aceasta este actualizata automat cu procentul curent.

**Sistemul de moarte** este declansat automat cand valoarea vietii scade la sau sub zero. In loc sa implementeze direct logica de moarte, componenta apeleaza o **interfata** pe actorul parinte, permitand fiecarui tip de actor sa defineasca propriul comportament de moarte. Aceasta abordare bazata pe interfete ofera flexibilitate si decuplare intre logica de statistici si comportamentul specific fiecarui actor.

### 5.4 Personajul Principal (BP_ThirdPersonCharacter)

**Personajul Principal** este blueprint-ul central al jucatorului, integrand toate componentele necesare pentru gameplay. Acesta mosteneste clasa **Character** din Unreal Engine, beneficiind astfel de functionalitati built-in precum miscarea, saltul si coliziunea. Blueprint-ul foloseste o arhitectura modulara prin compozitie: **BPC_Combat** pentru sistemul de lupta, **BPC_Stats** pentru gestionarea vietii, si componente mesh separate pentru fiecare arma (sabie, arc).

**Sistemul de input** este construit pe **Enhanced Input System**, configurand la **BeginPlay** contextul de mapare prin subsistemul local al player-ului. Pentru miscare, actiunea **IA_Move** primeste un vector 2D care este descompus in componente X (stanga/dreapta) si Y (inainte/inapoi). Directia miscarii este calculata relativ la camera: se obtine rotatia controller-ului, din care se extrage componenta Yaw, si se genereaza vectorii **Forward** si **Right** corespunzatori. Miscarea se aplica prin **AddMovementInput** cu acesti vectori scalati de valorile inputului. Camera este controlata separat prin actiunea **IA_Look**, care modifica direct rotatia controller-ului folosind **AddControllerYawInput** si **AddControllerPitchInput**.

**Implementarea interfetei BPI_Damage** asigura comunicarea cu sistemul de animatie. Evenimentele **Begin Sword Trace** si **End Sword Trace**, declansate de Animation Notifies din montaj-uri, sunt delegate catre componenta **BPC_Combat** care gestioneaza detectia coliziunilor armei. Aceasta separare permite animatorilor sa controleze precis momentele active ale atacului fara a modifica logica de combat.

**Raspunsul la damage** este procesat prin evenimentul **ReceiveAnyDamage**. Valoarea primita este negata si transmisa catre **BPC_Stats** pentru a reduce viata. Simultan, se porneste un **Animation Montage** de reactie la lovitura, iar viteza de miscare este temporar redusa pentru a crea efectul de stagger. Dupa completarea montaj-ului, viteza revine la normal. **Sistemul de moarte**, implementat prin interfata **Die**, dezactiveaza componenta de miscare si activeaza simularea fizicii pe mesh-ul personajului, creand efectul de ragdoll.

### 5.5 Inamicul de Baza (BP_Enemy)

**Inamicul de Baza** este blueprint-ul parinte pentru toate tipurile de inamici din joc, inclusiv bossul. Acesta mosteneste clasa **Character** si implementeaza interfata **BPI_Damage** pentru a permite comunicarea cu sistemul de animatie si damage. Blueprint-ul contine componenta **BPC_Stats** pentru gestionarea vietii, un **WidgetComponent** pentru afisarea barei de viata deasupra capului, si componente auxiliare pentru pozitionarea trace-urilor de atac.

**Sistemul de damage trace** foloseste un **Timer cu delegate** pentru a executa detectia coliziunilor in mod repetat pe durata unui atac. La evenimentul **Begin Sword Trace**, se porneste un timer care apeleaza functia **Damage Trace** la intervale foarte mici. Aceasta functie verifica mai intai daca inamicul este in viata, apoi executa un **SphereTraceMultiForObjects** intre pozitiile componentelor de start si end ale sabiei. Trace-ul returneaza o lista de hit results care este parcursa cu **ForEachLoopWithBreak**. Pentru fiecare actor lovit, se aplica damage folosind **ApplyDamage** cu valoarea din variabila **swordDamage**. Un macro **DoOnce** previne aplicarea multipla a damage-ului pe acelasi actor in cadrul aceluiasi swing.

**Raspunsul la damage** este similar cu cel al jucatorului: evenimentul **ReceiveAnyDamage** primeste valoarea, o inmulteste cu -1 pentru a o transforma in scadere, si o transmite catre **BPC_Stats.IncreaseValue**. La **BeginPlay**, blueprint-ul obtine referinta la widget-ul de viata si configureaza bara de progres in componenta de statistici.

**Sistemul de moarte** implementeaza evenimentul **Die** din interfata: dezactiveaza componenta de miscare, activeaza simularea fizicii pentru ragdoll.

---

## 6. Algoritmi

<!-- PLACEHOLDER: De completat cu descrierea algoritmilor, blueprint screenshots si analiza de complexitate -->

### 6.1 Algoritm Arc/Proiectile
<!-- De completat -->

### 6.2 Sistem de Coliziuni
<!-- De completat -->

### 6.3 Generare Procedurala - Capela
<!-- De completat -->

### 6.4 Implementare Spline cu Zone de Viteza
<!-- De completat -->

---

## 7. Unelte de Inteligenta Artificiala din Unreal

<!-- PLACEHOLDER: De completat cu AI tools folosite -->

---

## 8. Lista de Taskuri Realizate

<!-- PLACEHOLDER: De completat cu cerintele din barem -->

---

## 9. Lista de Utilitare Folosite

<!-- PLACEHOLDER: De completat -->

---

## 10. Lista de Pachete Externe Folosite

| Pachet | Sursa | Utilizare in Joc |
|--------|-------|------------------|
| **Good Sky** | [Epic Games Marketplace](https://www.unrealengine.com/marketplace/en-US/product/good-sky) | Sistem de skybox dinamic pentru atmosfera dark fantasy corupta |
| **Paragon: Kwang** | [Epic Games / Paragon Assets](https://www.unrealengine.com/marketplace/en-US/product/paragon-kwang) | Modelul si animatiile pentru caracterul jucatorului |
| **Paragon: Grux** | [Epic Games / Paragon Assets](https://www.unrealengine.com/marketplace/en-US/product/paragon-grux) | Modelul si animatiile pentru boss-ul principal |
| **Paragon: Agora and Monolith Environment** | [Epic Games / Paragon Assets](https://www.unrealengine.com/marketplace/en-US/product/paragon-agora-and-monolith-environment) | Stalpii si materialele utilizate in scena arenei |
| **Starter Content** | Inclus in Unreal Engine | Assets de baza pentru prototipare rapida |

---

## 11. Bibliografie

<!-- PLACEHOLDER: De completat -->

---