# edu-raspberry-os

> Setup raspberry docker machine
>
> Memorising not priotized, when you don't have more important parts to memorise.

## Premises

### iotnet exists

```bash
docker network create --driver bridge --subnet 192.168.2.0/24 --gateway 192.168.2.1 iotnet
```

### need to remove old conatiner

```bash
docker stop rpi5-dev
docker rm rpi5-dev
```

## Instructions

```bash
docker run -d --name rpi5-dev \
    --network iotnet \
    --hostname rpi5-dev \
    -p 2222:22 \
    -e TZ=UTC \
    balenalib/raspberrypi5-debian:bookworm \
    /bin/bash -c "while true; do sleep 30; done"
```

## Install c development environment

```bash
docker exec -it rpi5-dev bash -c "
apt-get update && apt-get install -y \
    build-essential \
    cmake \
    gcc-arm-none-eabi \
    libnewlib-arm-none-eabi \
    gdb-multiarch \
    openssh-server \
    sudo \
    vim \
    nano \
    gdb \
    git \
    && mkdir -p /var/run/sshd"
```

## Add dev user

> Change user and password

```bash
docker exec -it rpi5-dev bash -c "
useradd -m -s /bin/bash [user] && \
echo '[user]:[password]' | chpasswd && \
usermod -aG sudo [user]"
```

## Enable SSH

```bash
docker exec -it rpi5-dev bash -c "
sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config && \
sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config && \
service ssh restart"
```

## Login

```
ssh [user]@localhost -p 2222

# if WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!
# vi ~/.ssh/known_hosts
# remove key for localhost:2222

cd ~
mkdir ws

# ctrl-d to end session
```
