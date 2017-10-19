security_updates
---------------

The purpose of this playbook is to schedule the application of security updates upon a set of EC2 hosts.

The Project Playbooks perform the following tasks:

- Establish that the site-specific repo file is upto date 
- (Optionally) Performs a yum update on ALL Packages. There is a configurable flag for this
- Installs yum-cron & configures it to periodically perform security-related updates

Requirement to use these roles:

- Ansible v2.3.1

To correctly utilize this Project, a number of variables will need to be reviewed and an appropriate inventory allowing  you to target your hosts will need to be available.

A sample static inventory "may" look like below:

```ini
localhost

[myhosts]
R001CDGSEV00401 ansible_host=10.176.74.1 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00403 ansible_host=10.176.74.2 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00404 ansible_host=10.176.74.3 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00405 ansible_host=10.176.74.4 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00406 ansible_host=10.176.74.5 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00407 ansible_host=10.176.74.6 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00408 ansible_host=10.176.74.7 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00409 ansible_host=10.176.74.8 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
R001CDGSEV00410 ansible_host=10.176.74.9 ansible_ssh_private_key_file=/home/ec2-user/.ssh/privateKey.pem
```

The Project comprises of 2 roles namely:
- upd_cdgrepo
- schd_sec_updts

Review the files *upd_cdgrepo.yml* and *sec_update.yml*, modifying appropriately the vars section to ensure that the behaviour of the "play" is as required.

A brief description of each encountered variable is given below:

| Variable_Name | Description 
| --- | --- |
| repo_base_url | The correct Repository base URL used within the site-specific repo file
| repo_file | The name of the repo file eg. cdg_mirror.repo
| update_cache | Whether to update the cache as part of the (possible) yum update (yes/no)
| updt_all_pkgs | Whether to update all packages during the "play" (true/false)
| disable_gpg_check | If gpg_checking should be enabled or not (yes/no)
| 
| updateOption | The kind of yum update that should be scheduled (use **security** to schedule security updates ONLY)
| emailTo | An email to notify during the scheduled updates
| connectTo | A mailserver to be used during mailing activities 

A sample shell script has been provided to allow you to easily execute the playbook.

You may choose to execute the Project from an appropriate Control Box (with Ansible installed and with the necessary ssh keys available) thus:

```bash
ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i recovery_env.inv site.yml
```

More appropriately, use the playbook from Ansible Tower.

To do:
- Develop a version of the Project that uses an in-memory inventory, targetting appropriately tagges EC2 instances 
- Add functionality to specify a list of packages to exclude during yum updates


