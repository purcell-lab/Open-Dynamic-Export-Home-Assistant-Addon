ARG BUILD_FROM=ghcr.io/hassio-addons/debian-base:7.3.3
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install Node.js 20.x and dependencies
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        curl \
        ca-certificates \
        git \
        jq \
    && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y --no-install-recommends nodejs \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Verify Node.js installation
RUN node --version && npm --version

# Clone and build Open Dynamic Export
WORKDIR /app
RUN git clone --depth 1 https://github.com/longzheng/open-dynamic-export.git . \
    && npm ci --only=production \
    && npm run build \
    && ls -la /app \
    && ls -la /app/dist || echo "dist folder not found"

# Copy run script
COPY run.sh /
RUN chmod a+x /run.sh

# Expose port
EXPOSE 3000

WORKDIR /app

# Run
CMD ["/run.sh"]
