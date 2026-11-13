# AI Rentgenová diagnostika (429tmrob)

Webová full-stack aplikace pro asistovanou analýzu a detekci fraktur na rentgenových snímcích. Projekt vznikl v rámci České AI Olympiády 2026 (okresní kolo, Moravskoslezský kraj).

Systém kombinuje počítačové vidění pro detekci fraktur s jazykovým modelem pro generování strukturované lékařské zprávy v přirozeném jazyce.

## Architektura

- **Backend:** FastAPI, Python 3.11+, `uv` package manager
- **Detekční model:** Ultralytics YOLOv8 (`backend/ai_model.pt`) pro lokalizaci a klasifikaci fraktur
- **Interpretace nálezů:** Groq API (Llama 3 / Mixtral) pro generování srozumitelného souhrnu pro lékaře i pacienta
- **Frontend:** React, TypeScript, Vite, Tailwind CSS, Lucide ikony
- **Reverzní proxy:** Caddy (automatické HTTPS, směrování API a statických souborů)

## Struktura repozitáře

```text
429tmrob/
├── backend/
│   ├── main.py          # FastAPI server, inference pipeline, Groq integrace
│   ├── ai_model.pt      # Natrénovaný YOLO detekční model
│   ├── images/          # Ukázkové rentgenové snímky
│   └── uploads/         # Dočasné úložiště nahraných snímků
├── frontend/
│   ├── src/             # React komponenty, stav a UI
│   └── package.json
├── Caddyfile            # Konfigurace reverzní proxy
├── docker-compose.yml   # Kontejnerizace služeb
└── README.md
```

## Lokální spuštění

### Požadavky
- Python 3.11+ a správce balíčků `uv`
- Node.js 18+ a `npm`
- Groq API klíč (volitelný pro textovou interpretaci)

### 1. Backend

```bash
cd backend

# Nastavení prostředí a spuštění serveru
uv run uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

Při prvním spuštění vytvořte v kořeni projektu soubor `.env`:
```env
GROQ_API_KEY=your_groq_api_key_here
```

### 2. Frontend

```bash
cd frontend

npm install
npm run dev
```

Aplikace bude dostupná na `http://localhost:5173`.

### 3. Spuštění přes Docker Compose

```bash
docker-compose up --build
```

## Hlavní funkce

1. **Nahrání snímku:** Podpora formátů JPEG, PNG i DICOM exportů.
2. **Inference v reálném čase:** YOLO model provede lokalizaci podezření na frakturu a vrátí souřadnice bounding boxu a úroveň spolehlivosti.
3. **Strukturovaný lékařský report:** Groq LLM vygeneruje souhrn nálezu s doporučeným postupem dalšího vyšetření.
4. **Interaktivní prohlížeč snímků:** Možnost vizuálního porovnání původního a anotovaného rentgenu.

## Licence

MIT
