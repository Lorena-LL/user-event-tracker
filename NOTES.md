# NOTES — Lupulescu Lorena

---

## 1. Bug-urile găsite

### Bug #1
- **Unde era:** main.py - linia 32
- **Cum l-am găsit:** testul "test_create_event_returns_201" pica, asa ca m-am uitat in cod si am vazut ca metoda POST pentru create_user specifica si status_code, dar metoda POST pentru create_event nu specifica
- **Cum l-am fixat:** am adaugat "status_code=201" in "@app.post("/events", response_model=Event, status_code=201)" in main.py

### Bug #2
- **Unde era:** storage.py - linia 51
- **Cum l-am găsit:** testul "test_list_events_includes_created_items" pica, deci trebuie sa fie un bug fie in metoda care adauga un eveniment, fie in metoda de get -> in list_events se returneaza elementele decalate cu un elemet ceea ce e gresit
- **Cum l-am fixat:** am inlocuit linia 51 cu "return all_events[offset : offset + limit]" pentru a returna atatea evenimente cate specifica linits, incepand de la indexul offset din lista tuturor evenimentelor

### Bug #3
- **Unde era:** storage.py - linia 50
- **Cum l-am găsit:** testul "test_list_events_hides_soft_deleted_items" pica, si ma gandesc ca una dintre: stergerea "soft delete" sau metoda get pentru events trebuie sa aiba bug. Metoda soft_delete_event functioneaza, dar list_events nu tine cont de valoarea deleted_at al unui evnt cand returneaza lista. 
- **Cum l-am fixat:** am facut o filtrare pentru a returna numai evenimentele active

### Bug #4
- **Unde era:** storage.py - linia 49
- **Cum l-am găsit:** mi se specifica ca list_events trebuie sa returneze evenimentele in ordinea inserarii, dar nu se face nicio ordonare explicita. Daca datele ar fi luate din baza de date, evenimentele nu ar mai fi returnate in ordine
- **Cum l-am fixat:** am adaugat o sortare explicita pe baza momentului de creare in loc sa ma bazez pe ordinea dictionarului inainte de return: "active_events.sort(key=lambda e: e.created_at, reverse=True)" si am sters mesajul pentru ca acum codul respecta specificatia

### Bug #5
- **Unde era:** storage.py - linia 59
- **Cum l-am găsit:** testul "test_delete_same_event_twice_changes_response" pica si observ ca metoda soft_delete_event returneaza None numai daca evenimentul nu exista, iar numai acest caz returneaza in main 404, ceea ce nu corespunde cu asteptarile testului 
- **Cum l-am fixat:** metoda soft_delete_event trenuie sa verifice daca evenimentul nu este deja sters: "if event is None or event.deleted_at is not None: ..." 

---

## 2. Endpoint-ul nou

- **Decizii de design:** 
    1. delete_user: endpoint pentru soft delete user pentru ca mi se parea interesant sa vad cum pot sa mimez atomicitatea daca operatia de stergere se propaga mai departe la fiecare eveniment al userului respectiv
    2. get_user_events: endpoint pentru obtinerea tuturor evenimentelor non deleted pe care le are un user - nu la fel de interesant precum primul, dar l-am creat ca sa pot testa mai bine endpointul pentru soft delete user
- **Cazuri edge pe care le-ai acoperit:**
    1. delete_user: 
        - o eroare apare dupa ce unele evenimente sunt sterse, dar nu toate si atunci vreau sa nu se persiste stergerea doar pe o parte din obiectele implicate
- **Teste adăugate:** 
    1. delete_user:
        - userul are evenimente si stergerea reuseste cu succes
        - userul nu exista
        - userul este sters de 2 ori
        - testarea funstiei de rollback
    2. get_user_events:
        - userul are evenimente si stergerea reuseste cu succes
        - userul nu are evenimente
        - userul are numai evenimente sterse
        - userul nu exista

---

## 3. Folosirea AI-ului

Fii cinstit. Nu pierzi puncte dacă spui adevărul, dimpotrivă.

- **Ce ai folosit:** ChatGPT, Claude
- **Prompturi reprezentative folosite:** dupa cum se poate observa si in conversatiile pe care le-am distribuit, promturile mele difera. In prima parte cand lucram la bug-uri promturile contineam maxim 1-2 propozitii si se refereau la particularitati de sintaxa python. Cand am lucrat la endpointuri promturile contineau si bucati de cod reprezentative, erori si pareri proprii
- **Unde te-a ajutat cel mai mult:** Cel mai mult m-a ajutat sa realizez testul "test_soft_delete_user_with_error_does_rollback" pentru ca foloseste monkeypatch, ceea ce eu nu am mai vazut sau folosit pana acum. M-a ajutat sa inteleg cum functioneaza si mi-a oferit mai multe variante de patch. M-am ajutat destul de mult de AI pentru construirea testelor pentru a putea rezolva mai repede taskul 
- **Unde te-a încurcat sau ți-a dat un răspuns greșit:** Motivul pentru care am 3 linkuri de conversatie este pentru ca la un moment dat chatGPT a incetat sa mai raspunda relevant, probabil a schimbat modelul pentru ca mai purtasem conversatii cu el pentru alte proiecte inainte. In a doua conversatie se poate vedea ca Claude a oferit destul de multe versiuni de cod pentru patchul din "test_soft_delete_user_with_error_does_rollback". Destul de multe variante nu functionau sau nu aveau sens
- **Cum ai verificat ce-a generat:** Am citit codul si explicatiile generate de fiecare data. Am testat sa vad daca ruleaza solutiile si la final cand toate testele treceau, am mai citit odata codul sa ma asigur ca e cu adevarat ok ce se intampla cand se apeleaza functiile
- **Anexă opțională — export chat:** 
    1. https://chatgpt.com/share/6a1d664d-e720-83eb-8feb-b2708312875d
    2. https://claude.ai/share/886d5ab5-a383-49b2-a4af-6ed0adc388b6
    3. https://claude.ai/share/fb9060fb-a6b5-40be-9236-793c864bc449

---

## 4. Ce-ai face cu mai mult timp

- as adauga validatoare pentru user input ca sa nu pot introduce stringri goale sau date care exista deja in baza de date
- as adauga mecanisme care sa previna accesul simultan la datele din dictionar pentru operatiile care creaza modificari. Spre exemplu functia mea de atomic_soft_delete_user si functia soft_delete_event ar putea intra in race condition pentru modificarea valorii deleted_at al unui eveniment
- endpointurile pentru get /users/{user_id} si get /event/{event_id} returneaza si obiecte soft deleted, si ar trebui schimbat comportamentul (daca asta se doreste de la endpointurile respective)

---

## 5. Întrebări / observații

(Orice nu a fost clar, orice ai vrea să discuți cu noi.)
