FROM images.canfar.net/skaha/base-notebook@sha256:c7e8dbec40d1e74498919c0e2f629a8169c08e78fcb99b2c087646e3b4e75a1b

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV JUPYTER_RUNTIME_DIR=/root/.local/share/jupyter/runtime

# Set the working directory
WORKDIR /root/work

# Copy and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Expose necessary ports
EXPOSE 8888

# Set the default command to launch Jupyter Notebook
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--NotebookApp.token=''"]
