# Lesson 1: ad-hoc commands

In the first lesson, we're going to run some ad-hoc commands against a remote host.

This will first require us to add our credentials to ssh-agent so that we don't need
to identify the keys all the time.  YOURPEMFILE is the name of the PEM file you
have for those instances.

```
cd ~
ssh-agent bash
ssh-add credentials/YOURPEMFILE.pem
```

Second, you'll need to update the inventory.  Currently, the inventory has
one line pointing to the docker build host, and you'll need to set it to
the public IP address of your AWS instance.  $EDITOR is your choice of editor.

```
cd /root/ansible101
$EDITOR inventory.yml
```

Next, let's verify that we can connect:

```
ansible dockerbuild -i inventory.yml -m ping -u centos
52.89.12.136 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

However, when you ran that, you probably got host key validation.
It's also annoying to type "-i inventory.yml" all the time, so we're going
to set up a local ansible.cfg:

```
$EDITOR ansible.cfg
[defaults]
inventory = inventory.yml
host_key_checking = false
```

Now you should be able to connect without key challenges or naming the inventory:

```
ansible dockerbuild -m ping -u centos
```

That's still annoying, though; let's add the user to the inventory file:

```
$EDITOR inventory.yml
[dockerbuild]
52.89.12.136 ansible_user=centos ansible_become=true

ansible dockerbuild -m ping
```

Next, we're going to use ad-hoc commands, which simply run a shell command on
the remote host.  In this case, we're going to install docker.  That's why we
added "ansible_become" in the inventory file, so that we could sudo to install
it.

```
ansible dockerbuild -a 'yum install -y docker'

(lots of yum output)
```

Of course, ad-hoc commands aren't really very useful for repeatable automation.
For that, you need modules and playbooks.

Please check out the next git commit, and proceed to LESSON_02.md.
