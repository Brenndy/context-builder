# Master / Chief of Staff — Plan

## 1. Cel

Zbudować osobisty system Chief of Staff / Master nad Claude Code, który utrzymuje spójny model pracy użytkownika przez wiele repozytoriów, sesji Claude Code, spotkań, Jira, Git i ad-hoc informacji.

Master ma przede wszystkim zmniejszać chaos i koszt przełączania kontekstu. Nie zastępuje Claude Code i nie jest drugim runtime'em AI.

## 2. Model działania

Claude Code jest mózgiem i środowiskiem wykonawczym.

To repo dostarcza:
- pamięć i canonical state,
- skills i procedury,
- rejestr projektów,
- kontekst projektów,
- tooling do sesji,
- ingestion i reconciliation,
- relacje między tematami, projektami i zależnościami.

Użytkownik może:
- uruchomić Mastera ręcznie w `~/chief`,
- wejść bezpośrednio do dowolnego repo i uruchomić Claude Code,
- poprosić Mastera o spawn nowej sesji Claude Code dla projektu,
- później pozwolić Masterowi zebrać wiedzę z sesji.

Nie budujemy własnego Claude/LLM drivera jako podstawowej warstwy systemu.

## 3. Wiele repozytoriów i sesji

Każdy projekt/repo jest niezależny.

Może istnieć wiele sesji Claude Code dla jednego repo:
- uruchomionych przez Mastera,
- uruchomionych ręcznie przez użytkownika,
- odkrytych później przez ingestion.

Master nie zakłada, że jest właścicielem sesji.

Sesja ma być niezależna i może być ręcznie przejęta przez użytkownika.

## 4. Project Registry

`projects.yaml` jest rejestrem projektów i repozytoriów.

Skill `/project` obsługuje:
- add/register,
- list,
- show,
- update,
- archive/remove.

Minimalna rejestracja projektu wymaga identyfikatora i repozytorium. Reszta kontekstu ma być możliwie automatycznie odkrywana.

## 5. Project Context — kluczowy element

Master nie powinien rozumieć projektu wyłącznie przez skanowanie kodu.

Najpierw czyta dokumentację i guidelines repozytorium:

1. root/nested `CLAUDE.md`,
2. README,
3. `docs/`,
4. architecture/design docs,
5. contribution/development guides,
6. ADRs,
7. metadata/configuration,
8. kod, testy i infrastructure jako evidence/weryfikacja.

Project Context ma odpowiadać m.in.:
- po co projekt istnieje,
- jaki problem rozwiązuje,
- kto go używa,
- jaką pełni rolę,
- jakie ma główne capabilities,
- jakie ma granice,
- z czym się integruje,
- jak działa i jest wdrażany,
- jakie ma ograniczenia i zasady,
- jakie ma terminologie/aliasy,
- z jakimi projektami jest powiązany,
- co jest faktem, a co inferencją,
- czego jeszcze nie wiadomo.

Każda istotna informacja powinna mieć evidence i confidence.

Kod może potwierdzić albo zakwestionować dokumentowany obraz projektu. Konflikty nie są cicho nadpisywane.

Project Context jest wykorzystywany przez:
- `/project`,
- `/project-status`,
- `/delegate-to-project`,
- `/session`,
- `/find-related`,
- spawning sesji,
- cross-project correlation.

Odświeżanie powinno być incrementalne.

## 6. Najważniejszy obiekt: Topic

Jira ticket nie jest głównym obiektem systemu.

Topic reprezentuje rzeczywisty temat pracy i może łączyć:
- Jira,
- projekty,
- repozytoria,
- ludzi,
- meetingi,
- sesje Claude,
- commity/PR-y,
- decyzje,
- findings,
- status,
- next actions,
- ryzyka,
- zależności.

Master powinien szukać istniejącego Topic przed utworzeniem nowego.

## 7. Typy wiedzy

Należy rozróżniać:
- FACT,
- DECISION,
- COMMITMENT,
- TASK,
- DEPENDENCY,
- HYPOTHESIS,
- IDEA,
- OBSERVATION,
- RISK,
- OPEN_LOOP.

