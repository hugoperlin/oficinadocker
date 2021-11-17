FROM python:3-alpine

LABEL mantainer="Hugo Alberto Perlin <hugo.perlin@ifpr.edu.br>"

# install system-wide deps for python and node
RUN apk update
RUN apk add curl gnupg

# copy our application code
ADD flask-app /opt/flask-app
WORKDIR /opt/flask-app

# fetch app specific deps
RUN pip3 install -r requirements.txt

# expose port
EXPOSE 5000

# start app
CMD [ "python", "app.py" ]
