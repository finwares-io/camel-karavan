# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at

#      http://www.apache.org/licenses/LICENSE-2.0

# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

FROM maven:3.9.9-eclipse-temurin-17

LABEL "org.opencontainers.image.title"="karavan-devmode"
LABEL "org.opencontainers.image.description"="Apache Camel Karavan DevMode"
LABEL "org.opencontainers.image.url"="https://camel.apache.org"
LABEL "org.opencontainers.image.licenses"="Apache 2.0"
LABEL "org.opencontainers.image.version"="4.10.5"

RUN apt-get update && apt-get install git && apt-get clean

ENV JBANG_VERSION=0.126.3
ENV CAMEL_VERSION=4.10.5
ENV KARAVAN="/karavan"
ENV JBANG_DIR="$KARAVAN/.jbang"
ENV MAVEN_CONFIG="$KARAVAN/.m2"
ENV JBANG_REPO=$MAVEN_CONFIG
ENV BUILDER_PATH="$KARAVAN/builder"
ENV CODE_DIR="$KARAVAN/code"
ENV KAMELETS_DIR="$CODE_DIR/kamelets"
ENV PATH="$PATH:$KARAVAN/.jbang/bin"

RUN mkdir -p $JBANG_REPO \
    && mkdir -p $CODE_DIR \
    && mkdir -p $BUILDER_PATH

RUN useradd -rm -d /karavan -s /bin/bash -g root -G sudo -u 1001 karavan
RUN chown -R karavan $KARAVAN

RUN chgrp -R 0 $KARAVAN && chmod -R g+rwX $KARAVAN

USER karavan

RUN curl -Ls https://sh.jbang.dev | bash -s - app setup \
    && jbang trust add  --quiet  https://github.com/apache/camel \
    && jbang app install camel@apache/camel \
    && jbang config set cache-evict never

EXPOSE 8080
WORKDIR $CODE_DIR
CMD jbang $VERBOSE_OPTION -Dcamel.jbang.version=$CAMEL_VERSION camel@apache/camel run --source-dir=$CODE_DIR --console 