FROM node:current-buster

RUN apt-get update
RUN apt-get install multiarch-support wget build-essential libcairo2-dev libpango1.0-dev libgif-dev librsvg2-dev -y
RUN wget http://launchpadlibrarian.net/482988067/libjpeg-turbo8_1.5.2-0ubuntu5.18.04.4_armhf.deb
RUN dpkg -i libjpeg-turbo8_1.5.2-0ubuntu5.18.04.4_armhf.deb

WORKDIR /NineBot
COPY package.json /NineBot
COPY package-lock.json /NineBot
RUN npm ci /NineBot
COPY . /NineBot
CMD node index.js
