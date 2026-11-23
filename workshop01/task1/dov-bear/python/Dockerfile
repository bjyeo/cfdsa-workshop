FROM python:3.11-slim

ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PORT=3000

WORKDIR /app

# gunicorn 20.1.0 imports pkg_resources, which newer setuptools removed
COPY requirements.txt .
RUN pip install --no-cache-dir "setuptools<70" -r requirements.txt

COPY main.py utils.py ./
COPY public ./public
COPY templates ./templates

EXPOSE ${PORT}

HEALTHCHECK --interval=30s --timeout=3s --start-period=5s \
  CMD python -c "import os,urllib.request; urllib.request.urlopen(f'http://localhost:{os.environ[\"PORT\"]}/healthz')" || exit 1

CMD ["sh", "-c", "exec gunicorn -b \":${PORT}\" --log-level info main:app"]
