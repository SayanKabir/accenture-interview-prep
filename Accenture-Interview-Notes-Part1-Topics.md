# Accenture Interview — Part 1: Quick Theory Notes

> Skim before the interview. All your verbatim answers are in Part 2.

---

## Roles & Compensation

| Role | CTC (Tier-1) | CTC (Tier-2) | Bonus |
|------|-------------|-------------|-------|
| **ASE** | ₹4,64,580 | ₹4,19,900 | ₹25K |
| **AASE** | ₹6,51,340 | ₹5,90,700 | ₹50K |
| **AAE** | ₹11,18,800 | — | ₹1L |

Tier-1: Bangalore, Mumbai, Hyderabad, Gurgaon, Noida, Chennai, Pune, Kolkata
Tier-2: Indore, Jaipur, Nagpur, Bhubaneswar, Ahmedabad, Coimbatore, Thiruvananthapuram

---

## OOP — Four Pillars

| Pillar | What It Does | How It's Done |
|--------|-------------|---------------|
| **Encapsulation** | Bundles data + methods, restricts direct access to internals | Private fields, public getters/setters |
| **Abstraction** | Hides implementation, exposes only "what" not "how" | Abstract classes, interfaces |
| **Inheritance** | Child class reuses parent's properties + behavior | `extends` / `implements` |
| **Polymorphism** | Same method name, different behavior per object | Method overriding (runtime), overloading (compile-time) |

### Abstraction vs Encapsulation
- **Abstraction** = hiding complexity (design-level). *Steering wheel turns car — you don't need to know the combustion cycle.*
- **Encapsulation** = hiding data (implementation-level). *Engine under sealed hood — can't touch fuel injectors directly.*

### Other OOP Concepts
- **Class vs Object:** Class = blueprint, Object = instance.
- **Constructor:** Special method called when object is created. Initializes state.
- **Access Modifiers:** `private` (class only), `protected` (class + subclasses), `public` (everywhere).
- **Abstract class vs Interface:** Abstract class can have implementation + state; interface is pure contract (all abstract methods). A class can implement multiple interfaces but extend only one class.
- **Method Overloading:** Same name, different parameters (compile-time polymorphism).
- **Method Overriding:** Subclass redefines parent method (runtime polymorphism).
- **`this` vs `super`:** `this` = current object, `super` = parent class.
- **Static:** Belongs to the class, not to instances. Shared across all objects.

---

## Data Structures & Algorithms

### Data Structures at a Glance

| Structure | Access | Search | Insert/Delete | Notes |
|-----------|--------|--------|---------------|-------|
| **Array** | O(1) | O(n) | O(n) | Contiguous memory, fixed size |
| **Linked List** | O(n) | O(n) | O(1) at known pos | Dynamic size, pointer-based |
| **Stack** | O(n) | O(n) | O(1) push/pop | LIFO — DFS, parsing, undo |
| **Queue** | O(n) | O(n) | O(1) enqueue/dequeue | FIFO — BFS, scheduling |
| **HashMap/Set** | — | O(1) avg | O(1) avg | Hashing — frequency, dedup, two-sum |
| **BST** | O(log n) | O(log n) | O(log n) | When balanced; degrades to O(n) if skewed |
| **Heap** | O(1) peek | O(n) | O(log n) | Priority queue — top-K, median |
| **Graph** | — | — | — | BFS O(V+E), DFS O(V+E) |

### Algorithm Patterns
- **Sorting:** QuickSort O(n log n) avg, MergeSort O(n log n) guaranteed, stable
- **Binary Search:** Sorted array, O(log n)
- **Two Pointers:** Sorted arrays, pair sums, palindrome checks
- **Sliding Window:** Subarray/substring problems with fixed or variable window
- **Recursion & Backtracking:** Permutations, combinations, N-queens, Sudoku
- **Dynamic Programming:** Overlapping subproblems + optimal substructure (Fibonacci, knapsack, LCS)
- **Greedy:** Locally optimal → globally optimal (activity selection, Huffman)
- **BFS/DFS:** Graph traversal, shortest path (BFS for unweighted), connected components

