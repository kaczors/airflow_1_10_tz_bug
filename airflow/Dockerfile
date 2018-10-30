FROM python:3.6-slim

ENV TZ=Europe/Zurich
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ARG AIRFLOW_VERSION=1.10.0
ARG AIRFLOW_HOME=/usr/local/airflow
ENV AIRFLOW_GPL_UNIDECODE=yes

RUN mkdir -p /usr/share/man/man1
RUN apt-get update -yqq && apt-get upgrade -yqq
RUN apt-get install -yqq openssh-client ca-certificates-java openjdk-8-jre-headless python3-pip python3-dev libkrb5-dev libsasl2-dev libssl-dev libffi-dev build-essential libblas-dev liblapack-dev libpq-dev procps sudo
RUN useradd -ms /bin/bash -d ${AIRFLOW_HOME} airflow && adduser airflow sudo
RUN pip install apache-airflow[crypto,gcp_api,postgres,slack] pendulum==1.4.4 docker==2.7.0 paramiko==2.4.2
RUN apt-get purge --auto-remove -yqq $buildDeps && apt-get autoremove -yqq --purge && apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/man /usr/share/doc /usr/share/doc-base

COPY entrypoint.sh /entrypoint.sh

RUN chown -R airflow ${AIRFLOW_HOME}
RUN chmod a+rx /entrypoint.sh

EXPOSE 8080 8793

USER airflow
WORKDIR ${AIRFLOW_HOME}
RUN ssh-keygen -q -t rsa -N '' -f ${AIRFLOW_HOME}/.ssh/id_rsa

ENTRYPOINT ["/entrypoint.sh"] 
