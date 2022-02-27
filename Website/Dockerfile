FROM node:current-buster

CMD /bin/bash

ENV PROJECT_URL='https://github.com/NineBallo/MyWebsite.git'
ENV CRON_UPDATE='0 3 * * *'

RUN apt-get update;
RUN apt-get install -y git cron
#RUN npm i -g next

#Cron time I suppose
RUN touch /var/log/cron.log
RUN (cat /etc/crontab; echo "${CRON_UPDATE} cd /home/nonroot/site/; git pull; npm i; npm update --all; npm run build > /proc/1/fd/1 2>/proc/1/fd/2") | crontab

#Setup runtime
RUN useradd --create-home nonroot
WORKDIR /home/nonroot
USER nonroot

RUN git clone https://github.com/NineBallo/MyWebsite.git /home/nonroot/site
WORKDIR /home/nonroot/site/

RUN npm i
RUN npm run build

EXPOSE 3000
ENTRYPOINT npm run start
