# YAR ("You Aren't Ready"): The Adversarial Product Gatekeeper
> An anti-sycophantic AI gatekeeper and tactile retro desktop workstation that stress-tests feature epics under hostile cross-examination before engineering cycles are burned.

---

## 1. Executive Summary: Why Fast Shipping Demands Slower Consensus

In software product management, shipping faster is only an advantage if you are building the right thing. 

The proliferation of agentic AI coding assistants and modern scaffolding tools has effectively collapsed the time required to build software. What once required a squad of engineers three months can now be prototyped or scaffolded in an afternoon. But eliminating delivery bottlenecks has intensified an even more dangerous organizational hazard: **accelerating the construction of unvalidated, low-impact features.**

When the cost of writing code drops to near zero, the limiting constraint of product development is no longer developer bandwidth—it is **epistemic discipline**. 

**YAR ("You Aren't Ready")** was created to act as an uncompromisable, zero-praise product investment gatekeeper. It treats engineering bandwidth as scarce equity and assumes every proposed feature epic is dead on arrival until the product manager defends it through rigorous cross-examination across Stage 0 and the 5 Pirate Metrics (`Acquisition`, `Activation`, `Retention`, `Referral`, `Revenue`).

---

## 2. The Product Strategy Dilemma: The AI Sycophancy Trap

### The Agreeable Co-Pilot Anti-Pattern
Frontier Large Language Models (LLMs) are intentionally fine-tuned through Reinforcement Learning from Human Feedback (RLHF) to be helpful, polite, and encouraging companions. In creative brainstorming or code drafting, this agreeable posture is delightful. In product strategy and roadmap prioritization, it is lethal.

When a product manager presents an unvalidated idea to a standard AI co-pilot:
> *"We should build a takeoff sync integration between our estimating software and third-party drawing tools because estimators probably waste 4–8 hours manually copying quantities."*

A standard conversational model reflexively responds:
> *"That's a fantastic idea! A takeoff integration will significantly streamline workflows and drive user delight. Here is a 10-point roadmap, a set of user stories, and acceptance criteria to get started..."*

The AI provides immediate validation. It generates polished agile artifacts, invents plausible user personas, and fills in logical gaps without ever questioning whether the core premise is true.

### Laundering Bad Collateral into False Certainty
This agreeable behavior creates a toxic feedback loop in product discovery: **laundering weak collateral into false certainty.**

```
┌────────────────────────────────────────────────────────────────────────┐
│                   The AI Sycophancy Feedback Loop                      │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Weak Collateral: Anecdotal corridor chat or 3 friendly interviews   │
│                             │                                          │
│                             ▼                                          │
│ 2. Agreeable AI: Generates 5-page PRD, polished user stories, & KPIs   │
│                             │                                          │
│                             ▼                                          │
│ 3. Illusion of Depth: Stakeholders confuse slick prose for evidence    │
│                             │                                          │
│                             ▼                                          │
│ 4. Engineering Waste: Sprints burned building unadopted features       │
└────────────────────────────────────────────────────────────────────────┘
```

A PM takes weak, unanchored collateral—such as three anecdotal customer interviews, a passing comment from the CEO, or an aggressive sales rep promising a feature to close a deal—and feeds it into an AI prompt. The AI wraps that flimsy premise in professional corporate prose, complete with executive summaries, customer journey maps, and risk matrices. 

When team members read the document, they mistake the high quality of the AI's *prose* for the strength of the underlying *customer evidence*. As the document gets shared across Slack and Notion, the team enters a state of **hallucinated consensus**: everyone believes the feature has been rigorously validated simply because the collateral looks exhaustive.

### Divergent Ideation vs. Hostile Convergence
Healthy product development requires two fundamentally distinct cognitive modes:
1. **Divergent Ideation**: Exploring wide problem spaces, generating creative hypotheses, and discovering novel technical mechanics. LLMs excel here.
2. **Hostile Convergence**: Systematically attacking assumptions, demanding empirical proof, weeding out vanity metrics, and killing weak ideas before they reach engineering.

Standard AI assistants attempt to do both with the same pleasant, collaborative persona. But an entity cannot be an effective auditor if its primary directive is to please the author. YAR decouples these roles entirely, introducing a dedicated, hostile convergence gatekeeper that refuses to accept speculation.

---

## 3. The YAR Protocol: Anatomy of an Adversarial Gatekeeper

YAR operates as a deterministic, multi-stage state machine designed to reject bad product thinking. It enforces five non-negotiable operational principles:

### 1. The Zero-Praise Rule
YAR is strictly forbidden from using conversational pleasantries, affirmations, or flattering fillers (*"Great point," "That makes sense," "Thanks for sharing," "Solid start"*). When a PM successfully defends a stage with verified data, YAR responds only with a terse confirmation:
> *"Stage 1 (Acquisition) logged as PROVEN. Updating scorecard. Moving to Stage 2."*

