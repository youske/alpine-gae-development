# Alpine Linux miniconda with google_appengine sdk java

FROM youske/alpine-java:latest
MAINTAINER youske miyakoshi <youske@gmail.com>

ENV LANG=C.UTF-8
ENV APPENGINE_VERSION=1.9.38
ENV SDK=https://storage.googleapis.com/appengine-sdks/featured/appengine-java-sdk-${APPENGINE_VERSION}.zip

ENV PACKAGE="unzip wget git tmux " \
    BUILD_PACKAGE="build-base python-dev musl-dev linux-headers" \
    LANGUAGE=${LANG} \
    TARGETDIR=/google_appengine \
    WORK=/home/admin/projects \
    PORT=8080-8089 \
    PATH=${TARGETDIR}:${PATH}

# targetdir /google_appengine
RUN apk update --no-cache && apk upgrade --no-cache && \
    apk add ${PACKAGE} --no-cache && apk add ${BUILD_PACKAGE} --no-cache -t buildpack
RUN wget -q ${SDK} -O /tmp/gae.zip && unzip -q /tmp/gae.zip -d /tmp &&  mv /tmp/appengine-java-sdk-${APPENGINE_VERSION} /google_appengine

# remove buildpack
RUN apk del buildpack --no-cache && rm -rf /tmp/* /var/cache/apk/*

USER admin
COPY notes/ /home/admin/notes

EXPOSE ${PORT}

USER root
WORKDIR ${WORK}
ENTRYPOINT ["switch","shell=su admin -c /bin/bash","--","switch","root=/bin/bash","--","/init"]
CMD ["su","admin","-c","/bin/bash"]
