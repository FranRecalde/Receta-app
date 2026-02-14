# RecipeVault - TikTok Recipe Extractor

## Overview
RecipeVault is a mobile-friendly web app (built with Expo React Native) that allows users to paste a TikTok URL and extract a structured recipe using AI. Users can save, browse, and cook from their recipe collection.

## Architecture
- **Frontend**: Expo React Native (web mode), built as static files served by Express
- **Backend**: Express.js API server on port 5000
- **Database**: PostgreSQL (Replit built-in)
- **Auth**: Replit Auth (OpenID Connect) - supports Google, GitHub, Apple, and email sign-in
- **AI**: OpenAI via Replit AI Integrations (gpt-4o-mini for recipe extraction)

## Project Structure
```
/app          - Expo React Native app (TypeScript)
  /src
    /screens     - All app screens (Auth, Home, AddRecipe, Review, RecipeDetail, CookingMode, Profile)
    /services    - API client and offline storage
    /context     - React Context providers (Auth)
    /components  - Shared UI components
/server       - Express backend (TypeScript)
  /src
    index.ts        - Server entry point (port 5000)
    db.ts           - PostgreSQL database setup
    recipes.ts      - Recipe CRUD routes
    extraction.ts   - TikTok extraction + rate limiting
    tiktokOEmbed.ts - TikTok oEmbed metadata fetcher
    tiktokScrape.ts - Best-effort HTML scraper
    rawBlob.ts      - Raw data builder for LLM
    openaiClient.ts - OpenAI LLM extraction
    sanitize.ts     - Recipe sanitization utilities
  /replit_integrations
    /auth           - Replit Auth integration (OIDC, passport, sessions)
/shared       - Shared TypeScript types
```

## Key Features
- SSO authentication via Replit Auth (Google, GitHub, Apple, email)
- TikTok recipe extraction via oEmbed + HTML scraping + OpenAI
- Rolling 24-hour rate limiting (30 extractions per day)
- Recipe CRUD with pagination and tag filtering
- Cooking mode with large-font step display
- Offline recipe download support
- Account and recipe deletion

## Workflow
- **RecipeVault**: Builds Expo web app, then starts Express server on port 5000

## Database
- PostgreSQL with `users`, `recipes`, and `sessions` tables
- Users table: VARCHAR id (from Replit Auth), extraction tracking columns
- Recipes table: user_id VARCHAR references auth user
- Sessions table: for express-session/connect-pg-simple

## Environment Variables
- `DATABASE_URL` - PostgreSQL connection (auto-set by Replit)
- `SESSION_SECRET` - Session encryption key (auto-set by Replit)
- `AI_INTEGRATIONS_OPENAI_API_KEY` - OpenAI access via Replit AI Integrations (auto-set)
- `AI_INTEGRATIONS_OPENAI_BASE_URL` - OpenAI base URL (auto-set)
