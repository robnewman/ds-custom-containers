# Seqera base image
FROM public.cr.seqera.io/platform/connect-client:0.7 AS connect

# Rocker image
FROM rocker/rstudio:4.4.1

# Define Connect port
ENV TOWER_CONNECT_PORT="8787"

# Disable Auth and enable Root user
ENV DISABLE_AUTH=true
ENV ROOT=true
ENV HOME=/workspace

# 1. Add connect binary
COPY --from=connect /usr/bin/connect-client /usr/bin/connect-client

# 2. Install connect dependencies
RUN /usr/bin/connect-client --install

# 3. Install R packages needed
RUN R -e "install.packages(c('rmarkdown', 'png', 'reticulate'))" \
    -e "if (!requireNamespace('BiocManager', quietly = TRUE)) install.packages('BiocManager')" \
    -e "BiocManager::install(c('Gviz', 'GenomicRanges', 'Biostrings', 'VariantAnnotation'))" && \
    R -e "unlink('/tmp/Rtmp*', recursive = TRUE)" && \
    rm -rf /tmp/downloaded_packages/ /tmp/*.rds /var/lib/apt/lists/* /tmp/*

# 4. Add and run RStudio
COPY ./run /etc/services.d/rstudio/run

# 4. Set default workspace
RUN echo "session-default-working-dir=/workspace" >> /etc/rstudio/rsession.conf

# 5. Configure connect as the entrypoint
ENTRYPOINT ["/usr/bin/connect-client", "--entrypoint"]

# 6. Run the initialization script
CMD ["/usr/bin/bash","-c", "/init"]