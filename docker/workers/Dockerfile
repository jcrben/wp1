FROM python:3.7

# Meta
LABEL maintainer="kiwix"

# Python app
WORKDIR /usr/src
COPY . app
RUN apt-get update && \
    apt-get install -y --no-install-recommends cron && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
RUN pip3 install -r app/requirements.txt

# Schedule tasks through cron
RUN echo "0 0 * * * root /usr/src/app/cron/enqueue-all.sh > /var/log/enqueue-all.cron.log 2>&1" > /etc/cron.d/enqueue-all
RUN echo "0 4 * * * root /usr/src/app/cron/update-global-articles.sh > /var/log/update-global-articles.cron.log 2>&1" > /etc/cron.d/update-global-articles
RUN echo "0 5 * * * root /usr/src/app/cron/enqueue-global.sh > /var/log/enqueue-global.cron.log 2>&1" > /etc/cron.d/enqueue-global

# Start
CMD cron && supervisord -c /usr/src/app/supervisord.conf -n
