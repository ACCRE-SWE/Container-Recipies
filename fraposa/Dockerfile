FROM r-base:4.4.0

# Set labels
LABEL maintainer="Satkar Dhakal"
LABEL version="1.0"
LABEL description="Docker container with Python, R, PLINK, and Git"


RUN R -e "install.packages('hdpca',dependencies=TRUE, repos='http://cran.rstudio.com/')"

# Install common dependencies and tools
RUN apt-get update && apt-get install -y \
    build-essential \
    wget \
    curl \
    git \
    ca-certificates \
    libssl-dev \
    libcurl4-openssl-dev \
    libxml2-dev \
    software-properties-common \
    python3 \
    python3-venv \
    python3-pip \
    python3-dev \
    unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install PLINK
RUN wget http://s3.amazonaws.com/plink1-assets/plink_linux_x86_64_20210606.zip && \
    unzip plink_linux_x86_64_20210606.zip -d /usr/local/bin/ && \
    chmod +x /usr/local/bin/plink && \
    rm plink_linux_x86_64_20210606.zip

RUN python3 -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

RUN pip install --upgrade pip && \
    pip install numpy pandas matplotlib scikit-learn pyplink rpy2

RUN mkdir -p /home/fraposa && \
    git clone https://github.com/daviddaiweizhang/fraposa.git  /home/fraposa


WORKDIR /home/fraposa


# Set the PATH environment variable
ENV PATH="/usr/local/bin:${PATH}"

# Default command to run
CMD ["/bin/bash"]
