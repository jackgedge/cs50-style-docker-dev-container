
# Use an official Ubuntu 22.04 as a parent image
FROM ubuntu:22.04

# Set the maintainer label
LABEL maintainer="dev@ckge.io"

# Set environment variables to reduce package dialogs
ENV DEBIAN_FRONTEND=noninteractive

# Update apt and install some basic tools and dependencies
RUN apt-get update && apt-get install -y \
    curl \
    git \
    wget \
    build-essential \
    software-properties-common \
    apt-transport-https \
    locales \
    tzdata \
    && rm -rf /var/lib/apt/lists/*

# Set timezone
RUN ln -snf /usr/share/zoneinfo/$(cat /etc/timezone) /etc/localtime && echo $(cat /etc/timezone) > /etc/timezone

# Install Clang, Make, and other necessary tools for C development
RUN apt-get update && apt-get install -y \
    clang \
    make \
    && rm -rf /var/lib/apt/lists/*

# Add deadsnakes PPA for newer Python versions
RUN add-apt-repository ppa:deadsnakes/ppa

# Install Python 3.12
RUN apt-get update && apt-get install -y python3.12 python3.12-venv python3.12-dev \
    && rm -rf /var/lib/apt/lists/*

# Update the symlink for python3 to point to python3.12
RUN ln -sf /usr/bin/python3.12 /usr/bin/python3 && \
    ln -sf /usr/bin/python3.12 /usr/bin/python

# Install Node.js, Java and Python pip (pip might already be included in python3.12)
RUN apt-get update && apt-get install -y nodejs npm openjdk-11-jdk \
    && rm -rf /var/lib/apt/lists/*

# Download the PowerShell ARM64 tar.gz file
RUN wget https://github.com/PowerShell/PowerShell/releases/download/v7.4.2/powershell-7.4.2-linux-arm64.tar.gz -O powershell.tar.gz

# Extract PowerShell to /usr/local/bin
RUN mkdir -p /opt/powershell && \
    tar -xzf powershell.tar.gz -C /opt/powershell && \
    ln -s /opt/powershell/pwsh /usr/local/bin/pwsh

# Install the CS50 C Library
RUN apt-get update && apt-get install -y \
    libcs50-10

# Install the CS50 Python Library
RUN python3 -m pip install cs50

# Clean up downloaded tar.gz file
RUN rm powershell.tar.gz

# Add a new user "dev" and switch to it
RUN useradd -m -s /bin/bash dev && \
    mkdir -p /workspace && \
    chown -R dev:dev /workspace

USER dev

# Expose port 8080 for a web application
EXPOSE 8080

# Define mountable directories
VOLUME /workspace

# Run your application
CMD ["bash"]

# To run: docker build -t my_dev_image .

# To build: docker run -it --name my_dev_container my_dev_image