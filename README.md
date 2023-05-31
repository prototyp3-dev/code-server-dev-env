# Cartesi Development Environment

Developing the a distributed application for the Cartesi Machine has been
greatly simplified with the new docker-based build system. However, for the
build to work properly, you must be able to run a container in an emulated
RiscV. In a regular Linux environment, this can be easily achieved by installing
the correct qemu packages. However, in a cloud-based development environment
such as GitPod or GitHub Codespaces, where the development environment itself
runs within a container, it is not possible to install these packages on the
host, rendering them unsuitable for running the build system.

This repository contains an Ansible playbook to set up a proper build system,
including a [Code Server](https://github.com/coder/code-server) editor so that
the developer can use the environment through a familiar web-based VS Code
interface.

## Deploying on Cloud Providers

Currently the only streamlined cloud provider is
[Linode](https://www.linode.com/). However, have a look at the next section for
instructions on how to run on any Debian-based VM.

### Deploying with Linode Stackscripts

You can easily deploy a node in your account using a pre-existing StackScript:

1. Create your [Linode](https://www.linode.com/) account if you don't already
   have one, and log into the Linode Manager Console

2. Navigate to the
   [arcane/cartesi-dev-env](https://cloud.linode.com/linodes/create?type=StackScripts&subtype=Account&stackScriptID=1168216)
   StackScript Deployment Page

3. In the "cartesi-dev-env Setup" section, enter a password to access the editor
   and your local user.

4. Select the other parameters for your virtual machine, including:
   - Region, accodring to your preferences
   - Plan. A Shared CPU instance with 4GB is a good starting point. This can be
     changed later if needed.
   - Enter a Root password or a SSH key

5. Click "Create Linode"

6. Wait a several minutes for the node to set up and try to access the IP
address for your new VM in your browser. The default setup procedure installs
a self-signed certificate, so be sure to accept it in the browser.

## Manually running on your own VM

This Ansible playbook has been tested in an Ubuntu 22.04 virtual machine. To
manualy execute the playbook, follow the steps:

1. Become root:

    ```shell
    sudo -i
    ```

2. Make sure Git and Ansible are installed:

    ```shell
    apt update
    apt install git ansible
    ```

3. Clone this repository:

    ```shell
    git clone https://github.com/felipefg/cartesi-dev-env
    cd cartesi-dev-env
    ```

4. Run the playbook specifying the password for the editor:

    ```shell
    ansible-playbook -e code_password="<your_password>" site.yml
    ```

## Features

- VS Code Interface, using [Code Server](https://github.com/coder/code-server)
- Develop directly in the virtual machine: as the environment is not inside a
  container, Docker is fully accessible to you without any limitations.
- Standard development environment: standard set of compilers and build tools
- Docker Community Edition with RISC-V emulator preinstalled
- SSH Access: feel free to SSH into your virtual machine using the `cartesi`
  user and the same password you specified for the editor.