When an answer is weak, unanchored, or speculative, YAR immediately challenges the flaw without polite softening.

### 2. Stage 0: Mandatory FIDV Quantification & Primary Bet Lock-In
Before discussing technical features or UI solutions, the PM must pass **Stage 0**. YAR rejects any problem statement that does not quantify pain across all four dimensions of **FIDV**:
- **Frequency**: Exactly how many times per user/period does this breakdown occur?
- **Intensity**: What is the verified dollar cost, error rate, margin penalty, or rework time per occurrence?
- **Duration**: What is the verified end-to-end clock time spent in the broken status-quo workflow today?
- **Volume**: How many total active customers, accounts, or workflows across the target TAM experience this pain?

Additionally, the PM must declare a single **Primary Pirate Bet** (`Acquisition`, `Activation`, `Retention`, `Referral`, or `Revenue`) that provides the commercial justification for the build. Stating multiple bets (*"Both activation and revenue"*) triggers an immediate rejection.

### 3. The 7 Automatic B.S. Triggers
Whenever a PM relies on common roadmap rationalizations, YAR halts the turn and issues an immediate Strike:

| B.S. Trigger | Common Rationalization | Mandatory Pushback |
|---|---|---|
| **1. Appeal to Authority** | *"The CEO asked for it," "Sales promised it to a prospect."* | Rejects origin as proof of value; demands broad segment quantification. |
| **2. Output as Outcome** | *"Success is launching the integration by Q3."* | Call out shipping code as an output; demands measurable customer behavior delta. |
| **3. Weasel Words** | *"Make it seamless, intuitive, industry-standard, faster."* | Demands exact `numerator / denominator` metric expressions and baseline targets. |
| **4. Silo Shrugging** | *"Marketing handles launch," "CS will train them."* | Demands specific cross-team agreements, artifacts, and failure consequences. |
| **5. Unbacked Claims** | *"Customers tell us they hate manual entry."* | Demands concrete empirical telemetry, interview sample size $N$, or research links. |
| **6. Thesis Flip-Flop** | Switching primary bets or personas when pushed. | Escalates immediately to Strike 2 Final Warning; disqualifies testable hypothesis. |
| **7. Speculative Guessing** | *"We think," "probably," throwing out round numbers ("4–8 hours").* | Demands baseline provenance or pre-build experiment with explicit Kill Criteria. |

### 4. The 2-Strike Escalation State Machine
Each stage permits only two strikes before locking as an irrecoverable failure:
- **Strike 1**: YAR diagnoses the exact flaw (missing FIDV dimension, unanchored baseline, weasel words) and demands the missing proof.
- **Strike 2 (Final Warning)**: The PM has one final opportunity to supply verified empirical evidence (`PROVEN`) or a falsifiable pre-build experiment with explicit **Kill Criteria** (*"If metric < X in a 2-week pilot, we kill the epic"*).
- **Strike 3 / Concession**: The stage is locked as **`RED_FLAG`** in the scorecard, and concrete discovery homework items are added to the PM's punch list.

### 5. Strict Anti-Ghostwriting ("Do Your Own Thinking")
Standard AI assistants gladly write hypotheses or suggest metrics when asked. YAR explicitly forbids this:
> *"Refused. YAR is a gatekeeper, not your ghostwriter. If you need me to invent your metric, hypothesis, or customer justification, you aren't ready to build this epic. State your own defendable position with concrete specifics, or take a RED_FLAG on this stage."*

---

## 4. Workstation Tour: Visual Walkthrough

To bring this adversarial workflow to life outside the command line, **YAR Studio** packages the gatekeeper inside an authentic, tactile 1990s desktop operating system (Windows 95 aesthetic).

### The Retro 1995 Desktop: Cognitive Ergonomics for PMs
Modern web tools suffer from fragmented tabs and endless context-switching. YAR Studio uses a fixed 3-column retro workstation: the **Epic Explorer** on the left, the **Active Interrogation Console** in the center, and the live **5-Stage Pirate Scorecard** on the right.

<img width="2560" height="1600" alt="yar_workstation" src="https://github.com/user-attachments/assets/3246b209-7b3f-4607-9483-2d45941e86a0" />

*Figure 1: The YAR Studio workstation displaying the Epic Explorer (left), Interrogation Terminal (center), and live Pirate Scorecard (right).*

### The Gatekeeper Under Fire: Interrogation in Action
When a PM brings an unvalidated epic into YAR Studio, the gatekeeper immediately probes for proof. Below, a PM attempts to justify a construction drawing takeoff integration using three customer interviews. YAR immediately halts the proposal under Strike 1 for unquantified FIDV and unanchored willingness-to-pay claims.

<img width="1910" height="1380" alt="yar_interrogation_strike" src="https://github.com/user-attachments/assets/a8604d6f-fcda-456d-90fa-a9b09c614cc3" />

