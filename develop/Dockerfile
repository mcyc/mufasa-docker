FROM images.canfar.net/skaha/base-notebook@sha256:c7e8dbec40d1e74498919c0e2f629a8169c08e78fcb99b2c087646e3b4e75a1b

# Set up the home directory explicitly for all users
ARG USER_HOME=/home/canfar-user

# Create a user for CANFAR environments and set up directories
RUN useradd -ms /bin/bash canfar-user && \
    mkdir -p $USER_HOME/mufasa && \
    chown -R canfar-user:canfar-user $USER_HOME

# Switch to the new user
USER canfar-user
WORKDIR $USER_HOME/mufasa

# Clone the mufasa repository and install it in editable mode
RUN git clone https://github.com/mcyc/mufasa.git $USER_HOME/mufasa && \
    pip install --no-cache-dir -e $USER_HOME/mufasa

# Install Git for version control
USER root
RUN apt-get update && \
    apt-get install -y git && \
    git config --global credential.helper store && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Add runtime scripts to handle home and credentials setup
RUN echo '#!/bin/bash' > /usr/local/bin/setup_home.sh && \
    echo 'mkdir -p $HOME/mufasa && chown -R $(whoami):$(whoami) $HOME/mufasa' >> /usr/local/bin/setup_home.sh && \
    chmod +x /usr/local/bin/setup_home.sh

RUN echo '#!/bin/bash' > /usr/local/bin/check_git_credentials.sh && \
    echo 'CREDENTIALS_PATH="$HOME/.git-credentials"' >> /usr/local/bin/check_git_credentials.sh && \
    echo 'if [ ! -f "$CREDENTIALS_PATH" ] || [ ! -s "$CREDENTIALS_PATH" ]; then' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  echo "Git credentials not found. Please configure your credentials."' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  read -p "Enter your Git username: " git_user' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  read -p "Enter your Git email: " git_email' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  git config --global user.name "$git_user"' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  git config --global user.email "$git_email"' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  read -sp "Enter your Git password or PAT: " git_pass && echo' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  echo "https://${git_user}:${git_pass}@github.com" > "$CREDENTIALS_PATH"' >> /usr/local/bin/check_git_credentials.sh && \
    echo '  echo "Git credentials have been saved."' >> /usr/local/bin/check_git_credentials.sh && \
    echo 'fi' >> /usr/local/bin/check_git_credentials.sh && \
    chmod +x /usr/local/bin/check_git_credentials.sh

# Ensure runtime scripts are sourced in new terminal sessions
RUN echo "/usr/local/bin/setup_home.sh" >> /etc/bash.bashrc && \
    echo "/usr/local/bin/check_git_credentials.sh" >> /etc/bash.bashrc

# Switch back to the non-root user
USER canfar-user

# Default command to start Jupyter Notebook
CMD ["jupyter", "notebook", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--ServerApp.token=''"]
