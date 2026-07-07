# 8-Week Interview Prep Plan
**Target:** AI Research / AI Infra / Applied AI Engineer roles — deliberately stepping away from generic "AI SWE"
**Constraints:** Weekdays = 4hrs DASH + 2hrs gym + 2hrs research + ~8hrs interview prep (16hr day). Gaming is flexible decompression, not a fixed slot — see note below. Weekends = flexible, use for mocks + deep work + research + gaming.

**Positioning note:** DSA and general HLD/LLD are filter-round gates — companies use them to screen before the role-specific rounds, not because you'll design a URL shortener or a parking lot on the job. Given your target is AI Research / AI Infra / Applied AI, the right allocation is: get DSA and general HLD/LLD to a comfortable, reliable pass bar (not mastery), and put the reclaimed time into AI-specific system design, infra depth, and research output — since that's what actually gets you hired and what the later, role-specific rounds will judge you on. The hours below reflect that shift: general HLD/LLD gets less time than a generic plan would give it, and AI-specific system design (the track added earlier) gets correspondingly more.

**Which resume to use:** You now have two base resumes — an AI resume (leads "Software Engineer, AI Engineer," bullets framed around agent orchestration, evals, RAG) and a SWE resume (leads "Software Engineer," same underlying projects reframed around systems/backend/scale, no AI-specific vocabulary). Default rule:

- **AI resume** → any posting titled AI Research, AI Infra/Platform, Applied AI Engineer, ML Platform Engineer, Research Engineer, or Member of Technical Staus (Infra/Research). This is the default for almost everything on your target list.
- **SWE resume** → generic "Software Engineer" or "Backend Engineer" postings at companies you're using to widen the funnel, or where the posting explicitly signals a non-AI-specific systems role. Use this sparingly — it's a funnel-widener, not your primary track.
- When in doubt, use the AI resume. It's the one aligned with your actual positioning goal (breaking out of the generic "AI SWE" bucket), and the SWE resume exists mainly as a fallback for postings where AI-specific framing would read as a mismatch.

---

## Daily Weekday Template (16 hours)

| Block | Time | Focus |
|---|---|---|
| 1 | 2 hrs | **Research** — multi-agent experiment / self-blindness direction (protected block, first thing) |
| 2 | 4 hrs | DASH work |
| 3 | 3 hrs | DSA — patterns + timed problems (floor-level target, not deep mastery) |
| 4 | 2 hrs | Gym |
| 5 | 1.5 hrs | General HLD/LLD — filter-round floor (see rotation below) |
| 6 | 1.5 hrs | **AI-specific system design & infra** — RAG, serving/batching, evals, agent orchestration, your DASH work as case studies |
| 7 | 1.5 hrs | Applications + networking (tailored, not spray) |
| 8 | 0.5 hr | Behavioral stories / résumé & LinkedIn / daily review |

Order above is illustrative, not prescriptive — sequence blocks however fits your energy. Keep research and DSA in whichever slots have the most focus, since both degrade fastest with fatigue.

**Gaming:** Since it's occasional rather than daily, don't carve a fixed slot for it — treat it as the thing that fills gaps or caps off a day that went well (e.g. after a good mock, after clearing a DSA session), rather than something competing with the core blocks above. If a session runs long, let it eat into evening slack rather than the research/DSA/system-design blocks.

Weekends: 1 mock interview (DSA or system design, alternate weeks), 3–4 hrs applications, 2–3 hrs AI-specific system design deep dive (this is where the "real company prompts" list gets worked through in full), 2–3 hrs research, gaming/rest fills remaining time.

**General HLD/LLD rotation (floor-level, not the focus):** Keep this simple and time-boxed — the goal is competence, not depth.

