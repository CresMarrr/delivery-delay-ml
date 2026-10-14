# Delivery Delay ML

Late-delivery prediction models for CresMart: Python, PyTorch, FastAPI, Docker.

**Owner:** Head of Data / Data Scientist, Edwin Sunday UMANA  **Reviewer:** Precious Mojolaoluwa OJO
**Dataset:** Olist Brazilian E-Commerce, https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

**Goal:** predict, at the time an order is placed, whether it will be delivered after its estimated date, and eventually serve that prediction through an API. Phase 1 builds the foundation: data, database, exploration and a clearly defined target. Modelling begins in Phase 2.

---

## Repo structure

| Folder | What goes in it |
| --- | --- |
| `notebooks/` | Exploration and experiment notebooks, numbered |
| `src/` | Reusable Python code (data loading, features, training) |
| `tests/` | Tests for the code in `src/` |
| `models/` | Model files and metadata (large files excluded from Git) |
| `docker/` | Dockerfiles and compose files |
| `docs/` | Data quality report, feature list, decisions |

---

## Phase 1 tasks (Weeks 1 to 4)

### Task 0: Environment (Week 1)
1. Install Python 3.11, Git, Docker Desktop and VS Code (or JupyterLab).
2. Create a virtual environment and a `requirements.txt` listing pandas, numpy, scikit-learn, matplotlib, seaborn, SQLAlchemy and psycopg2.
3. Download the dataset into `data/raw/` and confirm `data/` and `models/*.pkl` are in `.gitignore`.
4. Write the setup steps in this README so anyone can reproduce your environment.

### Task 1: Data dictionary, my half (Week 1)
Document `customers`, `sellers`, `products`, `geolocation` and `product_category_name_translation`. The analyst documents the other four tables. Include row counts, columns, keys, missing values and oddities. Save in `data-platform/data-dictionary/` through a pull request.

### Task 2: Database and loading scripts (Weeks 1 to 2)
In `data-platform`:
1. Write a `docker-compose.yml` that starts a PostgreSQL container with a persistent volume.
2. Write `schema/` SQL files that create each table with the right data types and keys.
3. Write an `etl/` Python script that loads every CSV into the database.
4. Validate that the row counts in the database match the CSV files.
5. Make sure the analyst can reproduce the same database on her computer from these scripts.

**Deliverable:** a clean machine can run one command and end up with a loaded database.

### Task 3: Exploratory data analysis (Weeks 2 to 3)
In `notebooks/01_eda.ipynb`:
- Distribution of delivery days and delay days
- Share of late orders overall, and over time
- Missing values and outliers
- Relationships between lateness and state, seller, product weight, product size, freight value and order month

### Task 4: Define the target and check for leakage (Week 3)
1. Confirm the target: `is_late = 1` if the delivered date is after the estimated date.
2. List which columns are known **at purchase time** and which are only known **after** delivery. Columns known only after delivery (such as the actual delivery date or the review score) cannot be used as model inputs. This is called data leakage, and it makes a model look good while being useless.
3. Document your decisions in `docs/target_and_leakage.md`.

### Task 5: Feature list v0 and data quality report (Week 4)
- Write `docs/feature_list_v0.md` with candidate features, where each comes from, and why it might matter (for example, distance between seller and customer computed from coordinates, product weight and volume, freight value, seller history, and month).
- Write `docs/data_quality_report.md` summarising issues and how you will handle them.
- Read the analyst's three hypotheses and add them to the feature list where they apply.

### Phase 1 exit check
- [ ] Database loads from scripts on a clean machine
- [ ] EDA notebook reviewed by the analyst
- [ ] Target defined and leakage rules documented
- [ ] Feature list v0 and data quality report merged

---

## Looking ahead (Phase 2, not yet started)
Baseline models (logistic regression and tree-based models), with metrics suited to an imbalanced target (precision, recall, F1 and ROC-AUC), and a time-based train and test split. Experiment tracking with MLflow comes after that.

## How to work
1. Pick up an issue from the project board and move it to In Progress.
2. Create a branch, for example `ml/7-eda-delivery-days`.
3. Commit often with clear messages. Clear outputs from notebooks before committing if they are large.
4. Open a pull request and request a review. Wait for approval before merging.
5. Post a short update in GitHub Discussions each working day.
6. Show progress at the Friday demo.

## Definition of Done
The work meets the issue's description, is documented, has been reviewed and merged, and the issue is closed.

## Rules
Never commit raw data, passwords, tokens or personal information.
