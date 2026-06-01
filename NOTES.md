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

- **Decizii de design:** (ce-ai considerat? ce ai ales și de ce?)
- **Cazuri edge pe care le-ai acoperit:**
- **Teste adăugate:** (ce verifică fiecare)

---

## 3. Folosirea AI-ului

Fii cinstit. Nu pierzi puncte dacă spui adevărul, dimpotrivă.

- **Ce ai folosit:** (ChatGPT / Cursor / Copilot / altele)
- **Prompturi reprezentative folosite:** (scrie prompturile pe care le consideri relevante + context scurt: la ce te-au ajutat)
- **Unde te-a ajutat cel mai mult:**
- **Unde te-a încurcat sau ți-a dat un răspuns greșit:** (foarte interesant pentru noi!)
- **Cum ai verificat ce-a generat:**
- **Anexă opțională — export chat:** (dacă vrei, poți adăuga un export de chat relevant)

---

## 4. Ce-ai face cu mai mult timp

(Lista scurtă, 3-5 puncte. Arată-ne că ai văzut limitele actuale.)

---

## 5. Întrebări / observații

(Orice nu a fost clar, orice ai vrea să discuți cu noi.)
