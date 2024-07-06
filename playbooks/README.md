Sure, here is the updated `README.md` file with instructions on how to execute the playbook from the command line:

### README.md
```markdown
# Ansible Playbook for Oracle Containers

This playbook sets up and manages Oracle Database, Oracle REST Data Services, and Oracle WebLogic containers using Podman on RHEL 9. Each container is managed with a systemd service for starting, stopping, and restarting the containers.

## Prerequisites

- Ansible installed on your RHEL 9 system
- Podman installed on your RHEL 9 system
- Access to the Oracle Container Registry (username and password)

## Directory Structure

```plaintext
.
├── ansible.cfg
├── inventory
│   └── localhost
├── playbook.yml
├── roles
│   └── podman_containers
│       ├── tasks
│       │   └── main.yml
│       ├── templates
│       │   └── systemd-service.j2
│       └── vars
│           └── main.yml
└── README.md
```

## Setup

1. **Clone the repository:**

   ```sh
   git clone https://your-repository-url.git
   cd your-repository-url
   ```

2. **Update variables:**

   Edit the `roles/podman_containers/vars/main.yml` file to include your Oracle Container Registry username, password, and other necessary variables.

3. **Run the playbook:**

   Execute the following command to run the playbook:

   ```sh
   ansible-playbook playbook.yml
   ```

## Managing Podman Containers with Systemd

After running the playbook, you can manage the Podman containers using the created systemd service files. Examples of managing the Oracle Database container:

- **Start the Oracle Database container:**

  ```sh
  sudo systemctl start oracle_database.service
  ```

- **Stop the Oracle Database container:**

  ```sh
  sudo systemctl stop oracle_database.service
  ```

- **Restart the Oracle Database container:**

  ```sh
  sudo systemctl restart oracle_database.service
  ```

- **Enable the Oracle Database container to start on boot:**

  ```sh
  sudo systemctl enable oracle_database.service
  ```

- **Disable the Oracle Database container from starting on boot:**

  ```sh
  sudo systemctl disable oracle_database.service
  ```

Similar commands can be used for the `oracle_rds` and `oracle_weblogic` services.

## Logs

The logs for each container deployment are saved in their respective directories under `~/containers`.

- Oracle Database: `~/containers/oracle_database/database-deployment.logs`
- Oracle RDS: `~/containers/oracle_rds/oracle_rds.logs`
- Oracle WebLogic: `~/containers/oracle_weblogic/oracle_weblogic.logs`

## Additional Information

For further customization, you can modify the variables in `roles/podman_containers/vars/main.yml` and the templates in `roles/podman_containers/templates`.

If you encounter any issues or have questions, please refer to the [Ansible documentation](https://docs.ansible.com/) or the [Podman documentation](https://podman.io/getting-started/).

```

This `README.md` file now includes all the necessary instructions and an example of how to execute the playbook from the command line.

