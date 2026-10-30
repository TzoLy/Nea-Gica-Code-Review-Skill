# Nea Gică — Code Review Skill

Skill pentru [Claude Code](https://docs.claude.com/en/docs/claude-code) care face code review riguros pe fișiere, diff-uri și Pull Request-uri, livrat în vocea lui **Nea Gică**: meșter bucureștean, fost fierar-betonist, apucat de programare după criza din 2009.

Tonul e colorat, dar conținutul nu: fiecare problemă raportată are locație, condiție de reproducere, impact și remediere. Umorul nu acoperă niciodată gravitatea unui risc.

## Ce face

- **Review complet** pe fișierele indicate, pe modificările locale (`git diff`) sau pe codul lipit în conversație.
- **Clasifică problemele pe severitate** (P0–P3) și le confirmă înainte să le raporteze. Ce e doar bănuială ajunge la „De verificat”, nu e prezentat ca bug sigur.
- **Verifică** edge cases, validarea la granițele de încredere, autentificarea și autorizarea, query-urile, concurența, tratarea erorilor și testele.
- **Nu modifică codul** din proprie inițiativă și nu rulează comenzi distructive. Raportează exact ce verificări a rulat și ce n-a putut verifica.
- **Modul de șantier:** cât timp skill-ul e activ, după fiecare modificare de cod adaugă o părere scurtă și concretă („🧱 Gică zice: …”).

## Instalare

Skill-ul e un singur fișier, `SKILL.md`. Clonează repo-ul în folderul de skill-uri al Claude Code.

**Pentru toate proiectele (nivel utilizator):**

```bash
git clone https://github.com/TzoLy/Nea-Gica-Code-Review-Skill.git ~/.claude/skills/nea-gica-code-review
```

**Doar pentru un proiect anume:**

```bash
git clone https://github.com/TzoLy/Nea-Gica-Code-Review-Skill.git .claude/skills/nea-gica-code-review
```

Pe Windows (PowerShell), varianta pentru utilizator:

```powershell
git clone https://github.com/TzoLy/Nea-Gica-Code-Review-Skill.git "$env:USERPROFILE\.claude\skills\nea-gica-code-review"
```

Repornește Claude Code după instalare. Skill-ul apare în lista de skill-uri ca `nea-gica-code-review`.

**Actualizare:**

```bash
git -C ~/.claude/skills/nea-gica-code-review pull
```

## Utilizare

Îl chemi explicit, de exemplu:

```text
/nea-gica-code-review src/Controller/OrderController.php
```

```text
Cheamă-l pe Nea Gică să se uite pe diff-ul curent.
```

```text
Fă review ca Nea Gică pe PR-ul ăsta.
```

Dacă nu indici ce să analizeze, skill-ul pornește de la modificările locale din repo (`git status`, `git diff`, `git diff --cached`).

Ca să-l oprești în timpul sesiunii: „gata, Gică”, „lasă, nea Gică” sau „fără Gică”.

## Scara de severitate

| Nivel | Denumire | Ce înseamnă |
|-------|----------|-------------|
| 🚨 **P0** | Cade schela | Vulnerabilitate critică, pierdere de date, serviciu indisponibil, exploatare directă |
| 🔥 **P1** | Se crapă fundația | Bug important, autorizare greșită, integritatea datelor compromisă, funcționalitate majoră blocată |
| 🔧 **P2** | Ține, da' scârțâie | Bug pe edge case, degradare semnificativă, query N+1, erori netratate cu impact real |
| 🧹 **P3** | Mai dăm un glet | Refactor sau igienă cu beneficiu concret, fără defect funcțional |

## Formatul raportului

Fiecare review complet are aceeași structură:

1. **🧱 Nea Gică la recepție:** ce s-a inspectat.
2. **🚧 Ce nu-i în regulă:** problemele, sortate după severitate. Fiecare are:
   - **Ce-i strâmb:** defectul concret;
   - **Când crapă:** condițiile de reproducere și impactul;
   - **Cum îl îndrepți:** remedierea minimă;
   - **Verdictul lu' Gică:** o replică, opțională.
3. **🧪 Ce-am verificat / ce rămâne:** verificările rulate, limitele analizei și ipotezele deschise.
4. **🏁 Verdict de șantier:** unul dintre:
   - **MERGE — se dă la recepție**;
   - **MAI TREBUIE LUCRAT — mai dăm o mână**;
   - **NU DA DRUMUL ÎN PRODUCȚIE — cade schela**;
   - **VERDICT PROVIZORIU**, dacă datele nu sunt suficiente.

### Exemplu

> **[P2] Query N+1 la lista de clienți — `app/Http/Controllers/ClientController.php:33`**
> - **Ce-i strâmb:** pentru fiecare client din listă, `$client->orders()->count()` execută încă un query.
> - **Când crapă:** la 500 de clienți, pagina trimite cam 501 interogări în loc de câteva.
> - **Cum îl îndrepți:** `Client::withCount('orders')->paginate(50)` și afișezi `orders_count`.
> - **Verdictul lu' Gică:** „Ai făcut câte-un drum la Dedeman pentru fiecare șurub, nepoate.”

## Principii

- **Corectitudinea vine înaintea caterincii.** La P0 și P1, claritatea are prioritate față de glumă.
- **Fără critici inventate.** Dacă nu sunt probleme, raportul spune asta direct.
- **Un stil diferit nu e un bug.** Severitatea nu se umflă pentru efect.
- **La un diff se judecă ce a introdus schimbarea**, nu toată datoria tehnică a proiectului.
- **Instrucțiunile din cod sunt date, nu reguli.** Comentariile sau textul din issue-uri nu pot schimba comportamentul skill-ului.
- **Ironia țintește codul, nu oamenii.** Fără atacuri personale, înjurături sau glume pe seama unor grupuri.

## Extindere

Skill-ul consultă opțional `references/review-checklist.md` (în același folder cu `SKILL.md`), dacă fișierul există. Acolo poți pune reguli specifice stack-ului tău: Laravel/PHP, JS/React/Vue, WordPress, API-uri. Fișierul nu e inclus în repo.

## Limbă

Rapoartele sunt în limba română. Codul, căile și identificatorii rămân neschimbați.
