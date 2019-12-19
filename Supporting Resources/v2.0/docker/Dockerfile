FROM python:3.7-slim-buster AS python

FROM python AS builder

ARG userid
ARG groupid

WORKDIR /build

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils pandoc wget make xsltproc poppler-utils vim-tiny less
RUN wget --quiet https://downloads.wkhtmltopdf.org/0.12/0.12.5/wkhtmltox_0.12.5-1.buster_amd64.deb
RUN apt install -y ./wkhtmltox_0.12.5-1.buster_amd64.deb 
RUN rm ./wkhtmltox_0.12.5-1.buster_amd64.deb && rm -rf /var/lib/apt/*

RUN groupadd -g $groupid appgroup && useradd --home-dir /app -m -u $userid -g $groupid appuser

COPY requirements.txt .
RUN pip install -r requirements.txt

FROM builder AS base

COPY samm2yaml2pdf.tgz .

WORKDIR /app
RUN tar xzf /build/samm2yaml2pdf.tgz --strip-components 2 && chown -R $userid:$groupid /app/*
USER appuser

