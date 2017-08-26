stone-payments.win-gocd-agent
=========

Ansible role to install GoCD windows agent

Install one or multiple agent instances with autoregister and resources gathering.

Requirements
------------

- Ansible 2.3

Role Variables
--------------

Name                            | Default                                       | Description
------------------------------- | ----------------------------------------------| ----------------------------------------------------------------------
gocd_server_url                 | https://localhost:8154/go                     | GoCD server to connect
gocd_version                    | 17.8.0                                        | Version of GoCD agent to install
gocd_user                       | go                                            | Agent windows service account
gocd_user_pass                  |                                               | Agent windows service account password, please use this as a vault variable.
gocd_user_domain                | "." (stands for localhost aka no domain)      | Agent windows service account domain
gocd_user_groups                | Administrators                                | List of service account user groups
gocd_user_home                  | $SystemDrive\users\go                         | Service account user home
gocd_base_working_directory     | $SystemDrive\Agents\                          | Base installation dir for agents (final working dir is appended with {AGENT_NUMBER})
gocd_ssh_config                 | .ssh/config                                   | Fully qualified path to the SSH config (default to no checking keys from github.com)
gocd_ssh_public_key             |                                               | Fully qualified path to the SSH public key
gocd_ssh_private_key_content    |                                               | Used to render the agent private ssh key, please use this as a vault variable.
gocd_agent_total_instances      | processor_count * processor_cores             | Total of gocd agent instances to install
chocolatey_source               | https://www.chocolatey.org/api/v2/            | Chocolatey source to download gocd agent package
gocd_reinstall_agent            | no                                            | Whether reinstall the agent chocolatey package
gocd_auto_register_key          |                                               | Auto register key used to join gocd server, please use this as a vault variable.
gocd_agent_additional_resources |                                               | List of extra gocd agent resources. Default info about OS is gathered from ansible facts
gocd_environments               |                                               | List of gocd agent environments, applied when using auto register key.

Example Playbook
----------------


    - hosts: gocd_agents
      roles:
         - stone-payments.win-gocd-agent
      vars:
        gocd_server_url: https://myci.company.com:8154/go
        gocd_user: go
        #gocd_user_pass: defined in encryped file by vault
        #gocd_ssh_private_key_content: defined in encryped file by vault
        gocd_ssh_public_key some_path_to_my/pubkey
        #gocd_auto_register_key: defined in encryped file by vault
        gocd_agent_additional_resources:
          - chocolatey
          - dotnet-core
        gocd_environments:
          - Production

Know issues
-------

Due to a bug in win_chocolatey module in ansible 2.3 the var **gocd_reinstall_agent** must be set to **yes** in order to upgrade the agent package.

License
-------

MIT

Author Information
------------------
https://github.com/cnatan

https://github.com/stone-payments
