# Lesson 05: Roles

Roles are a way of supplying file organization to long playbooks.

Take a look at playbook.yml now, you'll see that it's been reduced to
5 lines:

```
---
- hosts: dockerbuild
  vars_files:
    - pass.yml
  roles:
    - dockerbuild
```

Where all the action went was into the group_vars and roles directories.

First, we created two files in the group_vars directory:

* all: variables for all groups
* dockerbuild: variables for the dockerbuild group only

Since we've made changes to the repo, you'll probably have to re-do any changes you made, such as to the owner_email variable.

Second, we created the roles/dockerbuild directory, and created three
subdirectories in it:

* templates: holds all templates
* handlers: file main.yml defines all handlers
* tasks: file main.yml defines all tasks

Check the contents of each to see what's included in them.

By defining the directory structure this way, those files are automatically
included when we reference the role.

Try running it again, you'll see.

```
PLAY RECAP *********************************************************************
52.89.12.136               : ok=6    changed=2    unreachable=0    failed=0
```

Then proceed to Lesson 06.
