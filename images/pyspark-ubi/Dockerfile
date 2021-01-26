FROM registry.access.redhat.com/ubi8/ubi-minimal:8.3

WORKDIR /opt/pyspark

COPY Pipfile /opt/pyspark/

# I'm using python 3.6 specifically, as it works broadly
# and the ODH spark-cluster images also use it.
# 'which' is used by pipenv to locate a python interpreter to run
# Install java as pyspark still requires jvm spark libs to operate.
RUN microdnf install python36 which java-1.8.0-openjdk-headless \
 && microdnf clean all \
 && pip3 install pipenv

# environment for doing the pipenv install of the Pipfile
# WORKON_HOME tells pipenv to put the virtualenv in same dir so
# it gets properly used when container runs using anonymous uid
ENV LANG="C.UTF-8" \
    LC_ALL="C.UTF-8" \
    LC_CTYPE="C.UTF-8" \
    WORKON_HOME=/opt/pyspark \
    PIPENV_NOSPIN=1 \
    JAVA_HOME=/usr/lib/jvm/jre-1.8.0

# after the install, deleting caches and /tmp saves over 800MB
RUN cd /opt/pyspark \
 && pipenv --python 3.6 install \
 && rm -rf /tmp/* /root/.cache /root/.local \
 && chown -R 9998:0 /opt/pyspark \
 && chmod -R g+rwX /opt/pyspark

# Define an entrypoint that does proper signal handling
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-static /tini
RUN chmod a+rx /tini
ENTRYPOINT ["/tini", "--"]

CMD [ "echo", "No default command" ]

# Emulate an anonymous uid, similar to executing in an OpenShift environment
USER 9999:0
