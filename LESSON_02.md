# Lesson 02: Playbooks, Modules and Variables

Ansible has hundreds of modules covering almost anything you'd want to automate.
You can call these on the command line using -m. For example, a better way
to install docker from Lesson 01 would have been:

```
ansible dockerbuild -m yum -a "name=docker state=present"
```

You'll notice it tells us docker is already installed.  This is because modules
are written to be idempotent.

Then we can start docker daemon and check for containers like so:

```
ansible dockerbuild -m service -a "name=docker state=started"
52.89.12.136 | SUCCESS => {
    "changed": true,
    "name": "docker",
    "state": "started"
}

ansible dockerbuild -m shell -a 'docker ps -a'
```

As nice as this is, it's not very repeatable.  So let's organize it into some
playbooks.  Take a look at the playbook.yml file:

```
$EDITOR playbook.yml
```
