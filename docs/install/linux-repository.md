---
last_updated: 2017-10-12
---

# Install GitLab Runner using the official GitLab repositories

NOTE: **Note:**
Any version that is marked as **_Deprecated_** is scheduled to be removed in
GitLab 12.0. Please follow
<https://gitlab.com/gitlab-org/gitlab-runner/issues/3473> for more details.

We provide packages for the currently supported versions of Debian, Ubuntu, Mint, RHEL, Fedora, and CentOS.

| Distribution | Version                    | End of Life date      |
|--------------|----------------------------|-----------------------|
| Debian       | buster                     |                       |
| Debian       | stretch                    | approx. 2022          |
| Debian       | jessie                     | June 2020             |
| Debian       | wheezy **_(Deprecated)_**  | May 2018              |
| Ubuntu       | bionic                     | April 2023            |
| Ubuntu       | artful **_(Deprecated)_**  | July 2018             |
| Ubuntu       | xenial                     | April 2021            |
| Ubuntu       | trusty **_(Deprecated)_**  | April 2019            |
| Mint         | sonya                      | approx. 2021          |
| Mint         | serena                     | approx. 2021          |
| Mint         | sarah                      | approx. 2021          |
| Mint         | rosa  **_(Deprecated)_**   | April 2019            |
| Mint         | rafaela **_(Deprecated)_** | April 2019            |
| Mint         | rebecca **_(Deprecated)_** | April 2019            |
| Mint         | qiana **_(Deprecated)_**   | April 2019            |
| RHEL/CentOS  | 7                          | June 2024             |
| RHEL/CentOS  | 6                          | November 2020         |
| Fedora       | 29                         | approx. November 2019 |
| Fedora       | 28 **_(Deprecated)_**      | approx. May 2019      |
| Fedora       | 27 **_(Deprecated)_**      | approx. November 2018 |
| Fedora       | 26 **_(Deprecated)_**      | approx. June 2018     |

## Prerequisites

If you want to use the [Docker executor], make sure to install Docker before
using the Runner. [Read how to install Docker for your distribution](https://docs.docker.com/engine/installation/).

## Installing the Runner

CAUTION: **Important:**
If you are using or upgrading from a version prior to GitLab Runner 10, read how
to [upgrade to the new version](#upgrading-to-gitlab-runner-10). If you want
to install a version prior to GitLab Runner 10, [visit the old docs](old.md).

To install the Runner:

1. Add GitLab's official repository:

    ```bash
    # For Debian/Ubuntu/Mint
    curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | sudo bash

    # For RHEL/CentOS/Fedora
    curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh | sudo bash
    ```

    >**Note:**
    _Debian users should use APT pinning_
    >
    Since Debian Stretch, Debian maintainers added their native package
    with the same name as is used by our package, and by default the official
    repositories will have a higher priority.
    >
    If you want to use our package you should manually set the source of
    the package. The best would be to add the pinning configuration file.
    Thanks to this every next update of the Runner's package - whether it will
    be done manually or automatically - will be done using the same source:
    >
    ```bash
    cat <<EOF | sudo tee /etc/apt/preferences.d/pin-gitlab-runner.pref
    Explanation: Prefer GitLab provided packages over the Debian native ones
    Package: gitlab-runner
    Pin: origin packages.gitlab.com
    Pin-Priority: 1001
    EOF
    ```

1. Install the latest version of GitLab Runner, or skip to the next step to
   install a specific version:

    ```bash
    # For Debian/Ubuntu/Mint
    sudo apt-get install gitlab-runner

    # For RHEL/CentOS/Fedora
    sudo yum install gitlab-runner
    ```

1. To install a specific version of GitLab Runner:

    ```bash
    # for DEB based systems
    apt-cache madison gitlab-runner
    sudo apt-get install gitlab-runner=10.0.0

    # for RPM based systems
    yum list gitlab-runner --showduplicates | sort -r
    sudo yum install gitlab-runner-10.0.0-1
    ```

1. [Register the Runner](../register/index.md)

After completing the step above, the Runner should be started already being
ready to be used by your projects!

Make sure that you read the [FAQ](../faq/README.md) section which describes
some of the most common problems with GitLab Runner.

## Updating the Runner

Simply execute to install latest version:

```bash
# For Debian/Ubuntu/Mint
sudo apt-get update
sudo apt-get install gitlab-runner

# For RHEL/CentOS/Fedora
sudo yum update
sudo yum install gitlab-runner
```
## Manually download packages

You can manually download the packages from the following URL:
<https://packages.gitlab.com/runner/gitlab-runner>

## Upgrading to GitLab Runner 10

To upgrade GitLab Runner from a version prior to 10.0:

1. Remove the old repository:

    ```
    # For Debian/Ubuntu/Mint
    sudo rm /etc/apt/sources.list.d/runner_gitlab-ci-multi-runner.list

    # For RHEL/CentOS/Fedora
    sudo rm /etc/yum.repos.d/runner_gitlab-ci-multi-runner.repo
    ```

1. Follow the same steps when [installing the Runner](#installing-the-runner),
   **without registering it** and using the new repository.

1. For RHEL/CentOS/Fedora, run:

    ```
    sudo /usr/share/gitlab-runner/post-install
    ```

    CAUTION: **Important:** If you don't run the above command, you will be left
    with no service file. Follow [issue #2786](https://gitlab.com/gitlab-org/gitlab-runner/issues/2786)
    for more information.

[docker executor]: ../executors/docker.md
