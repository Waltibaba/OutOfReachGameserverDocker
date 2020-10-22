###########################################################
# Dockerfile that builds an Out of Reach Gameserver
###########################################################
FROM steamcmd/steamcmd:latest

LABEL maintainer="admin@email.com"

ENV STEAMAPPID 406800
ENV STEAMAPPDIR /outofreach
ENV STEAMAPPTEMPDIR /tmp/outofreach
ENV STEAMCMDDIR /steamcmd

RUN set -x \
# Install Out of Reach server dependencies and clean up
	&& apt-get update \
	&& apt-get install -y --no-install-recommends --no-install-suggests \
		libsdl2-2.0-0 \
		libsdl1.2debian \
		wget \
	&& mkdir -p ${STEAMCMDDIR} \
	&& mkdir -p ${STEAMAPPDIR} \
	&& mkdir -p ${STEAMAPPTEMPDIR} \
	&& mkdir -p /usr/lib/steam \
	&& wget -qO- 'http://media.steampowered.com/installer/steamcmd_linux.tar.gz' | tar xvzf - -C "${STEAMCMDDIR}" \
	&& apt-get remove --purge -y steamcmd wget \
	&& apt-get clean autoclean \
	&& apt-get autoremove -y \
	&& rm -rf /var/lib/apt/lists/* \
	&& "${STEAMCMDDIR}/steamcmd.sh" +quit 

WORKDIR $STEAMAPPDIR

VOLUME $STEAMAPPDIR

# Set Entrypoint
ENTRYPOINT "${STEAMCMDDIR}/steamcmd.sh" +login anonymous +force_install_dir ${STEAMAPPDIR} +app_update ${STEAMAPPID} validate +quit \
		&& cp ${STEAMAPPDIR}/steamclient.so /usr/lib/ \
		&& echo "${STEAMAPPDIR}/SFS2X/lib/linux64" >> /etc/ld.so.conf.d/oor.conf \
		&& ldconfig \
		&& cp /sfs2x-service.vmoptions ${STEAMAPPDIR}/SFS2X/sfs2x-service.vmoptions || : \
	    && ${STEAMAPPDIR}/SFS2X/sfs2x-service start-launchd \
		&& ${STEAMAPPDIR}/SFS2X/sfs2x-service stop

# Expose ports
EXPOSE 27015-27030 27010 19933/tcp