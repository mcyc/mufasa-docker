# Use Python 3.12 base image
FROM python:3.12-slim

# Set environment variables to avoid interactive prompts during installation
ENV DEBIAN_FRONTEND=noninteractive

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    python3 \
    python3-pip \
    && apt-get clean

# Copy the requirements file into the working directory
COPY requirements.txt .

# Create symbolic links for Python and pip (optional but convenient)
RUN ln -sf /usr/bin/python3 /usr/bin/python && \
    ln -sf /usr/bin/pip3 /usr/bin/pip

# Install Python dependencies from the requirements file
RUN pip install --no-cache-dir -r requirements.txt

# Expose a port (adjust this if your application uses a different port)
EXPOSE 5000

# Define the default command (adjust this to launch MUFASA)
CMD ["python3", "-m", "http.server", "5000"]
