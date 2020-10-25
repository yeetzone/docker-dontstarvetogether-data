# Set build arguments.
ARG STEAM_APP="343050"
ARG STEAM_PATH="/data"
ARG STEAM_BRANCH
ARG STEAM_PASSWORD

# Create installation image.
FROM yeetzone/steamcmd as STEAMCMD

# Set environment.
ARG STEAM_APP
ENV STEAM_APP ${STEAM_APP}
ARG STEAM_PATH
ENV STEAM_PATH ${STEAM_PATH}
ARG STEAM_BRANCH
ENV STEAM_BRANCH ${STEAM_BRANCH}
ARG STEAM_PASSWORD
ENV STEAM_PASSWORD ${STEAM_PASSWORD}

# Install Don't Starve Together.
RUN set -ex \
	&& mkdir -p $STEAM_PATH \
    && chown $STEAM_USER:$STEAM_USER $STEAM_PATH \
	&& gosu "$STEAM_USER" steamcmd \
		+@ShutdownOnFailedCommand 1 \
		+login anonymous \
		+force_install_dir "$STEAM_PATH" \
		+app_update "$STEAM_APP" \
			$([ -n "$STEAM_BRANCH" ] && printf %s "-beta $STEAM_BRANCH") \
			$([ -n "$STEAM_PASSWORD" ] && printf %s "-betapassword $STEAM_PASSWORD") \
			validate \
		+quit

# Create final image.
FROM tianon/true
MAINTAINER YEET Zone <support@yeet.zone>

# Set environment.
ARG STEAM_APP
ENV STEAM_APP ${STEAM_APP}
ARG STEAM_PATH
ENV STEAM_PATH ${STEAM_PATH}
ARG STEAM_BRANCH
ENV STEAM_BRANCH ${STEAM_BRANCH}
ARG STEAM_PASSWORD
ENV STEAM_PASSWORD ${STEAM_PASSWORD}

# Copy game data.
COPY --from=STEAMCMD ${STEAM_PATH} ${STEAM_PATH}

# Set volume.
VOLUME "$STEAM_PATH"
