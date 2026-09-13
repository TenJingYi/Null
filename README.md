# OmniTrip by Null

**Team:** Ten Jing Yi, Lim Li Jing, Lim Li Ning, Ng Xuan Yee

**Problem Statement:** Travel Planner

**Video Presentation:** https://youtu.be/J7IenjhzMbc

**Presentation Slides:** https://canva.link/rr3uuwznh5s9gdv

---

## 1. Project Overview

### The Problem
Planning a trip today means juggling several disconnected tools: a notes app for the itinerary, a chat group for splitting bills, a folder of screenshots for tickets and visas, and a translation app for when you actually get there. This fragmentation gets worse depending on who is traveling:

- **Solo travelers** want fast, private, personalized planning without needing to negotiate anything with anyone.
- **Group travelers** (family/friends) need shared logistics — polls, itemized expense splitting, role permissions (e.g. a parent as "Host," a child as a read-only "Viewer") — which most itinerary apps don't handle well.
- **OKU (Orang Kurang Upaya) / travelers with disabilities** are almost entirely underserved: mainstream travel apps rarely surface accessibility information (ramps, elevators, barrier-free routes) at all.

Existing apps only solve one slice of this. Wanderlog and TripIt handle itinerary building but have weak group-finance tools and no accessibility layer. Splitwise handles group expenses well but has no itinerary, vault, or translation features. None of them adapt their entire interface based on who is traveling and how.

### Our Solution
OmniTrip is a single travel app that reshapes itself around three distinct travel contexts — Solo Mode, Group Mode, and OKU Mode — instead of forcing every user through the same generic interface. Switching is seamless: inviting a member to a solo trip unlocks Group Mode automatically, bringing in shared budgets, polls, and role-based permissions without the user starting over. Key features include:

- AI-generated, mode-aware itinerary planning (solo pacing vs. real-time multi-user editing with polls/voting)
- A "Vault" for travel documents — private for solo trips, dual-layer (shared + personal lockers) for groups
- Expense tracking that scales from a personal budget tracker to OCR receipt scanning, itemized splitting, and an automatic "Settle Up" debt minimizer
- Role-Based Access Control (Host / Member / Viewer) for group trips
- A dedicated OKU Mode with a live accessibility-routing radar, barrier alerts with AI-suggested alternate routes, and a voice assistant tuned for accessibility needs
- A global system-tools suite: camera/voice/text translation, offline phrasebook, and offline PWA access
- An evening safety check-in for solo travelers, with an automatic emergency data hand-off if a check-in is missed while offline

### Reach & Scalability
OmniTrip's mode-based architecture is built to extend, not just work for one trip. The OKU Mode's accessibility data starts curated for a handful of demo locations (e.g. KLCC, Batu Caves) but is structured to grow city-by-city as more barrier-free route data is added — the same pattern national accessibility-mapping initiatives already use. Beyond individual travelers, the Group Mode's RBAC and shared-vault infrastructure could extend to tour operators or corporate travel coordinators managing many travelers at once, and the underlying itinerary/expense engine is not Malaysia-specific, so expansion beyond the initial market is a data and localization problem, not an architectural one.

---

## 2. Ideation & Process

### 2.1 Ideas We Considered

| Idea | Why it was dropped / kept |
|---|---|
| Dynamic Solo-to-Group UX Mode Switching (Chosen) | Kept: Solves interface bloat by keeping the UI clean for solo users and dynamically introducing social/collaboration tools only when group members are invited. |
| Dual-Layer Security Vault (Chosen) | Kept: Essential for group security; balances shared stay details with strictly private lockers for sensitive individual documents (passport scans, personal boarding passes). |
| Receipt OCR with Exclusions (Chosen) | Kept: Directly solves group financial friction by letting users exclude individual members from specific line items (e.g., excluding non-drinkers from alcohol costs). |
| Full Travel Social Network / Feed | Dropped: Shifted focus away from core utility. Adding social feeds added unnecessary scope and distracted from solving immediate logistics pain points. |
| Automated Direct In-App Booking Engine | Dropped: High API integration complexity and regulatory overhead. Chosen instead to link out to providers while focusing deeply on itinerary, vault, and budgeting mechanics. |

