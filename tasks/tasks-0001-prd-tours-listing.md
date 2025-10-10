# Task List — tasks-0001-prd-tours-listing.md
Based on /tasks/0001-prd-tours-listing.md (Tours Listing PRD). This file contains high-level parent tasks only. Reply with "Go" to generate detailed sub-tasks.

## Relevant Files

- `src/pages/tours/index.(js|tsx)` - Main Tours Listing page component / route.
- `src/components/TourCard.(js|tsx)` - Tour summary card used in the listing.
- `src/components/FilterPanel.(js|tsx)` - Filter UI for location, date, price, category, duration.
- `src/components/SearchBar.(js|tsx)` - Debounced keyword search input.
- `src/components/Pagination.(js|tsx)` - Pagination / Load more / infinite scroll component.
- `src/lib/api/tours.(ts|js)` - API client for GET /api/tours and query param serialization.
- `src/lib/hooks/useDebounce.(ts|js)` - Debounce hook for search input (if not present).
- `src/lib/utils/queryString.(ts|js)` - Read/write URL query params helpers.
- `src/styles/*` - Existing style tokens/components to reuse.
- `spec/e2e/tours-listing.spec.(js|ts)` - E2E tests for search → filter → open detail flows.
- `src/components/__tests__/TourCard.test.(js|ts)` - Unit tests for TourCard.
- `src/pages/tours/__tests__/index.test.(js|ts)` - Unit tests for listing page behaviors (loading, empty, error).
- `cypress/integration/tours-listing.spec.(js)` or `playwright/tests/tours-listing.spec.(ts)` - E2E test file depending on existing test framework.

### Notes
- Place unit tests alongside components (same directory).
- Use existing HTTP client and routing in repo; adapt filenames/extensions to project conventions.
- Default to server-side paging. Confirm pagination UX before implementation (numbered, load more, infinite).

## Tasks

- [ ] 1.0 Project setup & discovery
  - Brief: Verify existing patterns, install any missing dev dependencies, and confirm test framework and router conventions.
- [ ] 2.0 API client & data models
  - Brief: Add/extend the tours API client to support search, filters, sort, pagination and response typing.
- [ ] 3.0 Core UI components
  - Brief: Implement or adapt TourCard, SearchBar, FilterPanel, and Pagination components using existing design tokens.
- [ ] 4.0 Tours Listing page
  - Brief: Build the listing page that composes components, reads/writes URL query params, handles loading/empty/error states, and navigates to tour details.
- [ ] 5.0 Performance & accessibility
  - Brief: Add lazy-loading for images, debounced search, ARIA attributes, and responsive behavior for mobile (filter modal / sheet).
- [ ] 6.0 Testing & QA
  - Brief: Add unit tests for components and logic, and E2E tests for the main flows (search → filter → open detail). Run CI and fix failures.
- [ ] 7.0 Integration & polish
  - Brief: Wire auth-dependent behavior (favorites/recommendations if applicable), analytics hooks, and finalize microcopy for empty/error states.

---

I have generated the high-level tasks based on the PRD. Ready to generate the sub-tasks? Respond with "Go" to proceed.