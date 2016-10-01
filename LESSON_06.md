# Lesson 06: AWS automation

This lesson is going to show off using Ansible to automate tasks
on AWS.  If you don't have an AWS account, or don't want to pay AWS
fees, stop here, you're done.

This lesson also requires considerably more prep.  You'll need to
log into AWS (preferably into a non-production account) and do the following:

1. *VPC*:  pick a test VPC to use and copy its name into the
  host_vars/localhost file.
2. *Security Group* create a security group in that VPC, with SSH,
  HTTP and HTTPS open to the world.  Record this also in host_vars/localhost.
3. *IAM* create an IAM role with full administrative access to your
  AWS account.  Generate keys for this IAM role and copy the secret key and secret.
4. *key* add the name of the PEM key you use for EC2 and add it to
  host_vars/localhost.

Once you've done that, you'll need to add the PEM file to your
ssh-agent:

```
ssh-add ~/credentials/demo-us-west.pem
```

You will need to create another encrypted file for the IAM
credentials and add them to the file aws.yml.  Make sure to
set the vault password to the exact same password as the file
with your docker credentials.

```
ansible-vault create aws.yml
```

```
---
aws_access_key: YOUR AWS ACCESS KEY
aws_secret_key: YOUR AWS SECRET KEY
```

Take a look at the new playbook, which calls three plays and three
roles.  The localhost play is because managing AWS instances has
to be done from localhost.

There's also some trickery with automatically adding the newly
created hosts to the inventory:

```
- name: Add new instance to host group
  add_host: hostname={{ item.public_ip }} groupname=webhosts
  with_items: '{{ec2.instances}}'
```

Due to this loop, this playbook is *not* idempotent; every time you run it, it will add and configure a new EC2 host.

Now that that setup is done, you should be able to run the playbook.

```
ansible-playbook playbook.yml
```

And if you navigate in your browser to the IP address of the node,
you should have a web page come up.

That's all we have for this version of the tutorial.  Maybe more
later; in the meantime, have fun with Ansible!
