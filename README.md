# Plex Playlist Reorder (Apple Music -> Plex)

Web app containerizzabile per Portainer che:
- carica un file playlist Apple Music (TXT/CSV in Unicode),
- mostra le playlist audio Plex non-smart,
- fa anteprima dei match,
- chiede conferma `Sicuro?`,
- riordina la playlist Plex via API `move`.

## Requisiti
- Plex Media Server raggiungibile dalla rete del container.
- Token Plex valido.

## Configurazione `.env`
Copia `.env.example` in `.env` e compila:

```env
PLEX_BASEURL=http://IP_DEL_TUO_PLEX:32400
PLEX_TOKEN=IL_TUO_TOKEN
MAX_UPLOAD_MB=8
PORT=8080
```

## Avvio locale
```bash
docker compose up --build -d
```
Poi apri: `http://localhost:8080`

## Deploy con Portainer
1. Crea uno stack usando il file `docker-compose.yml`.
2. Inserisci le variabili ambiente dello `.env` nella sezione Environment (oppure monta `.env`).
3. Deploy dello stack.
4. Apri `http://IP_DEL_SERVER:8080`.

## Formati Apple Music supportati
- Export tabellare con colonne `Name` e `Artist` (TXT/CSV, UTF-16 o UTF-8).
- Fallback righe singole tipo `Artist - Title`.

## Limiti attuali
- Le smart playlist Plex non sono ordinabili manualmente (vengono escluse).
- Matching basato su `Title + Artist` (con fallback su solo titolo).

## Nota su login Plex
In questa versione usi `PLEX_TOKEN` in `.env` (piu semplice e robusto in container).
Si puo aggiungere in seguito login Plex OAuth/PIN flow nella UI.
