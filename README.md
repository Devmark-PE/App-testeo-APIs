# App-testeo-APIs

Web de pruebas para validar las API keys de **Devmark AI** en apps reales y futuras.

- Un solo archivo: `index.html` (sin build, sin dependencias).
- La key se escribe en el navegador y **solo viaja a la API**. No hay `.env`, no hay backend, nada se guarda en un servidor (salvo la opción explícita "recordar en este navegador", que usa `localStorage`).
- Usa keys **`dmk_test_`** para probar. No uses `dmk_live_` de producción aquí.

## Probar en local

Abre `index.html` en el navegador, o sírvelo con:

```bash
python -m http.server 8000
# http://localhost:8000
```

## Publicar (GitHub Pages)

Repo → Settings → Pages → Deploy from branch → `main` / root. Quedará en `https://devmark-pe.github.io/App-testeo-APIs/`.

## Requisito: CORS en la API

Hasta que la API permita CORS desde este origen, el navegador bloqueará las llamadas (`Failed to fetch`).
La API debe responder al preflight con, por ejemplo:

```
Access-Control-Allow-Origin: https://devmark-pe.github.io
Access-Control-Allow-Headers: authorization, content-type
Access-Control-Allow-Methods: GET, POST, OPTIONS
```

Pedir a Claude (backend): commit que añada `CORSMiddleware` solo para `GET /v1/models`, `POST /v1/chat/completions` y `OPTIONS`, con `allow_origins` limitado al dominio donde se aloje esta app.
