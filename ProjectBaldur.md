# Project Baldur: Drawing Intelligence & Architectural Orchestration
This document serves as an architectural overview, product design journey, and technical post-mortem of **Project Baldur**—an intelligent blueprint review, smart portal cross-referencing, and visual RFI generation platform built for Architecture, Engineering, and Construction (AEC).

It outlines the product thinking, structural decisions, and technical learnings derived from transitioning from rapid "vibe-coding" experiments into a disciplined, spec-driven **Hexagonal Architecture** executed across 28 sequential development sprints with agentic AI pair programming.

---

## 1. Context & Objective: The AEC Documentation Problem
In commercial construction, multi-page drawing packages are the single source of truth—yet they are fundamentally fragmented. A typical commercial drawing package spans dozens to hundreds of dense, 24×36 or 30×42 sheets across multiple disciplines (Architectural, Structural, Mechanical, Electrical, Plumbing).

When an estimator or engineer spots a conflict—such as a 16-inch supply air duct colliding with a structural W14×90 steel girder—resolving it requires piecing together disconnected clues scattered across the drawing set:
1. The **Floor Plan** (`M-201`) shows the horizontal duct run.
2. The **Structural Framing Plan** (`S-201`) shows the beam location and column grid.
3. The **Section Detail** (`S-501`) shows beam depth and bottom-of-steel elevations.
4. The **Mechanical Equipment Schedule** (`M-601`) lists air volume and clearance requirements.

### The Friction of Modern Constructability Reviews
Today, resolving these cross-sheet discrepancies means opening multiple PDF viewer tabs, measuring coordinates manually, and assembling a **Request for Information (RFI)**. 

The traditional RFI authoring workflow is notoriously tedious:
- Snipping manual screenshots from different PDF viewers.
- Pasting images into PowerPoint, Word, or Bluebeam.
- Drawing manual redlines, arrows, and callout boxes.
- Typing out discrepancy narratives and email memos to send to the design team or upload into project management systems like Procore or Autodesk Construction Cloud.

This process routinely takes 30 to 45 minutes per discrepancy, introducing communication lag and constructability delays.

### The Objective of Project Baldur
Project Baldur was conceived to eliminate this friction by transforming static PDF drawing sets into an interactive, coordinate-aware intelligence workspace:
- **1:N Smart Portals**: Live, hyperlinked visual viewports linking callout annotations on a primary drawing to multiple supporting detail sheets and schedules.
- **Schedule-to-Grid Intelligence**: Direct on-sheet tabular data extraction parsing equipment, door, and window schedules into structured data grids.
- **RFI Context Packet Generator**: Instant, zero-backend assembly of publication-ready, multi-sheet visual RFI cards that can be copied directly to the clipboard (as high-res PNG) or exported to 1-page vector PDFs in seconds.

---

## 2. Architectural Underpinnings & AI Product Thinking

My prior AEC experimentation in *Project Asgard* demonstrated that while one-shot "vibe-coding" can rapidly scaffold single-purpose point solutions, it hits an impenetrable wall when applied to complex, stateful desktop applications. 

Building canvas coordinate transformations, multi-sheet relational state, dual persistence adapters, and client-side document rasterization required treating architecture as an uncompromisable constraint. Before writing application code, I mapped out a formal **Hexagonal Architecture (Ports and Adapters)** to enforce strict separation of concerns.

```
┌────────────────────────────────────────────────────────┐
│               Project Baldur Frontend                  │
│       Angular 21 • Tailwind CSS • Konva • jsPDF        │
└───────────────────────────┬────────────────────────────┘
                            │ REST APIs / JWT Auth
┌───────────────────────────▼────────────────────────────┐
│                Python FastAPI Backend                  │
│             Hexagonal (Ports & Adapters)               │
├───────────────────────────┬────────────────────────────┤
│ Inbound Adapters:         │ Outbound Adapters:         │
│ • REST API Routers        │ • SQLite / MongoDB Repos   │
│ • Document & Annotation   │ • S3 / Local File Storage  │
│ • Ingestion Endpoints     │ • Celery Background Jobs   │
└───────────────────────────┴────────────────────────────┘
```

### The Hexagonal Boundary as an AI Guardrail
Hexagonal Architecture proved to be the single most effective structural choice for agentic development:
- **Core Domain Isolation**: Domain models (`Document`, `Sheet`, `Annotation`, `PortalLink`, `RfiPacket`) and business rules contain zero external dependencies. They don't know about FastAPI, SQLite, MongoDB, S3, or the browser.
- **Outbound Ports**: Standard Python `Protocol` / ABC interfaces define contracts for persistence (`DocumentRepositoryPort`, `AnnotationRepositoryPort`), file storage (`StoragePort`), and background tasks (`JobQueuePort`).
- **Dual-Adapter Strategy**: This allowed us to build lightweight, zero-dependency adapters (**SQLite** and local filesystem storage) for instant local iteration and automated testing, while developing production adapters (**MongoDB**, **AWS S3**, **Celery + Redis**) behind the exact same interface contracts without touching business logic.

