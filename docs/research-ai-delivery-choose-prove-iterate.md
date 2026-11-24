# Research: metodyka AI u klienta (choose → prove → iterate)

**Retrieval:** 2026-11-24 (Europe/Warsaw)  
**Scope:** Oferta partnerska ISV/OEM + autonomiczny agent / MCP w orgie klienta  
**Baseline PEAK:** `main` / branch pracy `cursor/peak-business-presentation-24fc`  
**Metoda:** Salesforce Docs MCP + oficjalne Developer/Help docs + PEAK practitioner (`partner-journey.md`) + wybrane źródła partnerskie (blog/guidebook — oznaczone)

Evidence labels: **fact** (docs), **practitioner** (PEAK), **inference**, **unknown**.

---

## 1. Executive summary

Wdrożenie AI u klienta przez partnera ISV **nie kończy się install pakietu**. Oficjalny model Salesforce rozdziela:

1. **To, co partner pakuje** (actions, topics, templates, Apex/Flow, czasem BotTemplate).  
2. **To, co klient tworzy/konfiguruje** (własny agent z template, MCP server activation, ECA/OAuth, permissions, połączenie Claude/ChatGPT/Slack).  
3. **Dowód działania warstwami** (connection → tools → model selection → business result) — zgodne z PEAK *choose → prove → iterate*.

Dla oferty: sprzedajesz **outcome + pilot + handoff setup**, nie „włączymy AI”.

---

## 2. Choose — jakie ścieżki są realne

