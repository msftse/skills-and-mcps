# Product Requirements Document: Link-in-Bio Page Builder

## 1. Executive Summary

Link-in-Bio Page Builder is a self-hosted, multi-user Linktree alternative that enables users to create a personal landing page with their name, bio, avatar, and a curated list of links. Users select from layout-varying visual themes, receive a shareable public URL based on their chosen slug (e.g., `/cole`), and access a dedicated analytics dashboard tracking clicks per link over time.

The application is built as a full-stack Next.js app deployed on Vercel, backed by Neon serverless Postgres with Neon Auth for authentication. The editor features a live preview with side-by-side layout on desktop and a toggle mode on mobile, with full drag-and-drop link reordering. Public pages are server-rendered for optimal SEO and social sharing.

**MVP Goal:** Deliver a fully functional, production-ready link-in-bio platform across 4 sequential phases -- from profile editing through theming, public URLs with SEO, and click analytics -- with comprehensive E2E testing validating every user journey.

---

## 2. Mission

**Mission Statement:** Provide creators and professionals with a beautiful, self-hosted link-in-bio page they fully control -- no vendor lock-in, no premium paywalls for basic features, and complete ownership of their data and analytics.

**Core Principles:**

1. **Simplicity first** -- The editor should be intuitive enough that a user can create and publish their page in under 2 minutes.
2. **Visual quality** -- Public pages should look polished and professional out of the box, rivaling paid alternatives.
3. **Performance** -- Server-rendered public pages load fast, score well on Lighthouse, and render correctly for social media crawlers.
4. **Self-service** -- No admin intervention needed. Users sign up, build, publish, and track analytics independently.
5. **Test-driven confidence** -- Every user journey is validated with E2E tests using agent-browser. No feature ships without comprehensive test coverage.

---

## 3. Target Users

### Primary Persona: Content Creators & Professionals

- **Who:** Social media creators, freelancers, small business owners, developers, designers -- anyone who needs a single link to share across platforms.
- **Technical comfort:** Low to medium. They can fill out forms and pick themes but shouldn't need to write code or manage infrastructure.
- **Key needs:**
  - A single URL to put in their Instagram/TikTok/Twitter bio
  - A page that looks professional without design skills
  - Knowing which links get clicked and when
  - Ability to quickly update links as their content/projects change

### Secondary Persona: Self-Hosters / Developers

- **Who:** Developers who want to run their own Linktree alternative rather than depend on a SaaS.
- **Technical comfort:** High. They'll deploy to Vercel, configure Neon, and potentially customize themes.
- **Key needs:**
  - Full data ownership
  - Open-source codebase they can fork and extend
  - Clean, well-structured code they can understand and modify

---

## 4. MVP Scope

### In Scope

**Core Functionality:**
- User registration with username/slug selection
- Email/password authentication
- Google OAuth authentication
- Profile editor (name, bio, avatar URL)
- Link management (add, remove, reorder via drag-and-drop)
- Header and divider items between links
- Live preview alongside editor
- 4 layout-varying themes with instant preview
- Slug-based public pages (`/<username>`)
- OG meta tags for social sharing
- Click tracking per link with timestamps
- Analytics dashboard with click counts and time-series charts
- Marketing landing page at `/`

**Technical:**
- Server-side rendering for public pages
- Responsive design (mobile-first)
- TypeScript strict mode throughout
- Biome for linting and formatting
- Vitest for unit testing
- agent-browser for E2E testing of all user journeys
- Basic rate limiting on API routes
- Explicit save button (no auto-save)

**Deployment:**
- Vercel deployment
- Neon serverless Postgres
- Neon Auth integration
- Environment-based configuration

### Out of Scope

- Admin panel / moderation tools
- File upload for avatars (URL-only for MVP)
- Custom domains per user
- Embed support (YouTube, Spotify, etc.)
- Monetization features (tipping, paid links)
- Email notifications / transactional emails
- Auto-save / draft vs published states
- Link scheduling (show/hide by date)
- Geographic analytics (IP-based location data)
- Referrer tracking
- Custom CSS / theme editor per user
- Team/organization accounts
- API access for third-party integrations
- Mobile app
- Bot protection beyond basic rate limiting

---

## 5. User Stories

### Registration & Authentication

**US-1:** As a new user, I want to sign up with my email and choose a unique username, so that I get a personal URL like `/cole` for my link page.
> *Example: User visits `/`, clicks "Get Started", enters name, email, password, and desired slug `cole`. System checks slug availability in real-time. On success, user lands on the editor.*

**US-2:** As a returning user, I want to log in with my email/password or Google account, so that I can quickly access my editor.
> *Example: User clicks "Sign In", chooses "Continue with Google", authenticates via OAuth, and is redirected to their editor dashboard.*

### Profile Editing

