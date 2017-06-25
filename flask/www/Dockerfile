# A simple Flask app container.
FROM python:2.7
LABEL maintainer="Jeff Geerling"

# Place app in container.
COPY . /opt/www
WORKDIR /opt/www

# Install dependencies.
RUN pip install -r requirements.txt

EXPOSE 80
CMD python index.py
