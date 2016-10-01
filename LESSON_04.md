# Lesson 04: Secrets

First, set the EDITOR shell variable to your favorite editor (whatever it is):

```
export EDITOR=nano
```

Then create an encrypted password file using ansible-vault:

```
ansible-vault create pass.yml
```

This will prompt you for a passphrase, and then open your editor.  Remember this
passphrase!  Without it, you won't be able to read the file.  For this tutorial,
I suggest you use "ansible".  Add the following text, replacing the values
with your actual docker hub login:

```
---
docker_username: myUsername
docker_password: myHubPassword
```

Save, and the file's encrypted.

Now, look at the playbook again.  First, you'll see that we're including the
pass.yml file.

```
vars_files:
  - pass.yml
```

We use this in docker_login:

```
- name: log into docker hub
  docker_login:
    username: "{{ docker_username }}"
    password: "{{ docker_password }}"
    email: "{{ owner_email }}"
```

Second, you need to change the owner_email to match the email you use with your
Docker Hub account.  

```
    owner_email: "you@yourdomain.tld"
```

If you take a look at ansible.cfg, you'll see that we've added an
ask_vault_pass setting, so that it asks you for the passphrase.

```
ask_vault_pass = true
```

Finally, we've added an extra line to dockerfile.j2, to make sure that it triggers
a rebuild of the docker image because the file has changed.  If you changed the
owner_email, this would happen anyway.

Now run it.  It should upload the image to your Docker Hub account.

```
PLAY RECAP *********************************************************************
52.89.12.136               : ok=6    changed=3    unreachable=0    failed=0   
```

Now, please proceed to Lesson 05.
