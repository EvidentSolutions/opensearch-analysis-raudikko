ARG OS_VERSION=2.5.0
FROM opensearchproject/opensearch:${OS_VERSION}
# Needs to repeat the ARG here or it won't work
# https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG OS_VERSION
COPY build/distributions/opensearch-analysis-raudikko-0.1-os${OS_VERSION}.zip /install/opensearch-analysis-raudikko.zip
RUN /usr/share/opensearch/bin/opensearch-plugin install \
    --batch file:///install/opensearch-analysis-raudikko.zip \