### Time Complexity Cheat Sheet
| O(1) | O(log n) | O(n) | O(n log n) | O(n²) | O(2ⁿ) |
|------|----------|------|------------|-------|--------|
| Hash lookup | Binary search | Linear scan | Good sorts | Nested loops | Brute-force subsets |

---

## SQL & Databases

### SQL Fundamentals
- **DDL:** `CREATE`, `ALTER`, `DROP`, `TRUNCATE` — defines structure
- **DML:** `SELECT`, `INSERT`, `UPDATE`, `DELETE` — manipulates data
- **DCL:** `GRANT`, `REVOKE` — permissions

### Key Concepts
- **Primary Key:** Uniquely identifies a row. Not null, unique.
- **Foreign Key:** References primary key of another table. Enforces referential integrity.
- **Normalization:** Reducing redundancy. 1NF (atomic values) → 2NF (no partial dependency) → 3NF (no transitive dependency).
- **Index:** Data structure to speed up searches. B-tree default. Trade-off: faster reads, slower writes.
- **JOIN types:** `INNER` (matching rows), `LEFT` (all from left + matching right), `RIGHT` (all from right), `FULL OUTER` (all rows), `CROSS` (cartesian product).
- **GROUP BY + HAVING:** Aggregate rows, filter groups. `WHERE` filters rows before grouping, `HAVING` filters after.
- **Subquery:** Query inside another query. Can be in `SELECT`, `FROM`, or `WHERE`.
- **ACID:** Atomicity (all or nothing), Consistency (valid state), Isolation (concurrent txns don't interfere), Durability (committed = permanent).
- **Views:** Virtual table from a stored query. Simplifies complex queries.
- **Transactions:** `BEGIN`, `COMMIT`, `ROLLBACK`. Ensure data integrity.

### SQL vs NoSQL

| Aspect | SQL | NoSQL |
|--------|-----|-------|
| **Schema** | Fixed, predefined | Flexible, schema-less |
| **Scaling** | Vertical | Horizontal |
| **Consistency** | Strong (ACID) | Eventually consistent (usually) |
| **Joins** | Native support | Denormalization, embedded docs |
| **Best for** | Structured data, complex queries | Semi-structured, real-time, high volume |
| **Examples** | MySQL, PostgreSQL, SQLite | MongoDB, Firestore, Redis, Cassandra |

---

## REST APIs

- **REST** = Representational State Transfer — architectural style for web APIs
- **Stateless:** Every request carries all info needed; server holds no client state
- **Resources** identified by URLs: `/api/users/123`
- **HTTP Methods → CRUD:**

| Method | Action | Idempotent? |
|--------|--------|-------------|
| `GET` | Read | Yes |
| `POST` | Create | No |
| `PUT` | Update (full) | Yes |
| `PATCH` | Update (partial) | No |
| `DELETE` | Delete | Yes |

- **Status Codes:** 200 OK, 201 Created, 400 Bad Request, 401 Unauthorized, 404 Not Found, 500 Server Error
- **Data format:** JSON (mostly)
- **Authentication:** API keys, OAuth, JWT tokens

---

## Docker & Containers

| Concept | What It Is |
|---------|-----------|
| **Dockerfile** | Script to build an image (base OS, deps, code, CMD) |
| **Image** | Read-only template/snapshot of app + environment |
| **Container** | Running instance of an image, isolated from host |
| **Docker Compose** | Define & run multi-container apps (YAML config) |

### Container vs VM
| | Container | VM |
|---|-----------|-----|
| **Size** | MBs | GBs |
| **Startup** | Seconds | Minutes |
| **Isolation** | Process-level (shares host kernel) | Full OS |
| **Use case** | Microservices, CI/CD | Full isolation, different OS |

---

## Flutter & Dart

### Flutter Core
- Google's UI framework — single Dart codebase → Android, iOS, web, desktop
- Renders via own engine (Skia), NOT native components (unlike React Native)
- Everything is a **Widget** → compose into a widget tree
- **StatelessWidget:** Immutable, no internal state
- **StatefulWidget:** Has mutable state, calls `setState()` to rebuild

### State Management

| Approach | How | Best For |
|----------|-----|----------|
| **setState** | Local, simplest | Single widget state |
| **Provider** | `ChangeNotifier` + `notifyListeners()` → `Consumer` rebuilds | Small–medium apps |
| **Riverpod** | Compile-safe, no context dependency, better testing | Medium–large apps |
| **Bloc** | Events → Bloc → States. Strict separation of UI/logic | Large, testable apps |

### Dart Key Features
- Strongly typed, sound null safety (`?`, `!`, `late`)
- `async`/`await` + `Future`/`Stream` for async programming
- Single-threaded with event loop (like JS); `Isolate` for CPU-heavy work
- `final` (set once) vs `const` (compile-time constant)

---

## Firebase

- **Firebase Auth:** Email/password, Google, phone, anonymous sign-in. Handles sessions.
- **Firestore:** NoSQL document database. Collections → Documents → Fields. Real-time listeners. Offline persistence built-in.
- **Firebase Storage:** File uploads (images, videos).
- **Cloud Functions:** Serverless backend logic triggered by events.
- **Real-time listeners:** Subscribe to changes → UI updates automatically. Use judiciously (costs reads).

---

## Machine Learning Basics (for CNN/TFLite questions)

- **CNN (Convolutional Neural Network):** Specialized for image data. Conv layers extract features → Pooling reduces dimensions → Fully connected layers classify.
- **Transfer Learning:** Use a pre-trained model (MobileNet, EfficientNet, ResNet) as base, fine-tune on your dataset. Faster, better accuracy with less data.
- **TensorFlow Lite:** Optimized runtime for on-device ML inference (mobile, IoT). Convert TF model → `.tflite` → runs locally without internet.
- **Quantization:** Reduces model size by converting weights from float32 to int8. Some accuracy loss, major size reduction.
- **Data Augmentation:** Artificially expand training data (rotation, flip, brightness, crop) to improve generalization.
- **Overfitting:** Model memorizes training data, fails on new data. Fix with: more data, augmentation, dropout, regularization, early stopping.

---

## Encryption & Security

- **Symmetric encryption (AES):** Same key encrypts and decrypts. Fast. Good when sender = receiver (local apps).
- **Asymmetric encryption (RSA):** Public key encrypts, private key decrypts. Slower. Good for communication between parties.
- **AES-256:** 256-bit key, military-grade. Hardware-accelerated on modern devices.
- **Hashing (SHA-256, bcrypt):** One-way. Used for password storage, NOT for encryption (can't reverse).
- **SQL Injection:** Malicious SQL in user input. Prevented by **parameterized queries** / prepared statements.
- **Android Keystore:** Hardware-backed secure storage for cryptographic keys on Android.
- **Biometric Auth:** Uses device APIs (fingerprint, face). NOT the key itself — just gates access to the key.

---

## Git & Version Control

- **Repository:** Project folder tracked by Git.
- **Staging → Commit → Push:** `git add` → `git commit` → `git push`
- **Branching:** `git branch feature/x` → `git checkout feature/x` → work → merge/PR
- **Merge vs Rebase:** Merge creates a merge commit (preserves history). Rebase replays commits on top (linear history).
- **Pull Request (PR):** Propose changes, get code review, then merge.
- **Conflicts:** When two branches modify the same lines. Resolve manually.
- **`.gitignore`:** Files/folders Git should not track (build artifacts, secrets, node_modules).

---

## Software Engineering Concepts

- **SDLC:** Requirements → Design → Implementation → Testing → Deployment → Maintenance
- **Agile:** Iterative development in sprints (2–4 weeks). Daily standups, sprint reviews, retrospectives.
- **Waterfall:** Sequential phases. Rigid. Suitable for well-defined requirements.
- **CI/CD:** Continuous Integration (auto-build + test on push) + Continuous Deployment (auto-deploy). Tools: GitHub Actions, Jenkins.
- **Testing:** Unit (single function), Integration (modules together), E2E (full workflow). Write tests early.
- **DRY:** Don't Repeat Yourself. Extract shared logic.
- **SOLID Principles:**
  - **S**ingle Responsibility — one class, one job
  - **O**pen/Closed — open for extension, closed for modification
  - **L**iskov Substitution — subtypes must be substitutable for base types
  - **I**nterface Segregation — many small interfaces > one fat interface
  - **D**ependency Inversion — depend on abstractions, not concretions
- **Design Patterns (know at least):**
  - **Singleton:** One instance globally (e.g., database connection)
  - **Factory:** Create objects without specifying exact class
  - **Observer:** One-to-many dependency, notify on state change (pub-sub)
  - **MVC:** Model (data), View (UI), Controller (logic) separation

---

## Operating Systems (if asked)

- **Process vs Thread:** Process = independent program with own memory. Thread = lightweight unit within a process, shares memory.
- **Multithreading:** Multiple threads in one process. Needs synchronization (mutex, semaphore) to avoid race conditions.
- **Deadlock:** Two+ processes each waiting for a resource the other holds. Four conditions: mutual exclusion, hold & wait, no preemption, circular wait.
- **Memory Management:** Stack (function calls, local vars, LIFO) vs Heap (dynamic allocation, manual/GC managed).
- **Virtual Memory:** Uses disk as extension of RAM. Pages swapped in/out.
- **Scheduling:** FCFS, SJF, Round Robin, Priority. Determines which process runs next.

---

## DBMS Concepts (if asked beyond SQL)

- **ER Model:** Entity-Relationship. Entities, attributes, relationships. Cardinality (1:1, 1:M, M:N).
- **Normalization:** 1NF → 2NF → 3NF → BCNF. Reduces redundancy and anomalies.
- **Denormalization:** Intentionally adding redundancy for read performance (common in NoSQL).
- **Transactions & ACID:** Already covered above.
- **Indexing:** B-tree (range queries), Hash (exact match). Clustered (reorders data) vs Non-clustered (separate structure).
- **Concurrency Control:** Locks (shared/exclusive), MVCC (multi-version concurrency control), timestamps.

---

## Python — Core Language

### Data Types & Structures
- **Mutable:** `list`, `dict`, `set`, `bytearray`
- **Immutable:** `int`, `float`, `str`, `tuple`, `frozenset`
- **List vs Tuple:** List is mutable, tuple is immutable (hashable, can be dict key).
- **Dict:** Key-value pairs. O(1) average lookup. Keys must be hashable.
- **Set:** Unordered, unique elements. O(1) membership check.

### Key Concepts
- **List Comprehension:** `[x**2 for x in range(10) if x % 2 == 0]`
- **Lambda:** `lambda x, y: x + y` — anonymous function, single expression.
- **`*args, **kwargs`:** `*args` = variable positional args (tuple), `**kwargs` = variable keyword args (dict).
- **Decorators:** Functions wrapping other functions. `@decorator` syntax. Used for logging, auth, timing.
- **Generators:** `yield` instead of `return`. Lazy evaluation, memory-efficient for large data.
- **Context Managers:** `with open('f.txt') as f:` — auto handles setup/teardown (e.g., closing files).
- **Exception Handling:** `try` → `except` → `else` (no error) → `finally` (always runs).
- **`__init__` vs `__new__`:** `__new__` creates the instance, `__init__` initializes it.
- **`self`:** Reference to the current instance. Explicit in Python (unlike `this` in Java/Dart).
- **GIL (Global Interpreter Lock):** CPython allows only one thread to execute Python bytecode at a time. Use `multiprocessing` for CPU-bound parallelism.

### File I/O (common in scripting)
```python
# Read
with open('data.csv', 'r') as f:
    content = f.read()        # entire file as string
    lines = f.readlines()     # list of lines

# Write
with open('output.txt', 'w') as f:
    f.write('hello\n')

# Append
with open('log.txt', 'a') as f:
    f.write('new entry\n')
```

### Common Script Patterns
```python
# CSV processing without pandas
import csv
with open('data.csv') as f:
    reader = csv.DictReader(f)
    for row in reader:
        print(row['name'], row['score'])

# Command-line arguments
import sys
filename = sys.argv[1]  # first arg after script name

# JSON handling
import json
with open('config.json') as f:
    data = json.load(f)         # file → dict
json_str = json.dumps(data, indent=2)  # dict → string

# OS operations
import os
os.listdir('.')                 # list directory
os.path.exists('file.txt')     # check existence
os.makedirs('dir/sub', exist_ok=True)  # create dirs
```

---

## pandas

### Core Structures
- **Series:** 1D labeled array (like a column).
- **DataFrame:** 2D labeled table (rows × columns). The workhorse.

### Creating DataFrames
```python
import pandas as pd

df = pd.read_csv('data.csv')           # from CSV
df = pd.DataFrame({'name': ['A','B'], 'score': [90, 85]})  # from dict
```

### Essential Operations
```python
# Inspect
df.head(), df.tail(), df.shape, df.dtypes, df.info(), df.describe()

# Select
df['col']                    # single column (Series)
df[['col1', 'col2']]        # multiple columns (DataFrame)
df.iloc[0]                   # row by integer index
df.loc[0, 'col']             # row by label + column

# Filter
df[df['score'] > 80]
df[(df['score'] > 80) & (df['grade'] == 'A')]

# Add/modify column
df['new_col'] = df['score'] * 2

# Handle missing data
df.isnull().sum()            # count nulls per column
df.dropna()                  # drop rows with nulls
df.fillna(0)                 # replace nulls with 0

# Group & aggregate
df.groupby('department')['salary'].mean()
df.groupby('dept').agg({'salary': 'mean', 'name': 'count'})

# Sort
df.sort_values('score', ascending=False)

# Merge (like SQL JOIN)
pd.merge(df1, df2, on='id', how='inner')  # inner, left, right, outer

# Apply custom function
df['grade'] = df['score'].apply(lambda x: 'Pass' if x >= 50 else 'Fail')

# Pivot table
df.pivot_table(values='sales', index='region', columns='product', aggfunc='sum')

# Export
df.to_csv('output.csv', index=False)
df.to_json('output.json')
```

---

## NumPy

- **ndarray:** N-dimensional array. Faster than Python lists (contiguous memory, C-level ops).
- **Why faster:** Vectorized operations (no Python loops), typed arrays, SIMD optimizations.

### Key Operations
```python
import numpy as np

a = np.array([1, 2, 3, 4])
b = np.zeros((3, 4))         # 3×4 of zeros
c = np.ones((2, 3))          # 2×3 of ones
d = np.arange(0, 10, 2)      # [0, 2, 4, 6, 8]
e = np.linspace(0, 1, 5)     # 5 evenly spaced in [0,1]

# Element-wise ops (vectorized, no loops needed)
a + 10, a * 2, a ** 2, np.sqrt(a)

# Aggregations
a.sum(), a.mean(), a.std(), a.min(), a.max(), a.argmax()

# Reshaping
a.reshape(2, 2)               # 1D → 2×2
a.flatten()                    # any shape → 1D

# Boolean indexing
a[a > 2]                      # elements > 2

# Matrix operations
np.dot(A, B), A @ B           # matrix multiply
A.T                            # transpose
np.linalg.inv(A)              # inverse
```

### pandas + NumPy Together
```python
# NumPy under pandas — df values are NumPy arrays
df['col'].values               # returns np.ndarray
df['normalized'] = (df['score'] - df['score'].mean()) / df['score'].std()
```

---

## matplotlib (for data visualization)

```python
import matplotlib.pyplot as plt

# Line plot
plt.plot(x, y, label='Sales')
plt.xlabel('Month')
plt.ylabel('Revenue')
plt.title('Monthly Sales')
plt.legend()
plt.savefig('chart.png')
plt.show()

# Bar chart
plt.bar(categories, values)

# Scatter plot
plt.scatter(x, y)

# Histogram
plt.hist(data, bins=20)

# Subplots
fig, axes = plt.subplots(1, 2, figsize=(12, 5))
axes[0].plot(x, y1)
axes[1].bar(categories, values)
```

---

## Flask

### What It Is
- Lightweight Python web framework (micro-framework).
- No ORM, form validation, etc. built-in — you add what you need (like SQLAlchemy).
- Good for APIs, internal tools, small–medium apps.

### Core Concepts
```python
from flask import Flask, request, jsonify

app = Flask(__name__)

# Route — maps URL to function
@app.route('/')
def home():
    return 'Hello World'

# Route with parameter
@app.route('/users/<int:user_id>')
def get_user(user_id):
    return jsonify({'id': user_id, 'name': 'Sayan'})

# HTTP methods
@app.route('/api/data', methods=['GET', 'POST'])
def handle_data():
    if request.method == 'POST':
        data = request.get_json()       # parse JSON body
        return jsonify(data), 201
    return jsonify({'items': []})

# Query parameters
@app.route('/search')
def search():
    query = request.args.get('q', '')   # /search?q=hello
    return jsonify({'query': query})

if __name__ == '__main__':
    app.run(debug=True, port=5000)
```

### Key Concepts
- **Route:** URL pattern → Python function. Decorators define them.
- **Request object:** `request.args` (query params), `request.form` (form data), `request.get_json()` (JSON body), `request.method`.
- **Response:** Return string, `jsonify()` for JSON, or `make_response()` for custom headers/status.
- **Templates:** Jinja2 for HTML rendering (`render_template('page.html', data=data)`).
- **Blueprints:** Organize routes into modules for larger apps.
- **Error handling:** `@app.errorhandler(404)` to customize error responses.

---

## SQLAlchemy (ORM)

### What It Is
- Python SQL toolkit + ORM. Maps Python classes → database tables.
- Write Python, not raw SQL (but raw SQL is still available).

### Core Concepts
```python
from flask_sqlalchemy import SQLAlchemy

app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///app.db'
db = SQLAlchemy(app)

# Define a model (= table)
class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    name = db.Column(db.String(100), nullable=False)
    email = db.Column(db.String(120), unique=True)

    def __repr__(self):
        return f'<User {self.name}>'

# Create tables
with app.app_context():
    db.create_all()

# CRUD operations
# Create
new_user = User(name='Sayan', email='sayan@example.com')
db.session.add(new_user)
db.session.commit()

# Read
users = User.query.all()                     # SELECT *
user = User.query.get(1)                      # by primary key
user = User.query.filter_by(name='Sayan').first()

# Update
user.name = 'Sayan Kabir'
db.session.commit()

# Delete
db.session.delete(user)
db.session.commit()
```

### ORM vs Raw SQL
| | ORM (SQLAlchemy) | Raw SQL |
|---|---|---|
| **Pros** | Pythonic, prevents SQL injection, portable across DBs | Full control, complex queries easier |
| **Cons** | Abstraction overhead, complex queries can be clunky | DB-specific, injection risk if not parameterized |

### Key Terms
- **Session:** Unit of work. Tracks changes, commits/rollbacks atomically.
- **Migration:** Schema changes over time. Tools: Alembic (`flask db migrate`).
- **Relationship:** `db.relationship('Post', backref='author')` — ORM-level joins.

---

> **⚠️ Fill before the interview:**
> 1. Your preferred interview language (C++ or Python) and why
> 2. Which LLM API used in Sugam Krishi (OpenAI, Gemini, etc.)
> 3. CNN model architecture details + validation approach
> 4. Whether you've used Bloc in any project
