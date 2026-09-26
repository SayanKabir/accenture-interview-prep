# Accenture Technical Interview — Comprehensive Prep Notes
### Sayan Kabir | M.Tech CSE, KIIT | Compiled from resume + Q&A bank

This covers every technical thread your resume and answer bank touch — Python/pandas/NumPy automation, Flask + SQLAlchemy, SQL/DBMS, CNN + TensorFlow, LLM chatbot integration, AES encryption + biometrics, Flutter + Provider/BLoC, OOP, OS, DSA, REST APIs, Docker, Git, CI/CD, and design patterns — with concise explanations plus the specific follow-ups an interviewer is likely to fire based on *your* projects.

---

## 1. Python, Pandas & NumPy (KPIT automation scripts)

**What to say if asked to summarize:** At KPIT you wrote Python scripts using pandas/NumPy to automate repetitive data-handling tasks — reading raw data (CSV/DB exports), cleaning it, transforming it, and producing reports, replacing manual Excel work.

| Concept | Explanation |
|---|---|
| **pandas DataFrame vs Series** | DataFrame = 2D labeled table (rows+columns); Series = 1D labeled array (a single column). DataFrame is essentially a dict of Series. |
| **`loc` vs `iloc`** | `loc` selects by label/index name; `iloc` selects by integer position. `df.loc['a':'c']` is inclusive of both ends; `df.iloc[0:3]` is exclusive of the end (Python slicing rules). |
| **Vectorization** | NumPy/pandas operations run in compiled C loops instead of Python `for` loops — much faster. E.g., `df['x'] * 2` beats iterating row by row. |
| **Handling missing data** | `dropna()`, `fillna()`, `isnull().sum()` to audit. Interviewers may ask when to drop vs impute — mention: drop if missingness is small/random, impute (mean/median/forward-fill) if the column is important and data is scarce. |
| **`apply` vs vectorized ops** | `apply()` runs a Python function per row/column — flexible but slower than vectorized NumPy ops. Use vectorized first; `apply` only when logic can't be vectorized. |
| **`merge` vs `concat` vs `join`** | `merge` = SQL-style join on keys/columns; `concat` = stack DataFrames along an axis (rows or columns); `join` = merge on index by default. |
| **GroupBy** | Split-apply-combine: `df.groupby('col').agg(...)` — splits data into groups, applies a function (sum/mean/count), combines results. Core to any "automate a report" answer. |
| **NumPy arrays vs Python lists** | NumPy arrays are homogeneous, contiguous in memory, support vectorized math and broadcasting; Python lists are heterogeneous and slower for numeric work. |
| **Broadcasting** | NumPy's rule for applying operations between arrays of different shapes without explicit loops (e.g., adding a scalar to a whole array, or a (3,) array to a (3,4) matrix). |

**Likely interviewer questions (with how to answer using your KPIT context):**
- *"Walk me through one automation script you built."* → Pick one: e.g. "reads a raw CSV export, cleans nulls/duplicates with pandas, aggregates with groupby, writes a summary Excel/CSV, previously done manually — saved X hours."
- *"How did you handle bad/missing data in your automation scripts?"* → Explain your actual approach: validation checks, `fillna`/`dropna`, logging anomalies rather than silently failing.
- *"Why pandas and not writing raw Python loops or using Excel formulas?"* → Vectorization = speed, and pandas gives cleaner group/merge/pivot operations than manual loops.
- *"Have you profiled or optimized a slow pandas script?"* → If yes, mention avoiding `apply` in favor of vectorized ops, using appropriate dtypes (e.g. `category` for strings), chunked reading (`read_csv(chunksize=...)`) for large files.

---

## 2. Flask & SQLAlchemy (KPIT internal tool)

**What to say if asked to summarize:** You built a Flask-based internal web tool with SQLAlchemy as the ORM for database integration — a typical thin backend serving internal data to a small set of users.

