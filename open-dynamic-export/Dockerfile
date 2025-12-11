ARG BUILD_FROM=ghcr.io/hassio-addons/base:16.3.2
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install Node.js and dependencies
RUN apk add --no-cache \
    nodejs \
    npm \
    git

# Clone and build Open Dynamic Export
WORKDIR /app
RUN git clone https://github.com/longzheng/open-dynamic-export.git . && \
    npm ci && \
    npm run build

# Copy run script
COPY run.sh /
RUN chmod a+x /run.sh

# Expose port
EXPOSE 3000

WORKDIR /app

# Run
CMD ["/run.sh"]