Nie wolno zamieniać:
- sugestii w commitment,
- hipotezy w fakt,
- pomysłu w decyzję.

## 8. Naturalne wejście

Użytkownik nie powinien musieć używać struktury.

Przykład:

„Żeby zrobić B, DevOps musi jeszcze ustawić X.”

Master powinien wywnioskować:
- Topic B,
- dependency B blocked_by X,
- owner X = DevOps,
- status pending.

Później:

„DevOps zrobił X.”

Master powinien znaleźć istniejące X, zaktualizować dependency i sprawdzić downstream. Może poinformować, że B jest potencjalnie odblokowane.

Język niepewności musi być zachowany:
- „może” nie jest faktem,
- „wydaje mi się” nie jest faktem,
- „powinniśmy” nie jest automatycznie decyzją.

## 9. Entity Resolution

Master powinien korelować informacje przez:
- identyfikatory,
- Jira IDs,
- aliasy,
- projekty,
- repozytoria,
- osoby,
- keywords,
- znaczenie semantyczne,
- zależności,
- decyzje,
- historię,
- recent activity.

Ambiguity należy zachowywać zamiast arbitralnie rozstrzygać.

## 10. Evidence i konflikty

Każda ważna informacja powinna mieć source.

Źródła:
- user message,
- meeting,
- Jira,
- Git,
- Claude session,
- note,
- external system.

Jeżeli źródła się różnią, Master zachowuje obie informacje, oznacza konflikt i wskazuje, jakie evidence mogłoby go rozstrzygnąć.

`/reconcile` odpowiada na pytanie:

„Czy nowe informacje zmieniają obecny model świata?”

Możliwe efekty obejmują:
- NO_CHANGE,
- ENRICH_EXISTING,
- UPDATE_STATUS,
- CREATE_RELATIONSHIP,
- CREATE_DEPENDENCY,
- RESOLVE_DEPENDENCY,
- CREATE_COMMITMENT,
- RESOLVE_COMMITMENT,
- CREATE_CONFLICT,
- SUPERSEDE_PREVIOUS_INFORMATION,
- REOPEN_TOPIC,
- CREATE_NEW_TOPIC.

## 11. Claude sessions

Master powinien mieć tooling do:
- listowania sesji,
- statusu sesji,
- rejestrowania/linkowania sesji,
- przygotowania kontekstu,
- spawnienia sesji Claude Code,
- późniejszego ingestion.

Przy spawnowaniu Master przekazuje:
- projekt/repo,
- Project Context,
- tylko relevant Topics,
- Dependencies,
- Decisions,
- Commitments,
- objective,
- constraints.

Nie tworzymy własnego LLM drivera.

Sesja spawniona przez Mastera ma działać tak samo jak sesja uruchomiona ręcznie.

## 12. Ingestion

Model:

RAW → SESSION JOURNAL → MASTER KNOWLEDGE

Preferowane są strukturalne eventy sesji, np.:
- finding,
- change,
- decision,
- status,
- test,
- blocker,
- commitment.

Transcript jest fallbackiem/enrichmentem.

`/ingest-session` ma działać incrementalnie.

`/ingest-meeting` ma wyciągać:
- decisions,
- commitments,
- owners,
- deadlines,
- dependencies,
- topics,
- status,
- risks,
- open questions,
- Jira references.

## 13. Background ingestion

Użytkownik nie powinien ręcznie triggerować zbierania wiedzy z każdej sesji.

Docelowo scheduler:
1. wykrywa zmiany,
2. zbiera delta,
3. uruchamia odpowiednie tooling Claude,
4. wykonuje ingestion,
5. robi reconciliation,
6. aktualizuje state.

Na MVP nie potrzebujemy event busa ani ciężkiej infrastruktury.

Scheduler może być cron/systemd/launchd.

Dla każdego źródła utrzymujemy cursor/last_sync/fingerprint.

## 14. Canonical state

`state/` jest canonical operational model.

Raw sources są osobno w `sources/`.

Rekomendowana struktura:

