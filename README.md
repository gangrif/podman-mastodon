The goal of this repo is solely to share the work I did in setting up mastodon to run in a podman pod.  

podman lets you generate, and import kubernetes yaml definitions.  I created one for mastodon based on the information that is in the mastodon docker-compose file.  

In mastodon-pod.yaml you will find the definition I use for my mastodon instance, with a lot of the specifics emptied out and replaced with placeholders.  Things like my database password, mail config, the keys that mastodon uses, all have been removed so you can populate these with your own info.  

I used the following how-to as a reference to get mastodon up and running, but of course ditched the docker/docker-compose stuff and leveraged my own podman knowledge to translate it.  

For kubernetes, i think you'll need to define some way to manage ingress, as I did not do that in the pod, I run an external reverse proxy for things like ssl and web termination.  

There is also a lot of duplication of environment definitions, I do know know if there is a way to share these between containers instead of three identical blocks of the same info.  This may be a great place for improvement. 