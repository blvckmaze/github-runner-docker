FROM cruizba/ubuntu-dind:23.0.1 as base

ARG RUNNER_VERSION

ENV RUNNER_VERSION=$RUNNER_VERSION 
ENV RUNNER_URL=https://github.com/actions/runner/releases/download/v${RUNNER_VERSION}/actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz 
ENV DEBIAN_FRONTEND="noninteractive" 
ENV TZ=Etc/UTC

RUN groupadd docker \
    && useradd -m runner -s /bin/bash \
    && usermod -aG docker runner \
    && apt-get update -qq \
    && apt-get install git -yqq \
    && apt-get clean

WORKDIR /home/runner/actions-runner

RUN wget ${RUNNER_URL} \
    && tar xzf actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && rm actions-runner-linux-x64-${RUNNER_VERSION}.tar.gz \
    && ./bin/installdependencies.sh

ENTRYPOINT [ "/bin/bash" ]

FROM base

USER runner

ARG RUNNER_TOKEN
ARG REPOSITORY_URL

ENV RUNNER_TOKEN=$RUNNER_TOKEN
ENV REPOSITORY_URL=$REPOSITORY_URL 

RUN ./config.sh --url ${REPOSITORY_URL} --token ${RUNNER_TOKEN} --name github-runner-docker

CMD [ "/home/runner/actions-runner/run.sh" ]
