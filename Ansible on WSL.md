# Getting Started with Ansible on Windows with WSL

- WSL is Windows Subsystem for Linux, a Linux environment in Windows,
- Ansible is an automation tool written in Python that can be used with almost anything that can be reached over SSH. 

These are the steps I used to get an Ansible control node running in Windows. Ansible control nodes do not nativly run in Windows, however there are multiplke ways to run a control node in Windows using VMs, Cygwin, or WSL. I use WSL.

This is not intended to be comprehensive study of all Ansible, WSL and all the possible variations and use cases that can be implemented. These are based on my own personal notes for my own future reference. They describe the bare minimun needed to get off the ground with Ansible on Windows.

## Installing WSL

WSL does not come pre-installed in Windows, but can be installed easily with Powershell. Run Powershell as an administrator and run these commands:

```powershell
wsl --install
```

This command will install WSL with Ubuntu by default. Other versions of Linux can be installed using the -d flag if needed.

```powershell
wsl --install -d <distributiion name>
```

Choose a username and password when prompted.

## Install Ansible

Next, I use pip to install Ansible. Using apt will install an older version of Ansible and will cause problems. As of December 2023, pip is the officially supported means of installing Ansible.

## https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html

Pip is not installed by default on Ubuntu and can be installed using these commands:

```bash
sudo apt-get update
sudo apt install pip -y
```

Ansible can now be installed with pip.

```bash
pip install ansible --user ansible
```

In order to run commands, this directory will have to be added to the path. These are the commands I used:

```bash
export PATH="/home/<path to directory>/.local/bin:$PATH"
source ~/.bashrc
```

After running these commands, restart the terminal. I do this by closing the powershell window and opening a new one. WSL can be re-entered using the name of the Linux distro that was installed.

```powershell
ubuntu
```

If everything works, Ansible should be installed and ready to use.
