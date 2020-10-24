FROM	python:3.7-slim

RUN	pip install psycopg2-binary

ENV	PYTHONDONTWRITEBYTECODE 1
ENV	PYTHONUNBUFFERED 1

WORKDIR /code

COPY	app/Pipfile	/code
COPY	app/Pipfile.lock /code

RUN pip install pipenv && pipenv install --system
COPY app /code
