# Bore & Barrel

A gunshop storefront built as a Progressive Web App. It installs to the home
screen, runs offline from a precached app shell, and talks to a RESTful API
for its catalog, reviews, favorites, and image uploads. Built with React 19,
Vite, and `vite-plugin-pwa`.

## Setup

```bash
npm install
cp .env.example .env   # then fill in the values
npm run dev            # dev server with HMR
```

Other scripts:

```bash
npm run build    # production build to dist/
npm run preview  # serve the production build
npm run lint     # oxlint
```

## Environment

| Variable | Purpose |
| --- | --- |
| `VITE_API_BASE_URL` | Base URL of the REST backend. A Supabase project URL (PostgREST at `/rest/v1`, Storage at `/storage/v1`) or the lab server. Requests fall back to relative paths when unset. |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | Supabase anon key. Sent as the `apikey` header and as a Bearer token. Leave empty for a non-Supabase backend. |

Set both in Vercel under Project Settings > Environment Variables before
deploying, otherwise the catalog falls back to the bundled static data.

## Features

- Catalog with search, category filter, sorting, and an advanced filter panel
- Full CRUD on guns, including image upload and a delete confirmation modal
- Reviews with ratings, favorites, and a local user profile
- Draft auto-save to `localStorage`, so a half-finished form survives a reload
- Offline fallback to bundled static data when the API is unreachable
- Install prompt and update banner via `PWABadge`

## Structure

```
.
├── index.html              # HTML shell
├── vite.config.js          # Vite + vite-plugin-pwa (manifest + service worker)
├── public/                 # static assets served at /
│   ├── icon-192.png / icon-512.png / icon.svg
│   └── guns/               # product images
└── src/
    ├── main.jsx            # entry, mounts <App/>
    ├── App.jsx             # app shell: tab/mode state, routing, delete flow
    ├── App.css             # app shell styles
    ├── index.css           # global styles
    ├── config/
    │   └── api.js          # axios instance, auth headers, error normalising
    ├── services/           # one module per API resource
    │   ├── gunService.js
    │   ├── reviewService.js
    │   ├── favoriteService.js
    │   ├── uploadService.js
    │   └── userService.js
    ├── hooks/              # data fetching and local state
    │   ├── useGuns.js
    │   ├── useReviews.js
    │   └── useFavorites.js
    ├── components/
    │   ├── Header.jsx      # brand + nav
    │   ├── Footer.jsx
    │   ├── PWABadge.jsx    # install prompt and update banner
    │   ├── common/         # AdvancedFilter, FavoriteButton, SplashScreen
    │   ├── gun/            # GunCard, GunDetail
    │   └── modals/         # ConfirmModal
    ├── pages/
    │   ├── Catalog.jsx     # product grid
    │   ├── CreateGunPage.jsx / EditGunPage.jsx
    │   ├── FavoritesPage.jsx / ProfilePage.jsx
    │   └── About.jsx / Contact.jsx
    ├── data/
    │   └── guns.js         # static fallback product data
    └── utils/
        ├── draftStorage.js # form draft persistence
        └── helpers.js      # search, filter, sort, rating helpers
```
