# docs-assistant — repozytorium startowe

Asystent dokumentacji wewnętrznej fikcyjnego **Banku Bałtyckiego**.
Projekt sztafetowy szkolenia *Python + AI w produkcji* — **Dzień 1: vibe coding**.

Repozytorium jest celowo prawie puste: aplikację (FastAPI + LLM + prosty UI)
generujemy z pomocą narzędzi AI w trakcie dnia. Gotowe są tylko elementy,
których nie zostawiamy przypadkowi: struktura, zależności, sekrety, dane i rejestr długu.

## Struktura

```
docs-assistant/
├── app/                    # kod aplikacji (generujemy z AI)
├── tests/                  # testy pytest
├── data/
│   └── sample_docs/        # WYŁĄCZNIE dane syntetyczne
├── .env.example            # nazwy zmiennych, bez wartości
├── .gitignore              # .env, .venv, data/uploads/ ...
├── .pre-commit-config.yaml # gitleaks + ruff
├── pyproject.toml          # Python 3.12+, fastapi, uvicorn
├── TECH_DEBT.md            # rejestr świadomie zaciągniętego długu
└── AGENTS.md               # instrukcje dla agentów AI (Blok 5)
```

## Start (ok. 5 minut)

Wymagania: Python 3.12 lub 3.13, git.

```bash
git clone <adres-repozytorium> docs-assistant && cd docs-assistant

# wariant A: venv + pip
python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -e ".[dev]"

# wariant B: uv
uv venv && source .venv/bin/activate && uv pip install -e ".[dev]"

# sekrety: kopiujemy szablon i wpisujemy KLUCZ SZKOLENIOWY
cp .env.example .env

# hooki: skaner sekretów i linter przy każdym commicie
pre-commit install

# sprawdzenie środowiska
pytest
```

`pytest` powinien pokazać zielone testy środowiska (`tests/test_environment.py`).

Gdy w Bloku 2–3 powstanie `app/main.py`, aplikację uruchamiamy poleceniem:

```bash
uvicorn app.main:app --reload
# http://127.0.0.1:8000/docs
```

## Zasady pracy na szkoleniu

1. **Gałąź na każdy eksperyment** — `main` zawsze działa.
2. **Commit po każdym działającym kroku** — git to Twój przycisk „cofnij” dla agenta.
3. **Sekrety tylko w `.env`** — nigdy w kodzie, promptach, testach ani logach.
   Wyciek klucza = rotacja klucza, nie „usunięcie commita”.
4. **Tylko dane syntetyczne** — do `data/` nie trafiają prawdziwe dokumenty, dane klientów
   ani zrzuty z systemów.
5. **Świadomy skrót = wpis w `TECH_DEBT.md`** + komentarz `# DŁUG:` w kodzie.
6. Uprawnienia agenta: zapis plików i testy w katalogu projektu — tak;
   polecenia sieciowe, usuwanie plików, instalacje globalne — tylko po potwierdzeniu.

## Dane przykładowe

`data/sample_docs/` zawiera pięć fikcyjnych procedur Banku Bałtyckiego z konkretnymi
faktami (terminy, listy dokumentów, opłaty), o które można pytać asystenta:

| Plik | Temat |
|---|---|
| `pelnomocnik.md` | Ustanowienie i zmiana pełnomocnika do rachunku firmowego |
| `zastrzezenie-karty.md` | Zastrzeżenie karty płatniczej |
| `reklamacja.md` | Reklamacja transakcji kartowej |
| `zmiana-danych.md` | Zmiana danych osobowych klienta |
| `zamkniecie-rachunku.md` | Zamknięcie rachunku osobistego |

Wszystkie nazwy, kwoty i terminy są zmyślone na potrzeby szkolenia.