| Concept | Explanation |
|---|---|
| **What is Flask** | A lightweight (micro) Python web framework — unlike Django, it doesn't force a specific project structure or bundle an ORM/admin panel; you add what you need (Flask-SQLAlchemy, Flask-RESTful, etc.). |
| **What is an ORM / why SQLAlchemy** | Object-Relational Mapper — lets you interact with the DB using Python classes/objects instead of raw SQL strings. SQLAlchemy maps a Python class to a table; each instance = a row. Benefits: less SQL injection risk (parameterized queries by default), portability across DB engines, cleaner code. |
| **Flask routing** | `@app.route('/path', methods=['GET','POST'])` maps URLs to Python view functions. |
| **Flask app factory / Blueprints** | For bigger apps: `create_app()` factory pattern + Blueprints to modularize routes instead of one giant `app.py`. Good to mention even if your KPIT tool was simple — shows you know how it scales. |
| **SQLAlchemy Session** | The "unit of work" — tracks objects, and `session.add()` / `session.commit()` / `session.rollback()` control when changes actually hit the DB (transaction management). |
| **Migrations** | Alembic (often via Flask-Migrate) handles schema versioning/migrations when models change — worth mentioning even briefly. |
| **Request/response cycle** | Client sends HTTP request → Flask routes it to a view function → function queries DB via SQLAlchemy models → returns JSON/HTML → Flask sends HTTP response. |
| **Flask vs Django** | Flask: minimal, unopinionated, you assemble pieces. Django: "batteries included" (ORM, admin, auth built in), more opinionated project structure. Flask suits small internal tools (your case); Django suits larger full-featured apps. |

**Likely interviewer questions:**
- *"How exactly did you use SQLAlchemy — raw SQL or ORM models?"* → Describe defining model classes (`db.Model`), querying with `Model.query.filter_by(...)`, and why that's safer/cleaner than string-concatenated SQL.
- *"How did the Flask app talk to the database — synchronously, connection pooling?"* → SQLAlchemy manages a connection pool under the hood; each request opens a session, does work, commits/closes.
- *"What would you change if this tool needed to scale to 1000 users?"* → Add caching, move to Blueprints, use a production WSGI server (Gunicorn) behind Nginx instead of Flask's dev server, consider async or a heavier framework if I/O-bound.
- *"How did you prevent SQL injection?"* → ORM parameterizes queries automatically; also validate/sanitize any raw input.

---

## 3. SQL, DBMS Fundamentals & Databases Used

**Your DB exposure:** MySQL, SQLite (Passwordzzz), Firebase Firestore (Sugam Krishi), Supabase (listed skill).

