# Ansible installed in a docker image
Alpine container with ansible installed by using pip3.

- Alpine version: 3.9
- Ansible Version: 2.8.1

Docker image-versioning follows the ansible versions

## Running the container
```
docker run --rm -it -v <your-ansible-files>:/etc/ansible aiqu/ansible:latest
```

You should atleast include config-file & private ssh-key. Example minimal
configuration mount as volume (ansible.cfg):
```
[defaults]
inventory         = ./inventory/
roles_path        = ./playbooks/roles
private_key_file  = ./auth/id_rsa
host_key_checking = False
```

## Other options
If you use entrypoint/cmd commented out at bottom of Dockerfile and re-build
(or override entrypoint), you can run ansible without installing locally
(or even from Windows). E.g. create alias and run as normal

#### With custom image (Linux)
```
alias ansible='docker run --rm -it -v <your-ansible-files>:/etc/ansible <customized-ansible-image>:latest'
```

#### With override (Linux)
```
alias ansible='docker run --rm -it --entrypoint /usr/bin/ansible -v <your-ansible-files>:/etc/ansible aiqu/ansible:latest'
```
