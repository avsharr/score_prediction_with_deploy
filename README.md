# Student Math Score Prediction

Regression pipeline and Flask app predicting **math score** from demographics and reading/writing scores. 
## Stack

**ML:** Python 3, pandas, NumPy, scikit-learn, XGBoost, CatBoost, dill · 
**App:** Flask
**Ops:** venv, pip, Git · 
**AWS:** AWS CLI, EB CLI · 
**Containers:** Docker, ECR, ECS/Fargate or EB Docker

## Setup & run

```bash
python -m venv .venv && source .venv/bin/activate  # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

**Train** (ingestion → preprocess → train; requires `StudentsPerformance.csv` in project root):

```bash
python src/components/data_ingestion.py
```

**Serve** (default `http://0.0.0.0:8080`, form at `/predictdata`):

```bash
python application.py
```

Production: `gunicorn -b 0.0.0.0:8080 application:application` (add `gunicorn` to dependencies).

## Deploy

| Target | Notes |
|--------|--------|
| **Elastic Beanstalk** | `.ebextensions/python.config` sets `WSGIPath: application:application`. `eb init` → `eb create` / `eb deploy`. Optional `Procfile`: `web: gunicorn --bind 0.0.0.0:8080 application:application`. Ship `artifacts/` with the bundle or fetch at deploy. |
| **EC2** | Clone repo, venv, `pip install -r requirements.txt`, ensure `artifacts/` present or retrain. Run Gunicorn behind nginx; open SG for 80/443 or app port. |
| **Docker** | Build image with `requirements.txt`, `src/`, `application.py`, `templates/`, `artifacts/`. Push to **ECR**; run on **ECS**, **EKS**, or EB multi-container. Use Gunicorn in `CMD` for production. |

Use IAM roles for AWS access; keep secrets out of the repo.

## Author

Anastasiia · aasharovaa@gmail.com