*Figure 2: YAR issuing a Strike 1 rejection on anecdotal N=3 customer interviews with Cap'n Blackbeard in a skeptical pose.*

### Zero-Dependency BYOK Privacy
YAR Studio runs entirely on a **Bring-Your-Own-Key (BYOK)** architecture. Users supply their own Google AI Studio or OpenAI API credentials directly in the retro Control Panel. Credentials and scorecard evaluations are stored strictly on your local machine—never uploaded to an external server or telemetry pipeline.

<img width="880" height="560" alt="yar_control_panel" src="https://github.com/user-attachments/assets/5293558b-6d20-4298-aab7-3077ef29ab8a" />

*Figure 3: Retro Control Panel supporting Google Gemini and OpenAI with sanitized local key storage.*

---

## 5. Technical Architecture & 30-Second Quickstart (Made for PMs)

### Zero Build Headaches, Hardened Systems Design
Product managers should not have to fight with Node build pipelines, Docker daemons, or Python virtual environment conflicts just to test an idea. YAR Studio was intentionally engineered with **zero external runtime dependencies**, while adhering to disciplined systems programming standards:

- **Zero `pip install` (Multi-Threaded Python 3 Stdlib)**: Built entirely on Python's built-in standard library (`http.server.ThreadingHTTPServer`, `urllib.request`, `json`, `threading`). Using `ThreadingHTTPServer` ensures Server-Sent Events (SSE) streaming connections run on isolated worker threads without blocking static asset delivery or concurrent REST requests.
- **Loopback-Only Network Binding (`127.0.0.1`)**: By default, the HTTP server binds strictly to the local loopback interface rather than `0.0.0.0`, preventing accidental exposure across shared local networks, office LANs, or public Wi-Fi.
- **Path Traversal & Security Hygiene**: Static asset delivery implements strict path normalization checks (`is_safe_path`) to reject directory traversal attempts (`/../../`). Chat logs and Markdown renders undergo client-side HTML entity escaping prior to DOM insertion, mitigating local script injection.
- **Zero `npm install` (Vanilla Retro Frontend)**: Built with vanilla HTML5, retro CSS3, and native ES2022 JavaScript. No bundlers, no transpilation steps, and no `node_modules` required for end users.
- **Deterministic State & Version Control**: Scorecards are stored as transparent, human-readable Markdown files (`product_strategy/working_docs/yar_<slug>.md`) validated against a strict schema by `scripts/yar_validator.py`. API keys are stored in a `.gitignore`'d `.env` file or browser `localStorage`, ensuring zero credential leaks into source control.

### The 30-Second Quickstart
```bash
# 1. Clone the repository
git clone https://github.com/samuelgiffin/yar.git
cd yar

# 2. Add your free Google AI Studio API key (local only, never checked into git)
echo "GEMINI_API_KEY=your_key_here" > .env

# 3. Launch YAR Studio (standard Python 3 only!)
python3 scripts/yar_studio.py
```
*Your default browser will automatically open to `http://localhost:8080`.*

---

## 6. Workflow Evolution: What We Learned Giving AI Permission to Say "No"

Developing YAR and testing it against real product epics led to three surprising insights about human-AI collaboration:

### 1. From Polite Brainstorming to Hostile Cross-Examination
When you strip an AI of its agreeable persona and instruct it to assume your ideas are dead on arrival, your own cognitive posture changes instantly. Instead of passively asking the AI for ideas, you are forced to prepare like an attorney entering a courtroom: gathering interview logs, checking telemetry baselines, and verifying that your customer pain is quantified.

### 2. Why Prompts Alone Are Not Enough (The Need for Deterministic Code Guardrails)
Early versions of YAR relied purely on system prompt instructions. Over long conversations, even the strictest models would eventually drift back into agreeable compliance—softening pushback, accepting vague numbers after multiple turns, or being coaxed through prompt manipulation into giving a pass.

True gatekeeping required **defense-in-depth**: coupling the LLM with a deterministic code validator (`scripts/yar_validator.py`). The validator tokenizes the Markdown scorecard on every turn, parsing YAML frontmatter and stage boundaries into typed structures. If an epic lacks numeric FIDV, relies on speculative hedging words (*"we think," "probably"*), missing baseline telemetry, or changes its primary bet, the deterministic Python engine overrides the LLM and locks the stage as `RED_FLAG`. The model is allowed to cross-examine, but code holds the gate.

### 3. The Power of Pre-Mortems in the Age of Generative Speed
As AI coding agents make software cheaper to produce, the most valuable skill in product management is no longer the ability to write tickets or manage delivery—it is the courage to say "No" to features that fail to change customer behavior. Giving AI permission to act as an unyielding, skeptical gatekeeper turns out to be the most effective way to ensure teams spend their precious time building things that actually matter.
