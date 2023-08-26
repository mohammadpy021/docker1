FROM python:alpine
ENV PYTHONDONTWRITEBYTECODE 1 
ENV PYTHONUNBUFFERED 1 
ENV DJANGO_SUPERUSER_PASSWORD 123 
RUN mkdir app
WORKDIR /app
COPY requirements.txt .
RUN apk update \
    && apk add gcc python3-dev musl-dev \
    && apk add --no-cache mariadb-dev
RUN pip install --upgrade pip 
RUN pip install -r requirements.txt
# RUN pip install django gunicorn mysqlclient
# RUN apk del musl-dev gcc python3-dev

#wait untill db be created ||| /dev/null 2>&1 : set the errors in another termnal not to show the errors in main terminal
# while ! python3 manage.py sqlflush > /dev/null 2>&1 ; do sleep 1 ; done && \ 
#CMD sleep 10 && \
CMD while ! python3 manage.py sqlflush > /dev/null 2>&1 ; do sleep 0.5 ; done && \ 
	# pip install -r requirements.txt &&\ 
	python3 manage.py makemigrations --noinput && \
	python3 manage.py migrate --noinput && \
	python3 manage.py collectstatic --noinput && \
	python3 manage.py createsuperuser --user admin --email admin@gmail.com --noinput; \ 
	gunicorn -b 0.0.0.0:8000 config.wsgi