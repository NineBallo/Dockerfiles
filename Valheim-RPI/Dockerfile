FROM debian:latest


#Setup timestamps
ENV TZ=America/Edmonton
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#MULTIARCH
RUN dpkg --add-architecture armhf
#Update/Upgrade all the jazz
RUN apt-get update; apt-get upgrade -y; apt-get install libsdl2-2.0-0 curl:armhf wine python3 git cmake libtcmalloc-minimal4 gcc-arm-linux-gnueabihf build-essential libc6:armhf wget -y

#Download, compile, and install box86
WORKDIR /tmp/
RUN git clone https://github.com/ptitSeb/box86.git
RUN mkdir /tmp/box86/build;

WORKDIR /tmp/box86/build
RUN cmake .. -DRPI4ARM64=1 -DARM_DYNAREC=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo
RUN make CXXFLAGS="-marm" -j$(nproc); make install

#Download, Compile, and install box64
WORKDIR /tmp/
RUN git clone https://github.com/ptitSeb/box64.git
RUN mkdir /tmp/box64/build;

WORKDIR /tmp/box64/build
RUN cmake .. -DRPI4ARM64=1 -DARM_DYNAREC=1 -DCMAKE_BUILD_TYPE=RelWithDebInfo
RUN make CXXFLAGS="-marm" -j$(nproc); make install

#Install steamCMD...
ENV USER root
ENV HOME /root
WORKDIR $HOME

# Insert Steam prompt answers
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN echo steam steam/question select "I AGREE" | debconf-set-selections \
 && echo steam steam/license note '' | debconf-set-selections

# Update the repository and install SteamCMD
ARG DEBIAN_FRONTEND=noninteractive

#RUN apt-get update
RUN  apt-get update -y \
 && apt-get install -y --no-install-recommends ca-certificates locales locales-all

# Add unicode support
RUN locale-gen en_US.UTF-8
ENV LANG 'en_US.UTF-8'
ENV LANGUAGE 'en_US:en'
ENV LC_ALL 'en_US.UTF-8'     

#Install STEAMCMD
WORKDIR /tmp
RUN wget https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz
RUN mkdir /opt/steamcmd
#WORKDIR /opt/steamcmd
RUN tar -xzf /tmp/steamcmd_linux.tar.gz

#Run SteamCmd && install game
RUN mkdir ~/Games
RUN echo now we pray 1
RUN BOX86_LOG=1 BOX86_LD_LIBRARY_PATH="/tmp/linux32/:/tmp/linux64/:/lib/" LD_LIBRARY_PATH="/tmp/linux32/:/tmp/linux64/:$LD_LIBRARY_PATH:/usr/lib/arm-linux-gnueabihf/:/lib/arm-linux-gnueabihf/:/lib/" /usr/local/bin/box86 /tmp/linux32/steamcmd +@sSteamCmdForcePlatformType linux +login anonymous +force_install_dir ~/Games +app_update 896660 +quit; exit 0;
RUN BOX86_LOG=1 BOX86_LD_LIBRARY_PATH="/tmp/linux32/:/tmp/linux64/:/lib/" LD_LIBRARY_PATH="/tmp/linux32/:/tmp/linux64/:$LD_LIBRARY_PATH:/usr/lib/arm-linux-gnueabihf/:/lib/arm-linux-gnueabihf/:/lib/" /usr/local/bin/box86 /tmp/linux32/steamcmd +@sSteamCmdForcePlatformType linux +login anonymous +force_install_dir ~/Games +app_update 896660 +quit; exit 0;
RUN BOX86_LOG=1 BOX86_LD_LIBRARY_PATH="/tmp/linux32/:/tmp/linux64/:/lib/" LD_LIBRARY_PATH="/tmp/linux32/:/tmp/linux64/:$LD_LIBRARY_PATH:/usr/lib/arm-linux-gnueabihf/:/lib/arm-linux-gnueabihf/:/lib/" /usr/local/bin/box86 /tmp/linux32/steamcmd +@sSteamCmdForcePlatformType linux +login anonymous +force_install_dir ~/Games +app_update 896660 +quit; exit 0;


#mk world dir
RUN mkdir -p /root/.config/unity3d/IronGate/Valheim/worlds

WORKDIR /root/Games
# Set default command
EXPOSE 2465
CMD BOX86_LD_LIBRARY_PATH='./linux64' LD_LIBRARY_PATH='./linux64:$LD_LIBRARY_PATH' /usr/local/bin/box64 ./valheim_server.x86_64 -name "World" -port 2456 -world "Dedicated" -password "password"