### 2.2 Ideation Boards

We developed OmniTrip's concept directly around a core problem tree — mapping the causes of fragmented travel planning to the three travel contexts (Solo, Group, OKU) that became our three modes, then branching each mode out into its feature set.

![Ideation problem tree — mapping the core problem through causes and effects to the Solo, Group, and OKU mode feature sets](images/ideation-problem-tree.png)
*Figure: our full ideation map — from the root problem, through the three modes it produced, down to the specific features each mode needed.*

We also mapped how a trip actually moves *between* modes, since Solo and Group aren't meant to be separate apps — Group Mode unlocks automatically the moment a solo trip is shared:

![Dynamic UX Transition flow — how a trip evolves from Solo to Group Mode and assigns Host/Member/Viewer roles](images/ideation-ux-transition-flow.png)
*Figure: the Dynamic UX Transition flow — how a trip evolves from Solo to Group Mode without the user ever leaving the app.*

As we refined the idea, a few points shifted along the way:

| Stage | Changed | Why |
|---|---|---|
| Initial concept | Brainstormed feature mindmaps for general travel planning. | To list all possible travel pain points and feature ideas without constraints. |
| Refinement | Grouped features into 2 targeted modes (Solo, Group). | To address distinct user needs directly instead of offering a bloated, one-size-fits-all app. |
| Refinement | Added a third mode, OKU Mode, for users with disabilities. | Realized accessibility needs (barrier-free routing, voice-first navigation) couldn't just be a settings toggle inside Group Mode — they needed their own dedicated flow. |
| Refinement | Introduced Role-Based Access Control (Host / Member / Viewer) within Group Mode. | Not every group member should have equal edit rights — e.g. giving children or extended family a clean, read-only view instead of full editing access. |

### 2.3 Mentor Consultation

| Date | Mentor | Feedback Received | What Was Changed |
|---|---|---|---|
| 10/9/2026 | Iris Yan Ning | The overall features were considered useful and practical. However, the overall features were still quite common and not distinctive enough for a hackathon. Suggested focusing on one key feature and making it more in-depth. Basic/default features do not need to be explained in detail due to limited presentation time. The UI colour and font customisation was considered acceptable. | Selected the evening safety check-in as an important feature to focus on. Reduced emphasis on basic/default features. Strengthened the core feature with additional functionality to make it more distinctive. Prepared a more focused presentation flow to save pitching time. |
| 10/9/2026 | Varsha Selvakumar | Overall, the project was well done but make sure the time while presenting in video can be measured. | Try to practice presentation in short time. |

---

## 3. Design & Prototype

**UI Prototype:** https://www.figma.com/proto/IMN56OiQlsxW043pLKNvZd/Travel?node-id=0-1&t=MUZnGS69mXAfD2JH-1

**Figure 3.1 — Solo mode dashboard.** Shows a mood and personalisation engine that allows solo travellers to filter places using mood tags like chill, adventure and more. Distance and duration can also be chosen. The AI matches destinations to the user's current mood, duration and distance rather than rigid categories.

**Figure 3.2 — Solo mode vault (evening safety check-in).** Shows the core safety system: an evening safety check-in, a lightweight security mechanism designed specifically for non-ticketed, spontaneous daytime activities. Travellers simply tap "Check Safe" at night when confirming tomorrow's plan. This also supports the 24-hour offline countdown powered by PWA Service Workers. Each evening check-in resets a local safety timer to ensure full functionality even in zero-connectivity areas. If a traveller misses their daily check-in and loses connection, the background Vault system automatically transmits encrypted emergency data and Medical IDs to a pre-set emergency contact.

