# GitLab CI/CD

## Install and configure a runner on Fedora

The install of `gitlab-runner` was carried out on Fedora 30 from a repository with `rpm` packages.

First download the script for adding GitLab's repository:
```
$ curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh > script.rpm.sh
```

Next add GitLab's repository:

```
$ sudo os=fedora dist=29 bash script.rpm.sh
```

Eventually install `gitlab-runner`:

```
$ sudo dnf install gitlab-runner
```

The next step consists in [registering the runner](https://docs.gitlab.com/runner/register/) on the GitLab server. 
The following registration will make the runner run in local because of the [executor](https://docs.gitlab.com/runner/executors/README.html) `shell`.

```
$ sudo gitlab-runner register
[sudo] password for flaudanum: 
Runtime platform                                    arch=amd64 os=linux pid=11834 revision=de7731dd version=12.1.0
Running in system-mode.                            
                                                   
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://gitlab.my-company.com/
Please enter the gitlab-ci token for this runner:
1BNQypbQCmDKGFgiauDH
Please enter the gitlab-ci description for this runner:
[localhost.localdomain]: runner-fedora-fla-vm
Please enter the gitlab-ci tags for this runner (comma separated):
linux-fedora
Registering runner... succeeded                     runner=1BNQypbQ
Please enter the executor: virtualbox, kubernetes, custom, docker, docker-ssh, ssh, parallels, shell, docker+machine, docker-ssh+machine:
[,]: shell
```