FROM python:3.9-slim

WORKDIR /app

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        gcc \
        postgresql-client \
        curl \
    && rm -rf /var/lib/apt/lists/*

COPY backend/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Копируем код ПОСЛЕ установки зависимостей (для кэширования слоев)
COPY backend/ .

# Создаем директории и устанавливаем права
RUN mkdir -p /var/www/static /var/www/media \
    && chown -R www-data:www-data /var/www /app \
    && chmod -R 755 /var/www /app

USER www-data

EXPOSE 8000
CMD ["gunicorn", "--bind", "0.0.0.0:8000", "--access-logfile", "-", "kittygram_backend.wsgi:application"]