# Ansible 101 Repository

This github repo backs the Ansible 101 half-hour introduction, last given by
Josh Berkus at PyDX 2016.  As with other git-based tutorials, the way it works
is to go from simple to complex, one git commit at a time.  To follow along,
go back to the first git commit and advance.

## Prerequisites

Before beginning the tutorial, you must have the following set up on AWS:

* an AWS account
* 1 AWS instance running CentOS 7 or a compatible OS which uses DNF
* the public IP address and a PEM file for the above instance, saved to ~/credentials/
* an IAM admin account with keys for AWS login, saved to ~/credentials/

You will also need a Docker Hub account and its username and password.  In the
examples below we use the jberkus account on Hub, but you'll need to change that
to your account.

You'll need to download this git repo and put it on your local machine.  Again,
substitute /home/jberkus for wherever your homedir is.

Finally, you'll need to install ansible locally on your machine.  There's two
ways to do that.  

Way #1: Install ansible locally:

* Install ansible 2.1 or later
* Install boto and other requirements for ansible EC2 module
* Install git, if you don't already have it.

Way #2: use the docker image, if you run docker:

```
docker run -it --rm -v REPOLOC:/root/ansible101:Z \
   -v CREDLOC:/root/credentials:Z jberkus/fedora-ansible:24 /bin/bash
```

Where REPOLOC is wherever you've checked out this git repo, and CREDLOC is wherever
you've stashed your PEM files for AWS login.

The rest of the tutorial assumes you're using the docker container, so if you're
not, you may need to adjust some file paths.

Please proceed to the file "LESSON_01.md".

The way you go to lesson 01 is to proceed to the first commit on this repository:

```
git clone https://github.com/jberkus/ansible101.git
git checkout e947e27
```

After this, you can advance commits one at a time by:

```
git checkout HEAD@{1}
```
