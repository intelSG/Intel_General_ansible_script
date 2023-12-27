# Intel General AI Environment Setup

This repository hosts an Ansible playbook, `setup-general.yml`, meticulously crafted to streamline the establishment of a general AI environment, specifically tailored for Intel architectures. This playbook facilitates the installation of Miniconda, orchestrates the creation of a Conda environment named `Intel_AI`, and manages the deployment of a suite of packages and tools, including TensorFlow, PyTorch, and various Intel extensions.

For more insights into Intel's AI tools and their optimization capabilities, visit the [Intel AI Tools Selector](https://www.intel.com/content/www/us/en/developer/tools/oneapi/ai-tools-selector.html).

# Table of Contents
1. [Intel General AI Environment Setup](#intel-general-ai-environment-setup)
2. [Prerequisites](#prerequisites)
3. [Setting Up SSH Key-Based Authentication](#setting-up-ssh-key-based-authentication)
   - [Generate an SSH Key](#generate-an-ssh-key)
   - [Copy the SSH Public Key to the Remote Server](#copy-the-ssh-public-key-to-the-remote-server)
   - [Test the SSH Connection](#test-the-ssh-connection)
4. [Configuration](#configuration)
5. [Running the Playbook](#running-the-playbook)
6. [Components Installed](#components-installed)
7. [Conda Environment](#conda-environment)
8. [Notes](#notes)

## Prerequisites

To utilize this playbook effectively, ensure the following prerequisites are met:

- Ansible is installed on your control node, the system from which you'll execute the playbook.
- You have SSH access to all target nodes, the machines destined for the AI environment setup.
- You possess adequate permissions for installing packages and performing system modifications on the target nodes.

## Setting Up SSH Key-Based Authentication

To securely manage the connections and operations, this setup requires SSH key-based authentication. Follow these steps to generate an SSH key (if you don't already have one), copy it to the remote server, and test the connection.

### Generate an SSH Key

First, generate a new SSH key pair on your local machine. If you already have an SSH key and want to use that, you can skip this step.

Open a terminal and run the following command:

```bash
ssh-keygen -t rsa -b 4096
```
When prompted, you can specify the file path or press Enter to use the default location. It's recommended to set a secure passphrase for the key.

### Copy the SSH Public Key to the Remote Server
Next, copy your SSH public key to the remote server where the playbook will be executed. Replace ```username``` with your actual username on the remote server and `host-address` with the server's IP address.

```bash
# eg. ssh-copy-id -i ~/.ssh/id_rsa.pub your_username@192.168.2.60
ssh-copy-id -i ~/.ssh/id_rsa.pub [username]@[host-address]
```

### Test the SSH Connection
To ensure that the SSH key-based authentication is set up correctly, test the connection to the remote server:
```bash
# eg. ssh test@192.168.2.60
ssh [username]@[host-address]
```
After setting up the SSH key-based authentication, you can proceed with running the Ansible playbook as described in the Running the Playbook section.

## Configuration

The playbook uses several variables that you can customize according to your needs:

- `workspace_path`: The directory path for creating a workspace.
- `conda_installer_url`: The URL for the Miniconda installer.
- `conda_env_file`: The local path to the Conda environment file.
- `remote_conda_env_file`: The remote path where the Conda environment file will be copied.
- `conda_path`: The installation path for Miniconda.

## Running the Playbook

To run the playbook, follow these steps:

1. Clone the repository:

   ```bash
   git clone https://github.com/Yuandjom/Intel_General_Ansible_script.git
   cd Intel_General_Ansible_script
   ```
   
Customize the variables in setup-general.yml as needed.
Run the playbook:
```bash
ansible-playbook -i hosts.ini setup-general.yml -vv
```

## Components Installed

The playbook performs the following tasks:
- Installs Git and finds the Git executable path.
- Creates the specified workspace directory.
- Installs Miniconda and sets it up.
- Copies the Conda environment file and creates a Conda environment.
- Installs gperftools and configures it.
- Installs transformers from huggingface channel 
- Gathers CPU architecture information.
- Checks versions of PyTorch and Intel Extension for PyTorch.

## Conda Environment
The Conda environment, as defined in ```env/Intel_AI.yml```, includes:
- Python (python=3.10)
- TensorFlow (tensorflow=2.14) 
- Intel Extension for TensorFlow (intel-extension-for-tensorflow=2.14)
- Intel Optimization for Horovod (intel-optimization-for-horovod=0.28.1.1)
- PyTorch (pytorch=2.0.1)
- Intel Extension for PyTorch (intel-extension-for-pytorch=2.0.100)
- OneCCL Bindings for PyTorch (oneccl_bind_pt=2.0.0)
- Torchvision (torchvision=0.15.2)
- Jemalloc

## Notes
- Ensure you have the necessary permissions to run the playbook and make changes on the target machines.
- The setup process might take a significant amount of time, depending on network speed and the computational capabilities of the target machine.

