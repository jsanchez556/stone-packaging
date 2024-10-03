# Use the official Ubuntu 22.04 image
FROM ubuntu:22.04

# Set the working directory
WORKDIR /app

# Update the package list and install necessary packages
RUN apt-get update && \
    apt-get install -y \
    curl \
    git \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy any local files to the container (optional)
# COPY . .

# Set the command to run when the container starts
CMD ["bash"]
