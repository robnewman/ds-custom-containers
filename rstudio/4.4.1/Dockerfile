# Seqera base image
FROM public.cr.seqera.io/platform/connect-client:0.7 AS connect

# Rocker image
FROM rocker/rstudio:4.4.1

# Define Connect port
ENV TOWER_CONNECT_PORT="8787"

# Disable Auth and enable Root user
ENV DISABLE_AUTH=true
ENV ROOT=true

# 1. Add connect binary
COPY --from=connect /usr/bin/connect-client /usr/bin/connect-client

# 2. Add RStudio
COPY ./docker/rstudio_4.4.1/run /etc/services.d/rstudio/run

# 3. Install connect dependencies
RUN /usr/bin/connect-client --install

# 4. Configure connect as the entrypoint
ENTRYPOINT ["/usr/bin/connect-client", "--entrypoint"]
