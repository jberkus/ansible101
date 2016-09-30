# This dockerfile creates a Fedora-based image for running Ansible

FROM fedora:24

MAINTAINER Josh Berkus "josh@agliodbs.com"

# install ansible requirements and tools
RUN dnf install -y python-pip \
    && dnf install -y ansible ansible-lint ansible-review \
    && dnf install -y sudo openssh\
    && dnf install -y joe nano \
    && dnf install -y git \
    && dnf clean all \
    && pip install boto

# create ansible user and give it sudo
# commented out because of volume permissions issues
#RUN groupadd ansible --gid=2001 && \
#  useradd -m -g ansible --uid=2001 ansible && \
#  usermod -a -G wheel ansible && \
#  usermod -p cOc1.z5LUoxZs ansible
#
#USER ansible

CMD [ "ansible run "]