### Frontend Architecture: Angular 21 & High-Performance Canvas
For the user interface, we selected:
- **Angular 21 (Standalone Components & Signals)**: Reactive state management for drawing active tools, portal link graphs, selection states, and RFI staging models without boilerplate overhead.
- **Konva.js & HTML5 2D Canvas**: Smooth 60 FPS panning, zooming, coordinate mapping, and redline rendering on massive high-resolution architectural blueprint rasters.
- **Zero-Backend Client-Side Export**: Rather than requiring server-side headless browsers to compile RFI packages, we engineered an in-browser rasterization pipeline using `html2canvas` and `jsPDF`. This enables instant, zero-latency export directly to the system clipboard and downloadable vector PDFs.

---

## 3. The Implementation & Visual Walkthrough

Project Baldur was fully implemented and verified through automated end-to-end browser workflows using Playwright. The three primary surfaces illustrate the complete workflow from document intake to RFI resolution:

### 1. Document Hub & Package Management
The centralized landing interface for ingesting, indexing, and organizing multi-sheet blueprint sets.
- Drag-and-drop PDF ingestion with automatic page rasterization.
- Fast metadata indexing tracking sheet counts, annotation density, and discipline classifications (MEP, Structural, Architectural).
- Quick search, sheet filtering, and one-click workspace launching.

<p align="center">
<img width="1440" height="980" alt="baldur-document-hub" src="https://github.com/user-attachments/assets/00a8fbcb-2a7a-4df5-9d44-45292bb81a30" />
</p>

---

### 2. Interactive Drawing Workspace & Smart Portals
The primary canvas interface where users navigate drawing sheets and establish relational intelligence.
- **Precision Drawing Tools**: Bounding box selectors, pin markers, and tabular schedule extractors.
- **1:N Smart Portals**: Detail callouts on plan views (e.g., Level 2 HVAC Plan `M-201`) can link directly to multiple reference sheets (e.g., Structural Section `S-201` and Schedule `E-401`).
- **Live Draggable Viewports**: Clicking a portal link launches floating, synchronized preview windows that display real-time crops of the linked sheets without navigating away from the primary view.
- **Component Inspector**: Real-time property inspection, custom notes, discipline tagging, and schedule-to-grid extraction.

<p align="center">
<img width="1440" height="980" alt="baldur-drawing-workspace" src="https://github.com/user-attachments/assets/8247a0e7-1ab0-44c7-bd3f-f63a35f175d1" />
</p>

---

### 3. RFI Context Packet Generator
A dedicated compilation engine that transforms detected constructability discrepancies into publication-ready engineering cards in seconds.
- **One-Click Evidence Assembly**: Automatically gathers the primary conflict area and up to 4 linked reference callouts from the Smart Portal network.
- **Hero View Promotion**: Users can dynamically designate which viewport represents the primary clash ("Hero View") versus supporting reference drawings.
- **Live WYSIWYG Staged Card**: Real-time preview of a standardized 8.5×11 landscape engineering memo complete with drawing numbers, issue narratives, suggested solutions, and cost/schedule impact tags.
- **Zero-Latency Client-Side Output**:
  - 📋 **Copy to Clipboard (PNG)**: Instant system clipboard integration via the Web Clipboard API for pasting directly into Procore, Autodesk Construction Cloud, Slack, or Teams.
  - ⬇️ **1-Page Vector-Framed PDF**: High-resolution downloadable PDF memo formatted for formal engineering submittals.
  - ⬇️ **High-Res PNG**: High-fidelity graphic export for project records.

<p align="center">
<img width="1440" height="980" alt="baldur-rfi-context-packet" src="https://github.com/user-attachments/assets/f3ecf421-fb40-46e2-859b-e32ee0b17d76" />
</p>


---

## 4. Technical Learnings: Orchestrating Agentic Workflows Across 28 Sprints

Project Baldur represented a fundamental shift in how I build with AI agents. In previous projects like *Lord of the Keys* or *Project Asgard*, I experimented with single-threaded pair programming and autonomous vibe-coding. While exciting, those workflows regularly resulted in code drift, forgotten requirements, and fragile architectures as complexity scaled.

For Baldur, I instituted a disciplined **Spec-Driven Agentic Cadence** powered by the `superpowers` skill suite.

