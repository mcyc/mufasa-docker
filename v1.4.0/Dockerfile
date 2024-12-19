# Base image
FROM ubuntu:20.04

# Set environment variables to avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    python3 \
    python3-pip \
    && apt-get clean

# Create symbolic links for Python and pip (optional but convenient)
RUN ln -s /usr/bin/python3 /usr/bin/python && \
    ln -s /usr/bin/pip3 /usr/bin/pip

# Install required Python libraries
# Add MUFASA-specific dependencies here
RUN pip install numpy matplotlib flask

# Expose a port (adjust this if your application uses a different port)
EXPOSE 5000

# Define the default command (adjust this to launch MUFASA)
CMD ["python3", "-m", "http.server", "5000"]
