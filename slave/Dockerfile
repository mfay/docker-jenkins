FROM java:9

MAINTAINER Mark Fay <fay.mark@gmail.com>

RUN apt-get -q update && \
    apt-get install -y --no-install-recommends openssh-server && \
    apt-get -q autoremove && \
    apt-get -q clean -y && \
    rm -rf /var/lib/apt/lists/* && \
    rm -f /var/cache/apt/*.bin && \
    sed -i 's|session    required     pam_loginuid.so|session    optional     pam_loginuid.so|g' /etc/pam.d/sshd && \
    mkdir -p /var/run/sshd

RUN useradd -m -d /home/jenkins -s /bin/bash jenkins && \
    echo "jenkins:jenkins" | chpasswd

COPY *.jar /home/jenkins/

EXPOSE 22


CMD java -jar /home/jenkins/swarm-client-2.1-jar-with-dependencies.jar -master http://$MASTER_URL:8080/ -username $SLAVE_USERNAME -passwordEnvVariable SLAVE_PASSWORD -name "slave_${HOSTNAME}" -disableClientsUniqueId
