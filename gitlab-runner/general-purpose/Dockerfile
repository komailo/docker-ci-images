ARG GITLAB_RUNNER_IMAGE_TAG=alpine-v16.11.1
ARG GITLAB_RUNNER_IMAGE_TYPE=gitlab-runner

FROM gitlab/${GITLAB_RUNNER_IMAGE_TYPE}:${GITLAB_RUNNER_IMAGE_TAG}

RUN apk update --no-cache && apk upgrade --no-cache && rm -rf /var/cache/apk/*
