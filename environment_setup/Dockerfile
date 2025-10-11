FROM conda/miniconda3

LABEL org.label-schema.vendor="Microsoft" \
    org.label-schema.url="https://hub.docker.com/r/microsoft/mlopspython" \
    org.label-schema.vcs-url="https://github.com/microsoft/MLOpsPython"

# Copy dependency file
COPY diabetes_regression/ci_dependencies.yml /setup/

# 1. 🔧 Install Node.js, Python, and create the Conda environment
RUN conda update -n base -c defaults conda && \
    conda install -y -c conda-forge nodejs python=3.7.5 && \
    conda env create -f /setup/ci_dependencies.yml && \
    /bin/bash -c "source activate mlopspython_ci" && \
    az --version && \
    node -v && \
    chmod -R 777 /usr/local/envs/mlopspython_ci/lib/python3.7

# 2. 🔥 Set the runtime PATH environment variable
ENV PATH /usr/local/envs/mlopspython_ci/bin:$PATH

# 3. 📌 ADD THE CRUCIAL AZURE PIPELINES AGENT LABEL
# This explicitly tells the Azure Pipelines agent to look inside the Conda environment's bin for 'node'.
LABEL "com.azure.dev.pipelines.agent.handler.node.path"="/usr/local/envs/mlopspython_ci/bin/node"
