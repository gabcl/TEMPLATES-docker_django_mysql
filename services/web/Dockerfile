# ====================================
# COMPILE
# ====================================
FROM python:3.11-alpine AS compile-image

# Install packages
RUN apk update && \
    apk add --no-cache g++ unixodbc-dev mariadb-dev


# Create venv
RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"


# pip install
COPY requirements.txt .
RUN pip install -r requirements.txt



# ====================================
# BUILD
# ====================================
FROM python:3.11-alpine AS build-image


# Copy venv from compile-image
COPY --from=compile-image /opt/venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"


# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1


# User ------------------------------------
# Cria usuario
RUN addgroup --gid 1001 appuser && \
    adduser -S --uid 1001 -G appuser appuser


# set work directory
RUN install --directory --owner appuser --group appuser /usr/src/app
WORKDIR /usr/src/app



# Dependencies ------------------------------------
RUN set -x && \
    apk update && \
    apk add --no-cache bash mariadb-dev


# Project ------------------------------------
# Copia arquivos
COPY --chown=appuser:appuser . /usr/src/app/

# static dir
RUN mkdir -p /usr/src/app/static && chown appuser:appuser /usr/src/app/static

# Usuário
USER appuser


# ENTRYPOINT ------------------------------------
# ENTRYPOINT ["tail", "-f", "/dev/null"]


# LABELS
ARG IMAGE_NAME
ARG IMAGE_TITLE
ARG IMAGE_DESCRIPTION
ARG IMAGE_VERSION
ARG IMAGE_AUTHORS
ARG IMAGE_VENDOR
ARG IMAGE_CREATED
ARG IMAGE_LICENSES
ARG IMAGE_URL
ARG IMAGE_DOCUMENTATION
ARG IMAGE_SOURCE
ARG IMAGE_REVISION

LABEL org.opencontainers.image.created=$IMAGE_CREATED
LABEL org.opencontainers.image.authors=$IMAGE_AUTHORS
LABEL org.opencontainers.image.url=$IMAGE_URL
LABEL org.opencontainers.image.documentation=$IMAGE_DOCUMENTATION
LABEL org.opencontainers.image.source=$IMAGE_SOURCE
LABEL org.opencontainers.image.version=$IMAGE_VERSION
LABEL org.opencontainers.image.revision=$IMAGE_REVISION
LABEL org.opencontainers.image.vendor=$IMAGE_VENDOR
LABEL org.opencontainers.image.licenses=$IMAGE_LICENSES
LABEL org.opencontainers.image.ref.name=$IMAGE_NAME
LABEL org.opencontainers.image.title=$IMAGE_TITLE
LABEL org.opencontainers.image.description=$IMAGE_DESCRIPTION