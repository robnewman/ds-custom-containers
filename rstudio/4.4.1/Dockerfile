# Seqera Connect base image
FROM public.cr.seqera.io/platform/connect-client:0.7 AS connect

# Rocker RStudio image
FROM rocker/rstudio:4.4.1

# 1. Install R packages
RUN R -e "install.packages(c('rmarkdown', 'png', 'reticulate'))" \
    -e "if (!requireNamespace('BiocManager', quietly = TRUE)) install.packages('BiocManager')" \
    -e "BiocManager::install(version = '3.19', ask = FALSE)" \
    -e "BiocManager::install(c('Gviz', 'GenomicRanges', 'Biostrings', 'VariantAnnotation'))" && \
    rm -rf /tmp/Rtmp* /tmp/downloaded_packages/ /tmp/*.rds

# 2. Add connect binary
USER root
COPY --from=connect /usr/bin/connect-client /usr/bin/connect-client

# 3. Install connect dependencies
RUN /usr/bin/connect-client --install

# 4. Disable Auth, enable Root user, define workspace
ENV DISABLE_AUTH=true
ENV ROOT=true
ENV HOME=/workspace

# 5. Add and run RStudio
COPY ./run /etc/services.d/rstudio/run

# 5. Set default workspace
RUN echo "session-default-working-dir=/workspace" >> /etc/rstudio/rsession.conf

# 6. Configure connect as the entrypoint
ENTRYPOINT ["/usr/bin/connect-client", "--entrypoint"]

# 7. Run the initialization script
CMD ["/usr/bin/bash","-c", "/init"]