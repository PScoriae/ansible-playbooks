<div align='center'>
<p>
  <a href="https://linkedin.com/in/pierreccesario">
    <img src="https://img.shields.io/badge/-LinkedIn-black.svg?style=for-the-badge&logo=linkedin&colorB=555">
  </a>

  <a href="https://github.com/PScoriae/ansible-ec2/blob/main/LICENSE.md">
        <img src="https://img.shields.io/badge/license-WTFPL-brightgreen?style=for-the-badge">
  </a>
</p>
<p>
  <img src="./docs/ansible.svg" width=300>
</p>

## Ansible CaC

</div>
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about">About</a>
    </li>
    <li><a href="#installation">Installation</a></li>
    <li><a href="#usage">Usage</a></li>
  </ol>
</details>
<hr/>

# About

This repository is concerned with the configuration and bootstrapping of **AWS EC2 RHEL/CentOS/Rocky Linux** instances for my personal projects using Ansible CaC.

Currently, this is what the bootstrap script `playbooks/bootstrap-servers.yaml` does:

| Item                                   | Rationale                                                                     |
| -------------------------------------- | ----------------------------------------------------------------------------- |
| 2GB swapfile                           | EC2 Amazon Linux 2 AMI doesn't come with swapfile and may run into OOM issues |
| Docker                                 | Required to host PCPartsTool software architecture                            |
| Docker Registry                        | Private registry for push and pull of PCPartsTool images between servers      |
| .pem key                               | Authentication for CICD server to SSH into Web server                         |
| pnpm                                   | pnpm is the package manager used for PCPartsTool                              |
| Jenkins                                | CICD software for the build server                                            |
| libappindicator-gtk3, liberation-fonts | Packages required for Playwright E2E testing                                  |
| Ansible                                | For Jenkins post-build deployment process to web server                       |
| Prometheus, Grafana, Node Exporter     | Monitoring and Observability on CICD and Web server                           |
| Nginx                                  | Reverse proxy server from DNS A record to app-specific ports                  |

**Note:** This is just one of multiple repositories that contribute to my personal projects. Here are all the related repositories:

| Repository                                                             | Built With                                                                                                                                                                                                                                                               | Description                                                         |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
| [PCPartsTool](https://github.com/PScoriae/PCPartsTool)                 | [SvelteKit](https://kit.svelte.com), [TypeScript](https://www.typescriptlang.org/), [Tailwind CSS](https://tailwindcss.com), [MongoDB](https://mongodb.com), [Jenkins](https://www.jenkins.io/), [Docker](https://www.docker.com/), [Playwright](https://playwright.dev) | The SvelteKit MongoDB WebApp                                        |
| [PCPartsTool-Scraper](https://github.com/PScoriae/PCPartsTool-Scraper) | [JavaScript](https://www.javascript.com/), [Jenkins](https://www.jenkins.io/), [Docker](https://www.docker.com/)                                                                                                                                                         | Scraping Script to Gather E-commerce Item Data                      |
| [terraform-infra](https://github.com/PScoriae/terraform-infra)         | [Terraform](https://terraform.com), [Cloudflare](https://cloudflare.com), [AWS](https://aws.amazon.com)                                                                                                                                                                  | Terraform IaC for PCPartsTool Cloud Infrastructure                  |
| [ansible-ec2](https://github.com/PScoriae/ansible-ec2)                 | [Ansible](https://ansible.com), [Prometheus](https://prometheus.io), [Grafana](https://grafana.com), [Nginx](https://nginx.com), [AWS](https://aws.amazon.com)                                                                                                           | Ansible CaC for AWS EC2 Bootstraping, Observability and Maintenance |
| [shuttleday](https://github.com/Kirixi/shuttleday)                     | [React](https://reactjs.org), [TypeScript](https://www.typescriptlang.org/), [MUI](https://mui.com), [Node.js](https://nodejs.org/en/), [Docker](https://www.docker.com/), [Express](https://expressjs.com), [MongoDB](https://mongodb.com)                              | Badminton Scheduling and Information Webapp                         |

# Installation

This section guides you on how to setup this repo for use within the context of my personal projects' infrastructure.

1. In your desired project folder, clone the project with the following command:

   ```bash
   git clone https://github.com/PScoriae/ansible-ec2
   ```

2. Create a `inventory` file in the root directory of your project. It holds the **private** IP addresses to your server infrastructure. You may refer to `inventory.example`
3. Add `config-files/main-key.pem` or whatever public key you use to authenticate the CICD build server to the web server.

# Usage

After installation, simply run the following commands from your root directory to execute the playbooks:

```bash
# bootstrap new EC2 servers
ansible-playbook playbooks/bootstrap-servers.yaml

# update all packages on EC2 servers
ansible-playbook playbooks/yum-update.yaml
```
