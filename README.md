# Headless Ubuntu/Xfce containers with VNC/noVNC

## Project `accetto/ubuntu-vnc-xfce-g3`

Version: G3v4

***

[User Guide][this-user-guide] - [Docker Hub][this-docker] - [Changelog][this-changelog] - [Wiki][this-wiki] - [Discussions][this-discussions]

![badge-github-release][badge-github-release]
![badge-github-release-date][badge-github-release-date]
![badge-github-stars][badge-github-stars]
![badge-github-forks][badge-github-forks]
![badge-github-open-issues][badge-github-open-issues]
![badge-github-closed-issues][badge-github-closed-issues]
![badge-github-releases][badge-github-releases]
![badge-github-commits][badge-github-commits]
![badge-github-last-commit][badge-github-last-commit]

***

- [Headless Ubuntu/Xfce containers with VNC/noVNC](#headless-ubuntuxfce-containers-with-vncnovnc)
  - [Project `accetto/ubuntu-vnc-xfce-g3`](#project-accettoubuntu-vnc-xfce-g3)
    - [Introduction](#introduction)
    - [Building images](#building-images)
    - [Image generations](#image-generations)
    - [Project versions](#project-versions)
    - [Project goals](#project-goals)
    - [Changes and new features](#changes-and-new-features)
      - [Naming scheme](#naming-scheme)
      - [Slimmer images](#slimmer-images)
      - [Fewer and more flexible Dockerfiles](#fewer-and-more-flexible-dockerfiles)
      - [Concept of features](#concept-of-features)
      - [Faster building with `g3-cache`](#faster-building-with-g3-cache)
      - [Overriding container user and group](#overriding-container-user-and-group)
      - [Overriding environment variables and VNC/noVNC parameters](#overriding-environment-variables-and-vncnovnc-parameters)
      - [Different use of version sticker](#different-use-of-version-sticker)
      - [Image metadata](#image-metadata)
      - [Simple self-containing CI](#simple-self-containing-ci)
      - [Separated builder and deployment repositories](#separated-builder-and-deployment-repositories)
      - [Separate README files for Docker Hub](#separate-readme-files-for-docker-hub)
      - [New startup script](#new-startup-script)
    - [Getting help](#getting-help)
    - [Credits](#credits)

### Introduction

This GitHub repository contains resources and tools for building Docker images for headless working.

The images are based on [Ubuntu 22.04 LTS and 20.04 LTS][docker-ubuntu] and include [Xfce][xfce] desktop, [TigerVNC][tigervnc] server and [noVNC][novnc] client.
The popular web browsers [Chromium][chromium] and [Firefox][firefox] are also included.

This [User guide][this-user-guide] describes the images and how to use them.

The content of this GitHub project is intended for developers and image builders.

Ordinary users can simply use the images available in the following repositories on Docker Hub:

- [accetto/ubuntu-vnc-xfce-g3][accetto-docker-ubuntu-vnc-xfce-g3]
- [accetto/ubuntu-vnc-xfce-chromium-g3][accetto-docker-ubuntu-vnc-xfce-chromium-g3]
- [accetto/ubuntu-vnc-xfce-firefox-g3][accetto-docker-ubuntu-vnc-xfce-firefox-g3]

There is also a sibling project [accetto/debian-vnc-xfce-g3][accetto-github-debian-vnc-xfce-g3] containing similar images based on [Debian][docker-debian].

There are also another sibling projects containing images for headless diagramming, drawing, image processing and modelling ([accetto/headless-drawing-g3][accetto-github-headless-drawing-g3]) and for headless programming ([accetto/headless-coding-g3][accetto-github-headless-coding-g3]).

### Building images

You can execute the individual hook scripts in the folder [/docker/hooks/][this-folder-docker-hooks].
However, the provided utilities are more convenient.

The script [builder.sh][this-readme-builder] builds individual images.
The script [ci-builder.sh][this-readme-ci-builder] can build various groups of images or all of them at once.

Before building the images you have to prepare and source the file `secrets.rc` (see [example-secrets.rc][this-example-secrets-file]).

Features that are enabled by default can be explicitly disabled via environment variables.
This allows building even smaller images by excluding the individual features (e.g. noVNC).

The resources for building the individual images and their variations (tags) are in the subfolders of the [/docker/][this-folder-docker] folder.

The individual README files contain quick examples of building the images:

- [accetto/ubuntu-vnc-xfce-g3][this-readme-ubuntu-vnc-xfce-g3]
- [accetto/ubuntu-vnc-xfce-chromium-g3][this-readme-ubuntu-vnc-xfce-chromium-g3]
- [accetto/ubuntu-vnc-xfce-firefox-g3][this-readme-ubuntu-vnc-xfce-firefox-g3]

Each image also has a separate README file intended for Docker Hub.
The final files should be generated by the utility [util-readme.sh][this-readme-util-readme-examples] and then copied to Docker Hub manually.

The following resources describe the image building subject in details:

- [readme-local-building-example.md][this-readme-local-building-example]
- [readme-builder.md][this-readme-builder]
- [readme-ci-builder.md][this-readme-ci-builder]
- [readme-g3-cache.md][this-readme-g3-cache]
- [readme-util-readme-examples.md][this-readme-util-readme-examples]
- [Wiki][this-wiki]

### Image generations

This is the **third generation** (G3) of my headless images.
The **second generation** (G2) contains the GitHub repository [accetto/xubuntu-vnc-novnc][accetto-github-xubuntu-vnc-novnc].
The **first generation** (G1) contains the GitHub repository [accetto/ubuntu-vnc-xfce][accetto-github-ubuntu-vnc-xfce].

### Project versions

This file describes the **fourth version** (G3v4) of the project.

However, also this version keeps evolving.
Please check the [CHANGELOG][this-changelog] for more information about the changes.

The **first version** (G3v1, or simply G3), the **second version** (G3v2, only 20.04 images) and the **third version** (G3v3, 22.04 and 20.04 images) are still available in this **GitHub** repository as the branches `archived-generation-g3v1`, `archived-generation-g3v2` and `archived-generation-g3v3`.

The version `G3v3` brings the following major changes comparing to the previous version `G3v2`:

- The updated startup scripts that support overriding the user ID (`id`) and group ID (`gid`) without needing the former build argument `ARG_FEATURES_USER_GROUP_OVERRIDE`, which has been removed.
- The user ID and the group ID can be overridden during the build time (`docker build`) and the run time (`docker run`).
- The `user name`, the `group name` and the `initial sudo password` can be overridden during the build time.
- The permissions of the files `/etc/passwd` and `/etc/groups` are set to the standard `644` after creating the user.
- The content of the home folder and the startup folder belongs to the created user.
- The created user gets permissions to use `sudo`.
The initial `sudo` password is configurable during the build time using the build argument `ARG_SUDO_INITIAL_PW`.
The password can be changed inside the container.
- The default `id:gid` has been changed from `1001:0` to `1000:1000`.
- Features `NOVNC` and `FIREFOX_PLUS`, that are enabled by default, can be disabled via environment variables.
- If `FEATURES_NOVNC="0"`, then
  - image will not include `noVNC`
  - image tag will get the `-vnc` suffix (e.g. `latest-vnc`, `20.04-firefox-vnc` etc.)
- If `FEATURES_FIREFOX_PLUS="0"` and `FEATURES_FIREFOX="1"`, then
  - image with Firefox will not include the *Firefox Plus features*
  - image tag will get the `-default` suffix (e.g. `latest-firefox-default` or also `latest-firefox-default-vnc` etc.)
- The images based on `Ubuntu 22.04 LTS` and `Ubuntu 20.04 LTS` (formerly only on `Ubuntu 20.04 LTS`).

The version `G3v3` has brought the following major changes comparing to the previous version `G3v2`:

- The images based on `Ubuntu 22.04 LTS (jammy)` and `Ubuntu 20.04 LTS (focal)`.
- An extended, but simplified `tag` set for publishing on the **Docker Hub**.
- The improved builder scripts, including support for the `--target` building parameter

The version `G3v2` has brought the following major changes comparing to the previous version `G3v1`:

- Significantly improved building performance by introducing a local cache (`g3-cache`).
- Auto-building on the **Docker Hub** and using of the **GitHub Actions** have been abandoned.
- The enhanced building pipeline moves towards building the images outside the **Docker Hub** and aims to support also stages with CI/CD capabilities (e.g. the **GitLab**).
- The **local stage** is the default building stage now.
However, the new building pipeline has already been tested also with a local **GitLab** installation in a Docker container on a Linux machine.
- Automatic publishing of README files to the **Docker Hub** has been removed, because it was not working properly any more.
However, the README files for the **Docker Hub** can still be prepared with the provided utility `util-readme.sh` and then copy-and-pasted to the **Docker Hub** manually.

  The changes affect only the building pipeline, not the Docker images themselves.
  The `Dockerfile`, apart from using the new local `g3-cache`, stays conceptually unchanged.

### Project goals

Unlike the first two generations, this `G3` generation aims to support CI/CD.

The main project goal is to develop a **free, simple and self-containing CI/CD pipeline** for **building sets of configurable Docker images** with minimal dependencies outside the project itself.

There are indeed only three service providers used, all available for free:

- [**GitHub**][github] repository contains everything required for building the Docker images.
Both public and private repositories can be used.
**GitHub Gists** are used for persisting data, e.g. badge endpoints.

- [**Docker Hub**][dockerhub] hosts the repositories for the final Docker images.
Public or private repositories can be used.

- [**Badgen.net**][badgen] is used for generating and hosting most of the badges.

None of the above service providers is really required.
All images can be built locally under Linux or Windows and published elsewhere, if needed.

Building process is implemented to minimize image pollution.
New images are pushed to the repositories only if something essential has changed.
This can be overridden if needed.

### Changes and new features

**Hint:** More detailed information about new features can be found in [User guide][this-user-guide] and [Wiki][this-wiki].

#### Naming scheme

Unlike the first two generations, this one will aim to use less Docker Hub **image repositories** with more **image tags**.
For example, previously there have been two Docker Hub repositories `xubuntu-vnc` and `ubuntu-vnc-novnc`.
Now there will be only one Docker Hub repository `accetto/ubuntu-vnc-xfce-g3`.

#### Slimmer images

New images are significantly slimmer than the previous ones.
It's because that the most of the packages are installed with the `apt-get` switch `--no-install-recommends` by default.
This could have consequences, so it is configurable.

#### Fewer and more flexible Dockerfiles

Image variations are build from fewer Dockerfiles.
This is allowed by using *multi-stage builds* and the [Buildkit][docker-doc-build-with-buildkit].
On the other hand, flexible and configurable Dockerfiles are slightly more complex.

#### Concept of features

Flexibility in Dockerfiles is supported by introducing the concept of **features**.
These are variables that control the building process.
For example, the variable **FEATURES_BUILD_SLIM** controls the `--no-install-recommends` switch, the variable **FEATURES_NOVNC** controls the inclusion of `noVNC` and so on.
Some other available features include, for example, the **FEATURES_SCREENSHOOTING** and **FEATURES_THUMBNAILING** variables.
Also the web browsers [Chromium][chromium] and [Firefox][firefox] are defined as features controlled by the variables **FEATURES_CHROMIUM**, **FEATURES_FIREFOX** and **FEATURES_FIREFOX_PLUS**.

Selected features that are enabled by default can be explicitly disabled via environment variables.
See [readme-local-building-example.md][this-readme-local-building-example] for more information.

#### Faster building with `g3-cache`

Building performance has been significantly improved by introducing a local cache (`g3-cache`), which contains the external packages that would be otherwise downloaded by each build.
Refreshing the cache is part of the building pipeline.
The Dockerfiles fall back to the ad-hoc downloading if the local cache is not available.

#### Overriding container user and group

Overriding the container user and group is described in [User guide][this-user-guide-using-containers].

#### Overriding environment variables and VNC/noVNC parameters

Overriding environment variables and VNC/noVNC parameters is described in [User guide][this-user-guide-using-containers].

#### Different use of version sticker

The concept of version sticker has been introduced in the second generation and later implemented also in the first generation.
Check this [Wiki page](https://github.com/accetto/xubuntu-vnc/wiki/Version-sticker) for more information.

However, the usage of the version sticker has been changed in the third generation.

Previously it has been used for testing, if there are any newer packages available by following the *try-and-fail* pattern.
That was sufficient for human controlled building process, but it became a problem for CI/CD.
Therefore it is used differently now.

The verbose version sticker is used for minimizing image pollution.

The short form of the version sticker is available as an image *label* and a *badge* in the README file.
The *version sticker badge* is also linked with the *verbose version sticker gist*, so it is possible to check the actual image configuration even without downloading it.

The version sticker feature is also described in [User guide][this-user-guide-version-sticker].

#### Image metadata

The image metadata are now stored exclusively as image *labels*.
The previous environment variables like **REFRESHED_AT** or **VERSION_STICKER** have been removed.
Most of the labels are namespaced according the [OCI IMAGE SPEC](https://github.com/opencontainers/image-spec) recommendations.
However, the `version-sticker` label is in the namespace `any.accetto` for obvious reasons.

#### Simple self-containing CI

The **first version** of the third generation (G3v1) implemented a relatively simple self-containing CI by utilizing the **Docker Hub** builder *hooks*.
The same build pipeline could be executed also manually if building locally.
For example, an image could be refreshed by executing the `/hooks/pre_build` and `/hooks/build` scripts.
The script `/hooks/push` would push the image to the deployment repository.
The script `/hooks/post_push` would update the *gist* data and trigger the **GitHub Actions** workflow, which would publish the image's README file to the **Docker Hub**.

However, in the middle of the year 2021 the **Docker Hub** removed the auto-building feature from the free plan.
Because one of the main objectives of this project is not to depend on any paid services, I had to remove the dependency on the Docker Hub's auto-building.
There has also not been any use for the **GitHub Actions** any more.

The **second version** (G3v2) of the building pipeline does not depend on the **Docker Hub**'s auto-building feature or the **GitHub Actions** any more.
The original hook scripts have been enhanced and some new ones have been introduced (.e.g. `/hooks/cache`).
The provided utility script `builder.sh` not only allows executing the individual hook scripts, but also implements the complete workflow for building the individual images.
The another utility script `ci-builder.sh` makes use of it and adds the workflow for building sets of images.
Both scripts can optionally also publish the images to the **Docker Hub**.

The **local stage** is the default for the new building pipeline.
Also a local **GitLab** installation in a Docker container has already been successfully tested as a CI building stage.

#### Separated builder and deployment repositories

While there is only one GitHub repository, containing the resources for building all the images,
the **first pipeline version** (G3v1) have used two kinds of repositories on the **Docker Hub**.
A single *builder repository* has been used for building all the images.
The final images have then been published into one or more *deployment repositories*.
This separation allowed to keep permutations by naming reasonable.
Not all repositories had to have the same visibility, they could be private or public as required.

The **second pipeline version** (G3v2) does not really need the **builder repository** for building the images, because it's done outside the **Docker Hub**, which previously hosted the builder repository.
However, the pipeline is still based on the original hook scripts and therefore it depends on the **builder repository** object by managing the names and tags of the images first by building and later by publishing to the **Docker Hub**.

All the images are still build in a single **building repository** and then published to one or more **deployment repositories** on the **Docker Hub**.

The **builder repository** can also server as a **secondary deployment repository** during development and testing.

#### Separate README files for Docker Hub

Each **deployment repository** has its own `README` file for the **Docker Hub**.

The **first pipeline version** (G3v1) has originally published it using the **GitHub Actions** workflows after the image has been pushed.
However, some time later it has stopped working properly.

The **second pipeline version** (G3v2) does not try to publish the `README` file to the **Docker Hub** any more.
However, there is still a utility script, which can prepare the `README` version for the **Docker Hub**, which can be then copy-and-pasted there manually.

The source `README` files for the **Docker Hub** are split into two parts.
The part containing the badge links is separated into a **template file**.
The final `README` files are then generated by the utility script.
These files are usually shorter, because their length is limited by the **Docker Hub**.
Therefore there are also the full-length versions, that are published only on the **GitHub**.

#### New startup script

The startup script has been completely redesigned with the help of the [argbash][argbash-doc] tool and the image [accetto/argbash-docker][accetto-docker-argbash-docker].
Several new startup switches has been added.
For example, there are startup switches `--wait`, `--skip-startup`, `--tail-null`, `--tail-vnc`, `--version-sticker` and `--version-sticker-verbose`.
There are also startup modifiers `--skip-vnc`, `--skip-novnc`, `--debug` and `--verbose`.
Also the utility switches `--help-usage`, `--help` and `--version` are available.

***

### Getting help

If you have found a problem or you just have a question, please check the [User guide][this-user-guide], [Issues][this-issues] and [Wiki][this-wiki] first.
Please do not overlook the closed issues.

If you do not find a solution, you can file a new issue.
The better you describe the problem, the bigger the chance it'll be solved soon.

If you have a question or an idea and you don't want to open an issue, you can use the [Discussions][this-discussions].

### Credits

Credit goes to all the countless people and companies, who contribute to open source community and make so many dreamy things real.

***

[this-user-guide]: https://accetto.github.io/user-guide-g3/

[this-user-guide-version-sticker]: https://accetto.github.io/user-guide-g3/version-sticker/

[this-user-guide-using-containers]: https://accetto.github.io/user-guide-g3/using-containers/

[this-docker]: https://hub.docker.com/u/accetto/

[this-changelog]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/CHANGELOG.md

[this-discussions]: https://github.com/accetto/ubuntu-vnc-xfce-g3/discussions

[this-issues]: https://github.com/accetto/ubuntu-vnc-xfce-g3/issues

[this-wiki]: https://github.com/accetto/ubuntu-vnc-xfce-g3/wiki

[this-folder-docker]: https://github.com/accetto/ubuntu-vnc-xfce-g3/tree/master/docker

[this-folder-docker-hooks]: https://github.com/accetto/ubuntu-vnc-xfce-g3/tree/master/docker/hooks

[this-example-secrets-file]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/examples/example-secrets.rc

[this-readme-ubuntu-vnc-xfce-g3]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/docker/xfce/README.md

[this-readme-ubuntu-vnc-xfce-chromium-g3]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/docker/xfce-chromium/README.md

[this-readme-ubuntu-vnc-xfce-firefox-g3]: https://github.com/accetto/ubuntu-vnc-xfce-g3/tree/master/docker/xfce-firefox

[this-readme-local-building-example]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/readme-local-building-example.md

[this-readme-builder]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/readme-builder.md

[this-readme-ci-builder]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/readme-ci-builder.md

[this-readme-g3-cache]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/readme-g3-cache.md

[this-readme-util-readme-examples]: https://github.com/accetto/ubuntu-vnc-xfce-g3/blob/master/utils/readme-util-readme-examples.md

[accetto-docker-ubuntu-vnc-xfce-g3]: https://hub.docker.com/r/accetto/ubuntu-vnc-xfce-g3

[accetto-docker-ubuntu-vnc-xfce-chromium-g3]: https://hub.docker.com/r/accetto/ubuntu-vnc-xfce-chromium-g3

[accetto-github-xubuntu-vnc-novnc]: https://github.com/accetto/xubuntu-vnc-novnc/

[accetto-docker-ubuntu-vnc-xfce-firefox-g3]: https://hub.docker.com/r/accetto/ubuntu-vnc-xfce-firefox-g3

[accetto-docker-argbash-docker]: https://hub.docker.com/r/accetto/argbash-docker

[accetto-github-headless-coding-g3]: https://github.com/accetto/headless-coding-g3

[accetto-github-headless-drawing-g3]: https://github.com/accetto/headless-drawing-g3

[accetto-github-debian-vnc-xfce-g3]: https://github.com/accetto/debian-vnc-xfce-g3

[accetto-github-ubuntu-vnc-xfce]: https://github.com/accetto/ubuntu-vnc-xfce

[docker-ubuntu]: https://hub.docker.com/_/ubuntu/

[docker-debian]: https://hub.docker.com/_/debian/

[docker-doc-build-with-buildkit]: https://docs.docker.com/develop/develop-images/build_enhancements/

[argbash-doc]: https://argbash.readthedocs.io/en/stable/index.html
[badgen]: https://badgen.net/
[chromium]: https://www.chromium.org/Home
[dockerhub]: https://hub.docker.com/
[firefox]: https://www.mozilla.org
[github]: https://github.com/
[novnc]: https://github.com/kanaka/noVNC
[tigervnc]: http://tigervnc.org
[xfce]: http://www.xfce.org

[badge-github-release]: https://badgen.net/github/release/accetto/ubuntu-vnc-xfce-g3?icon=github&label=release

[badge-github-release-date]: https://img.shields.io/github/release-date/accetto/ubuntu-vnc-xfce-g3?logo=github

[badge-github-stars]: https://badgen.net/github/stars/accetto/ubuntu-vnc-xfce-g3?icon=github&label=stars

[badge-github-forks]: https://badgen.net/github/forks/accetto/ubuntu-vnc-xfce-g3?icon=github&label=forks

[badge-github-releases]: https://badgen.net/github/releases/accetto/ubuntu-vnc-xfce-g3?icon=github&label=releases

[badge-github-commits]: https://badgen.net/github/commits/accetto/ubuntu-vnc-xfce-g3?icon=github&label=commits

[badge-github-last-commit]: https://badgen.net/github/last-commit/accetto/ubuntu-vnc-xfce-g3?icon=github&label=last%20commit

[badge-github-closed-issues]: https://badgen.net/github/closed-issues/accetto/ubuntu-vnc-xfce-g3?icon=github&label=closed%20issues

[badge-github-open-issues]: https://badgen.net/github/open-issues/accetto/ubuntu-vnc-xfce-g3?icon=github&label=open%20issues
