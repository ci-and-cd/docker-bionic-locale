
FROM ubuntu:18.04


ARG IMAGE_ARG_APT_MIRROR

ARG IMAGE_ARG_ENCODING
ARG IMAGE_ARG_LANGUAGE
ARG IMAGE_ARG_LOCALE
ARG IMAGE_ARG_REGION
#ARG IMAGE_ARG_TZ
ARG IMAGE_ARG_TZ_AREA
ARG IMAGE_ARG_TZ_ZONE


COPY --chown=root:root docker /data


RUN set -ex \
    && echo ===== Set locale ===== \
    && echo "tzdata tzdata/Areas select ${IMAGE_ARG_TZ_AREA:-Etc}\n\
tzdata tzdata/Zones/${IMAGE_ARG_TZ_AREA:-Etc} select ${IMAGE_ARG_TZ_ZONE:-UTC}\n\n\
locales locales/locales_to_be_generated    multiselect ${IMAGE_ARG_LOCALE:-en_US}.${IMAGE_ARG_ENCODING:-UTF-8} ${IMAGE_ARG_ENCODING:-UTF-8}\n\
locales locales/default_environment_locale select      ${IMAGE_ARG_LOCALE:-en_US}.${IMAGE_ARG_ENCODING:-UTF-8}\n" > /etc/debconf.txt \
    && sed -i "s/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/${IMAGE_ARG_APT_MIRROR:-archive.ubuntu.com}\/ubuntu\//g" /etc/apt/sources.list \
    && apt-get -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get -qy install --reinstall locales tzdata debconf \
    && debconf-set-selections /etc/debconf.txt \
    && echo "${IMAGE_ARG_TZ_AREA:-Etc}/${IMAGE_ARG_TZ_ZONE:-UTC}" > /etc/timezone \
    && dpkg-reconfigure -f noninteractive tzdata \
    && echo "${IMAGE_ARG_LOCALE:-en_US}.${IMAGE_ARG_ENCODING:-UTF-8} ${IMAGE_ARG_ENCODING:-UTF-8}" > /etc/locale.gen \
    && dpkg-reconfigure -f noninteractive locales \
    && apt-get -q -y autoremove \
    && apt-get -q -y clean && rm -rf /var/lib/apt/lists/* && rm -f /var/cache/apt/*.bin


ENV LANG ${IMAGE_ARG_LOCALE:-en_US}.${IMAGE_ARG_ENCODING:-UTF-8}
ENV LC_ALL ${IMAGE_ARG_LOCALE:-en_US}.${IMAGE_ARG_ENCODING:-UTF-8}
