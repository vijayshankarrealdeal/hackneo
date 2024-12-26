# ------------------------------------------------------------------------------
# Dockerfile Example for Ubuntu-based Dev Container with Neo4j 5.x
# ------------------------------------------------------------------------------

# Use the official Dev Containers base image (Ubuntu 22.04+)
FROM mcr.microsoft.com/devcontainers/base:ubuntu

# ------------------------------------------------------------------
# Set Environment Variables for Versions (adjust as needed)
# ------------------------------------------------------------------
ENV NODE_VERSION=22.x

# ------------------------------------------------------------------
# Install Essential Packages
# ------------------------------------------------------------------
RUN apt-get update && \
    apt-get install -y \
        xz-utils \
        wget \
        curl \
        gnupg \
        software-properties-common \
        lsb-release \
        ca-certificates \
        openjdk-17-jdk \
        apt-transport-https \
        dirmngr && \
    rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------------
# Add the Neo4j GPG Key and Repo (stable 5)
# ------------------------------------------------------------------
RUN wget -O - https://debian.neo4j.com/neotechnology.gpg.key \
    | gpg --dearmor -o /usr/share/keyrings/neo4j-archive-keyring.gpg

RUN echo "deb [signed-by=/usr/share/keyrings/neo4j-archive-keyring.gpg] \
          https://debian.neo4j.com stable 5" \
    | tee /etc/apt/sources.list.d/neo4j.list

# ------------------------------------------------------------------
# Install Neo4j (latest 5.x from stable 5)
# ------------------------------------------------------------------
RUN apt-get update && \
    apt-get install -y neo4j && \
    rm -rf /var/lib/apt/lists/*

# ------------------------------------------------------------------
# Install TinyGo
# ------------------------------------------------------------------
RUN wget https://github.com/tinygo-org/tinygo/releases/download/v0.35.0/tinygo_0.35.0_amd64.deb
RUN sudo dpkg -i tinygo_0.35.0_amd64.deb
# Verify TinyGo installation
RUN tinygo version

# ------------------------------------------------------------------
# Install Node.js 22.x
# ------------------------------------------------------------------
RUN curl -fsSL https://deb.nodesource.com/setup_${NODE_VERSION} | bash - && \
    apt-get update && \
    apt-get install -y nodejs && \
    rm -rf /var/lib/apt/lists/*
# Verify Node.js and npm installation
RUN node -v && npm -v

# ------------------------------------------------------------------
# Install Latest Git Version
# ------------------------------------------------------------------
RUN add-apt-repository ppa:git-core/ppa -y && \
    apt-get update && \
    apt-get install -y git && \
    git config --global init.defaultBranch main && \
    git --version

# ------------------------------------------------------------------
# Configure Neo4j
# ------------------------------------------------------------------
# Expose Neo4j ports
EXPOSE 7474 7687

# Set default Neo4j authentication (CHANGE this password in real usage!)
ENV NEO4J_AUTH=neo4j/password

# ------------------------------------------------------------------
# Final Cleanup
# ------------------------------------------------------------------
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Install Modus CLI
RUN sudo npm install -g @hypermode/modus-cli

# ------------------------------------------------------------------
# Default Command (start Neo4j)
# ------------------------------------------------------------------
CMD ["neo4j", "console"]
