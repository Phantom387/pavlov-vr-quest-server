FROM ubuntu:20.04

LABEL maintainer "XavierSJC"

ENV SERVER_NAME=Your_name_server
ENV RCON_PWD=ChangeThisPassword
ENV RCON_PORT=9100
ENV UPDATE_SERVER=True

# Install steam and create user
RUN apt update && apt install -y gdb curl wget nano lib32gcc1 libc++-dev unzip 
RUN useradd -m steam
USER steam

# Install Pavlov Shack Server
COPY --chown=steam:steam ../configfiles/install_pavlov.sh /home/steam/pavlovserver/
RUN chmod +x /home/steam/pavlovserver/install_pavlov.sh
RUN /home/steam/pavlovserver/install_pavlov.sh

# Server configuration
RUN mkdir -p /home/steam/pavlovserver/Pavlov/Saved/Logs
RUN mkdir -p /home/steam/pavlovserver/Pavlov/Saved/Config/LinuxServer
RUN mkdir -p /home/steam/pavlovserver/Pavlov/Saved/maps

RUN touch /home/steam/pavlovserver/Pavlov/Saved/Config/mods.txt
RUN touch /home/steam/pavlovserver/Pavlov/Saved/Config/blacklist.txt
RUN touch /home/steam/pavlovserver/Pavlov/Saved/Config/whitelist.txt

# Configuration and map list
COPY --chown=steam:steam ../configfiles/Game.ini /home/steam/pavlovserver/Pavlov/Saved/Config/LinuxServer/

# Configure RCON
COPY --chown=steam:steam ../configfiles/RconSettings.txt /home/steam/pavlovserver/Pavlov/Saved/Config/

# Configure main script that runs server
COPY --chown=steam:steam ../configfiles/ConfigureServer.sh /home/steam/pavlovserver/
RUN chmod +x /home/steam/pavlovserver/ConfigureServer.sh

EXPOSE 7000/udp 7400/udp 7777 7777/udp 8177 8177/udp 9100

ENTRYPOINT ["/home/steam/pavlovserver/ConfigureServer.sh"]