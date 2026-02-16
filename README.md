# Plex Playlist Reorder (Apple Music -> Plex)

Web app containerizzabile per Portainer che:
- carica un file playlist Apple Music (TXT/CSV in Unicode),
- supporta login Plex via OAuth (PIN flow) oppure token da `.env`,
- mostra le playlist audio Plex non-smart,
- fa anteprima dei match,
- chiede conferma `Sicuro?`,
- riordina la playlist Plex via API `move`.

## Requisiti
- Plex Media Server raggiungibile dalla rete del container.
- `PLEX_BASEURL` corretto.
- Uno tra:
  - `PLEX_TOKEN` in `.env`, oppure
  - login via pulsante `Accedi con Plex` nella UI.

## Configurazione `.env`
Copia `.env.example` in `.env` e compila:

```env
PLEX_BASEURL=http://IP_DEL_TUO_PLEX:32400
PLEX_TOKEN=
MAX_UPLOAD_MB=8
PORT=8080
```

Se lasci `PLEX_TOKEN` vuoto, fai login dalla UI.
La porta esterna e configurabile con `HOST_PORT` (default `8090`).
Durante il login OAuth si apre un popup dedicato Plex che poi si chiude automaticamente.

## Avvio locale
```bash
docker compose up --build -d
```
Poi apri: `http://localhost:8090`

## Deploy con Portainer (Repository mode)
1. In `Create stack`, scegli `Repository` e usa il repo GitHub.
2. Seleziona `docker-compose.yml` come compose path.
3. Nella sezione Environment imposta almeno:
   - `PLEX_BASEURL=http://IP_DEL_TUO_PLEX:32400`
   - `PLEX_TOKEN` (opzionale, puoi usare OAuth da UI)
   - `HOST_PORT` (es. `8090`, `8091`, ...)
   - opzionali: `MAX_UPLOAD_MB`, `PORT`
4. Deploy dello stack.
5. Apri `http://IP_DEL_SERVER:HOST_PORT`.

Nota: il compose non usa piu `env_file: .env`, quindi in Repository mode non avrai errore `env file ... not found`.

## Formati Apple Music supportati
- Export tabellare con colonne `Name` e `Artist` (TXT/CSV, UTF-16 o UTF-8).
- Export iTunes/Apple Music in italiano con colonne `Nome` e `Artista`.
- Fallback righe singole tipo `Artist - Title`.

## Limiti attuali
- Le smart playlist Plex non sono ordinabili manualmente (vengono escluse).
- Matching basato su `Title + Artist` (con fallback su solo titolo).
- Il token OAuth ottenuto da UI resta in memoria solo per la sessione browser corrente.
