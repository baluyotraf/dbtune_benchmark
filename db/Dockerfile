FROM postgres:14-bookworm

RUN apt-get update \
 && apt-get -y install \
    git \
    python3-pip \
    python3.11-venv \
    libpq-dev \
    openjdk-17-jre-headless

RUN python3 -m venv /python \
 && . /python/bin/activate \
 && pip install --upgrade pip \
 && pip install psutil psycopg2

RUN mkdir /benchbase \
 && cd /benchbase \
 && git clone https://github.com/cmu-db/benchbase.git code \
 && cd code \
 && git checkout 979b53b043f934220f703b149f27a7ee0f992b63 \
 && ./mvnw clean package -P postgres \
 && cd target \
 && tar xvzf benchbase-postgres.tgz \
 && mv benchbase-postgres /benchbase \
 && rm -rf /benchbase/code

COPY synthetic_workload/* /benchbase/benchbase-postgres

