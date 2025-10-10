# PRD: Tours Listing (browse / search tours)

## 1. Introduction / Overview
Provide a responsive, discoverable Tours Listing page allowing end customers (travelers) to find and view tours. The feature enables browsing, searching, filtering, sorting and basic tour preview so users can quickly discover tours and proceed to booking. Goal: improve discoverability of tours, increase bookings, and provide a faster, mobile-friendly browsing experience using the current project tech stack.

## 2. Goals (measurable)
- Increase tour discovery actions (click-to-details) by 20% within 6 weeks of release.
- Reduce time-to-first-book by 15% (measured as time from listing load to booking initiation).
- Listing must be fully responsive and meet usability on narrow mobile screens (viewport ≤ 420px).

## 3. Target user
- Primary: End customer (traveler) searching and comparing tours.

## 4. User Stories
1. As a traveler, I want to search tours by keywords so I can find relevant experiences.
2. As a traveler, I want to filter tours by date, location, price range, and category so I can narrow results.
3. As a traveler, I want to sort results (recommended, price low→high, duration) so I can prioritize options.
4. As a traveler on mobile, I want results to load fast and be easy to scroll so I can browse on the go.
5. As a traveler, I want to see tour summary cards with key info (title, thumbnail, price, rating, short description) so I can decide which to open.

## 5. Functional Requirements (explicit, numbered)
1. The system must display a paginated (or infinite-scroll) list of tour cards returned from the backend.
2. The system must provide a keyword search input that queries tour title, short description, and tags.
3. The system must provide filters:
   3.1. Date / availability (start date, optional end date)  
   3.2. Location (city/region)  
   3.3. Price range slider or min/max fields  
   3.4. Category / type (multi-select)  
   3.5. Duration (e.g., half-day, full-day, multi-day)  
4. The system must provide sorting options: Recommended (default), Price ascending, Price descending, Duration, Popularity / Rating.
5. Each tour card must show: thumbnail image, title, short description (≤ 140 chars), starting price, duration, rating (if available), and a CTA "View details" or "Book".
6. The system must support server-side search/filter/sort via API query parameters to avoid loading all tours client-side.
7. The system must show loading state(s) and handle empty results with helpful copy and a CTA to broaden filters.
8. The system must handle and display API errors (network / server) with a retry action.
9. The listing must be responsive: desktop grid (≥ 2 columns), tablet (2 columns), mobile (1 column). Mobile-specific filter UI (sheet or modal).
10. The system must maintain filter & sort state in the URL (query parameters) so results are shareable and linkable.
11. The system must lazy-load images and support accessible alt text.
12. The system must respect existing auth state (show user-specific information only if user is authenticated).
13. All interactions must have unit tests for UI logic and API layer; main flows must have E2E tests (search, filter, open detail).
14. Performance: initial listing load (first contentful paint) should be under 2s on mobile 3G simulated network (or as close as current tech allows).

## 6. Non-Goals (Out of Scope)
- Payment processing and booking completion flows (only linking/CTA to existing booking flow).
- Major redesign of global styles; reuse existing style system/components.
- Full offline-first support or caching beyond standard HTTP caching.
- Importing or migrating data — assume existing tour data is present and served by current APIs.

## 7. Design Considerations
- Reuse existing card/list components and style tokens in the repo.
- Mobile: filters open in a bottom sheet or full-screen modal; ensure large tappable targets.
- Desktop: show a left-side collapsible filter panel and a top search bar.
- Accessibility: keyboard focusable, semantic HTML, proper aria labels for filters and error states.
- Provide clear microcopy for empty states and error retries.

## 8. Technical Considerations
- Use existing API endpoints in the repo. Expected API contract (examples):
  - GET /api/tours?query=...&location=...&minPrice=...&maxPrice=...&category=...&startDate=...&sort=...&page=...&limit=...
  - Response: { data: [{ id, title, slug, summary, thumbnailUrl, priceFrom, duration, rating, categories, availability }], meta: { total, page, limit } }
- Implement server-side paging; client requests should pass page & limit params.
- Implement debounced search (300ms) to avoid excessive API calls.
- Cache list responses at HTTP layer (Cache-Control) where applicable.
- Integrate with existing auth module: if user is logged in, fetch personalized recommendations / favorites (optional).
- Timezone: normalize startDate queries using ISO 8601 (UTC offset) to avoid date mismatches.
- Tests: unit tests for filter logic + API client mocks; E2E tests using existing test framework in repo.
- Error handling: classify errors (network, 4xx, 5xx) and show corresponding messages.

## 9. Success Metrics
- 20% increase in click-to-details rate within 6 weeks.
- 10% increase in bookings originating from listing page.
- 90%+ passing tests for the listing feature in CI (unit + E2E).
- Page load performance targets met or improved vs baseline.

## 10. Acceptance Criteria (mapping to Goals)
- End-to-end booking flow is reachable from listing: clicking "Book" / "View details" navigates to the existing detail/booking flow (A).
- Unit tests cover filter & sort logic; E2E tests cover search→filter→open details (B).
- API errors are surfaced with user-friendly messages and retry option (C).
- UI is responsive and matches existing style guide; mobile filter modal available (D).

## 11. Data Requirements
- Tour model fields required on listing:
  - id, title, slug, summary, thumbnailUrl, priceFrom, currency, duration, rating, categories[], location{city,region,country}, availability windows.
- API must return total count for pagination metadata.

## 12. Edge Cases & Error Conditions
- No results: show friendly message and suggestions to broaden filters.
- Partial availability across selected dates: show tours but mark limited availability.
- API returns large result set: ensure server-side pagination limits & client shows "Load more" or infinite scroll.
- Timezone mismatches for date filters — always use ISO timestamps.
- Image load failures: show fallback thumbnail.

## 13. Open Questions (need answers before development)
1. Confirm main tech stack used in repo (frontend framework/library + routing + state management). You answered "use current tech" — confirm it's acceptable to reuse components and HTTP client present in repo.
2. Preferred pagination UX: numbered pages, "Load more" button, or infinite scroll?
   - Options: A) Numbered pages B) Load more C) Infinite scroll (choose one)
3. Do we need to show personalized recommendations on the listing for logged-in users? (Yes / No)
4. Expected default result sorting: Recommended or Price ascending?
   - Options: A) Recommended B) Price low→high
5. Are there any analytics events we must send when users search / filter / click a tour? Provide event names/format if required.
6. Any legal/privacy constraints around location-based searches and storage of user queries?

## 14. Implementation notes for a junior developer
- Locate the listing page component (likely src/pages or src/routes). Reuse existing Card and Filter components.
- Add API client method for GET /api/tours with typed request/response models.
- Implement URL-driven state: read/write query params when filters change. Use router's push/replace to avoid full reloads.
- Add unit tests for:
  - filter-to-query param serialization
  - debounced search behavior
  - empty and error states
- Add E2E tests covering: perform search, apply filter(s), sort, open a tour detail.
- Keep UI components small and composable; extract FilterPanel, TourCard, Pagination components.

---

If you confirm choices for the open questions (pagination UX, default sorting, personalization, analytics), I will update the PRD and generate the final file.  