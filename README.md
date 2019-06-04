# ksync
A file synchronizer and service bouncer to speed up Kubernetes container development cycle. It uses rsync and "kubectl exec" internally.

## Requirements

The local machine must have rsync and kubectl installed. The remote container must have rsync installed. If it is not installed, the --install option can be used to dynamically install it. That works for a quick test, but you'll eventually want to bake rsync permanently into the container image by updating its Dockerfile.

## Usage ##
```
ksync namespace pod container src dst [--cmd command] -- [rsync options]
ksync namespace pod container --install
```

The first form synchronizes the "src" local directory or file set with the "dst" directory in the container. For details on how the paths are interpreted, consult the rsync man page. An optional command can be executed after file synchronization with the --cmd option. This can be used to restart a service. For example, if the container uses supervisord to manage processes, this option can be useful: --cmd "supervisorctl restart SERVICE_NAME".

The second form attempts to install the rsync package in the container. Currently, CentOS/RHEL is supported, with planned future support for Debian/Ubuntu.
