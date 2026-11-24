# Choose → Prove → Iterate — prompty i discovery (oferta AI u klienta)

Materiał dla partnera ISV/OEM używającego **ISV/OEM Advisor** (`/oem-isv-advisor`).
Metodyka: **choose → prove → iterate** (Headless/MCP + Agentforce).

---

## A. Co muszę wiedzieć o kliencie?

Zanim powstanie oferta, story mapa albo pilot autonomicznego agenta — zbierz odpowiedzi na poniższe. Brak odpowiedzi = `unknown` w readoutcie, nie zgaduj.

### 1. Biznes i outcome
- Jaki jest **mierzalny wynik**, którego klient oczekuje (nie „wdrożyć AI”)?
- Jaki jest **pierwszy use case** (1 zdanie: kto → robi co → z jakim rezultatem)?
- Co jest sukcesem pilota vs sukcesem produkcji?
- Jakie KPI / acceptance (czas, jakość, volume, redukcja ticketów)?
- Co jest **poza zakresem** oferty (wyłączenia)?

### 2. Persona i kanał
- Kto jest użytkownikiem końcowym (rola, język, device)?
- Gdzie ma działać AI: **Agentforce / Slack / Claude / ChatGPT / inny**?
- Czy to autonomiczny agent, asystent z HITL (human-in-the-loop), czy tylko rekomendacje?
- Czy klient akceptuje, że model **sam wybiera tool**, czy tylko stały flow?

### 3. Dane i compliance
- Jakie obiekty / systemy czyta i zapisuje agent?
- Czy są dane wrażliwe (PII, finanse, zdrowie, tajemnice handlowe)?
- Wymagania retencji, residency, audit log?
- Kto jest **execution identity** (user? integration user? named credential)?
- Czy dane mogą **opuścić** Salesforce (external MCP / LLM)?

### 4. Org i produkt Salesforce
- Edition / produkty już kupione (Sales, Service, Data 360, Agentforce…)?
- Czy to nowy greenfield, czy istniejący org z legacy?
- Sandbox / scratch / production — gdzie wolno testować write?
- Czy klient ma już agentów, Flow, Apex, integracje do reuse?

### 5. Pakowanie i dostawa (ISV)
- Czy rozwiązanie idzie jako **managed 2GP**, usługi, czy konfiguracja w orgie klienta?
- Co ma być **package-owned**, a co **customer-owned setup**?
- Czy będzie AppExchange / prywatna dystrybucja / tylko ten klient?
- Harmonogram: data pilota, go-live, okna change freeze?

### 6. Bezpieczeństwo i governance
- Czy będzie Security Review / wewnętrzny security gate?
- Kto approves Act (zmiany w orgie) po stronie klienta?
- Polityka prompt injection, external endpoints, Connected Apps?

### 7. Operacje
- Kto supportuje po go-live (partner / klient / shared)?
- Jak mierzyć regresję po upgrade pakietu / zmianie promptów?
- Rollback: co jest odwracalne?

---

## B. Co chcę zobaczyć na środowisku?

**Assess (read-only)** — poproś o dostęp / artefakty. Nie mutuj orga bez Act + approval.

### Minimum do pilota (Prove)
| Artefakt | Po co |
|---|---|
| Alias orga + typ (scratch/sandbox/prod) | Gdzie wolno testować |
| Lista włączonych produktów / features | Agentforce, Data 360, MCP eligibility |
| `project-scratch-def.json` (jeśli partner DX) | Shape vs customer org |
| Przykładowe dane / rekordy use case | Reprezentatywny input pilota |
| Profile / Permission Sets / user do testów | Execution identity |
| Istniejące Flow / Apex / actions / agents | Reuse vs nowa praca |

### Jeśli ścieżka MCP / Headless
| Artefakt | Po co |
|---|---|
| `sfdx-project.json` + package dirs | Granica 2GP |
| `mcpServerDefinition` / tool refs (jeśli są) | Hosted MCP vs external |
| HXL / Lightning types (jeśli UI) | Rendering na kanale |
| Auth: Named Credential / External Client App | Kierunek wywołań |
| Lista tools i opisy | Model selection vs direct invoke |

### Jeśli Agentforce
| Artefakt | Po co |
|---|---|
| Agent / topics / actions (metadata lub Setup export) | Granice autonomii |
| Agent Script + carrier pattern (jeśli dotyczy) | Package vs customer create |
| Prompt templates / data sources | Injection i grounding |
| Scratch features Agentforce | Enablement |

### Packaging / upgrade
| Artefakt | Po co |
|---|---|
| Namespace / Dev Hub / package IDs | Fundament 2GP |
| Ancestry / dependencies | Upgrade path |
| LMA trial vs customer license | Trial ≠ produkcja |

### Security (przed produkcją)
| Artefakt | Po co |
|---|---|
| Data flow diagram (nawet szkic) | Co wychodzi z orga |
| Lista external endpoints | Scope review |
| Wyniki skanerów / poprzednie findings | Remediacja |

### Czego NIE zakładaj z samego „działającego dema”
- Połączenie MCP ≠ poprawne tool selection
- Read-only sukces ≠ write jest bezpieczny
- Scratch features ≠ entitlements u klienta
- Jedno konto testowe ≠ multi-tenant / customer policy

---

## C. Prompty (kopiuj do Agenta)

Start zawsze: `/oem-isv-advisor` (potem routuje do Headless/MCP, Agentforce, Security, Packaging…).

### 1. Oferta dla klienta (scope + wartość)

