# Argus video/API maintenance patch

## Changes

- Video selection now opens a dedicated browser preview, independent of the image workspace.
- Video preview uses a local `blob:` URL. The video is not uploaded to the server and is not automatically sent to the analysis API.
- Video metadata (dimensions, file name and size) is shown after the browser loads metadata.
- Temporary object URLs are revoked when another file is selected or the view is reset.
- Unsupported/empty files and browser codec failures now produce explicit messages.
- The same file can be selected again because file input values are reset after selection.
- The API client now reports network, HTTP, JSON and response-shape errors instead of silently substituting a fabricated local result.
- `/api/health` reports whether a provider key is configured without exposing upstream URLs or model details. This is a configuration check, not a live provider ping.
- Unknown `/api/*` routes return JSON 404 responses; malformed/oversized API request bodies receive JSON errors.
- Production start now uses `tsx server.ts`; `tsx` is a production dependency and Node >=22.12 is declared.
- TypeScript includes Node types; HTML language is set to Russian.

## Scope

This patch addresses ordinary file preview and general server/API diagnostics only. It does not add video-to-AI processing or change biometric identification, age estimation, face matching, person tracking, or detection of minors. The existing `/api/analyze` route is left as-is.

## Verification

- ZIP input passed `unzip -t` before modification.
- TypeScript syntax transpilation passed for `src/ArgusApp.tsx`, `src/services/argusApi.ts` and `server.ts`.
- Full `npm run build` and type-check could not be completed in this environment because `npm install` timed out and dependencies (`vite`, `@types/node`, `vite/client`) were unavailable. Re-run `npm install` and `npm run build` in the project or on Render.
