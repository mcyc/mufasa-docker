FROM images.canfar.net/skaha/base-notebook@sha256:c7e8dbec40d1e74498919c0e2f629a8169c08e78fcb99b2c087646e3b4e75a1b

# Set up the home directory explicitly for all users
ARG USER_HOME=/home/canfar-user

# Create a user for CANFAR environments and set up directories
RUN useradd -ms /bin/bash canfar-user && \
    mkdir -p $USER_HOME && \
    chown -R canfar-user:canfar-user $USER_HOME

# Switch to the new user
USER canfar-user
WORKDIR $USER_HOME

# Ensure Git is installed
USER root
RUN apt-get update && \
    apt-get install -y git wget && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Add Sublime Text repository and install it
RUN wget -qO - https://download.sublimetext.com/sublimehq-pub.gpg | apt-key add - && \
    echo "deb https://download.sublimetext.com/ apt/stable/" | tee /etc/apt/sources.list.d/sublime-text.list && \
    apt-get update && \
    apt-get install -y sublime-text && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install required Python tools and extensions
RUN pip install --no-cache-dir jupyter-server-proxy dask[complete] distributed

# Add Sublime Text to the Jupyter launcher
RUN mkdir -p /usr/local/share/jupyter-server-proxy && \
    echo "def setup_sublime():\n" > /usr/local/share/jupyter-server-proxy/sublime_text.py && \
    echo "    return {'command': ['/usr/bin/sublime_text'], 'timeout': 60, 'launcher_entry': {'title': 'Sublime Text', 'icon_path': ''}}" >> /usr/local/share/jupyter-server-proxy/sublime_text.py

# Add Dask Dashboard to the Jupyter launcher
RUN echo "def setup_dask():\n" > /usr/local/share/jupyter-server-proxy/dask.py && \
    echo "    return {'command': ['dask-scheduler'], 'timeout': 60, 'launcher_entry': {'title': 'Dask Dashboard', 'icon_path': ''}}" >> /usr/local/share/jupyter-server-proxy/dask.py

# Add the setup script for mufasa
RUN echo '#!/bin/bash' > $USER_HOME/setup_mufasa.sh && \
    echo 'cd $HOME' >> $USER_HOME/setup_mufasa.sh && \
    echo 'if [ -d "$HOME/mufasa" ]; then' >> $USER_HOME/setup_mufasa.sh && \
    echo '  if [ -d "$HOME/mufasa/.git" ] && [ "$(git -C $HOME/mufasa config --get remote.origin.url)" = "https://github.com/mcyc/mufasa.git" ]; then' >> $USER_HOME/setup_mufasa.sh && \
    echo '    echo "Pulling latest changes from mufasa repository."' >> $USER_HOME/setup_mufasa.sh && \
    echo '    git -C $HOME/mufasa pull' >> $USER_HOME/setup_mufasa.sh && \
    echo '    if ! pip show mufasa | grep -q "Location: $HOME/mufasa"; then' >> $USER_HOME/setup_mufasa.sh && \
    echo '      pip install --no-cache-dir -e $HOME/mufasa' >> $USER_HOME/setup_mufasa.sh && \
    echo '    fi' >> $USER_HOME/setup_mufasa.sh && \
    echo '  else' >> $USER_HOME/setup_mufasa.sh && \
    echo '    echo "Directory exists but is not a valid mufasa repository. Removing and cloning fresh."' >> $USER_HOME/setup_mufasa.sh && \
    echo '    rm -rf $HOME/mufasa && git clone https://github.com/mcyc/mufasa.git $HOME/mufasa && pip install --no-cache-dir -e $HOME/mufasa' >> $USER_HOME/setup_mufasa.sh && \
    echo '  fi' >> $USER_HOME/setup_mufasa.sh && \
    echo 'else' >> $USER_HOME/setup_mufasa.sh && \
    echo '  echo "Cloning mufasa repository."' >> $USER_HOME/setup_mufasa.sh && \
    echo '  git clone https://github.com/mcyc/mufasa.git $HOME/mufasa && pip install --no-cache-dir -e $HOME/mufasa' >> $USER_HOME/setup_mufasa.sh && \
    echo 'fi' >> $USER_HOME/setup_mufasa.sh && \
    chmod +x $USER_HOME/setup_mufasa.sh

USER root
RUN echo "$USER_HOME/setup_mufasa.sh" >> /etc/bash.bashrc
USER canfar-user

# Default command to start Jupyter Notebook and Dask Scheduler
CMD ["sh", "-c", "dask-scheduler & jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --ServerApp.token=''"]