FROM nfcore/base:1.14

LABEL \
  author="Jacob Munro" \
  description="Example container for NPWG" \
  maintainer="Bahlo Lab"

# set the conda env name
ARG NAME='npwg-example'

# Install the conda environment
COPY environment.yml /
RUN conda env create -f /environment.yml && conda clean -a -y

# set path
ENV PATH="/opt/conda/envs/$NAME/bin:/opt/conda/bin:${PATH}"
