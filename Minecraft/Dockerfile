FROM ubuntu

#Setup timezones
ENV TZ=America/Edmonton
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

CMD /bin/bash
WORKDIR /tmp

#Install prerequisites
RUN apt-get update
RUN apt-get install wget nano zip -y
#RUN apt-get install software-properties-common -y

#Setup adaptopenjdk repo
#RUN wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | apt-key add -
#RUN add-apt-repository --yes https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/
#RUN apt update

#Install java
#RUN apt-get install adoptopenjdk-8-hotspot -y 
RUN apt-get install openjdk-8-jdk-headless -y

WORKDIR /root/
RUN wget -O /tmp/installer https://api.modpacks.ch/public/modpack/31/2051/server/linux

#Start Server
RUN echo "eula=true" > "/root/eula.txt"
RUN chmod +x /tmp/installer

RUN /tmp/installer 31 --auto

EXPOSE 25565 25565

ENTRYPOINT /root/start.sh
