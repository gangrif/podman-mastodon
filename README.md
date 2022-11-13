The goal of this repo is solely to share the work I did in setting up mastodon to run in a podman pod.  

podman lets you generate, and import kubernetes yaml definitions.  I created one for mastodon based on the information that is in the mastodon docker-compose file.  

mastodon-configmaps.yaml holds the env variables that mastodon uses for its confit. 
mastodon-pod.yaml depends on those configmaps, and contains all of the pod/container definitions, plus mappings from the env variables to the config map
You will need to define volumes in podman for mastodon-vol-db, mastodon-vol-redis and mastodon-vol-pubsys

I used the following how-to as a reference to get mastodon up and running, but of course ditched the docker/docker-compose stuff and leveraged my own podman knowledge to translate it.  

For kubernetes, i think you'll need to define some way to manage ingress, as I did not do that in the pod, I run an external reverse proxy for things like ssl and web termination.  

There is also a lot of duplication of environment definitions, I do know know if there is a way to share these between containers instead of three identical blocks of the same info.  This may be a great place for improvement. 