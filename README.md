# uos is my silverblue variant based on the ublue startingpoint

## Installation

> **Warning**
> [This is an experimental feature](https://www.fedoraproject.org/wiki/Changes/OstreeNativeContainerStable) and should not be used in production, try it in a VM for a while!

To rebase an existing Silverblue/Kinoite installation to the latest build:

- First rebase to the unsigned image, to get the proper signing keys and policies installed:
  ```
  sudo rpm-ostree rebase ostree-unverified-registry:ghcr.io/grantmacken/uos:latest
  ```
- Reboot to complete the rebase:
  ```
  systemctl reboot
  ```
- Then rebase to the signed image, like so:
  ```
  sudo rpm-ostree rebase ostree-image-signed:ghcr.io/grantmacken/uos:latest
  ```
- Reboot again to complete the installation
  ```
  systemctl reboot
  ```

<!--

This repository builds date tags as well, so if you want to rebase to a particular day's build:

```
sudo rpm-ostree rebase ostree-image-signed:docker://ghcr.io/ublue-os/startingpoint:20230403
```

This repository by default also supports signing.

The `latest` tag will automatically point to the latest build. That build will still always use the Fedora version specified in `recipe.yml`, so you won't get accidentally updated to the next major version.

-->


## Staying Up to Date with Upstream

<!--
- Last tracking [github.com/ublue-os/startingpoint](https://github.com/ublue-os/startingpoint) at [a700132](https://github.com/ublue-os/startingpoint/commit/a70013277e209a042d546d5a43ea3d39e26b1a08)
If necessary, set upstream with ublue-os

-->

```shell
git remote add upstream https://github.com/ublue-os/startingpoint.git
```

Fetch & rebase

```shell
# Retrieve latest changes from upstream's template.
git fetch upstream template
git checkout template
git rebase upstream/template
# resolve any conflicts
git push

# Rebase your own "live" changes onto the latest template.
git checkout live
git rebase --onto live template

# Perform a force-push to update your "live" branch on GitHub, to deploy.
# The "lease" ensures that you won't overwrite "live" if GitHub's version
# is different than your local version (ie. if a team member pushed to it).
git push --force-with-lease
```
<!--

## ISO

This template includes a simple Github Action to build and release an ISO of your image. 

To run the action, simply edit the `boot_menu.yml` by changing all the references to startingpoint to your repository. This should trigger the action automatically.

The Action uses [isogenerator](https://github.com/ublue-os/isogenerator) and works in a similar manner to the official Universal Blue ISO. If you have any issues, you should first check [the documentation page on installation](https://universal-blue.org/installation/). The ISO is a netinstaller and should always pull the latest version of your image.

Note that this release-iso action is not a replacement for a full-blown release automation like [release-please](https://github.com/googleapis/release-please).

-->

## `just`

The [`just`](https://just.systems/) command runner is included in all `ublue-os/main`-derived images.

You need to have a `~/.justfile` with the following contents and `just` aliased to `just --unstable` (default in posix-compatible shells on ublue) to get started with just locally.
```
!include /usr/share/ublue-os/just/main.just
!include /usr/share/ublue-os/just/nvidia.just
!include /usr/share/ublue-os/just/custom.just
```
Then type `just` to list the just recipes available.

The file `/usr/share/ublue-os/just/custom.just` is intended for the custom just commands (recipes) you wish to include in your image. By default, it includes the justfiles from [`ublue-os/bling`](https://github.com/ublue-os/bling), if you wish to disable that, you need to just remove the line that includes bling.just.

See [the just-page in the Universal Blue documentation](https://universal-blue.org/guide/just/) for more information.