### The 28-Sprint Trajectory
Rather than attempting to build the platform in broad strokes, the entire project was decomposed into a dependency-mapped graph of 28 sequential, bite-sized sprints:
- **Sprints 00–03**: Project scaffolding, domain models, outbound port contracts, and domain services.
- **Sprints 04–06**: SQLite repository adapters, local filesystem adapters, and inbound use case application services.
- **Sprints 07–12**: JWT authentication, REST API routing for documents, annotations, portals, and sync ingestion pipelines.
- **Sprints 13–15**: Angular authentication, upload workflows, and annotation synchronization.
- **Sprints 16–20**: Production adapters (MongoDB repository, AWS S3 storage, Celery background queues) and full integration test suites.
- **Sprints 21–25**: Schedule-to-grid intelligence, document persistence services, toolbar workflows, and the Document Hub landing interface.
- **Sprints 26–28**: Headless crop engines, RFI modal components, client-side export pipelines, and end-to-end workspace triggers.

### Key Operational Learnings

#### 1. Hexagonal Boundaries Prevent AI Architectural Drift
When an AI agent is asked to implement a new feature in a monolithic codebase, it frequently introduces tight coupling—importing database models into UI controllers or mixing API logic with business rules. 
By enforcing Hexagonal Architecture, the agent was strictly forbidden from crossing port boundaries. If an agent was assigned Sprint 04 (SQLite Adapter), it could only touch files inside `backend/src/adapters/outbound/repositories/sqlite/` and implement the exact contract defined in `ports/outbound/`. If it attempted to import FastAPI or alter domain models, the tests failed immediately.

#### 2. The Power of Written Specs Over Conversational Memory
Conversational context windows inevitably degrade over long development sessions. To eliminate this vulnerability, every major capability (Smart Portals, Document Hub, Schedule Extraction, RFI Packet Generator) was preceded by a formal architectural design specification committed to `docs/superpowers/specs/`. 

These specs outlined:
- User stories and acceptance criteria.
- Mathematical coordinate formulas for canvas scaling and viewport crops.
- Data structures and JSON payloads.
- Testing strategies and edge cases.

Each sprint plan (`docs/superpowers/sprintplans/`) was then derived directly from the spec, providing the executing agents with unambiguous, self-contained instructions.

#### 3. Test-Driven Development (TDD) as the Agentic Feedback Loop
TDD is often treated as optional in AI prototyping, but in multi-sprint agentic development, it is essential. Every sprint followed strict Red-Green-Refactor cycles:
- Backend: Pytest suites verifying domain logic, port implementations, and REST endpoints.
- Frontend: Vitest suites verifying Angular signal state and canvas math.
- The agent was not allowed to declare a sprint complete until the verification command passed with zero errors. This eliminated regressions across all 28 sprints.

#### 4. Automated Visual Verification with Playwright
One of the most satisfying workflows developed during Baldur was using Playwright not just for end-to-end smoke testing, but for **automated documentation capture**. 

Instead of taking manual screenshots or relying on mock graphics:
- We authored `e2e/capture-screenshots.spec.ts`, which programmatically boots the application, provisions mock blueprint documents, draws an architectural floor plan on the HTML5 canvas, simulates a clash scenario, opens Smart Portals, configures the RFI modal, and captures pixel-perfect 1440×980 production screenshots into `docs/images/`.
- Every image featured in this document was generated dynamically by that Playwright test suite, ensuring documentation remains 100% synchronized with actual code execution.

---

## 5. Key Learnings & Collaboration Manifest

* **The Collaboration Manifest**: *Spec-driven architecture / Multi-sprint agentic execution.*  
  I maintained tight control as Lead Product Architect—defining domain requirements, user journeys, coordinate schemas, and architectural boundaries. The AI agents operated as specialized, high-autonomy engineering teams executing bounded sprint plans under strict TDD verification.
* **Client-Side Zero-Latency Beats Cloud Complexity**: In early planning, we debated building a server-side PDF rendering worker using Headless Chrome or Puppeteer on Celery. However, evaluating real-world field workflows showed that estimators and engineers value instant responsiveness above all else. Engineering the RFI Generator to rasterize directly in the browser via `html2canvas` and the Web Clipboard API allowed users to click "Copy to Clipboard" and immediately paste high-res images into Procore or Slack with zero server roundtrip delay.
* **Separation of Specification from Execution**: Spending the time to write thorough specs and decomposed sprint plans upfront saved dozens of hours of debugging and rework. When the AI agent has a clear, bounded contract to fulfill, its code quality, precision, and velocity increase exponentially.

---

## 6. What's Next for Project Baldur

- **Enterprise Scale Ingestion**: Expanding the backend Celery ingestion pipeline to handle 500+ sheet drawing sets with asynchronous background tile rendering.
- **Multi-User Real-Time Collab**: Integrating WebSockets into the Konva canvas layer to enable real-time collaborative redlining and live portal sharing across field and office teams.
- **Offline Field Mode**: Implementing Service Worker caching and local IndexedDB synchronization to support offline blueprint navigation on jobsite tablets.

*Note: The Project Baldur codebase remains in a private repository. I built this solely for my own exploration on the concept, not as a commercial venture.  
This document serves as a technical record of the architectural patterns, product decisions, and agentic development workflows that brought the platform to life.*

