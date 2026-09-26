# App-testeo-APIs

Web de pruebas para validar las API keys de **Devmark AI** (`POST /v1/chat/completions` y `GET /v1/models`).

- Un solo archivo: `index.html`. Sin build, sin dependencias, sin backend.
- La key se escribe en el navegador y **solo viaja a la API** configurada. Opcionalmente se recuerda en `localStorage` de ese equipo.
- Usa keys **`dmk_test_`** (créalas en el dashboard → API Keys, entorno *Test*) y revócalas al terminar.

## Qué permite probar

- **Conexión**: valida la key contra `GET /v1/models` y carga los modelos disponibles.
- **Chat**: system prompt, temperatura, `max_tokens`, con o sin historial.
- **Métricas por respuesta**: modelo, latencia, tokens (entrada + salida) y aviso si la respuesta se cortó por `max_tokens`.
- **Diagnóstico de errores** explicado: 401 (key inválida/revocada/expirada), 403, 404, 429, 503/504 y **CORS** (indica el origen exacto que hay que permitir).
- **JSON crudo** de la última petición/respuesta y **Copiar cURL** (con `$DEVMARK_API_KEY` en lugar de la key real).
- Estadísticas de la sesión: requests, errores, tokens, latencia media.

## Probar en local

Abrir `index.html` con doble clic **no funciona**: el navegador envía el origen `null`, que la API no permite. Sírvelo así:

```bash
python -m http.server 8000
# abre http://localhost:8000
```

y añade ese origen en el servidor de la API (SSH):

```bash
sed -i '/^CORS_ALLOWED_ORIGINS=/d' ~/ai-server/.env
echo "CORS_ALLOWED_ORIGINS=https://devmark-pe.github.io,http://localhost:8000" >> ~/ai-server/.env
sudo systemctl restart devmark-ai
```

## Publicar (GitHub Pages)

Repo → Settings → Pages → *Deploy from branch* → `main` / root.
Quedará en `https://devmark-pe.github.io/App-testeo-APIs/` (origen `https://devmark-pe.github.io`, ya permitido en la API).

## CORS en la API

La API permite CORS **solo** en `GET /v1/models` y `POST /v1/chat/completions`, para los orígenes de
`CORS_ALLOWED_ORIGINS` (por defecto `https://devmark-pe.github.io`), sin credenciales.