```
/oem-isv-advisor
Tryb Ask. Metodyka: choose → prove → iterate.

Przygotuj materiał do OFERTY dla klienta końcowego (nie cennik SF credits).
Kontekst: partner ISV wdraża AI (Agentforce i/lub Headless/MCP) w orgie klienta.

Dostarcz:
1) Problem klienta i measurable outcome
2) 2–3 opcje architektury (choose) z plus/minus
3) Zakres pilota (prove): 1 use case, acceptance criteria, co NIE wchodzi
4) Co w iteracjach po pilocie (iterate)
5) Podział: package-owned vs customer-owned setup
6) Ryzyka, unknowns, założenia
7) Draft sekcji oferty: zakres, deliverables, wyłączenia, kryteria sukcesu, next step

Oznacz evidence: fact / practitioner / inference / assumption / unknown.
```

### 2. User stories + story map

```
/oem-isv-advisor
Na bazie metodyki choose → prove → iterate zrób STORY MAPę wdrożenia AI u klienta.

Persona: [np. handlowiec / agent support / manager]
Cel: [np. autonomiczna obsługa zapytania w Salesforce]

Dostarcz:
- Epic → User stories (As a… I want… so that…)
- Acceptance criteria (Given/When/Then)
- Podział: Must (pilot) / Should (iteracja 2) / Could (później)
- Które stories wymagają Agentforce vs MCP vs API
- Co jest w managed package, a co konfiguruje klient
- Definition of Done dla pilota
```

### 3. Planning handbook (playbook projektu)

```
/oem-isv-advisor
Zrób PLANNING HANDBOOK (1–2 strony) dla partnera wdrażającego AI u klienta.

Struktura:
A. Discover — task, dane, tożsamość, limity
B. Choose — porównanie ścieżek (native / API / hosted MCP / external)
C. Prove — smallest useful pilot + pass/fail
D. Hand off — brief do build (Act dopiero po approval)
E. Iterate — diagnoza warstw: auth → connection → tool → model → business → UI
F. Customer handoff — instrukcja setupu u klienta
G. Security & packaging checkpoints

Dla każdej fazy: input, output, owner (partner/klient), typowe błędy.
```

### 4. Autonomiczny agent w środowisku klienta

```
/oem-isv-advisor → headless-mcp-advisor / agentforce-implementation-advisor
Metodyka: choose → prove → iterate.

Chcemy AUTONOMICZNEGO agenta w środowisku klienta Salesforce:
- Klient: [Agentforce / Slack / Claude / inny]
- Zadanie: [opis]
- Dane wrażliwe: [tak/nie — co]
- Write vs read-only: [zakres]

KROK CHOOSE:
Porównaj native Agentforce actions vs hosted MCP vs external integration.
Rekomenduj 1 ścieżkę + 1 alternatywę i fakt, który zmienia wybór.

KROK PROVE:
Zdefiniuj smallest useful pilot:
- 1 task, 1 client, min. tools
- identity / permissions
- measurable acceptance condition
- co pilot ROZSTRZYGA (jaka niepewność)

KROK ITERATE:
Tabela diagnozy przy fail + następny najmniejszy krok.
Oddziel package-owned od customer setup.
Nie deployuj — tylko plan; Act wymaga mojego OK.
```

### 5. Z oferty → pilot (jeden ciąg)

```
/oem-isv-advisor
Połącz: oferta + story map + pilot autonomicznego agenta.

1) 3 bullet value proposition do oferty
2) 5 user stories Must na pilot
3) Choose: rekomendowana architektura (1 akapit)
4) Prove: acceptance criteria pilota (pass/fail)
5) Iterate: 3 kolejne rozszerzenia po sukcesie
6) Lista pytań do klienta, bez których nie startujemy

Tryb Ask. Nie wymyślaj org/entitlements — oznacz unknown.
```

### 6. Assess środowiska (gdy jest dostęp)

```
/oem-isv-advisor
Tryb Assess (read-only).
Oceń, czy jesteśmy gotowi na pilot autonomicznego agenta u klienta.

Sprawdź (jeśli dam dostęp): scratch-def / package boundary / MCP server defs / Agentforce metadata.
Zwróć: gotowe / brakuje / blocker; mapuj na choose→prove→iterate.
Na końcu: czy wolno iść do Act (tak/nie + dlaczego).
```

### 7. Discovery — uzupełnij luki o kliencie i orgie

```
/oem-isv-advisor
Tryb Ask. Nie zgaduj.

Na podstawie tego, co już wiem:
[wklej notatki]

1) Wypisz pytania MUST (blokują choose/prove) vs NICE.
2) Wypisz artefakty środowiska, które mam dostarczyć do Assess.
3) Zaproponuj smallest useful pilot TYLKO jeśli nie brakuje MUST.
4) Oznacz każde twierdzenie: fact / assumption / unknown.
```

---

## D. Mini-checklist przed wysłaniem oferty

- [ ] Jest 1 mierzalny outcome i 1 use case pilota  
- [ ] Są 2 opcje architektury (choose) + fakt rozstrzygający  
- [ ] Acceptance pass/fail (prove) jest binarne  
- [ ] Package vs customer setup rozdzielone  
- [ ] Write/read i dane wrażliwe nazwane  
- [ ] Unknowns wypisane (nie ukryte w marketingu)  
- [ ] Act / deploy nie jest obiecany bez approval i Assess  

---

## E. Źródło metodyki

Practitioner pattern w PEAK: `skills/headless-mcp-advisor/references/partner-journey.md`  
Wejście: `oem-isv-advisor` → `implementation-best-practices` → `headless-mcp-advisor` / `agentforce-implementation-advisor`.
