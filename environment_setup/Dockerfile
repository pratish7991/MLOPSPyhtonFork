FROM conda/miniconda3

LABEL org.label-schema.vendor="Microsoft" \
    org.label-schema.url="https://hub.docker.com/r/microsoft/mlopspython" \
    org.label-schema.vcs-url="https://github.com/microsoft/MLOpsPython"

# Copy dependency file
COPY diabetes_regression/ci_dependencies.yml /setup/

# 🔧 Install Node.js + npm
RUN apt-get update && \
    apt-get install -y curl && \
    curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    node -v && npm -v

# 🔥 Activate environment
ENV PATH /usr/local/envs/mlopspython_ci/bin:$PATH

# 🔧 Set up Conda environment and Azure CLI
RUN conda update -n base -c defaults conda && \
    conda install python=3.7.5 && \
    conda env create -f /setup/ci_dependencies.yml && \
    /bin/bash -c "source activate mlopspython_ci" && \
    az --version && \
    chmod -R 777 /usr/local/envs/mlopspython_ci/lib/python3.7
