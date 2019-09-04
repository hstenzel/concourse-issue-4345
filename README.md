# Concourse bug 4345 recreate

A project to demonstrate but 4345

https://github.com/concourse/concourse/issues/4345

## This works as expected

```
fly -t dev execute --config task.yml --load-vars-from vars.yml --input src=.
```

The results are:

```
executing build 7193 at ...
initializing
src: 90.37 KiB/s 0s
running echo hello world
hello world
succeeded
```

Notice how vars.yml is structured yaml and the map is placed properly in the task and the task succeeds.

## This fails

```
fly -t dev set-pipeline --config pipeline.yml -p concourse-issue-4345
fly -t dev unpause-pipeline -p concourse-issue-4345
fly -t dev trigger-job --watch --job concourse-issue-4345/hello-world
```

The results are:

```
started concourse-issue-4345/hello-world #1

Cloning into '/tmp/build/get'...
fetch: Fetching reference HEAD
WARNING: 'git lfs checkout' is deprecated and will be removed in v3.0.0.
'git checkout' has been updated in upstream Git to have comparable speeds
to 'git lfs checkout'.
da2607f feat: recreate task
failed to create task config from bytes src/task.yml: 1 error(s) decoding:

* 'image_resource' expected a map, got 'string'
errored
```

Notice how `image_resource` was not treated as a map thus the build failed.
