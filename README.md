# docker-bionic-locale

Ubuntu 18.04 (bionic) locale and timezone setting for multi-stage docker image build.

Dockerfile [ci-and-cd/docker-bionic-locale on Github](https://github.com/ci-and-cd/docker-bionic-locale)

[cirepo/locale on Docker Hub](https://hub.docker.com/r/cirepo/locale/)

## Use this image as a “stage” in multi-stage builds

```dockerfile

FROM ubuntu:18.04
COPY --from=cirepo/locale:en_US.UTF-8_Etc.UTC-bionic-archive /data/root /

```
