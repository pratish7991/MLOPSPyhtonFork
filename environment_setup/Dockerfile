FROM conda/miniconda3

LABEL org.label-schema.vendor="Microsoft" \
    # ... (labels)
    org.label-schema.vcs-url="https://github.com/microsoft/MLOpsPython"

# Copy dependency file
COPY diabetes_regression/ci_dependencies.yml /setup/

# 🔥 Activate environment
# Setting PATH BEFORE the main Conda RUN block ensures it's available for the script
ENV PATH /usr/local/envs/mlopspython_ci/bin:$PATH

# 🔧 Set up Conda environment, Azure CLI, and Node.js (via Conda)
RUN conda update -n base -c defaults conda && \
    # --- Start Conda Installations ---
    # Install nodejs into the base environment or ensure it's available
    conda install -y -c conda-forge nodejs python=3.7.5 && \ 
    # ^ Installing nodejs via Conda is safer than apt-get in this base image
    conda env create -f /setup/ci_dependencies.yml && \
    /bin/bash -c "source activate mlopspython_ci" && \
    # --- Check for both Python & Node dependencies ---
    az --version && \
    node -v && \ # Verify Node.js is found now
    # --- Final cleanup/permissions ---
    chmod -R 777 /usr/local/envs/mlopspython_ci/lib/python3.7
