# Provision Ubuntu 2004 on WSL2

Automating provisioning Ubuntu 20.04 with Ansible on WSL 2

Uses ansible, in a [pipenv](https://docs.pipenv.org/), in a WSL 2 Ubuntu instance to provision locally.

Intended to be re-runnable (idempotent) to maintain and update when required.

## Getting Started

### Prerequisites

1. Windows 10.
1. WSL 2
1. Ubuntu 20.04 installed via Windows Store.

### Update the package lists

`sudo apt-get update`

### Configure Python & pipenv

1. `sudo apt install --yes python3-pip`
1. `sudo pip3 install pipenv`

### Clone and Run

1. Clone this repo.
2. Change the directory
```shell
cd provision-ubuntu2004-on-wsl2
```
3. Copy `secret_vars.yml.example` to `secret_vars.yml`
```shell
cp secret_vars.yml.example secret_vars.yml
```
4. Modify the `secret_vars.yml` file and replace `<moj-pttp-dev-aws-account-id>` and `<moj-pttp-shared-services-aws-account-id>` with respective AWS account ids.
5. Execute the following commands
```shell
pipenv install --dev
pipenv shell
```
6. Install Ansible requirements
```shell
ansible-galaxy install -r requirements.yml
```
7. Go ahead and run the playbook
```shell
ansible-playbook playbook.yml -i inventory --ask-become-pass
```
6. Enter your Ubuntu account password at the prompt.
7. All done ! :boom:. Now enjoy :sunglasses:.

## What is Installed?

- See the [playbook.yml](playbook.yml) task: `Install apt cmd line apps` for apt packages
- [yarn](tasks/yarn.yml)
- [tfenv](tasks/tfenv.yml)
- [aws vault](tasks/aws-vault.yml)
- [rbenv](tasks/rbenv.yml)
- [eksctl](tasks/eksctl.yml)
- [kubectl](tasks/kubectl.yml)


## Notes

|Description           | Command                                                                       |
|--------------------- | ----------------------------------------------------------------------------- |
|Find all local info   | `ansible localhost -m setup`                                                  |
|Run only rbenv        | `ansible-playbook playbook.yml -i inventory --ask-become-pass --tags "rbenv"` |
|Purge deps            | `pipenv uninstall --all`                                                      |

See [vars.yml](vars.yml) to configure which tasks get run.

## References

- [Jeff Geerling's Ansible for Devops](https://leanpub.com/ansible-for-devops/c/J2V7E1SOETu3)
- See [Brad's Pipenv Crash Course](https://youtu.be/6Qmnh5C4Pmo)
- Brad's [Pipenv cheatsheet](https://gist.github.com/bradtraversy/c70a93d6536ed63786c434707b898d55)
