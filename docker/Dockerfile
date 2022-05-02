# Project builder image
FROM didstopia/base:nodejs-16-ubuntu-20.04 AS BUILDER
WORKDIR /app
COPY . /app
RUN chown -R 1000:1000 /app
USER docker
ENV NODE_ENV development
ENV PKG_CACHE_PATH /app/node_modules/.cache
RUN npm install
RUN npm run test



## FIXME: We really need to be able to use a more minimal image,
##        but for some reason we can't use scratch with Zeit's pkg?!
# Final runtime image
# FROM scratch AS FINAL
FROM didstopia/base:nodejs-16-ubuntu-20.04 AS FINAL
# RUN mkdir -p /app
WORKDIR /app
COPY --from=BUILDER /app/dist/ /app
# COPY --from=BUILDER /app/dist/dishorde /app/dishorde
# COPY --from=BUILDER /app/dist/config.json /app/config.json
# RUN ls -lah /app; chmod a+x /app/dishorde
ENV DISHORDE_PASSWORD "7dtd_server_password"
ENV DISHORDE_TOKEN "discord_token"
ENV DISHORDE_CHANNEL "discord_channel_id"
ENV DISHORDE_PORT "8081"
ENV DISHORDE_IP "localhost"
## TODO: Add the rest of the config variables (allow-exec-command etc.)
CMD [ "/app/dishorde" ]
# CMD [ "/app/dishorde", "--test" ]
