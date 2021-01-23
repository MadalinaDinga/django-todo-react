FROM python:3.7-alpine

RUN apk update && apk add postgresql-dev gcc python3-dev musl-dev

RUN pip install psycopg2-binary

WORKDIR /backend
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 8081
COPY /backend .
ENTRYPOINT ["python", "manage.py"]
CMD ["runserver", "0.0.0.0:8081"]