# NeuroSQL_version2
# NeuroSQL: Reflex-Driven Neuro-Symbolic SQL Engine

NeuroSQL is a modular SQL generation and execution engine that converts natural language questions into executable SQLite queries. It utilizes a multi-agent architecture to handle query parsing, schema mapping, and self-correction.

## 🛠 System Architecture

The project follows a **Neuro-Symbolic** approach: it uses pattern-matching (NLU) to extract intent and symbolic logic (SQL Planner) to construct valid relational queries.

### Agent Workflow

1. **NLUAgent**: Parses user input using regex to detect tables, columns, aggregations, and `WHERE` conditions.
2. **SchemaAgent**: Introspects the SQLite database to retrieve table structures and foreign key relationships.
3. **SQLPlannerAgent**: Determines if a single-table or two-table join is required and builds the SQL string.
4. **ReflexAgent**: Provides a self-correction layer to fix common SQL errors (e.g., column name aliasing).
5. **ExecutionAgent**: Executes the final SQL and renders results using the `rich` library.
6. **ExplanationAgent**: Translates the generated SQL and internal intent back into a human-readable summary, detailing which filters and aggregations were applied.
7. **Orchestrator**: The central hub that manages data flow between agents and enforces a safety layer.



## 🚀 Features

* **Multi-Table Joins**: Automatically detects foreign key relationships to perform `JOIN` operations between two tables.
* **Automatic Schema Correction**: The `ReflexAgent` can identify specific column errors and re-map them (e.g., `score` to `marks`).
* **Safety Layer**: A dedicated `sql_safety.py` module prevents SQL injection by blocking multi-statement queries and validating table/column existence against the schema.
* **Dynamic CSV Import**: Supports importing external CSV files as new database tables via terminal commands.
* **Pretty Printing**: Uses the `Rich` console library to display schemas and query results in formatted tables.

---

## 📁 Project Structure

```text
.
├── agents/
│   ├── execution_agent.py    # Database execution and formatting
│   ├── explanation_agent.py  # Generates natural language explanations
│   ├── nlu_agent.py          # Regex-based intent extraction
│   ├── reflex_agent.py       # Error correction logic
│   ├── schema_agent.py       # Database introspection
│   └── sql_planner_agent.py  # Symbolic SQL generation logic
├── core/
│   ├── orchestrator.py       # Central pipeline management
│   └── sql_safety.py         # SQL validation and safety checks
├── evaluation/
│   ├── evaluator.py          # Benchmarking script (Spider Dataset)
│   ├── test_cases.py         # Gold-standard SQL test cases
│   └── plot_results.py       # Visualization of accuracy metrics
├── data/
│   └── init_db.py            # Local database initialization
├── main.py                   # Interactive CLI entry point
└── requirements.txt          # Project dependencies

```

---

## 📊 Evaluation & Metrics

The system includes an evaluation suite to measure performance against a gold-standard set of queries (based on the Spider Dataset structure).

* **Exact SQL Match**: Compares generated SQL against ground truth.
* **Execution Success**: Verifies if the SQL runs without errors and returns data.
* **Reflex Usage Rate**: Tracks how often the system requires self-correction.

---

## ⚙️ Installation & Usage

### 1. Requirements

Ensure you have Python 3.8+ installed. Install dependencies:

```bash
pip install -r requirements.txt

```

### 2. Initialize the Database

Before running the engine, create the sample database:

```bash
python data/init_db.py

```

### 3. Run NeuroSQL

Start the interactive CLI:

```bash
python main.py

```

### 4. Running Benchmarks

To evaluate the system's accuracy:

```bash
python evaluation/evaluator.py

```

---