**US-3:** As a logged-in user, I want to edit my name, bio, and avatar URL with a live preview, so that I can see exactly how my page will look before saving.
> *Example: User types in the bio field "Designer & coffee enthusiast" and the preview panel on the right instantly updates to show the new bio text.*

**US-4:** As a logged-in user, I want to add, remove, and reorder links using drag-and-drop, so that I can organize my page the way I want.
> *Example: User has 5 links. They grab the drag handle on "My Portfolio" and drag it from position 4 to position 1. The preview updates immediately. They click "Save" to persist the change.*

**US-5:** As a logged-in user, I want to add section headers and dividers between my links, so that I can visually group related links.
> *Example: User adds a header "Social Media" above their Twitter and Instagram links, and a divider before their "Projects" section.*

### Themes

**US-6:** As a logged-in user, I want to pick from 4 visual themes and see the result instantly in the preview, so that I can choose the look that best represents me.
> *Example: User clicks the "Colorful" theme thumbnail. The preview immediately switches to a vibrant gradient background with rounded, colorful link buttons. They try "Professional" next -- the preview shifts to a clean, muted layout with serif typography.*

### Public Pages

**US-7:** As a visitor, I want to view someone's link page at their public URL and click their links, so that I can find their content.
> *Example: Visitor opens `example.com/cole` in their browser. They see Cole's avatar, bio, and list of links rendered with the "Dark" theme. They click "My YouTube Channel" and are redirected to YouTube.*

### Analytics

**US-8:** As a logged-in user, I want to see how many times each of my links has been clicked and view click trends over time, so that I can understand what content resonates with my audience.
> *Example: User navigates to their analytics dashboard. They see a bar chart showing "YouTube: 342 clicks, Portfolio: 128 clicks, Twitter: 89 clicks" and a line chart showing daily clicks over the past 30 days.*

---

## 6. Core Architecture & Patterns

### High-Level Architecture

```
+-----------------------------------------------------------+
|                        Vercel                             |
|  +-----------------------------------------------------+  |
|  |                   Next.js App                       |  |
|  |                                                     |  |
|  |  +-----------+  +--------+  +-----------+           |  |
|  |  | SSR Public|  |  API   |  |SPA Editor |           |  |
|  |  |  Pages    |  | Routes |  |+ Dashboard|           |  |
|  |  |  /<slug>  |  | /api/* |  | /editor   |           |  |
|  |  +-----------+  +--------+  +-----------+           |  |
|  |                      |                              |  |
|  +----------------------|------------------------------+  |
|                         |                                 |
+-------------------------|---------------------------------+
                          |
                +---------+---------+
                |   Neon Postgres   |
                |  + Neon Auth      |
                |  (Serverless)     |
                +-------------------+
```

### Directory Structure

```
link-in-bio-page-builder/
├── src/
│   ├── app/                          # Next.js App Router
│   │   ├── (marketing)/              # Marketing/landing route group
│   │   │   └── page.tsx              # Landing page at /
│   │   ├── (auth)/                   # Auth route group
│   │   │   ├── login/page.tsx
│   │   │   └── signup/page.tsx
│   │   ├── (dashboard)/              # Authenticated route group
│   │   │   ├── editor/page.tsx       # Profile editor + live preview
│   │   │   ├── analytics/page.tsx    # Analytics dashboard
│   │   │   └── settings/page.tsx     # Account settings
│   │   ├── [slug]/page.tsx           # Public profile pages (SSR)
│   │   ├── api/
│   │   │   ├── auth/[...all]/route.ts
│   │   │   ├── profile/route.ts
│   │   │   ├── links/route.ts
│   │   │   ├── links/reorder/route.ts
│   │   │   ├── click/route.ts
│   │   │   └── analytics/route.ts
│   │   ├── layout.tsx
│   │   └── globals.css
│   ├── components/
│   │   ├── ui/                       # shadcn/ui components
│   │   ├── editor/
│   │   ├── preview/
│   │   ├── themes/
│   │   ├── analytics/
│   │   └── marketing/
│   ├── lib/
│   │   ├── auth.ts
│   │   ├── db/
│   │   │   ├── index.ts
│   │   │   ├── schema.ts
│   │   │   └── migrations/
│   │   ├── rate-limit.ts
│   │   └── utils.ts
│   ├── hooks/
│   └── types/
├── tests/
│   ├── unit/
│   └── e2e/
├── public/
├── drizzle.config.ts
├── biome.json
├── next.config.ts
├── tailwind.config.ts
├── tsconfig.json
├── vitest.config.ts
└── package.json
```

### Key Design Patterns

1. **Route Groups** -- Use Next.js route groups `(marketing)`, `(auth)`, `(dashboard)` to organize layouts without affecting URL structure.
2. **Server Components by default** -- All pages and layouts are React Server Components unless they need interactivity.
3. **Server Actions for mutations** -- Use Next.js Server Actions for profile saves, link CRUD, and settings changes.
4. **Optimistic UI** -- The editor preview updates instantly on the client; the save button persists to the database.
5. **SSR for public pages** -- The `[slug]` dynamic route fetches profile data server-side and renders the full HTML with OG meta tags.

