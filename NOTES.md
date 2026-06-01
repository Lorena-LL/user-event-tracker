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
- **Unde era:**
- **Cum l-am găsit:**
- **Cum l-am fixat:**

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
