# Replit Instructions — Mobile App Development (Expo React Native)

You are building a mobile app in Replit. Follow these global rules for ALL tasks.

## 0) Core Principles (must follow)
- Prefer the simplest working solution.
- Offline-first by default unless the prompt explicitly requires online features.
- Avoid scope creep. Do exactly what is requested—nothing extra.

## 1) Stack & Defaults
- Use Expo + React Native + TypeScript.
- Prefer Expo SDK defaults and React Native built-in components.
- Use `@react-native-async-storage/async-storage` for local persistence when needed.
- Avoid adding heavy libraries (Redux, Zustand, React Query, Firebase, UI kits) unless explicitly required.

## 2) Architecture & File Organization
Keep structure small and predictable:

- `app/` or root `App.tsx` (choose whichever matches the template)
- `src/`
  - `components/` (only reusable UI)
  - `screens/` (if multiple screens exist)
  - `lib/` (storage helpers, date helpers)
  - `types/` (shared types)

Rules:
- Do not create excessive folders.
- Keep files small and focused.
- Prefer plain functions over complex abstractions.

## 3) UI/UX Standards
- Use simple, clean layout with spacing and readable typography.
- Avoid heavy animations and expensive UI effects.
- Provide clear empty/loading/error states when applicable.
- Use platform-safe touch targets and accessibility labels where easy.

## 4) Performance
- No infinite loops, no aggressive polling, no background intervals unless required.
- Avoid large lists without virtualization (use `FlatList` for lists).
- Avoid unnecessary re-renders (use `useCallback` / `React.memo` only when it matters).
- Prefer local logic; avoid network calls unless the prompt requires them.

## 5) Data & Persistence
- Default to local-only storage.
- If persistence is needed:
  - Use AsyncStorage with versioned keys (e.g., `habits_v1`).
  - Use small JSON payloads; avoid storing huge blobs.
- Validate user input and guard against null/undefined.

## 6) Error Handling & Reliability
- Handle expected failures gracefully (storage read/write, invalid input).
- Never crash on missing data; fall back to defaults.
- Log errors with a short message (don’t spam logs).

## 7) Development Workflow (Replit-friendly)
When asked to build/modify the app:
1. Restate the plan in 4–8 bullets.
2. List files to create/change.
3. Implement in small steps.
4. Provide run instructions (Expo).
5. Provide a short test checklist (manual).

## 8) Output Format Rules
- Always provide complete code for any file you change.
- Avoid partial snippets unless asked.
- If you add a dependency, explain why in ONE sentence.
- Keep explanations short; prioritize working code.

## 9) Security & Privacy Defaults
- Do not add analytics, tracking, or invasive permissions by default.
- Do not store secrets in the client.
- If online features are required, use env vars and document setup.

## 10) If the prompt is ambiguous
- Make the smallest reasonable assumption that keeps scope minimal.
- Document assumptions in a short list.
- Proceed without asking questions unless absolutely necessary.