**Figure 3.3 — Solo mode expenses.** Shows the expense module, where users can look back at data by choosing a past date. Currency is automatically converted into the user's chosen currency — e.g. if the user OCR-scans a receipt in Thailand but the chosen currency is MYR, all amounts are automatically converted to MYR and shown in both the chart and the table.

**Figure 3.4 — Solo mode itinerary.** Users can pick and add their preferred top activities, and the timeline displays them; users can edit or add events directly on the timeline. The AI personalisation panel lists items the user should prepare based on the activities they picked. Accommodation is already filtered based on budget and rating, and can be booked directly by tapping "Book".

**Figure 3.5 — Group dashboard.** Acts as the shared command centre for a multi-traveller trip, bringing group consensus, budget, debt settlement, and document readiness into a single view. Group Consensus (88%) shows how aligned members are while flagging pending disagreements; Group Budget Pool tracks spend against total; Debt Settlement lists pending debts with a one-tap "Settle Up" that minimises the number of transactions; Vault Readiness confirms all members' documents are synced. Below, Today's Group Schedule shows an AI-optimised day plan with a proactive AI Insight banner (e.g. warning about rain), the Active Group Poll shows live voting, the Shared Preparation Checklist tracks role-assigned to-dos, and Group Real-Time Activity gives a live feed of member actions.

**Figure 3.6 — Group mode expense.** A user scans a receipt, OCR extracts line items, and the split screen lets the user exclude specific members from specific items — e.g. excluding non-drinkers from the alcohol line. The Settle Up button then reduces all outstanding group debts to the minimum number of transactions.

**Figure 3.7 — Group mode vault.** Uses a dual-layer structure: a shared folder for group documents (hotel confirmations, flight itineraries) and personal lockers for sensitive items (passport scans, individual boarding passes). Access is enforced by Supabase Row Level Security, so a Viewer role can see the shared folder but not another member's personal locker.

**Figure 3.8 — OKU mode route.** The Barrier-Free Route Radar is designed on GIS spatial data and plans routes that bypass stairs and steep slopes. When a user plans to visit a location like Batu Caves, the prototype triggers a Barrier Alert warning of the 272 stairs ahead, and the AI calculates a Step-Free Alternative (e.g. the ground-level cultural centre elevator) with one tap. The Voice-First AI Companion provides full two-way voice interaction and screen-reading for visually impaired users — e.g. "Where is the nearest OKU toilet?" — with audio instructions paired with haptic vibration cues.

---

## 4. What Makes It Different

- **Evening safety check-in with automatic emergency hand-off** — a lightweight nightly "Check Safe" tap that resets a 24-hour offline safety timer; if a solo traveller misses a check-in and loses connection, the app automatically transmits encrypted emergency data and Medical IDs to a pre-set contact. This was the feature our mentor specifically pushed us to deepen rather than spread effort thin across many "common" features, since most itinerary apps have no safety mechanism for spontaneous, non-ticketed solo activities at all.
- **OKU Mode as a first-class mode, not an accessibility settings toggle** — a full route-accessibility index, barrier alerts with auto-suggested alternatives, and a voice assistant tuned for slower speech rate and haptic feedback.
- **Dynamic mode-switching** — the UI transitions from Solo to Group automatically the moment a trip is shared, instead of requiring a separate "group trip" app or mode.
- **Role-Based Access Control for travel groups** — Host/Member/Viewer roles are uncommon in consumer travel apps and solve a real problem (e.g. giving kids or older relatives a clean read-only schedule).
- **"Settle Up" automatic debt minimization** — reduces however many group debts exist down to the minimum number of transactions needed, rather than everyone paying everyone back individually.

### Comparison with Existing Solutions

