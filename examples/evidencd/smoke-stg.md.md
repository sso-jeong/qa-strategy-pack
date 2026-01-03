# Smoke Result (STG) - 21.2.0.0.31

## Health
- GET /health → 200 (UP) ✅

## Core API
- Auth token issue → 200 ✅
- Auth refresh → 200 ✅
- Core GET /resource/1 → 200 ✅
- Core POST /resource → 201 ✅
- Unauthorized check → 401 ✅

## UI Smoke (R3)
- Main page → 200 ✅
- Invalid route → 404 page ✅
- Console error → none ✅

Result: PASS