# LESSON_03: handlers, loops and templates

Here we're going to make the playbook both more powerful and elegant by
adding a variable loop, and a handler for the repo checkout.

```
$EDITOR playbook.yml
```

Some explanations:  this part now uses a loop over the built-in variable {{ items }}`
to install packages from a list:

```
tasks:
  - name: Install required packages
    yum:
      name: "{{ item }}"
      state: present
    with_items:
      - docker
      - git
      - python-docker-py
```

This part uses a jinja template stored at dockerfile.j2, and combines it with our
variables to produce the file /root/Dockerfile on the remote server:

```
- name: write dockerfile
  template:
    src: dockerfile.j2
    dest: /root/Dockerfile
```

Take a look at `dockerfile.j2`.  You'll see that it's a standard jinja template.

Now, we also have a handler which is called by both the dockerfile template and
the git checkout:

```
notify:
  - build docker image
handlers:
- name: build docker image
docker_image:
  path: /root/
  dockerfile: /root/Dockerfile
  name: "{{ image_name }}"
  tag: "{{ image_tag }}"
  force: true
```

What this does is that, if either Dockerfile or the git checkout are updated,
it triggers a rebuild of the docker image.  However, there's a problem with that:

```
RUNNING HANDLER [build docker image] *******************************************
fatal: [52.89.12.136]: FAILED! => {"changed": false, "failed": true, 
"msg": "Error: configuration for docker.io not found. Try logging into docker.io first."}

```

Proceed to Lesson 04 to see how we resolve that.
