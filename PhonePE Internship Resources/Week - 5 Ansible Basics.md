# Week - 5

**1-** **Introduction to Ansible:**

- Intent: Learn the fundamentals of Ansible
- Task: Run a shell command on 3 VMs from your local machine and save its output in a file locally.

**2- Galera cluster setup using Ansible:**

**Intent:** Explore different modules

**Tasks:** Write a ansible playbook to automate the entire setup of a galera cluster.

The playbook should include:

1. Setup of 3 node galera cluster
2. Demote the 3rd node to an async slave.
3. Provisions to uninstall/destroy the cluster
4. The playbook should be idempotent (i.e running the playbook multiple times shouldn't affect the setup)
5. The cluster once setup should have only 2 new users - one for you with read/write permissions and another called monitoring with only read permissions.

**3- Integrate with Hashicorp’s Vault:**

**Intent: Explore Hashicorp’s Vault**

**Tasks:**

1. Setup Hashicorp’s vault and have your Ansible playbook(created in task 2) reference it to create MariaDB users.
2. Ansible should use your credentials (username and private key) stored in vault to run the playbooks in the corresponding VMs
3. Ansible playbook should connect to vault over https (for which you will need to use self signed certificates)
