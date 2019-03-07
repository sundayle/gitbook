Error response from daemon: conflict: unable to delete f435814af1f1 (cannot be forced) - image has dependent child images

docker image inspect --format='{{.RepoTags}} {{.Id}} {{.Parent}}' $(docker image ls -q --filter since=f435814af1f1)





 for i in `docker images | grep none | awk '{print $3}'`;do docker image inspect --format='{{.RepoTags}} {{.Id}} {{.Parent}}' $(docker image ls -q --filter since=$i);done