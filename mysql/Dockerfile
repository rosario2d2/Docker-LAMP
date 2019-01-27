FROM mysql:5.6.34

ARG PRIVILEGED_USER
ARG TIMEZONE
ARG LANGUAGE
ARG LOCALE

RUN apt-get update && apt-get install -y \
        tzdata \
        locales

# System Timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone

# Set Locales
RUN sed -i "s/# ${LOCALE}.UTF-8 UTF-8/${LOCALE}.UTF-8 UTF-8/" /etc/locale.gen && locale-gen

ENV LANG ${LOCALE}.UTF-8
ENV LANGUAGE ${LOCALE}:${LANGUAGE}
ENV LC_ALL ${LOCALE}.UTF-8

# User ID & Group ID
RUN usermod -u 1000 mysql && groupmod -g 1000 mysql

# Privileged user
RUN echo "GRANT ALL PRIVILEGES ON \`db\_%\`.* TO '${PRIVILEGED_USER}'@'%';" > /docker-entrypoint-initdb.d/privileges.sql

# Custom Prompt .bashrc
RUN echo "PS1='\[\033[1;36m\][\u@\h:\[\033[1;34m\]\w\[\033[0;35m\]\[\033[1;36m\]]# \[\033[0m\]'" > /root/.bashrc
