FROM nginx:latest

ADD default.conf /etc/nginx/conf.d

# Install curl.
RUN apt-get update \
    && apt-get install -y curl \
    && rm -rf /var/lib/apt/lists/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
    && apt-get clean

# Add healthcheck.
HEALTHCHECK CMD curl --fail http://localhost/ || exit 1
