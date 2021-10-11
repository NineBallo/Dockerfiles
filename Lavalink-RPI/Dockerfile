FROM adoptopenjdk/openjdk15

CMD /bin/bash
RUN apt-get update
RUN apt-get install wget -y

RUN mkdir /lavalink
WORKDIR /lavalink

RUN wget -O /Lavalink.jar https://github.com/freyacodes/Lavalink/releases/latest/download/Lavalink.jar
RUN chmod +x /Lavalink.jar

ENTRYPOINT java -Xmx500m -jar /Lavalink.jar