| Feature | OmniTrip | Wanderlog | TripIt | Splitwise |
|---|---|---|---|---|
| AI itinerary generation | ✅ | ✅ | ❌ | ❌ |
| Group polls / voting | ✅ | Partial | ❌ | ❌ |
| Role-based permissions | ✅ | ❌ | ❌ | ❌ |
| Receipt OCR + itemized split | ✅ | ❌ | ❌ | ✅ |
| Document/ticket vault | ✅ | Partial | ✅ | ❌ |
| Dedicated accessibility mode | ✅ | ❌ | ❌ | ❌ |
| Solo safety check-in | ✅ | ❌ | ❌ | ❌ |
| Built-in translation tools | ✅ | ❌ | ❌ | ❌ |

---

## 5. Technical Architecture & Feasibility

### Tech Stack

**Frontend: React / Next.js (TypeScript) + Tailwind CSS**
- Why: Rapid UI development, strict type safety, and seamless Progressive Web App (PWA) support for offline document access.
- Constraints: Managing real-time local state syncing with offline storage (IndexedDB).

**Backend & Real-Time Engine: Node.js / Express with WebSockets (Socket.io)**
- Why: High-performance bidirectional communication required for multi-user live itinerary editing and voting presence.
- Constraints: Managing race conditions during simultaneous drag-and-drop itinerary updates.

**Database & Auth: Supabase (PostgreSQL)**
- Why: Native Row Level Security (RLS) enforces our RBAC model and Dual-Layer Private Lockers efficiently.
- Constraints: Free-tier rate limits require efficient querying and data caching.

**APIs & Services:**
- **Google Maps API** — interactive map pinning, route optimization, and distance calculations.
- **Tesseract.js / Cloud Vision API** — client-side/serverless OCR for itemized receipt parsing.
- **OpenAI / Gemini API** — generating personalized itineraries based on user constraints.

**Hosting:** Vercel (Frontend & Serverless Functions) + Render (WebSocket Server)

### System Architecture Diagram

![System architecture diagram — Next.js frontend connecting via REST to Node/Express and via WebSocket to Socket.io, both backed by Supabase](images/system-architecture.png)
*Figure: Live/collaborative features (itinerary editing, voting presence) run over the Socket.io WebSocket server; everything else is a standard REST call from the Express API into Supabase, which also enforces RBAC directly via Postgres Row Level Security.*

### Build Plan & Scope

We are scoping the 3-week build phase around making Solo Mode and Group Mode fully functional, since together they cover the core value proposition (AI itinerary, vault, expense splitting) and are the most technically de-risked given our stack. OKU Mode will be delivered as a high-fidelity prototype (Figma/static UI) rather than a live feature, since real-time accessibility data (ramp/elevator status) isn't available via any public API and would require manually curated data to function — we'd rather be upfront about that than overclaim a "live" barrier-detection system we can't back with real data in three weeks.

**Week 1 — Foundations**
- Set up Next.js + TypeScript + Tailwind project, Supabase project (schema, Auth, RLS policies for Host/Member/Viewer)
- Build Solo Mode: dashboard, AI itinerary generation (OpenAI/Gemini call), map pinning (Google Maps API)
- Basic Vault: personal document upload/storage via Supabase Storage

**Week 2 — Group Mode & Real-Time**
- Stand up Socket.io server on Render; wire up live multi-user itinerary editing and the group poll/voting widget
- Group Vault: dual-layer storage (shared folder + personal lockers) with RLS-enforced access
- Expense module: manual expense entry + OCR receipt scanning (Tesseract.js first, Cloud Vision as fallback if accuracy is too low) + itemized split + "Settle Up" debt-minimization logic

**Week 3 — Polish, Offline, OKU prototype**
- PWA/offline caching (IndexedDB) for the Vault and itinerary views
- RBAC UI (permission management screen, not just backend enforcement)
- OKU Mode static prototype screens + voice assistant demo (can be mocked/scripted for the video rather than fully wired)
- Bug fixing, deployment (Vercel), and demo-data seeding for the presentation

**Explicitly out of scope for this build:** live accessibility data feeds for OKU Mode, WhatsApp export integration, offline translation packs (translation will require internet in this version).