| Ścieżka | Kiedy rozważać | Co ustalić u klienta | Źródło |
|---|---|---|---|
| Native Agentforce (actions/topics w Agentforce) | Task zostaje w Salesforce agent surface | Agentforce enablement, permissions, template vs custom agent | **fact** [Agent templates 2GP](https://developer.salesforce.com/docs/platform/pkg2-dev/guide/dev2gp-package-agent-templates.html) |
| Salesforce Hosted MCP (standard/custom) | Klient MCP (Claude, ChatGPT, Cursor…) musi działać na danych/logice SF | Flex Credits / Foundations, ECA + `mcp_api`, per-user OAuth | **fact** [Hosted MCP overview](https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/hosted-mcp-servers-overview.html) |
| External integration (partner MCP → SF APIs) | Logika poza SF; klient nie jest „Salesforce MCP boundary” | API/credentials owner, nie dziedziczy setupu hosted MCP | **practitioner** PEAK `partner-journey.md` |
| Direct API | Stała integracja bez tool discovery | Czy MCP w ogóle dodaje wartość | **practitioner** PEAK |

**Decydujący fakt (choose):** czy jest **MCP client + potrzeba tool selection**, czy wystarczy native agent / API.  
**Nie zakładaj MCP** tylko dlatego, że jest dostępne (**practitioner**).

---

## 3. Prove — co oficjalnie mówi o „najpierw udowodnij”

### Hosted MCP — oficjalna kolejność (**fact**)
1. Setup org (feature + External Client App)  
2. Connect MCP client (Claude / ChatGPT / Postman…)  
3. Test connection  
4. Best practices przed szerszym rollout  

Źródło: [Getting Started — Hosted MCP](https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/hosted-mcp-servers-overview.html)

**Prove separation (**fact** + **practitioner**):**
- Testuj raw MCP (np. Postman) **osobno** od zachowania modelu w Claude/ChatGPT — oddziela auth/server od tool-selection.  
  Docs: [Postman testing](https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/postman-testing.html); PEAK `channel-rollout.md`.
- Connection ≠ tool visibility ≠ business correctness (**practitioner** diagnoza warstw).

### Security przy Prove (**fact**)
- Każda transakcja MCP = **uwierzytelniony user**; CRUD/FLS/sharing/PS obowiązują.  
- Audit trail przypisuje akcję użytkownikowi.  
- Best practice: tool-level restrictions / approval przed write; najpierw sandbox.  
  Źródło: Headless 360 MCP security notes w Docs MCP search (Hosted MCP servers reference).

### Agentforce ISV packaging — krytyczne dla oferty (**fact**)
- ISV dystrybuuje **agent templates**, nie gotowego „agenta zainstalowanego jako Bot”.  
- Po install: klient w UI Agentforce **tworzy agenta z template**.  
- **Nie** pakujesz Bot/BotVersion (usuń przed package version).  
- **Agent Script nie jest wspierany** jako źródło packageable agent template (oficjalne Important).  
- Dev Hub: Einstein Chatbot enablement + scratch-def z wymaganym metadata.  

Źródło: [Develop and Package Agent Templates](https://developer.salesforce.com/docs/platform/pkg2-dev/guide/dev2gp-package-agent-templates.html)

**Implikacja oferty (**inference**):** w SoW musi być linia *customer creates agent from template* + walidacja w orgie klienta — nie „agent pojawi się po install”.

### Agent Script path (**practitioner** PEAK)
Jeśli partner używa Agent Script: osobny pattern carrier + customer setup/import/upgrade (`agent-script-packaging-pattern.md`). To **nie** jest ten sam flow co BotTemplate 2GP powyżej — nie mieszaj w ofercie bez wyboru ścieżki.

---

## 4. Iterate — warstwy awarii (PEAK × docs)

| Warstwa | Co udowodnić | Typowy next step |
|---|---|---|
| Environment / auth | Feature, ECA, scopes, login do właściwego orga | Multitenant: wyloguj inne orgy przed OAuth (**fact** troubleshooting CRM Analytics MCP) |
| Connection / discovery | Endpoint + widoczne tools | Setup MCP Servers / API Catalog, PKCE ECA |
| Direct tool invoke | Znany input → expected output | Postman / MCP Inspector |
| Model selection | Intent wybiera właściwy tool | Opisy tools, overlapping tools |
| Business behavior | Dane/side effects pod real identity | Permissions, partial errors |
| Rendering (HXL) | UI na kanale | Osobny check per Claude/ChatGPT/Slack |

Źródła: PEAK `partner-journey.md`; Hosted MCP troubleshooting/Postman docs.

---

## 5. Commercial / credits — co wolno mówić w ofercie

| Twierdzenie | Status |
|---|---|
| Hosted MCP intended for customers with **Flex Credits**; usage may be billed | **fact** — [Hosted MCP overview — Billing](https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/hosted-mcp-servers-overview.html) |
| Exact credit burn dla Twojego use case | **unknown** bez customer AE / metering evidence — nie licz w PEAK |
| Agentforce for ISVs GA; flex credits wholesale / partner packaging | **fact** (marketing) — [Salesforce blog Oct 2025](https://www.salesforce.com/blog/agentforce-for-isv-partners/); szczegóły kontraktu → PAM / Partner docs (**unknown** bez konta partnera) |
| Install ≠ aktywacja MCP / połączenie Claude | **fact** + **practitioner** channel-rollout |

**Oferta:** sekcja „Commercial assumptions” + „Client must confirm Flex Credits / Agentforce entitlement with AE” — nie udawaj cennika.

---

## 6. Research → pytania do klienta (uzupełnienie discovery)

### MUST (blokują choose/prove)
1. Jaki **runtime client** w pilocie: Agentforce UI / Claude / ChatGPT / Slack / inny?  
2. Czy klient ma (lub kupi) **Agentforce** i/lub **Flex Credits / Foundations**?  
3. Write vs read-only w pilocie? Które obiekty?  
4. Czy dostawa to **managed 2GP template**, config-only, czy usługi?  
5. Kto tworzy agenta po install (admin klienta) — czy jest w scope?  
6. Czy ścieżka to **BotTemplate** czy **Agent Script** (wykluczają się w packageable template flow)?  
7. Czy dane mogą iść do zewnętrznego LLM (Claude/ChatGPT) pod polityką klienta?

### MUST na środowisku (Assess)
1. Org non-prod z włączonym Agentforce (jeśli ta ścieżka)  
2. Potwierdzenie MCP Servers / feature enablement (jeśli Hosted MCP)  
3. **External Client App** + scope `mcp_api` — **tylko** gdy pilot używa Hosted MCP
   z klientem OAuth (Claude / ChatGPT / Cursor itd.). Nie wymagaj ECA dla native
   Agentforce-only ani dla Slack (osobny connection/mapping flow). W scratch:
   Setup nie tworzy ECA bezpośrednio — użyj udokumentowanej trasy Dev Hub/package
   (**practitioner** `channel-rollout.md`; **fact** Hosted MCP ECA guides).  
4. User testowy z realnymi permissionami (nie tylko admin „na wszystko”)  
5. Reprezentatywne dane use case  
6. Jeśli ISV: scratch Agentforce-enabled + Dev Hub Einstein Chatbot (pod BotTemplate)  
7. Lista istniejących actions/flows/Apex do reuse  

### NICE
- Poprzednie Security Review / data flow  
- Multi-channel plan (Slack osobno — inny connection path)  
- Consumption alerts / credit owner po stronie klienta  

---

## 7. Research → struktura oferty (rekomendacja)

| Sekcja oferty | Treść z research |
|---|---|
| Outcome | 1 measurable business result |
| Choose | Rekomendacja + fakt rozstrzygający; alternatywa tylko jeśli wykonalna, inaczej constraint |
| Prove (pilot) | 1 client, 1 task, pass/fail; Postman/raw przed modelem jeśli MCP |
| Deliverables | Package assets **vs** customer setup (agent-from-template; ECA tylko dla Hosted MCP OAuth clients; Slack osobno) |
| Exclusions | Credits/cennik SF; pełny change-mgmt; inne kanały poza pilotem |
| Commercial assumptions | Flex Credits / Agentforce entitlement = client+AE |
| Security | Per-user auth, FLS, tool restrictions, sandbox-first |
| Iterate plan | 3 rozszerzenia po pass pilota |
| Unknowns | Jawna lista |
| Authority | Assess = technical readiness only; Act wymaga bounded plan + jawnego approval (readiness ≠ permission) |

---

## 8. Mapowanie na PEAK skills

| Faza | Skill |
|---|---|
| Wejście | `oem-isv-advisor` |
| Docs preflight | `advisor-capability-preflight` |
| Choose / prove / iterate AI | `headless-mcp-advisor` (+ `partner-journey.md`) |
| Agentforce design/packaging | `agentforce-implementation-advisor` |
| 2GP / packageability | `packaging-distribution` / focused packaging skills |
| Env / scratch | `development-environment` / `scratch-org-shape` |
| Security | `security-readiness` |
| Execution po approval | delegacja `sf-skills` / Agentforce ADLC (**nie** w advisorze) |

---

## 9. Konflikty / uwagi (nie ukrywaj w ofercie)

1. **Agent Script vs BotTemplate packaging** — oficjalnie Agent Script ≠ packageable agent template; PEAK ma osobny practitioner pattern. Wybór musi być jawny.  
2. **Install package ≠ agent live / MCP connected** — customer steps obowiązkowe.  
3. **Billing Flex Credits** — oficjalne ostrzeżenie; PEAK nie liczy kredytów.  
4. **Guidebook GTP** (PDF partnerski) może mieć treści historyczne (np. „2GP coming soon”) — **nie** traktuj jako current packaging truth; preferuj Developer Guide 2GP agent templates.  
5. Partner Community / Quip „Agentforce Packaging & Security Review” — **unknown** bez dostępu (wymaga konta partnera).

---

## 10. Źródła (do cytowania)

| # | Typ | URL / path |
|---|---|---|
| 1 | fact | https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/hosted-mcp-servers-overview.html |
| 2 | fact | https://developer.salesforce.com/docs/platform/hosted-mcp-servers/guide/postman-testing.html |
| 3 | fact | https://developer.salesforce.com/docs/platform/pkg2-dev/guide/dev2gp-package-agent-templates.html |
| 4 | fact | https://developer.salesforce.com/docs/platform/appexchange-install-guide/guide/appexchange-install-installation.html |
| 5 | fact | https://developer.salesforce.com/docs/platform/hxl/guide/hxl-mcp-setup-client.html |
| 6 | practitioner | `skills/headless-mcp-advisor/references/partner-journey.md` |
| 7 | practitioner | `skills/headless-mcp-advisor/references/channel-rollout.md` |
| 8 | practitioner | `skills/agentforce-implementation-advisor/references/agent-script-packaging-pattern.md` |
| 9 | marketing | https://www.salesforce.com/blog/agentforce-for-isv-partners/ |
| 10 | related prompts | `docs/choose-prove-iterate-prompty-oferta-ai.md` |

---

## 11. Definition of Done tego researchu

- [x] Oficjalna ścieżka Hosted MCP (setup → connect → test)  
- [x] Oficjalny model ISV Agent Template (nie packageable Bot)  
- [x] Billing Flex Credits oznaczony  
- [x] Mapowanie na choose/prove/iterate + pytania discovery  
- [x] Konflikt Agent Script vs BotTemplate nazwany  
- [ ] Live verify w konkretnym orgie klienta — **nie uruchomione** (brak orga)  
- [ ] Partner-only Quip/guidebook aktualność — **unknown** bez Partner Community  

**Następny krok:** wypełnić MUST z sekcji 6 dla konkretnego klienta, potem prompt #5 z `choose-prove-iterate-prompty-oferta-ai.md`.