- Mon/Wed/Fri: HLD — one classic design (URL shortener, rate limiter, chat system) per session, aim for "solid, not exhaustive"
- Tue/Thu: LLD — one OOD problem (parking lot → elevator → vending machine → library management, in that order), full class diagram + interfaces
- Once you can reliably produce both in ~30 min without freezing (should happen by end of Phase 2), stop adding new general HLD/LLD problems — switch remaining time in this block to review/spaced repetition only, and let the AI-specific block absorb any extra capacity.
- LLD resources: *Head First Design Patterns*, Educative's "Grokking the Object Oriented Design Interview," or the free [algomaster.io/learn/lld](https://algomaster.io/learn/lld) course / [ashishps1/awesome-low-level-design](https://github.com/ashishps1/awesome-low-level-design) GitHub repo.
- HLD resources: *System Design Interview* Vol 1 & 2 (Alex Xu), ByteByteGo blog, or [systemdesignschool.io](https://systemdesignschool.io/fundamentals/what-is-system-design-interview) (the site you linked).

---

## Phase 1 — Weeks 1–2: Rebuild DSA Foundations + Start Applying
**Goal:** Get from "rusty" to "fluent on mediums," get applications moving immediately, and get the research track into a steady daily rhythm.

- **Research:** Use these two weeks to pick a clear focus — likely converging on likelihood ratios/scoring for the multi-agent experiment first since it's closer to done, while letting the self-blindness direction stay in a lighter reading/scoping mode until it has room to breathe.

- **DSA:** Work through NeetCode 150 in pattern order (arrays/hashing → two pointers → sliding window → stack → binary search → linked list → trees → tries → heap → backtracking → graphs → DP). ~4-5 problems/day. Redo any you can't solve clean within 25 min two days later.
- **General HLD/LLD (floor-level):** Read fundamentals only — scalability basics, caching, load balancing, DB indexing/sharding. One easy full design (URL shortener) and one easy OOD problem (parking lot) to establish the format for each.
- **AI-specific system design:** Read Chip Huyen's *Designing ML Systems* ch 1-3 for framing. Start the "AI Infra / Applied AI Engineer" topic table from earlier in this doc — pick 2-3 rows/week (transformer fundamentals, RAG basics, fine-tuning vs RAG tradeoffs) and write a one-paragraph explanation in your own words for each, out loud.
- **Applications:** Build target list (20-30 companies), weighted toward AI Research / AI Infra / Applied AI titles specifically — avoid generic "AI Software Engineer" postings, which pull the crowd you're trying to differentiate from. Use the AI resume as default; keep the SWE resume in reserve for funnel-widening applications. Apply to 5-8/week — don't wait for "ready."
- **Behavioral:** Draft 6-8 STAR stories from DASH incidents (OOM crash root-cause, PSU overcurrent debug, cryptominer remediation, leading 10+ engineers) and the multi-agent research. These double as system design talking points.
- **Milestone (end wk2):** 40+ LeetCode mediums done, 10-15 applications out, résumé finalized with infra/research framing.

## Phase 2 — Weeks 3–4: Deepen Patterns + AI-Specific Design Takes the Lead
**Goal:** Comfortable with mediums under time pressure, starting hards. General HLD/LLD reaches a reliable floor. AI-specific system design becomes the primary design focus.

- **Research:** Aim for a checkpoint by end of week 4 — either aligned likelihood ratios/scoring implemented for the multi-agent experiment, or a scoped-out plan/pilot for self-blindness. A tangible checkpoint here also becomes interview material.

- **DSA:** Continue NeetCode 150 (graphs, DP, greedy, intervals). Start timing yourself strictly (medium = 25 min, hard = 40 min). Mix in 1-2 hards/week.
- **General HLD/LLD:** One classic HLD design (rate limiter, chat system) and one medium OOD problem (elevator system, vending machine) per week — this is close to the target floor, don't over-invest further.
- **AI-specific system design:** This is where the extra weekly hours should go. Work through LLM serving fundamentals (KV cache, continuous batching, tensor/pipeline parallelism — lean directly on your DASH work), RAG as a distributed systems problem (chunking, vector DB indexing, reranking, multi-tenant partitioning), and start on the "real company prompts" list — do 1 full write-up (e.g. "design an end-to-end batching system for LLM queries") this phase.
- **Applications:** Ramp to 8-10/week, still weighted toward Research/Infra/Applied AI titles (AI resume default). Start warm outreach — comment/DM people at target labs, reference your paper and DASH work.
- **Milestone (end wk4):** First recruiter screens likely landing. General HLD/LLD at reliable floor (~30 min per problem, no freezing). 1 full AI-specific system design write-up done.

## Phase 3 — Weeks 5–6: Mocks + Full AI Infra Depth
**Goal:** Simulate real interview pressure. General HLD/LLD moves to maintenance-only; AI-specific system design and research narrative become the center of gravity.

- **Research:** By this phase you should be able to explain the self-blindness angle in under 2 minutes — the human self-other asymmetry framing, why it matters for eval-faking and agent oversight, and where it sits relative to the multi-agent experiment. Rehearse this alongside your behavioral stories, not separately.

- **DSA:** 1 formal mock/week (Pramp, interviewing.io, or a friend) + daily timed mediums/hards. Start tracking pattern-recognition speed, not just correctness.
- **General HLD/LLD:** Maintenance only — light spaced repetition on problems already done, no new topics. Reclaim the rest of this time for AI-specific work.
- **AI-specific system design:** Go deep — evaluation methodology (golden datasets, LLM-as-judge and its blind spots, regression testing), agent orchestration/oversight (directly your research angle), multi-tenant GPU scheduling, cost-optimized inference routing (your $1M/yr project *is* a case study here — rehearse presenting it as a system design answer). Work through 2 more "real company prompts" (e.g. GPU credit management, multi-tenant AI chatbot platform).
- **Applications:** Keep 8-10/week; shift effort toward interview prep for companies that responded. Tailor behavioral answers per company's stated values.
- **Behavioral:** Add negotiation prep, "why this company" answers, and 2-3 stories specifically about leading engineers / conflict resolution.
- **Milestone (end wk6):** Should have 1-3 active interview loops. Comfortable narrating an AI infra/system design end-to-end in ~35 min, with general HLD/LLD as a reliable but no-longer-growing floor.

## Phase 4 — Weeks 7–8: Interview-Ready Polish
**Goal:** Tight, confident, company-specific execution.

- **DSA:** Taper volume, raise intensity — mock interviews 2-3x/week, focus only on your logged weak patterns.
- **General HLD/LLD:** Cold mock reps only, no new prep — confirm the floor holds under pressure.
- **AI-specific system design:** Company-specific prep — research their actual infra/engineering blog posts where available (how a given lab serves models, handles evals, structures agent oversight). Practice trade-off articulation out loud, not just diagrams. Work through remaining "real company prompts" relevant to companies you're actually interviewing with.
- **Applications:** Maintain pipeline but prioritize interview prep for live processes over cold applying.
- **Behavioral:** Full mock loops (DSA + AI system design + behavioral back-to-back) at least twice in this phase to build stamina.
- **Milestone (end wk8):** Ready for onsite/final loops. Clear "why me" narrative tying DASH infra + research + leading engineers into a coherent AI Research / AI Infra / Applied AI Engineer pitch — explicitly distinct from a generic "AI SWE" positioning.

---

## Resources
- **DSA:** NeetCode 150, Blind 75 (as overflow), LeetCode Premium for company-tagged questions
- **General system design (HLD):** *System Design Interview* Vol 1 & 2 (Alex Xu), ByteByteGo blog
- **Low-level design (LLD/OOD):** *Head First Design Patterns*, Educative's "Grokking the Object Oriented Design Interview"
- **LLD fundamentals site (systemdesignschool.io equivalent):** [algomaster.io/learn/lld](https://algomaster.io/learn/lld) — free, structured course covering OOP fundamentals, design principles, patterns, and UML, organized the same way as the HLD site you linked. Backed by the [ashishps1/awesome-low-level-design](https://github.com/ashishps1/awesome-low-level-design) GitHub repo (12k+ stars) if you want the raw problem set + solutions instead of the course wrapper.
- **ML system design:** *Designing Machine Learning Systems* (Chip Huyen), *Machine Learning System Design Interview* (Ali Aminian & Alex Xu)
- **Mocks:** interviewing.io, Pramp, or peers from your DASH team

---

## Detailed Topic Breakdown

### DSA — Patterns (in build order)
Work these roughly in this sequence; each pattern should reach "recognize + solve clean in <25 min" before moving on.

| Pattern | Key problems to anchor on | Resource |
|---|---|---|
| Arrays & Hashing | Two Sum, Group Anagrams, Top K Frequent Elements | NeetCode 150 (Arrays & Hashing section) |
| Two Pointers | Valid Palindrome, 3Sum, Container With Most Water | NeetCode 150 + Grokking the Coding Interview (Two Pointers pattern) |
| Sliding Window | Longest Substring Without Repeating Chars, Minimum Window Substring | NeetCode 150 + Grokking (Sliding Window) |
| Stack | Valid Parentheses, Daily Temperatures, Min Stack | NeetCode 150 (Stack section) |
| Binary Search | Search Rotated Sorted Array, Find Minimum in Rotated Array | NeetCode 150 + "Binary Search" pattern on LeetCode Explore |
| Linked List | Reverse Linked List, LRU Cache, Merge K Sorted Lists | NeetCode 150 (Linked List section) — LRU Cache also doubles as an LLD warm-up |
| Trees | Invert Binary Tree, Validate BST, Lowest Common Ancestor, Serialize/Deserialize | NeetCode 150 (Trees section) |
| Tries | Implement Trie, Word Search II | NeetCode 150 (Trie section) |
| Heap/Priority Queue | Kth Largest Element, Task Scheduler, Merge K Sorted Lists | NeetCode 150 (Heap section) |
| Backtracking | Subsets, Permutations, N-Queens, Word Search | NeetCode 150 (Backtracking section) |
| Graphs | Number of Islands, Clone Graph, Course Schedule (topological sort), Dijkstra basics | NeetCode 150 (Graphs section) + "Graph Theory" playlist on NeetCode YouTube |
| DP | Climbing Stairs, Coin Change, LIS, Edit Distance, House Robber | NeetCode 150 (DP section) — this is usually the longest grind, budget extra days |
| Greedy / Intervals | Merge Intervals, Jump Game, Meeting Rooms II | NeetCode 150 (Greedy + Intervals sections) |

Once base patterns are solid (end of Phase 2), shift remaining DSA time to: company-tagged questions on LeetCode Premium for your actual target list, and a rotating mix of hards from Blind 75 overflow.

### HLD — Core Building Blocks
These are components to reason about and combine on the fly, not memorize as fixed answers.

| Topic | What to actually know | Resource |
|---|---|---|
| Scalability basics | Vertical vs horizontal scaling, stateless services, load balancer algorithms (round robin, least connections) | *System Design Interview* Vol 1, ch 1 |
| Caching | Cache-aside vs write-through vs write-back, eviction policies (LRU/LFU), CDN basics | *System Design Interview* Vol 1, ch 2 + ByteByteGo caching posts |
| Databases | SQL vs NoSQL tradeoffs, indexing, replication (leader-follower), sharding strategies | *System Design Interview* Vol 1, ch 3-4 |
| Consistency & availability | CAP theorem in practice, eventual vs strong consistency, quorum reads/writes | *System Design Interview* Vol 2, consistency chapter + *Designing Data-Intensive Applications* (Kleppmann) ch 5 & 9 if you want to go deeper |
| Message queues | Kafka vs RabbitMQ use cases, pub/sub vs point-to-point, backpressure handling | ByteByteGo "Message Queue" post + *System Design Interview* Vol 2 |
| Rate limiting | Token bucket, leaky bucket, sliding window counter — tradeoffs, not just names | *System Design Interview* Vol 1, ch 5 (also a good LLD crossover problem) |
| Classic full designs | URL shortener, chat system (WhatsApp-style), news feed, ride-sharing, job scheduler | *System Design Interview* Vol 1 & 2, one chapter per design |

### ML System Design — Topics Specific to Your Target Roles
DASH experience gives you a real edge here — treat these as "explain what I already did" rather than "learn from scratch" wherever possible.

| Topic | What to actually know | Resource |
|---|---|---|
| LLM serving fundamentals | KV cache mechanics, continuous batching, tensor/pipeline parallelism, quantization tradeoffs | Chip Huyen's *Designing ML Systems* ch 7 (Model Deployment) + your own vLLM/Blackwell work as primary source |
| Inference cost optimization | Request routing (local vs API), batching strategy, autoscaling for GPU workloads | Your $1M/yr routing project — write this up formally as a case study, it's your strongest material here |
| RAG architecture | Chunking strategy, embedding model choice, retrieval + reranking pipeline, freshness/staleness tradeoffs | *Designing ML Systems* ch 6 + *ML System Design Interview* (Aminian & Xu), RAG chapter |
| Training infra at scale | Data pipeline design, distributed training basics (data vs model parallelism), checkpointing | *Designing ML Systems* ch 4-5 |
| Feature stores | Online vs offline feature serving, freshness requirements, point-in-time correctness | *ML System Design Interview* (Aminian & Xu) — dedicated feature store chapter |
| Eval systems | Offline vs online eval, human-in-the-loop eval, eval-set contamination/overfitting risk | *Designing ML Systems* ch 6 (Evaluation) — ties directly into your self-blindness research angle |
| Agent orchestration & oversight | Multi-agent coordination patterns, oversight/monitoring of agent behavior, failure detection | Mostly you — your DASH orchestration work + self-blindness research are your primary sources, more than any book |
| Multi-tenant GPU scheduling | Fair-share scheduling, priority queues, resource isolation | Your dual-Blackwell tensor-parallel deployment work is the strongest source; supplement with Kubernetes GPU scheduling docs for standard vocabulary |

### AI Infra / Applied AI Engineer — The AI-Specific Layer
This is the layer generic SWE/system-design prep won't cover, and it's exactly where your target roles (frontier labs, Applied AI Engineer, AI Infra) will spend real interview time. Treat it as a fifth track alongside DSA/HLD/LLD/ML-system-design above — it overlaps with ML system design but goes narrower and more current (interviewers are testing 2026-era practice, not textbook ML).

| Topic | What to actually know | Resource / where it's tested |
|---|---|---|
| Transformer & LLM fundamentals | Attention mechanism, tokenization, KV cache, autoregressive decoding, decoding strategies (beam search, top-k, top-p) | Jay Alammar's illustrated transformer post; you already know KV cache deeply from DASH — make sure you can explain it from first principles too, not just "how I configured it" |
| Fine-tuning vs RAG vs prompting | When each is the right call, cost/maintenance tradeoffs, hybrid approaches, break-even token volume for self-hosting | Asked explicitly in 2026 loops — know the "it depends on X" framework, not a fixed answer |
| RAG as a distributed systems problem | Chunking strategies, embedding choice, vector DB indexing (HNSW/IVF), reranking, multi-tenant partitioning, caching hot queries | This is reported as one of the single most common opening questions across 2026 loops — practice a full design end to end |
| Agentic systems | Tool use, planner-executor patterns, multi-agent coordination, failure modes (runaway tool calls, retrieval drift) | ReAct paper + your own multi-agent research is a stronger source than any guide here |
| Evaluation methodology | Golden datasets, LLM-as-judge (and naming its blind spots), regression testing on prompts, offline vs online eval | Called out repeatedly as "the new system design" — a vague "we look at outputs" answer reads as junior |
| Production observability for LLM systems | Trace logging (prompt, response, latency, cost, token counts), TTFT vs inter-token latency, drift detection | Directly maps to monitoring work you've already done at DASH — reuse that material |
| Inference serving & batching | Continuous/dynamic batching, throughput vs latency tradeoffs, GPU memory management across multiple models, request queuing/priority scheduling | Anthropic and OpenAI loops repeatedly ask a version of "design a batching system for a GPU serving many requests" — your vLLM/tensor-parallel work is direct prep for this |
| Cost & infra tradeoffs | Open-source self-hosted vs proprietary API economics, the token-volume break-even point, shadow-mode testing before committing | Your $1M/yr routing project answers this almost verbatim — just rehearse it as a general framework too, not only your specific case |
| Safety/alignment framing | RLHF pipeline (SFT → reward model → PPO), DPO vs PPO, why DPO has become more common, eval-faking and oversight risk | Increasingly common at AI labs specifically — this is also where your self-blindness research is a genuine differentiator, not just a nice-to-have |

**Real company-reported system design prompts worth mock-practicing directly (sourced from 2026 candidate reports):**
- Design an end-to-end batching system for LLM queries, including request orchestration (Anthropic)
- Design a GPU credit/resource management system (OpenAI)
- Design a RAG system that handles conflicting information across sources, with cost control (Scale AI–style)
- Design a multi-tenant AI chatbot platform where each customer gets a customized deployment
- Design an AI-powered anomaly detection system for cloud infrastructure — this one maps almost directly onto your DASH SIEM/monitoring work

**How to sequence this within the 8 weeks:** fold this track into your existing ML-infra HLD days (Mon/Wed/Fri) rather than adding a sixth parallel track — by week 3-4 start picking one of the "real company prompts" above per week and doing a full 35-40 min design write-up, same format as your HLD reps.
| Topic | What to actually know | Resource |
|---|---|---|
| Design patterns vocabulary | Strategy, Factory, Singleton, Observer, Decorator, Builder — when to use each, not just definitions | *Head First Design Patterns* |
| SOLID principles | Be able to critique a design against SOLID out loud during the interview | *Head First Design Patterns* intro chapters + Educative's OOD course |
| Easy OOD problems | Parking lot, vending machine, library management | Educative's "Grokking the OOD Interview" |
| Medium OOD problems | Elevator system, tic-tac-toe/chess, rate limiter as a class hierarchy | Educative's "Grokking the OOD Interview" + LeetCode OOD-tagged problems |
| Harder OOD problems | Chess engine with move validation, ride-sharing matching logic, in-memory cache with eviction as a class design | Educative's "Grokking the OOD Interview" (advanced section) |
| API design | Designing clean interfaces (REST or RPC-style), versioning, idempotency | *System Design Interview* Vol 2 (short API chapter); otherwise practice from real API design you've done at DASH |

## Weekly Tracking (add to this weekly)
- [ ] LeetCode count + weak patterns
- [ ] System design write-ups completed
- [ ] Applications sent / responses / interviews scheduled
- [ ] Mock interviews done + feedback themes
