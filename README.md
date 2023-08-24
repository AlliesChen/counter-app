
# Counter App

This app is followed the instruction-- [Deploy Web Applications All Over the World](https://www.epicweb.dev/tutorials/deploy-web-applications) by Kent C. Dodds


## Documentation

[Docker docs - Dockerfile reference](https://docs.docker.com/engine/reference/builder/)-- Container and images that are used in this app.

[Fly.io](https://fly.io/)-- The platform for running the app and database.

[Github Action](https://docs.github.com/en/actions)-- For CI/CD.


## Lessons Learned

`[experimental]` is not needed in `fly.toml`, fly.io has the features be enabled by default now.

> Somehow `auto_rollback` is not working for my apps.

To make LiteFS `consul` work, use `litefs attach` instead, refer to [New commands: `fly consul attach` and `fly consul detach`](https://community.fly.io/t/new-commands-fly-consul-attach-and-fly-consul-detach/12418).

As I have the features run on staging server, `-a` flag should be added to the CLI command followed by the `{app_name}`. For example:

`fly ssh -a my-app-staging console -C bash`

The other thing that different from the course as using staging server is that `[primary_region]` should be included in `fly.toml`. Otherwise, the deployment would fail and show an error like: `nrt != iad`.

### Multi-region part

`fly vol create --region` followed by `fly scale count {number}` does not have the created volume with different region attaches to it, but to create one new volume with the primary region and assign a new machine to that volume.

Use `fly machine clone {machine_id} --attach-volume {volume_id}:{destination}` instead for assigning the manually created volume in different region to a new machine, refer to [Add volume storage #Clone the Machine](https://fly.io/docs/apps/volume-storage/#clone-the-machine).

For testing the multi-region does work, I use [modheader](https://modheader.com/) instead.