```
state/
  projects/
  project-context/
  topics/
  commitments/
  dependencies/
  decisions/
  evidence/
  sessions/
```

Każda encja zachowuje source references i temporal metadata.

## 15. Lifecycle

Encje mają m.in.:
- created_at,
- first_seen,
- last_activity_at,
- last_meaningful_change,
- resolved_at.

Lifecycle:

ACTIVE → STALE → DORMANT → ARCHIVED

Staleness nie jest wyłącznie funkcją wieku. Uwzględnia:
- activity,
- open commitments,
- dependencies,
- deadlines,
- recent mentions,
- status.

Przed archiwizacją sprawdzamy:
- open commitment,
- blocking dependency,
- future deadline,
- unresolved decision,
- recent mention/activity.

Archiwizacja jest odwracalna.

## 16. Daily Review

`/daily-review` ma pokazywać tylko rzeczy istotne:
- material changes,
- open/overdue commitments,
- blocked/unblocked work,
- ważne decisions,
- conflicts,
- risks,
- stale topics,
- rzeczy wymagające decyzji użytkownika.

Nie ma produkować kolejnego strumienia szumu.

## 17. Project Status

`/project-status`:

- FACTS,
- DECISIONS,
- OPEN LOOPS,
- COMMITMENTS,
- BLOCKERS,
- RISKS,
- UNKNOWN,
- NEXT ACTIONS.

## 18. Delegowanie

`/delegate-to-project` wybiera właściwe repo na podstawie:
- projects.yaml,
- Project Context,
- Topic,
- dependencies,
- relevant history.

Zadanie może dotyczyć wielu repozytoriów.

Master ma przekazać agentowi tylko potrzebny kontekst.

## 19. Granica automatyzacji

Automatycznie można:
- zbierać źródła,
- korelować wiedzę,
- aktualizować lokalny state,
- wykrywać dependencies,
- wykrywać konflikty,
- wykrywać stale topics,
- przygotowywać raporty.

Explicit authorization jest wymagana dla zewnętrznych side effects:
- zmiany Jira,
- wysyłanie wiadomości,
- merge,
- deploy,
- zmiany produkcyjne,
- inne nieodwracalne akcje.

## 20. MVP

Pierwsza działająca wersja powinna obejmować:

1. centralne `~/chief`,
2. `CLAUDE.md`,
3. `projects.yaml`,
4. Project Context,
5. Topic,
6. Commitment,
7. Dependency,
8. Decision,
9. Evidence,
10. project management tooling,
11. session tooling,
12. session ingestion,
13. meeting ingestion,
14. `/ingest`,
15. `/reconcile`,
16. `/find-related`,
17. state query/update,
18. stale detection,
19. daily review,
20. Claude Code session spawning.

Pierwsze źródła:
- user messages,
- Claude sessions,
- meetings/transcripts,
- Git.

Jira jako kolejny adapter.

## 21. Etap 2

- Jira adapter,
- meeting sources,
- session event extraction,
- entity resolution,
- reminders,
- project status,
- cross-repo dependencies,
- lepszy incremental sync,
- automatyczne Project Context refresh.

## 22. Etap 3

Dopiero gdy file-first model okaże się ograniczeniem:
- vector search,
- graph DB,
- bardziej zaawansowane retrieval,
- autonomous delegation,
- Slack/Teams,
- calendar,
- external actions,
- GUI.

GUI ma być tylko kolejną warstwą nad tym samym state/tooling. Nie powinien wymagać przebudowy modelu wiedzy.

## 23. Główna zasada projektowa

Nie budować systemu dlatego, że technicznie „da się” go zautomatyzować.

System ma przede wszystkim:
- pamiętać za użytkownika,
- łączyć rozproszone informacje,
- pilnować open loops,
- wykrywać zależności i konflikty,
- dostarczać właściwy kontekst Claude Code,
- umożliwiać szybkie delegowanie pracy,
- zmniejszać liczbę ręcznych czynności.

Claude Code pozostaje głównym interfejsem i reasoning engine. Master jest warstwą pamięci, kontekstu i koordynacji.
