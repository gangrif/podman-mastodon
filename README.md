# Mastodon in Podman managed by Quadlet

The goal of this repo is solely to share the work I did in setting up mastodon to run in a podman pod.  

podman lets you generate, and import kubernetes yaml definitions.  I created one for mastodon based on the information that is in the mastodon docker-compose file.  

## Deployment information

mastodon-configmaps.yaml holds the env variables that mastodon uses for its confit.
mastodon-pod.yaml depends on those configmaps, and contains all of the pod/container definitions, plus mappings from the env variables to the config map
You will need to decide how you would like to host mastodon's persistent data (database, cache, things like that) and modify the mastodon-pod.yaml accordingly.  I used hostpath definitions.

I used the following how-to as a reference to get mastodon up and running, but of course ditched the docker/docker-compose stuff and leveraged my own podman knowledge to translate it.  

[https://www.linode.com/docs/guides/install-mastodon-server-on-centos-stream/]

For kubernetes, i think you'll need to define some way to manage ingress, as I did not do that in the pod, I run an external reverse proxy for things like ssl and web termination.  

## Oauth and Vapid keys

In [https://www.linode.com/docs/guides/install-mastodon-server-on-centos-stream/](the howto above), there is information on how to generate the OAuth and Vapid keys that you will rquire (these go into the configmap once generated)

## File placement, and systemd

This uses quadlet, once you have modified the configmap, place the configmap and pod yaml files in a common directory (you could even place them in /etc/containers/systemd if you would like), then inside of quadlet/mastodon-pod.kube, modify the paths to where you stored these yaml files.

Then place quadlet/mastodon-pod.kube in /etc/containers/systemd, and issue the command systemctl daemon-reload

Once systemd reloads, you should bel able to systemctl start mastodon-pod, and it will start up.

Updating mastodon after that is generally easy.  Check the mastodon release notes on whether any tasks are required after the container is updated.  Then update the container images in mastodon-pod.yaml, and systemctl restart mastodon-pod.service

Quadlet will destroy and re-create the pod on restart, and this will pull the latest mastodon images.
