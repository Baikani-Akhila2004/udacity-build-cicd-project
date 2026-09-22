FROM public.ecr.aws/docker/library/python:3.10

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN useradd -u 1000 -m app

RUN apt-get update && \
    apt-get install -y \
    libpcre3 \
    libpcre3-dev \
    build-essential \
    libssl-dev \
    libffi-dev \
    && rm -rf /var/lib/apt/lists/* \
    && pip install pipenv

RUN mkdir -p /var/www/app && mkdir /app

COPY Pipfile* /app

RUN cd /app && pipenv install --system --deploy --verbose

COPY . /app

CMD uwsgi --http :5000 --master --enable-threads -s /var/www/app/app.sock --manage-script-name --mount /=app:app