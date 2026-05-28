# ProjectTAP: Architecting an Agentic Task Ecosystem
This document serves as an architectural overview and technical post-mortem of ProjectTAP—an AI-powered, location-aware task management PWA. It outlines the product thinking, structural decisions, and technical learnings derived from building a complex application via an agentic AI development workflow.

## 1. Context & Objective
Project TAP (Time and Place) was built to solve the cognitive load of "notification failure." 
Traditional time-based reminders fail because they lack real-world context. There's a significant cognitive load required to keep track of to-do lists, much less execute on them.  
How many times have you come back from the grocery store only to realize that you needed something else from the supermarket next door but it was on a different list?

ProjectTAP bridges this gap using "Agentic Geofencing"—ensuring that tasks are surfaced when they are immediately actionable based on geographic proximity and user intent.

The objective was to build a cross-platform application that handles multi-modal input (voice, text, manual) and routes it through an intelligence core to manage execution.

Initial research into the market space showed that while many applications endeavor to provide this type of location-aware notification, most of them gate it behind high paywalls with limited functionality. 

## 2. Architectural Underpinnings & AI Product Thinking
Before implementation, my primary concerns were balancing my desire to build fast and test iterative capabilities against my lack of direct software application experience. 

I settled on building ProjectTAP as a multi-modal application, with a desktop interface alongside a mobile Progressive Web Application (PWA). My thinking was that with speed and learning as priorities, I didn't want to get bogged down in app store development and cross-platform dependencies between Android and iOS. A PWA elegantly solves that problem, but not without some loss of functionality...

### Task Entry View on Desktop:
<p align="center">
<img width="1915" height="907" alt="ProjectTAP_DesktopView_1" src="https://github.com/user-attachments/assets/5315c6fd-683f-4447-95c6-8efd61e07f17" />
</p>

### Task Management View on Mobile:
<p align="center">
<img width="615" height="901" alt="ProjectTAP_MobileView_1" src="https://github.com/user-attachments/assets/42646c72-3b37-4990-a720-1981d7d16c3b" />
</p>

### Balancing Data Models (Recurring Tasks): 
Designing the recurring task engine required evaluating multiple approaches. I leaned heavily on multiple interactive brainstorming processes to weigh a "Spawn-on-Complete" model (high history, high Firestore read/write costs) against a "Reset-on-Complete" model. 
I ultimately architected the system around "Reset-on-Complete" for the Beta phase. Why? This minimized document multiplication and kept Firestore operations lean while integrating cleanly with the Daily Recap logic (more on that later).

**Smart Suppression & Context Awareness:** To prevent sending a "Buy Milk" reminder while a user is driving past a grocery store on the highway, I engineered a Smart Suppression logic. The system analyzes accelerometer and GPS velocity data, suppressing triggers until the user has remained stationary at a destination for at least 60 seconds. We later added user configurable settings around this to account for different transportation models. 

**Privacy-First Analytics:** Tracking feature adoption without compromising PII was a hard requirement. I instrumented an "Aggregated Pulse Tracking" system utilizing daily atomic increments in Firestore. This provides robust weekly feature adoption metrics without tying events to individual users.

## 3. The Beta Implementation
TAP is currently in a stabilized Beta, relying on a hardened Firebase infrastructure and Google APIs.

**The Agentic Parser:** Powered by Google Gemini 2.5 Flash, the central AI agent processes unformatted voice or text input. It automatically structures the action, maps it to a saved Location Category (e.g., "grocery"), and infers priority.

**Background Geofencing:** The proximity engine relies on a Haversine formula running inside a dedicated Service Worker. It monitors location locally (ensuring privacy) and interfaces with Firebase Cloud Messaging (FCM) to trigger smart push notifications exactly when a user crosses a geofence threshold.

**State Machine & Forced Resolution:** The "Daily Recap" is a sequential state machine that forces a one-by-one audit of incomplete tasks. It leverages Google Calendar Insights to suggest "Smart Slots" for rescheduling. The AI handles temporal natural language commands (e.g., "reschedule for next time I'm at school") to handle task deferred execution.

## 4. Technical Learnings: Orchestrating Agentic Workflows
Building TAP was a deep dive into agentic orchestration—coordinating AI agents to build a full-stack application while maintaining some semblence of technical rigor.

**Agent Skills and PM orchestration:** 
- After some challenges and learnings in the first few development sprints, I settled on agent skills using the superpowers repo, which added tremendously to the performance of Gemini 3 Flash in particular. 
- I found that my spec and sprint writing worked best when I leveraged Claude (Sonnet 4.6 mostly) to write sprints targeted at agents of various models (Gemini 3.1 Pro and 3 Flash led the pack). 
- That enabled me to maintain strong artifact repos of sprint guidelines and directions as md files without adding too much human variability.

**Prompt Engineering is Systems Engineering:** Transitioning the Gemini parser from a prototype to a reliable production service required treating prompts like hardened code. When the AI parser experienced runtime failures, I stabilized the backend by enforcing strict schema definitions, standardizing lowercase types, and implementing few-shot prompting. This explicit constraint management yielded a 15% accuracy gain in task interpretation.

**Wrestling with Mobile OS Restrictions:** One of the hardest technical hurdles was ensuring background notification reliability. iOS and Android aggressively kill background processes. Debugging this required diving into Service Worker integrity, geofencing limitations, and ultimately architecting a robust FCM implementation to bypass OS sleep states and guarantee delivery. Although it's clear the better solution for notifications is to build mobile apps, for this first experiment I wanted to focus on UX dev and learning.

**Production vs. Local Contexts:** Local agentic development is fast, but production environments require strict boundary management. I had to resolve critical CORS policy errors blocking the AI parser in production, clarify the financial implications of Firebase deployment costs, and ensure that local development could run safely against production-like services without mutating real user data.
Managing AI-Introduced Technical Debt: AI agents are powerful but prone to hallucinating architectural patterns or clinging to legacy dependencies. I learned the hard way several iterations in that AI output requires as much systematic debugging as any development team. 
A prime example was guiding the AI to methodically isolate and safely prune residual Leaflet.js dependencies after migrating the application's mapping engine to the Google Maps API.

## 5. Key Learnings
- Separate your PM agent from your development and CI/CD agents: it took some experimentation but I found a process that really worked after I started writing specs and prompts with an agent running Sonnet while sending parallel agents running various Gemini models to do the software development. This allowed me to spend a few days each week brainstorming and exploring while my tokens refreshed, and then hammer out 4-6 full sprints work of work in one night.
- Even with the increased speed of development, you have to understand your users' real needs. I had one very active BETA user who sat down with me once a week and walked through the things she hated about current task applications...this led to my most successful feature, the Daily Recap. Built in a single week, this feature was inspired directly by this user who remarked that a certain un-named app was worthless because it was too easy to ignore overdue notifications. So I built an unavoidable daily recap flow that not only forces users to make a decision on what to do with an overdue tas, but integrates directly with the user's Google Calendars to find the most appropriate time to reschedule.

**The Daily Recap workflow**:

<p align="center">
  <img width="609" height="869" alt="ProjectTAP_MobileRecap_1" src="https://github.com/user-attachments/assets/ee446791-a82e-45dc-a773-956552665b84" />
</p>
  
  
## 6. What's Next for ProjectTAP
- BETA users are really enjoying the concept and UI, but the workflow is very constrained by notification limits in PWAs.
- If research proves successful, I'd like to try my hand at another challenge and build this into a mobile-native application.

Note: The ProjectTAP codebase remains closed-source as I evaluate paths to commercialization. This document serves as a record of the architectural decision-making and technical orchestration required to bring the system online.




