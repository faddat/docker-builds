
################ USE DEBIAN, BECAUSE UBUNTU SUCKS.#####################
FROM debian:8

#############UPDATE PACKAGES AND GET DEPENDENCIES####################

RUN apt-get update 
RUN apt-get install -y curl imagemagick build-essential python-pip git gcc libc6-dev make build-essential aria2
RUN curl -sL https://deb.nodesource.com/setup_iojs_2.x | bash -
RUN apt-get update 
RUN apt-get install -y iojs
RUN apt-get upgrade -y

########################REDIS ENVIRONMENT VARIABLES################
ENV REDIS_VERSION 3.0.1
ENV REDIS_DOWNLOAD_URL http://download.redis.io/releases/redis-3.0.1.tar.gz
ENV REDIS_DOWNLOAD_SHA1 fe1d06599042bfe6a0e738542f302ce9533dde88

############################INSTALL REDIS####################################
RUN  set -x && \
	 apt-get update && apt-get install -y $buildDeps --no-install-recommends && \
	 rm -rf /var/lib/apt/lists/* && \
	 mkdir -p /usr/src/redis && \
	 curl -sSL "$REDIS_DOWNLOAD_URL" -o redis.tar.gz && \
	 echo "$REDIS_DOWNLOAD_SHA1 *redis.tar.gz" | sha1sum -c - && \
	 tar -xzf redis.tar.gz -C /usr/src/redis --strip-components=1 && \
	 rm redis.tar.gz && \
	 make -C /usr/src/redis && \
	 make -C /usr/src/redis install && \
	 rm -r /usr/src/redis && \
	 apt-get purge -y --auto-remove $buildDeps && \
	 useradd redis -p password
RUN mkdir /data
 
RUN chown redis:redis /data 
RUN cd /data ; nohup redis-server&


###################################INSTALL NODEBB############################
RUN git clone https://github.com/nodebb/nodebb /nodebb
RUN cd /nodebb ; npm install

########################GET SHIT ROLLING#####################
WORKDIR /nodebb
EXPOSE 4567
ENTRYPOINT ["node","./nodebb","start"]
