# FB UID Checker Pro

Premium dark dashboard based on the supplied Vue 3 + Vite project. Includes bulk UID parsing, duplicate removal, a 1,000-UID batch cap, progress, pause/resume/stop, search/filter, history, and CSV/TXT/JSON export.

## Important status accuracy note

This project does **not** claim that every Facebook UID can be reliably classified as live or dead. It only labels a UID `live` when the public picture endpoint returns the expected image metadata. Every ambiguous response, HTTP error, CORS failure, timeout, or unsupported case becomes `unknown`. `dead` is intentionally not inferred from a failed request. This public endpoint is unofficial and may change or restrict access.

## Run frontend locally

1. Install Node.js 20 or newer.
2. Run `npm install`.
3. Run `npm run dev`.
4. Open the local Vite URL shown in the terminal.

## Connect the Cloudflare Worker backend

1. In `worker/`, run `npm install`.
2. Authenticate with Cloudflare using `npx wrangler login`.
3. Run `npm run deploy`.
4. Copy the deployed Worker URL.
5. Set `VITE_API_BASE_URL=https://YOUR-WORKER.workers.dev` in `.env.local` before building the frontend.
6. Run `npm run build`; deploy the generated `dist/` folder to a static host.

The Worker is a free-tier-friendly adapter, not a guaranteed Facebook status API. It processes requests in groups of ten and accepts up to 1,000 unique UIDs per request. The free plan has limits and Facebook may rate-limit or block the upstream endpoint.

## Deploy frontend to GitHub Pages

For a project site, configure `base` in `vite.config.js` to `'/REPOSITORY_NAME/'` before running the build. Upload the repository to GitHub, enable Pages with the appropriate build workflow, and publish `dist/`. Netlify is also suitable for static frontend hosting.

## Security

Do not put Facebook passwords, session cookies, access tokens, or private API secrets in the frontend. The supplied adapter does not require login credentials.
