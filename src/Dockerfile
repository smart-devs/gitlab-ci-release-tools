# hadolint ignore=DL3007
FROM smartdevs/debian-stretch:latest

ARG DEBIAN_FRONTEND=noninteractive

WORKDIR /tmp/

ARG PACKER_VERSION="1.3.2"
ARG TERRAFORM_VERSION="0.11.10"
ARG KUBERNETES_CLI_VERSION="1.10.3/2018-07-26"
ARG AWS_IAM_AUTHENTICATOR_VERSION="1.10.3/2018-07-26"
ARG HELM_VERSION="v2.11.0"
ARG AWS_CLI_VERSION="1.16.60"
ARG DOCKER_VERSION="17.06.2~ce-0~debian"
ARG RELEASE_TOOLS_RUNTIME_REQUIREMENTS="openssh-client python3 rsync curl"
ARG RELEASE_TOOLS_BUILD_REQUIREMENTS="gnupg apt-transport-https software-properties-common"

COPY FS/ /

# hadolint ignore=DL4006,DL3008,SC2086
RUN set -Eeu; \
    apt-get update -y; \
    apt-get install --no-install-recommends -y ${RELEASE_TOOLS_RUNTIME_REQUIREMENTS:-} ${RELEASE_TOOLS_EXTRA_RUNTIME_REQUIREMENTS:-} ${RELEASE_TOOLS_BUILD_REQUIREMENTS} ${RELEASE_TOOLS_EXTRA_BUILD_REQUIREMENTS:-}; \
    apt-mark auto ${RELEASE_TOOLS_BUILD_REQUIREMENTS} ${RELEASE_TOOLS_EXTRA_BUILD_REQUIREMENTS:-}; \
    curl -fsSL https://download.docker.com/linux/debian/gpg  | apt-key add -; \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian stretch stable"; \
    apt-get update -y; \
    apt-get install -y --no-install-recommends docker-ce=${DOCKER_VERSION}; \
    update-alternatives --install /usr/bin/python python /usr/bin/python3 0; \
	curl -o /usr/local/bin/kubectl -s https://amazon-eks.s3-us-west-2.amazonaws.com/${KUBERNETES_CLI_VERSION}/bin/linux/amd64/kubectl; \
    chmod +x /usr/local/bin/kubectl; \
	curl -o  /usr/local/bin/aws-iam-authenticator -s https://amazon-eks.s3-us-west-2.amazonaws.com/${AWS_IAM_AUTHENTICATOR_VERSION}/bin/linux/amd64/aws-iam-authenticator; \
    chmod +x /usr/local/bin/aws-iam-authenticator; \
	curl -o /tmp/helm.tar.gz -s https://kubernetes-helm.storage.googleapis.com/helm-${HELM_VERSION}-linux-amd64.tar.gz; \
    tar --strip 1 -xvvzf helm.tar.gz; \
    cp helm /usr/local/bin/helm; \
    chmod +x /usr/local/bin/helm; \
	curl -o /tmp/packer.zip -s https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_linux_amd64.zip; \
    unzip packer.zip; \
    cp packer /usr/local/bin/packer; \
    chmod +x /usr/local/bin/packer; \
	curl -o /tmp/terraform.zip -s https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_amd64.zip; \
    unzip terraform.zip; \
    cp terraform /usr/local/bin/terraform; \
    chmod +x /usr/local/bin/terraform; \
    curl -s https://bootstrap.pypa.io/get-pip.py -o get-pip.py; \
    python get-pip.py; \
    pip install awscli==${AWS_CLI_VERSION} --upgrade; \
    chmod +x /usr/local/bin/modprobe;\
    chmod +x /usr/local/bin/docker-entrypoint; \
    apt-get purge --auto-remove -y; \
    dpkg -la | awk '{print $2}' | grep '\-dev' || true | xargs --no-run-if-empty apt-get remove -y; \
    apt-get clean; \
    apt-get --yes --quiet autoremove --purge; \
    find /var/log -type f -print0 | xargs -0 truncate -s0; \
    find /tmp/ -mindepth 1 -exec rm -rf {} +; \
    find /var/tmp/ -mindepth 1 -exec rm -rf {} +; \
    rm -rf \
        /var/lib/apt/lists/* \
        /usr/share/doc/* \
        /usr/share/groff/* \
        /usr/share/info/* \
        /usr/share/linda/* \
        /usr/share/lintian/* \
        /usr/share/man/*/* \
        ;