| Concept | Explanation |
|---|---|
| **ACID properties** | Atomicity (all-or-nothing transactions), Consistency (DB moves between valid states), Isolation (concurrent transactions don't interfere), Durability (committed data survives crashes). |
| **Normalization** | Organizing tables to reduce redundancy. 1NF: atomic column values, no repeating groups. 2NF: 1NF + no partial dependency on part of a composite key. 3NF: 2NF + no transitive dependency (non-key columns depend only on the key). |
| **Primary key vs Foreign key** | PK uniquely identifies a row in its table; FK is a column referencing a PK in another table, enforcing referential integrity. |
| **Indexing** | A data structure (usually B-Tree) that speeds up lookups on a column at the cost of extra storage and slower writes (index must be updated on insert/update). |
| **JOIN types** | INNER (only matching rows), LEFT/RIGHT (all rows from one side + matches), FULL OUTER (all rows from both, matched where possible). |
| **Transactions** | A sequence of operations treated as a single unit — `BEGIN`, `COMMIT`, `ROLLBACK`. Needed whenever multiple writes must succeed or fail together (e.g., debit one account, credit another). |
| **SQL vs NoSQL** | SQL = structured schema, relations, strong consistency, good for complex queries/joins (MySQL). NoSQL = flexible/schemaless, horizontally scalable, good for high write throughput or unstructured/nested data (Firestore documents). Choose based on whether your data is relational and how much you need horizontal scale vs strict consistency. |
| **Firebase Firestore specifics** | Document-oriented NoSQL, real-time listeners (`onSnapshot`) push live updates to clients — this is *why* you chose it for Sugam Krishi's real-time market data sync, instead of polling a SQL DB. |
| **SQLite specifics (Passwordzzz)** | Serverless, file-based, zero-configuration — ideal for a fully offline, local-first mobile app with no network dependency. Trade-off: not built for concurrent multi-user write access, but that's irrelevant for a single-user local vault. |

**Likely interviewer questions:**
- *"Why Firestore over MySQL for Sugam Krishi?"* → Needed real-time sync across farmer devices without managing polling/websocket infra yourself; Firestore's listeners + Firebase Auth gave that out of the box, and schema flexibility suited evolving product data (crop listings, chat messages).
- *"Why SQLite over Firebase/cloud DB for Passwordzzz?"* → Core design goal was zero data transmission — a password manager should never touch the network. SQLite is embedded/local so nothing leaves the device.
- *"Explain a JOIN query you've written."* → Have one ready (even simple) from KPIT SQL work — e.g., joining a transactions table with a lookup/reference table.
- *"What's database normalization and did you apply it anywhere?"* → Explain 1NF–3NF briefly; relate to any schema you designed (KPIT internal tool tables, or how you avoided data duplication).

---

## 4. CNN & Deep Learning — Crop Disease Detection (Sugam Krishi)

| Concept | Explanation |
|---|---|
| **Why CNN for image classification** | Convolutional layers exploit spatial locality (nearby pixels are related) and use shared weights (filters/kernels), so they need far fewer parameters than a fully-connected network on raw pixels and are translation-invariant. |
| **Core CNN layers** | Convolution (learns local feature filters), Activation (ReLU, adds non-linearity), Pooling (Max/Avg — downsamples, adds spatial invariance, reduces compute), Fully Connected (final classification), Softmax (output probabilities across disease classes). |
| **Training pipeline** | Collect/label leaf images → preprocess (resize, normalize pixel values) → augment (rotate/flip/zoom to fight overfitting, crucial with a small agri dataset) → split train/val/test → train with a loss function (categorical cross-entropy) and optimizer (Adam) → evaluate (accuracy, confusion matrix, precision/recall per disease class). |
| **Transfer learning** | If you used a pretrained backbone (MobileNet/ResNet fine-tuned) rather than training from scratch — mention this if true, since agri image datasets are typically small; transfer learning gives much better accuracy with less data. If you trained from scratch, be ready to justify why (dataset size/domain specificity). |
| **On-device inference** | Converting the trained model to **TensorFlow Lite** (`.tflite`) for mobile deployment — quantization (reducing weight precision, e.g. float32→int8) shrinks model size and speeds up inference on-device, at a small accuracy cost. |
| **Overfitting & how you fought it** | Small agri datasets overfit easily. Mitigations: data augmentation, dropout layers, early stopping, transfer learning, regularization (L2). |
| **Evaluation metrics beyond accuracy** | For imbalanced disease classes, accuracy alone is misleading — precision/recall/F1 and a confusion matrix matter more (a rare disease class could be ignored by the model while accuracy still looks fine). |

**Likely interviewer questions:**
- *"Walk me through how you trained the crop disease model."* → Dataset source (e.g. PlantVillage-style or self-collected), preprocessing, architecture choice, training, and how you validated it before deploying.
- *"Why CNN and not a simpler ML model (e.g. SVM on handcrafted features)?"* → CNNs learn features automatically from raw pixels instead of hand-engineering them — better for complex, high-variance image data like leaf textures/lesions under different lighting.
- *"How did you deploy it for on-device inference — what were the constraints?"* → Model size/latency matter on farmers' (often low-end) phones — mention TFLite conversion/quantization if you did it, or acknowledge it as the "if I rebuilt it" improvement.
- *"How would you improve accuracy today?"* → More/better-labeled data, transfer learning from a stronger pretrained backbone, augmentation tuned to real field conditions (lighting, occlusion), active learning from misclassified farmer-submitted images.

---

## 5. LLM Chatbot Integration (Sugam Krishi)

| Concept | Explanation |
|---|---|
| **What "integrating an LLM chatbot" typically means** | Calling an LLM API (or a wrapper library) with the farmer's natural-language query — possibly with a system prompt scoping it to agriculture — and returning the response through your app's chat UI. |
| **Prompt engineering basics** | System prompt sets persona/scope/constraints; user prompt is the actual query; you may inject context (retrieved crop data) into the prompt for grounding. |
| **RAG (Retrieval-Augmented Generation)** | If you grounded chatbot answers in your own crop/market data rather than relying purely on the LLM's general knowledge, that's RAG — retrieve relevant docs/data, feed them into the prompt alongside the question. Mention if applicable; otherwise be honest it was a simpler direct-API integration. |
| **Handling hallucination/reliability** | For a farming advice bot, wrong answers have real consequences — mention any guardrails: scoping the system prompt tightly, falling back to human/expert resources for uncertain queries, or disclaiming "this is general guidance." |
| **Latency/cost tradeoffs** | LLM API calls add latency and cost per query — worth mentioning you'd cache common Q&A or use a smaller/cheaper model for simple queries if scaling up. |

**Likely interviewer questions:**
- *"How did the chatbot understand farmer queries — did you fine-tune a model?"* → Most likely you called an existing LLM API (be honest — this is a normal, respected approach) rather than fine-tuning; explain the integration point in your Flutter/Firebase stack.
- *"What if the LLM gives wrong farming advice?"* → Discuss guardrails/scoping and the reality that any LLM feature for a sensitive domain needs human-reviewable fallbacks.

---

## 6. Encryption & Security — AES-256 + Biometrics (Passwordzzz)

| Concept | Explanation |
|---|---|
| **Symmetric vs Asymmetric encryption** | Symmetric (AES): same key encrypts and decrypts — fast, ideal for encrypting local data you'll decrypt yourself. Asymmetric (RSA): public/private key pair — used for secure key exchange or signatures, slower. Password managers use symmetric encryption (AES) for the vault since only the user needs to decrypt it. |
| **AES-256 basics** | Advanced Encryption Standard, 256-bit key length, block cipher (operates on fixed-size blocks, typically 128 bits). "256" = key size, not block size. |
| **Why AES over something like DES** | DES's 56-bit key is brute-forceable today; AES-256 is the current industry/government standard for strong symmetric encryption. |
| **Mode of operation** | AES needs a mode (e.g., **GCM** or CBC) to handle multi-block data. GCM is preferred in modern apps because it provides both confidentiality *and* integrity/authentication (detects tampering) via an auth tag, whereas plain CBC only gives confidentiality. If asked which mode you used, GCM is the strong, defensible answer for a password vault. |
| **Key derivation** | The AES key itself shouldn't be the user's raw master password — it should be derived via a KDF like **PBKDF2**, **bcrypt**, or **Argon2** (adds salt + many iterations, making brute-force/rainbow-table attacks impractical). Mention this even if implementation detail — shows you understand *why* "just AES-256" isn't the whole story. |
| **Salting** | A random value added per-user/per-password before hashing/deriving keys, so identical passwords don't produce identical hashes/keys — defeats precomputed rainbow-table attacks. |
| **Biometric authentication (fingerprint)** | On mobile, biometric auth is handled by the OS's secure hardware (Secure Enclave/TEE, Android Keystore) — the app never actually "sees" the fingerprint data; it just receives a yes/no from the OS's biometric API (e.g., Android `BiometricPrompt`) after which it's allowed to unlock/decrypt the local vault key. |
| **Local-first / zero data transmission** | The entire point of Passwordzzz's design: no cloud sync, no network calls with credential data — smallest possible attack surface (nothing to intercept in transit, no server breach risk). |

**Likely interviewer questions:**
- *"Where do you store the AES key — hardcoded, derived, or in the keystore?"* → Best answer: key derived from the user's master password via a KDF (PBKDF2/Argon2) with a salt, or stored using Android Keystore-backed encryption — never hardcoded.
- *"What mode of AES did you use, and why does that matter?"* → Explain GCM vs CBC as above — shows depth beyond "AES-256" as a buzzword.
- *"What happens if the user forgets their master password?"* → Honest answer for a true zero-knowledge local design: there's no recovery — that's the security/UX trade-off of local-first, no-cloud-backup encryption. Good to acknowledge as a real limitation/improvement area.
- *"How is biometric auth different from just typing a password?"* → It doesn't replace the encryption key — it's a convenience gate to unlock access to the already-derived key, handled by secure OS hardware rather than app code.

---

## 7. Flutter & State Management (Provider / BLoC)

**Used in:** Sugam Krishi and Passwordzzz (both Flutter apps).

| Concept | Explanation |
|---|---|
| **Why Flutter** | Single codebase (Dart) compiles to native ARM code for both iOS and Android — faster cross-platform development than maintaining two native codebases, with a rich widget system for custom UI. |
| **Widget tree / Stateless vs Stateful widgets** | Flutter UI = a tree of widgets. `StatelessWidget` has no mutable state (rebuilds only when parent rebuilds); `StatefulWidget` holds a `State` object that can call `setState()` to trigger a rebuild. |
| **Why state management beyond `setState`** | `setState` only works within a single widget/small subtree; as an app grows, you need to share state across distant widgets (e.g., cart state, auth state) without prop-drilling — that's what Provider/BLoC solve. |
| **Provider** | A wrapper around Flutter's `InheritedWidget` — exposes a state object to descendant widgets via `context.watch`/`context.read`, and rebuilds only the widgets that listen to it (`Consumer`/`Selector`) when notified via `ChangeNotifier.notifyListeners()`. Simpler, less boilerplate — good default choice. |
| **BLoC (Business Logic Component)** | Stricter, event-driven pattern: UI dispatches **Events** → BLoC processes them and emits **States** → UI rebuilds via `BlocBuilder`. Enforces a clean separation between UI and business logic using Streams internally. More boilerplate but more testable/predictable at scale. |
| **Provider vs BLoC — when to choose which** | Provider: smaller/medium apps, faster to write, less ceremony. BLoC: larger apps, teams, when you want a very explicit unidirectional event→state flow and strong testability. If you used Provider, justify it as "right-sized for the app's scope" — not a knowledge gap. |
| **Async in Flutter** | `Future`/`async`/`await` for one-off async ops (an API call); `Stream`/`StreamBuilder` for continuous data (Firestore's real-time listeners map naturally to Streams — ties directly into Sugam Krishi's real-time sync). |

**Likely interviewer questions:**
- *"Why did you choose Provider (or BLoC) specifically?"* → Team size/project complexity — Provider gave enough structure without BLoC's boilerplate for an app this size; can add BLoC is the app grows.
- *"How does Firestore's real-time data reach your Flutter UI?"* → Firestore streams document changes → wrapped in a `StreamBuilder` (or piped into a Provider/BLoC state) → widget rebuilds automatically on new data.
- *"What's the widget lifecycle?"* → `createState()` → `initState()` (once, setup) → `build()` (called whenever state changes) → `dispose()` (cleanup, e.g. close streams/controllers).

---

## 8. OOP Fundamentals (Four Pillars + Follow-ups)

| Pillar | Explanation | Follow-up traps |
|---|---|---|
| **Encapsulation** | Bundling data + methods that operate on it into one unit (class), restricting direct access to internal state via access modifiers (private fields + public getters/setters). | *Abstraction vs encapsulation:* Abstraction hides **implementation complexity** (what it does, not how); encapsulation hides/protects **internal data** (via access control). They're related but distinct — don't conflate them. |
| **Abstraction** | Exposing only essential features/behavior while hiding internal complexity — e.g., an abstract class/interface defines *what* to do, subclasses define *how*. |
| **Inheritance** | A class (subclass) acquires properties/behavior of another (superclass) — promotes code reuse. Watch for: "favor composition over inheritance" — know this as a real design principle, not just theory. |
| **Polymorphism** | One interface, many implementations. **Compile-time (static)**: method overloading — same method name, different parameter lists. **Runtime (dynamic)**: method overriding — subclass provides its own implementation, resolved at runtime via virtual dispatch. |

**Other common OOP questions:**
- *"What is a constructor / destructor?"* — Constructor initializes an object at creation; destructor (C++) cleans up resources when an object goes out of scope (Python/Dart rely on garbage collection instead of explicit destructors).
- *"Difference between class and object"* — Class = blueprint/template; object = a concrete instance of that blueprint with actual data.
- *"What is method overriding vs overloading?"* — Overriding = subclass redefines a parent method (same signature, runtime polymorphism); overloading = same method name, different parameters (compile-time, and not supported the same way in Python/Dart as in C++/Java — worth knowing this language nuance since you list C++ *and* Python/Dart).
- *"What are interfaces / abstract classes, and the difference?"* — Abstract class can have some implemented methods + state; interface (pure abstract) only declares method signatures, no implementation (Dart: any class can implicitly be used as an interface; C++ simulates interfaces via pure virtual functions).
- *"Composition vs inheritance"* — Composition ("has-a") builds objects out of other objects instead of inheriting ("is-a"); generally preferred for flexibility and avoiding deep, fragile inheritance hierarchies.

---

## 9. Operating Systems Fundamentals

| Concept | Explanation |
|---|---|
| **Process vs Thread** | Process = independent program in execution with its own memory space; Thread = a lighter-weight unit of execution *within* a process, sharing that process's memory. Threads are cheaper to create/switch but risk race conditions since they share memory. |
| **Multithreading vs Multiprocessing** | Multithreading: concurrency within one process (shared memory, risk of races, needs locks). Multiprocessing: true parallelism across processes (separate memory, safer but heavier, needs IPC to communicate). |
| **Deadlock — 4 necessary conditions** | Mutual exclusion, Hold-and-wait, No preemption, Circular wait. All four must hold simultaneously for deadlock; prevention strategies break at least one (e.g., resource ordering breaks circular wait). |
| **CPU Scheduling algorithms** | FCFS (simple, can cause convoy effect), SJF (optimal average wait time but needs burst-time prediction), Round Robin (time-sliced, fair, good for interactive systems), Priority Scheduling (risk of starvation, mitigated by aging). |
| **Paging vs Segmentation** | Paging: physical memory split into fixed-size frames, logical memory into same-size pages — avoids external fragmentation but can have internal fragmentation. Segmentation: variable-sized logical units (code, stack, heap) — matches program structure better but risks external fragmentation. |
| **Virtual memory** | Abstraction giving each process its own large, contiguous address space, backed by a mix of physical RAM and disk (swap) via page tables — enables running programs larger than physical RAM and isolates processes from each other. |
| **Race condition & Critical section** | Race condition: outcome depends on timing/interleaving of concurrent operations on shared data. Critical section: the code segment accessing shared resources, protected via mutexes/semaphores to ensure mutual exclusion. |
| **Semaphore vs Mutex** | Mutex: binary lock, owned by the thread that locked it (only that thread can unlock). Semaphore: a counter allowing N threads to access a resource concurrently, not tied to ownership — used for signaling between threads, not just mutual exclusion. |

**Likely interviewer angle:** These are usually asked as generic CS-fundamentals questions, not tied to your projects — just be crisp and give an example (e.g., "a race condition would be two threads incrementing a shared counter without a lock").

---

## 10. Data Structures & Algorithms

| Topic | What to know |
|---|---|
| **Time/space complexity (Big-O)** | Be able to state Big-O for basic operations on Array, Linked List, Stack, Queue, Hash Map, BST — e.g., array access O(1), search O(n); hash map average O(1) insert/lookup, worst-case O(n) on collisions. |
| **Array vs Linked List** | Array: contiguous memory, O(1) random access, O(n) insert/delete (shifting). Linked List: non-contiguous, O(1) insert/delete at a known node, O(n) access (traverse). |
| **Stack vs Queue** | Stack: LIFO (push/pop from same end) — used in recursion/call stack, undo functionality, DFS. Queue: FIFO — used in scheduling, BFS. |
| **Hashing** | Maps keys to array indices via a hash function; collisions handled via chaining (linked lists per bucket) or open addressing (probing). Underlies Python dict / Dart Map / hash-based sets. |
| **Trees** | BST: left < node < right, O(log n) average search/insert (O(n) worst case if unbalanced). Balanced trees (AVL, Red-Black) guarantee O(log n) by rebalancing. |
| **Graph traversal** | BFS (queue-based, level-by-level, shortest path in unweighted graphs), DFS (stack/recursion-based, explores depth-first, good for cycle detection/topological sort). |
| **Sorting** | Know at least: Merge Sort (O(n log n), stable, needs extra space), Quick Sort (O(n log n) average, O(n²) worst, in-place), and when you'd pick one over the other (stability requirement, memory constraints). |
| **Recursion & recursion vs iteration** | Recursion trades stack space for simpler code on problems with natural recursive structure (trees, divide-and-conquer); risk of stack overflow on deep recursion without tail-call optimization (which Python/Dart don't reliably provide). |

**Prep tip:** They likely won't do a full DSA round for this drive given the profile, but expect 1–2 rapid-fire questions (e.g., "what's the time complexity of binary search," "difference between array and linked list") mixed into the technical round.

---

## 11. REST APIs & Web Architecture

| Concept | Explanation |
|---|---|
| **What is a REST API** | An architectural style for networked apps: stateless client-server communication over HTTP, resources identified by URLs, manipulated via standard HTTP methods. |
| **HTTP methods** | GET (read, safe/idempotent), POST (create, not idempotent), PUT (full update, idempotent), PATCH (partial update), DELETE (remove, idempotent). |
| **Statelessness** | Each request contains all the information the server needs — server holds no client session state between requests (any session/auth token is sent with each request, e.g. JWT in headers). |
| **Status codes** | 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 500 Internal Server Error — be ready to explain 401 vs 403 (401 = not authenticated; 403 = authenticated but not permitted). |
| **REST vs GraphQL (bonus)** | REST: fixed endpoints, can over/under-fetch data. GraphQL: single endpoint, client specifies exactly the fields it needs. Mention only if pushed further. |
| **Idempotency** | An idempotent operation produces the same result no matter how many times it's repeated (GET, PUT, DELETE) — important for retry-safe network calls, e.g. in your mobile apps' API calls. |

**Tie to your work:** Your Flask backend at KPIT and Sugam Krishi's Firebase functions both expose(d) endpoints/APIs consumed by a client — be ready to describe one concrete request/response flow end to end.

---

## 12. Docker & Containerization

| Concept | Explanation |
|---|---|
| **What is Docker / why used** | Packages an application with all its dependencies (libraries, runtime, OS-level tools) into a single portable **image**, run as a **container** — solves "works on my machine" by guaranteeing the same environment everywhere (dev, test, prod). |
| **Image vs Container** | Image = a read-only template/blueprint (built from a `Dockerfile`); Container = a running (or stopped) instance of that image. |
| **Dockerfile basics** | A script of instructions (`FROM`, `COPY`, `RUN`, `CMD`) that defines how to build an image layer by layer. |
| **VM vs Container** | VMs virtualize an entire OS (heavier, own kernel each); containers share the host OS kernel and only isolate the process/filesystem — much lighter and faster to start. |
| **`docker-compose`** | Defines and runs multi-container applications (e.g., a Flask app + its database) with one YAML file/command, useful for local dev environments mirroring production. |

**Likely question:** *"Did you containerize anything?"* — If you used Docker mainly for a consistent dev environment or a small deployment, describe that honestly; if it was just exploratory/coursework, say so and pivot to *why* it matters (reproducibility, easier onboarding, consistent envs across dev/staging/prod).

---

## 13. Git / GitHub & Version Control (KPIT collaboration)

| Concept | Explanation |
|---|---|
| **Branching workflow** | Feature branches off `main`/`develop`, merged via Pull Requests after review — this is almost certainly how you collaborated at KPIT; be ready to describe it concretely. |
| **Merge vs Rebase** | Merge preserves full history with a merge commit (non-destructive); Rebase replays your commits on top of the target branch for a linear history (rewrites commit history — avoid rebasing shared/pushed branches). |
| **Merge conflicts** | Occur when the same lines are changed differently on two branches — Git can't auto-resolve; you manually pick the correct content and commit. Have a real example ready if you hit one at KPIT. |
| **`git pull` vs `git fetch`** | `fetch` downloads remote changes without merging into your working branch; `pull` = `fetch` + `merge` (or rebase) automatically. |
| **Code review practices** | PR review, comments, required approvals before merge — mention this since your resume explicitly says "collaborated through shared codebases, branching, and code review practices." |

---

## 14. CI/CD & DevOps (IBM DevOps Certificate)

| Concept | Explanation |
|---|---|
| **CI (Continuous Integration)** | Developers frequently merge code into a shared repo; each merge triggers automated builds + tests to catch integration issues early. |
| **CD (Continuous Delivery/Deployment)** | Delivery: code is automatically prepared/tested for release but a human triggers the actual deploy. Deployment: fully automated — every passing change goes straight to production. |
| **Typical pipeline stages** | Source (commit trigger) → Build → Test (unit/integration) → Deploy (staging → production), often with a manual approval gate before prod. |
| **Common tools** | GitHub Actions, Jenkins, GitLab CI, CircleCI — know the concept even if you haven't used a specific one hands-on; the IBM cert gives you enough vocabulary to speak generally. |
| **Why CI/CD matters** | Catches bugs earlier (fail fast), reduces manual deployment error, enables frequent small releases instead of risky big-bang deployments. |
| **DevOps culture, not just tools** | Emphasize it's also about breaking down the wall between Dev and Ops — shared responsibility for reliability, monitoring, fast feedback loops (mention if they probe beyond tooling). |

**Honest framing:** Since this is certificate knowledge rather than hands-on project experience, answer conceptually and clearly rather than overclaiming hands-on pipeline-building — interviewers respect "I understand the concepts and pipeline stages from my IBM DevOps certification; I haven't built a production CI/CD pipeline myself yet, but I'm comfortable with the ideas and tools like GitHub Actions."

---

## 15. Design Patterns (commonly asked, not explicitly on your resume — good to have ready)

| Pattern | Category | One-line explanation |
|---|---|---|
| **Singleton** | Creational | Ensures a class has only one instance and provides a global access point to it (e.g., a single DB connection manager). |
| **Factory** | Creational | Delegates object creation to a factory method/class instead of calling constructors directly — decouples client code from concrete classes. |
| **Observer** | Behavioral | An object (subject) maintains a list of dependents (observers) and notifies them automatically on state changes — this is *exactly* the pattern behind `ChangeNotifier`/Provider in Flutter and Firestore's real-time listeners, so tie it back to your own projects. |
| **Builder** | Creational | Constructs a complex object step by step, separating construction from representation. |
| **MVC / MVVM** | Architectural | Separates concerns: Model (data), View (UI), Controller/ViewModel (logic connecting them) — BLoC is essentially an MVVM-flavored pattern for Flutter. |
| **Strategy** | Behavioral | Encapsulates interchangeable algorithms behind a common interface, selectable at runtime (e.g., swapping your password-strength scoring logic without touching calling code). |

**Best move if asked "which design patterns have you used?"**: Point to the Observer pattern underlying Provider/`ChangeNotifier` in your Flutter apps and Firestore's listener model — genuine, defensible, ties theory to your real code instead of reciting textbook definitions.

---

## 16. Quick-Fire Round (rapid one-liners to have loaded)

- **Compiler vs Interpreter:** Compiler translates the whole program to machine code before execution (C++); interpreter executes line by line (Python).
- **Static vs Dynamic typing:** Static = types checked at compile time (C++); Dynamic = checked at runtime (Python, Dart with `var`/inference still type-safe but flexible).
- **Pass by value vs reference:** Value copies the data; reference passes a pointer/reference to the same memory — mutating it affects the original.
- **Exception handling:** `try/except` (Python) / `try/catch` (Dart, C++) — catch and handle runtime errors gracefully instead of crashing.
- **API vs SDK:** API = a contract/interface to interact with a service; SDK = a full toolkit (libraries, docs, sometimes APIs) to build on a platform.
- **Cache:** Fast, small storage layer holding frequently accessed data to avoid recomputation/re-fetching — relevant if asked how you'd speed up the Sugam Krishi chatbot or backend.
- **Load balancer:** Distributes incoming traffic across multiple servers for reliability/scale — good to mention if asked "how would you scale this app to a million users."

---

## How to Use These Notes
1. Read each section once, then try explaining it out loud in 30–60 seconds without looking — that's roughly the length of a good spoken answer.
2. For every project-specific section (2, 4, 5, 6, 7, 9), always be ready with the **"why did you choose X"** framing — Accenture interviewers probe design decisions on your actual projects far more than they quiz raw theory.
3. Where a section says "be honest" (CI/CD, chatbot fine-tuning) — don't overclaim. A clear, self-aware answer beats a shaky confident-sounding one.
