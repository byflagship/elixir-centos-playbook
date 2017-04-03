# Elixir + Phoenix framework on CentOS 7 playbook

### WIP Alert
This is a work in progess, but please feel free to test and help :)

## Introduction

CentOS 7 is a state-of-the-art, enterprise-ready distribution and our choice for running our Phoenix applications.
We use Ansible to configure our servers and deploy our app.

This playbook will be made of two parts:

* **Setup:** Getting machines ready to run Elixir and Phoenix framework.
* **Deploying:** (continuously, eventually) Deploy our apps, run db migrations, assets compilations, etc.

## Setup

Assuming you know your way around installing ansible and running ansible-playbook.
You need to create two files before running: `ansible.cfg` and `inventory`

(See `ansible.cfg.example` and `inventory.example` for examples)

- **ansible.cfg:** Configuration file for ansible.
- **inventory:** List of your AWS servers by name or IP.

## Running Setup
`ansible-playbook setup.yml` :)

## Deploying

### Pulling from a private Github repository

First you'd need to copy ssh key, that can access your private Github repository.

Copy the ssh key to ssh_keys directory. The files should be named as `id_rsa` and `id_rsa.pub` respectively.

Once they are in place, run:

`ansible-playbook ssh_keys.yml`

This will put your (organization) ssh keys under `/home/centos/.ssh` so centos user can pull from your private Github repository.

### Deploying the application

**Part 1: Set github repository to pull from**

Create `group_vars/all.yml` with `github_repo` and `phoenix_path` variables. (See `group_vars/all.yml.example` for example).

This will clone from your repository to the selected path.

**Part 2: Create production secret**

A production Phoenix framework application needs a secret base key, and connection to a database. Those are defined in the application's `config/prod.secret.exs`

Please create a `config/prod.secret.exs` with your secret and database connection data. (See `config/prod.secret.exs.example`)

**Part 3: let's deploy!**

Once $everything is ready, we can deploy our app with `ansible-playbook deploy.yml`.

This will pull latest master branch from the repository, and run Phoenix deploy process: `npm`, `mix deps.get, compile`, etc.


## Contributing
Please contribute by making a pull request to this repository. We will be continually monitoring and responsive. If you feel that our response has been lacking, please message us here: `build-at-exeq.io`.

## Credits
[Shlomi Zadok](https://github.com/shlomizadok/) and the [Exeq team](https://github.com/exeqapp)
