FROM python:3.7-alpine

RUN apk update && apk add postgresql-dev gcc python3-dev musl-dev

RUN pip install psycopg2-binary

WORKDIR /backend
COPY requirements.txt requirements.txt
COPY run_backend.sh run_backend.sh
RUN chmod +x run_backend.sh
COPY /backend .


RUN pip install -r requirements.txt
EXPOSE 8081

ENTRYPOINT ["./run_backend.sh"]