```markdown
# Detailed Implementation Plan: Real-time News RAG with Fact-Checking (Free/Demo Mode)

This project will simulate a full-stack real-time news ingestion and fact-check system using mock data. The backend API endpoints will be built using Next.js API Routes, while the frontend dashboard will be implemented using modern React components in Next.js. The UI will apply clean typography, spacing, and layout without external icons or image libraries.

---

## 1. Backend API Endpoints (Next.js App Router)

### File: src/app/api/news/route.ts
- **Purpose:** Serve a list of mock news articles.
- **Steps:**
  - Create a GET handler that returns an array of news articles (id, title, snippet, timestamp).
  - Use try/catch to handle errors and return a 500 error with a JSON message on failure.
  - Example response structure:
    ```typescript
    const newsArticles = [
      { id: '1', title: 'Breaking News: Demo Event', snippet: 'This is a demo news article...', timestamp: new Date().toISOString() },
      // ...more items
    ];
    ```
    
### File: src/app/api/fact-check/route.ts
- **Purpose:** Return fact-check details for a selected article.
- **Steps:**
  - Accept a query parameter (e.g., article id).
  - Validate the parameter; if missing, respond with 400 error.
  - Return a mock fact-check JSON with fields like "credibilityScore", "evidence", and "commentary".
  - Use error handling to catch unexpected issues.

### File: src/app/api/stream/route.ts
- **Purpose:** Provide Server-Sent Events (SSE) for real-time updates.
- **Steps:**
  - Set the requisite SSE headers and maintain an open connection.
  - Use a timer (e.g., setInterval) to send simulated news update events (as JSON strings).
  - Ensure proper error handling and connection closure on client disconnect.

---

## 2. Frontend Components and Hooks

### File: src/hooks/useRealtime.ts
- **Purpose:** Create a custom React hook for subscribing to SSE.
- **Steps:**
  - Use the browser’s EventSource to open a connection to `/api/stream`.
  - Update the component state with new articles as they arrive.
  - Handle errors and connection retries gracefully.

### File: src/components/NewsFeed.tsx
- **Purpose:** Display the list of news articles.
- **Steps:**
  - Fetch the news list from `/api/news` via the API client.
  - Render each article as a “card” with title, snippet, and timestamp.
  - Implement an onClick handler to select an article for fact-checking.
  - Use modern CSS classes (or Tailwind) for spacing and typography.

### File: src/components/FactCheckDisplay.tsx
- **Purpose:** Show fact-check results for a selected article.
- **Steps:**
  - Accept fact-check data via props.
  - Display details such as credibility score, supporting evidence (list of sources), and commentary.
  - Render fallback text if data isn’t available.

### File: src/components/SearchInterface.tsx
- **Purpose:** Provide a search/filter input to query news articles.
- **Steps:**
  - Implement a controlled input field.
  - On input changes, filter the news articles list locally.
  - Add debounce logic to limit re-render frequency.

### File: src/lib/api.ts
- **Purpose:** Centralize API calls.
- **Steps:**
  - Write utility functions: `fetchNews()`, `fetchFactCheck(id: string)` using fetch API.
  - Include error handling (check response status, throw errors if not 200).

---

## 3. Main Dashboard Integration

### File: src/app/page.tsx
- **Purpose:** Main dashboard page presenting the news feed, search bar, and fact-check results.
- **Steps:**
  - Import and use the `useRealtime` hook to get live updates.
  - Render the `SearchInterface` at the top.
  - Layout using a flex container: left pane for `NewsFeed`, right pane for `FactCheckDisplay`.
  - Manage state for selected news article to pass to the fact-check component.
  - Include error/loading states and proper responsiveness.

---

## 4. Styling & Error Handling
- **globals.css:** Update global styles (typography, spacing, colors) for a modern, clean UI.
- **Error Handling:**
  - API endpoints: use try-catch and proper HTTP status codes.
  - Frontend: display error messages or fallback content where API calls fail.
- **Best Practices:**
  - Modular file structure with clear responsibilities.
  - Consistent use of TypeScript types.
  - Comments and documentation in code for clarity.

---

## 5. Testing & Documentation
- **README.md:** Update with instructions on running the demo, including endpoints, and features.
- **Testing:** Use curl commands like:
  ```bash
  curl http://localhost:3000/api/news
  curl "http://localhost:3000/api/fact-check?id=1"
  ```
- Validate functionality of SSE by checking event stream responses.

---

## Summary

• Added new API endpoints in Next.js for news, fact-check, and SSE streaming with error handling.  
• Created custom hook (useRealtime) for real-time updates via EventSource.  
• Developed UI components (NewsFeed, FactCheckDisplay, SearchInterface) with clean, modern design using typography and layout.  
• Integrated a main dashboard page that dynamically displays and filters news articles with fact-check details.  
• Updated utility API functions and global styles to ensure consistency.  
• Included robust error handling and logging at both frontend and backend levels.  
• Documentation in README explains running the demo and testing endpoints.
