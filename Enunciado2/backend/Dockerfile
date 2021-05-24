FROM python:3.7-slim-buster



RUN mkdir /apps
WORKDIR /apps


ENV DJANGO_SETTINGS_MODULE=backend.settings
ENV ENDPOINT=http://0.0.0.0:8000
ENV SECRET_KEY=craftech

RUN apt-get update 
RUN pip3 install --upgrade pip 
RUN pip3 install psycopg2-binary 
RUN apt-get install libpq-dev -y gcc -y python3-dev -y libgraphviz-dev -y




COPY ./requirements.txt .

RUN pip install -r requirements.txt


COPY . .


EXPOSE 8000

#execute backend
CMD [ "python3", "manage.py", "runserver", "0.0.0.0:8000" ]