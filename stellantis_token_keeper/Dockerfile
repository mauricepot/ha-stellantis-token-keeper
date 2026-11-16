ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base-debian:bookworm
FROM ${BUILD_FROM}

# The Supervisor passes the config.yaml version as BUILD_VERSION
ARG BUILD_VERSION=dev
ENV ADDON_VERSION=${BUILD_VERSION}

ENV LANG=C.UTF-8 \
    PIP_BREAK_SYSTEM_PACKAGES=1 \
    PLAYWRIGHT_BROWSERS_PATH=/opt/ms-playwright \
    PYTHONUNBUFFERED=1

# Debian base (not Alpine): Playwright's Chromium needs glibc.
RUN apt-get update \
    && apt-get install -y --no-install-recommends python3 python3-pip libatomic1 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app
COPY rootfs/app/requirements.txt .
RUN pip3 install --no-cache-dir -r requirements.txt \
    && playwright install --with-deps chromium \
    && rm -rf /var/lib/apt/lists/*

COPY rootfs/ /
RUN chmod +x /run.sh

EXPOSE 3000
CMD ["/run.sh"]
