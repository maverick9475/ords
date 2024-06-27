# ORDS and WebLogic Deployment using Docker, GitHub, and Ansible Tower

## Project Overview

This project aims to deploy Oracle REST Data Services (ORDS) and Oracle WebLogic Server in containers. The deployment process will be automated using GitHub and Ansible Tower. The project involves creating Dockerfiles and Ansible playbooks to facilitate the deployment. Additionally, an Autonomous Database will be deployed using provided container images.

## Prerequisites

Before you begin, ensure you have the following installed:
- Docker
- Git
- Ansible Tower

## Repository Structure

```
.
├── dockerfiles
│   ├── database
│   ├── ords
│   └── weblogic
├── inventory
├── playbooks
│   ├── deploy_oracle_db.yml
│   ├── deploy_ords.yml
│   └── deploy_weblogic.yml
├── README.md
└── scripts
    ├── build_database_image.sh
    ├── build_ords_image.sh
    └── build_weblogic_image.sh

4 directories, 11 files
```

## Docker Images

1. **Oracle REST Data Services (ORDS) Docker Image**
   - Link: [ORDS Docker Image](https://container-registry.oracle.com/ords/ocr/ba/database/ords)

2. **Oracle WebLogic Server 14.1.1.0 Container Image**
   - Link: [WebLogic Docker Image](https://container-registry.oracle.com/ords/ocr/ba/middleware/weblogic)

3. **Oracle Database Server Release 21c (21.3.0.0)**
   - Link: [Oracle Database Docker Image](https://container-registry.oracle.com/ords/ocr/ba/database/enterprise)

## Documentation

- [Oracle REST Data Services Documentation](https://github.com/oracle/docker-images/blob/main/OracleRestDataServices/README.md)
- [Oracle WebLogic Documentation](https://github.com/oracle/docker-images/blob/main/OracleWebLogic/README.md)
- [Building WebLogic Server Images on Docker](https://docs.oracle.com/middleware/1213/wls/DOCKR/configuration.htm#DOCKR121)
- [Oracle WebLogic Server on Docker Containers](https://www.oracle.com/us/products/middleware/cloud-app-foundation/weblogic/weblogic-server-on-docker-wp-2742665.pdf)
- [Docker : Oracle REST Data Services (ORDS) on Docker](https://oracle-base.com/articles/linux/docker-oracle-rest-data-services-ords-on-docker)
- [Container Registry Locations for Oracle Autonomous Database Free Container Image](https://docs.oracle.com/en-us/iaas/autonomous-database-serverless/doc/autonomous-docker-container.html#GUID-4F3F3B4D-04A9-49AA-B3FE-21B255251B02)

## Setup Instructions

### Clone the Repository

```sh
git clone https://github.com/yourusername/your-repo.git
cd your-repo
```

### Build Docker Images

Navigate to the `scripts` directory and execute the build scripts:

```sh
cd scripts
sh build_ords_image.sh
sh build_weblogic_image.sh
sh build_database_image.sh
```

### Deploy Containers using Ansible Tower

1. Create an inventory file for your hosts.
2. Update the `playbook.yml` with the necessary tasks to deploy ORDS, WebLogic, and the Autonomous Database.
3. Trigger the playbook execution from Ansible Tower.

### Example Inventory File

```ini
[ords]
host1 ansible_host=your_host_ip

[weblogic]
host2 ansible_host=your_host_ip

[autonomous_database]
host3 ansible_host=your_host_ip
```

