FROM debian

ARG ts_source="https://files.teamspeak-services.com/releases/server/3.11.0/teamspeak3-server_linux_amd64-3.11.0.tar.bz2"
ARG path="teamspeak3-server_linux_amd64"

WORKDIR ~/teamspeak

ENV TS3SERVER_LICENSE=accept

EXPOSE 9987/udp 30033 10011

RUN apt update && apt install -y \
    wget \
    bzip2 && \
    rm -rf /var/lib/apt/lists && \
    wget $ts_source && \
    tar -xvjf $path"-3.11.0.tar.bz2"

WORKDIR $path

ENTRYPOINT ["sh", "ts3server_minimal_runscript.sh"] 
