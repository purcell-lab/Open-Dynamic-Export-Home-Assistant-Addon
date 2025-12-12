ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:latest
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install Node.js and dependencies
RUN apk add --no-cache \
    nodejs \
    npm \
    git \
    bash \
    jq \
    python3 \
    make \
    g++

# Verify versions
RUN node --version && npm --version

# Clone repository
WORKDIR /app
RUN git clone --depth 1 https://github.com/longzheng/open-dynamic-export.git .

# Install dependencies and build
RUN npm ci && npm run build

# Verify build output
RUN echo "=== Build complete. Checking structure ===" && \
    ls -la dist/src/ && \
    echo "=== Looking for entry points ===" && \
    find dist -name "*.js" -path "*/src/*" | head -10

# Create .env file
RUN echo "SERVER_PORT=3000" > /app/.env && \
    echo "SERVER_HOST=0.0.0.0" >> /app/.env

# Copy run script
COPY run.sh /
RUN chmod a+x /run.sh

EXPOSE 3000
WORKDIR /app

CMD ["/run.sh"]
