# Extended AdTech Analytics

> **Archived repository.** This project is retained as a portfolio/reference snapshot and is not actively maintained or supported.

FastAPI and Streamlit application for ingesting campaign CSV data, calculating CTR and conversion rates, and exploring results by date and channel. It also contains an experimental, auditable RFP intake workflow: PDF/DOCX upload, requirement extraction, source location, and human review.

Repository: [JinyanShao/Extended-AdTech-Analytics](https://github.com/JinyanShao/Extended-AdTech-Analytics)

## Archived status

Do not use this repository to process production or customer data. It has no active support, release process, or security maintenance commitment. The included authentication/configuration examples are for local evaluation only and never provide usable default credentials.

## Local evaluation

Requirements: Python 3.11+.

```bash
git clone https://github.com/JinyanShao/Extended-AdTech-Analytics.git
cd Extended-AdTech-Analytics
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
# Configure unique local secrets in .env; never commit it.
alembic upgrade head
uvicorn main:app --host 127.0.0.1 --port 8000
```

In a second terminal, with the same environment activated:

```bash
streamlit run dashboard.py --server.port 8501
```

The API documentation is at <http://127.0.0.1:8000/docs>; the dashboard is at <http://127.0.0.1:8501>. For an existing local Analytics database, make a verified backup before running migrations. Do not use `alembic downgrade` as a data-reset command.

## API

- `POST /auth/login` — obtain a JWT using a configured account.
- `POST /data/upload` — upload a CSV (requires `Authorization: Bearer <token>`).
- `GET /data/stats` — retrieve optional date/channel-filtered statistics (requires a token).
- `POST /rfp/documents` — upload a PDF/DOCX RFP and persist an extracted requirement matrix.
- `GET /rfp/documents/{document_version_id}/requirements` — view the matrix and source locations.
- `PUT /rfp/requirements/{requirement_id}` — record a human wording correction and review note.
- `GET /health` — service health response.

Input CSV requires `date`, `channel`, `clicks`, and `conversions`; `campaign`, `impressions`, and `cost` are optional.

## License

Licensed under the [MIT License](LICENSE).