---

## 7. Features

### 7.1 Marketing Landing Page

**Route:** `/`

A public homepage that explains the product and drives signups.

- Hero section with tagline, description, and CTA buttons
- Brief feature highlights (themes, analytics, custom URL)
- Example preview showing what a link page looks like
- Footer with minimal links

### 7.2 Authentication

**Routes:** `/login`, `/signup`

Powered by Neon Auth (built on Better Auth).

- **Signup flow:** name, email, password, desired slug with real-time availability check
- **Login flow:** email/password OR "Continue with Google"
- **Reserved slugs:** `login`, `signup`, `editor`, `analytics`, `settings`, `api`, `admin`, `about`, `help`, etc.

### 7.3 Profile Editor + Live Preview

**Route:** `/editor`

- Profile section: display name, bio (160 chars), avatar URL
- Links section: drag-and-drop with dnd-kit, add/remove links, headers, dividers
- Save button with toast feedback
- Side-by-side on desktop, toggle on mobile

### 7.4 Theme System

4 themes with distinct layouts:

| Theme | Vibe | Layout |
|---|---|---|
| **Minimal** | Clean, white, sans-serif | Centered column, rectangular buttons |
| **Dark** | Dark bg, neon accents | Pill-shaped buttons, glow effects |
| **Colorful** | Vibrant gradients, bold | Card-based links, gradient bg |
| **Professional** | Muted, serif headings | Two-column on desktop |

### 7.5 Public Pages + SEO

**Route:** `/[slug]`

- Server-rendered with selected theme
- OG meta tags for social sharing
- Click tracking via `navigator.sendBeacon`
- 404 for non-existent slugs

### 7.6 Click Analytics

**Dashboard route:** `/analytics`

- Summary cards (total clicks, this week, active links)
- Top links table ranked by clicks
- Time-series chart (7d/30d/90d)
- Per-link daily breakdown

---

## 8. Technology Stack

| Category | Technology |
|----------|------------|
| Framework | Next.js 15.x (App Router) |
| Language | TypeScript 5.x (strict) |
| Styling | Tailwind CSS 4.x + shadcn/ui |
| Database | Neon serverless Postgres |
| Auth | Neon Auth |
| ORM | Drizzle ORM |
| DnD | @dnd-kit/core + sortable |
| Charts | Recharts |
| Validation | Zod |
| Linting | Biome |
| Testing | Vitest + agent-browser (E2E) |
| Hosting | Vercel |

---

## 9. Security & Configuration

- Neon Auth for all authentication flows
- Middleware protects `/editor`, `/analytics`, `/settings`
- All queries filter by authenticated user ID
- Rate limiting: 60/min on click endpoint, 30/min on profile/links, 10/min on auth
- Input sanitization for XSS prevention
- URL and slug validation

---

## 10. Database Schema

```sql
-- Profiles (extends Neon Auth user)
CREATE TABLE profiles (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id       TEXT NOT NULL UNIQUE REFERENCES neon_auth.users(id) ON DELETE CASCADE,
  slug          TEXT NOT NULL UNIQUE,
  display_name  TEXT NOT NULL DEFAULT '',
  bio           TEXT NOT NULL DEFAULT '',
  avatar_url    TEXT NOT NULL DEFAULT '',
  theme         TEXT NOT NULL DEFAULT 'minimal',
  created_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Link items (links, headers, dividers)
CREATE TABLE link_items (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  profile_id    UUID NOT NULL REFERENCES profiles(id) ON DELETE CASCADE,
  type          TEXT NOT NULL DEFAULT 'link',
  title         TEXT NOT NULL DEFAULT '',
  url           TEXT NOT NULL DEFAULT '',
  sort_order    INTEGER NOT NULL DEFAULT 0,
  created_at    TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  updated_at    TIMESTAMPTZ NOT NULL DEFAULT NOW()
);

-- Click events
CREATE TABLE click_events (
  id            UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  link_item_id  UUID NOT NULL REFERENCES link_items(id) ON DELETE CASCADE,
  clicked_at    TIMESTAMPTZ NOT NULL DEFAULT NOW()
);
```

---

## 11. Implementation Phases

1. **Phase 1:** Profile Editor + Live Preview (auth, editor, links, preview)
2. **Phase 2:** Theme System (4 themes, picker, persistence)
3. **Phase 3:** Public URLs + SEO (SSR pages, OG tags, landing page)
4. **Phase 4:** Click Analytics (tracking, dashboard, charts)

---

## 12. Success Criteria

- User can sign up, build, and publish a link page in under 2 minutes
- All 4 themes render correctly on mobile and desktop
- Public pages score 90+ on Lighthouse
- E2E tests pass for every user journey
- TypeScript strict mode with zero errors
- Biome passes with zero warnings
