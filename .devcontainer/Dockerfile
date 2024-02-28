FROM ubuntu

RUN apt-get update
RUN apt-get install vim sudo curl jq git -y

# Download the latest stable version of kubectl
RUN curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"

# Download the kubectl checksum file
RUN curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"

# Verify the kubectl binary against its checksum
RUN echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check

# Make the kubectl binary executable
RUN chmod +x ./kubectl

# Move the kubectl executable to /usr/local/bin
RUN mv ./kubectl /usr/local/bin/kubectl

# Fetch the latest release of k9s from GitHub and download it
RUN LATEST_VERSION=$(curl -s https://api.github.com/repos/derailed/k9s/releases/latest | jq -r ".tag_name") && \
    curl -L "https://github.com/derailed/k9s/releases/download/${LATEST_VERSION}/k9s_Linux_amd64.tar.gz" -o k9s.tar.gz && \
    tar -xzf k9s.tar.gz -C /tmp && \
    mv /tmp/k9s /usr/local/bin/k9s && \
    chmod +x /usr/local/bin/k9s && \
    rm -f k9s.tar.gz

RUN useradd -m -s /bin/bash coder && \
    echo "coder ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/coder && \
    chmod 0440 /etc/sudoers.d/coder

USER coder

# Let's add a fancy prompt!
RUN echo "PS1='🐳 \[\033[1;36m\]\h \[\033[1;34m\]\W\[\033[0;35m\] \[\033[1;36m\]# \[\033[0m\]'" > ~/.bashrc
