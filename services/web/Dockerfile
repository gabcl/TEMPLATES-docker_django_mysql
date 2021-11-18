# pull official base image
FROM python:3.9.5-slim-buster


# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1


# USUÁRIO -------------------------------------------------------------------------
# Cria usuario
RUN groupadd --gid 1001 appuser && \
    useradd --create-home --system --uid 1001 --gid appuser appuser


# set work directory
RUN install --directory --owner appuser --group appuser /usr/src/app
WORKDIR /usr/src/app



# Dependências -------------------------------------------------------------------------
#RUN apt-get update && apt-get install -y unixodbc g++ default-libmysqlclient-dev unixodbc-dev

# install Microsoft SQL Server requirements.
ENV ACCEPT_EULA=Y
RUN apt-get update -y \
  && apt-get install -y --no-install-recommends curl g++ gnupg unixodbc-dev default-libmysqlclient-dev 


# Add SQL Server ODBC Driver 17 for Ubuntu 18.04
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
  && curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list \
  && apt-get update \
  && apt-get install -y --no-install-recommends --allow-unauthenticated msodbcsql17 mssql-tools \
  && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile \
  && echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc

# Configurações para o pyodbc
RUN sed -i 's/TLSv1.2/TLSv1/g' /etc/ssl/openssl.cnf
RUN sed -i 's/SECLEVEL=2/SECLEVEL=1/g' /etc/ssl/openssl.cnf



# PACOTES -------------------------------------------------------------------------
RUN pip install --upgrade pip
COPY --chown=appuser:appuser ./requirements.txt /usr/src/app/requirements.txt
RUN pip install -r requirements.txt


# PROJETO -------------------------------------------------------------------------
# Copia arquivos
COPY --chown=appuser:appuser . /usr/src/app/

# static dir
RUN mkdir -p /usr/src/app/static && chown appuser:appuser /usr/src/app/static

# Usuário
USER appuser


# ENTRYPOINT -------------------------------------------------------------------------
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