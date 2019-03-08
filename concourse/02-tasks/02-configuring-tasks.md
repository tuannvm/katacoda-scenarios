Create a new task configuration via the CLI: `touch task_ubuntu_ls.yml`{{execute terminal}}

Open the file: `hello-world/task_ubuntu_ls.yml`{{open}}

First we need to specify the `platform` which is required and determines the pool of workers that the task can run against. The base deployment provides Linux workers. Traditionally other options are `windows` or `darwin` depending on your workers available.

<pre class="file" data-filename="hello-world/task_ubuntu_ls.yml" data-target="replace">---
platform: linux
</pre>

Next we define the base image for the containerized workspace the task will be ran in with the `image_resource` configuration. this allows your task to have any prepared dependencies that it needs to run. Instead of installing dependencies each time during a task you might choose to pre-bake them into an image to make your tasks much faster.

Only `type` and `source` are required. Concourse has very [basic requirements](https://concourse-ci.org/tasks.html#task-image-resource) on what such a base image resource should look like, but the reference implementation is the [Docker image resource](https://github.com/concourse/docker-image-resource) type. A future scenario will look into building and using your own Docker images.

For the `docker-image` resource only the `repository` is required, the `tag` is optional and is `latest` by default.

<pre class="file" data-filename="hello-world/task_ubuntu_ls.yml" data-target="append">
image_resource:
  type: docker-image
  source:
    repository: ubuntu
</pre>

Finally we define the command to execute in the container using the `run` configuration. Only `path` is required (commonly a script or command) a string array of arguments is optionally provided through `args`.

<pre class="file" data-filename="hello-world/task_ubuntu_ls.yml" data-target="append">
run:
  path: ls
  args:
  - "-alR"
</pre>

Run this task as follows:

```
fly -t tutorial e -c task_ubuntu_ls.yml
```{{execute terminal}}

Note: `e` is the alias for `execute`