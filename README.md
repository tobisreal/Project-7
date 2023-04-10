# Project-7
## DEVOPS TOOLING WEBSITE SOLUTION
In previous **Project-6**, you implemented a WordPress-based solution that is ready to be filled with content and can be used as a full-fledged website or blog. Moving further we will add some more value to our solutions that your DevOps team could utilize. We want to introduce a set of DevOps tools that will help our team in day-to-day activities in managing, developing, testing, deploying and monitoring different projects.

The tools we want our team to be able to use are well-known and widely used by multiple DevOps teams, so we will introduce a single DevOps Tooling Solution that will consist of:
1. **Jenkins** – free and open source automation server used to build CI/CD pipelines.
2. **Kubernetes** – an open-source container-orchestration system for automating computer application deployment, scaling, and management.
3. **Jfrog Artifactory** – Universal Repository Manager supporting all major packaging formats, build tools and CI servers. Artifactory.
4. **Rancher** – an open-source software platform that enables organizations to run and manage Docker and Kubernetes in production.
5. **Grafana** – a multi-platform open-source analytics and interactive visualization web application.
6. **Prometheus** – An open-source monitoring system with a dimensional data model, flexible query language, efficient time series database and modern alerting approach.
7. **Kibana** – Kibana is a free and open user interface that lets you visualize your Elasticsearch data and navigate the Elastic Stack.

Note: Do not feel overwhelmed by all the tools and technologies listed above, we will gradually get ourselves familiar with them in upcoming projects!

### Setup and technologies used in Project 7
As a member of a DevOps team, you will implement a tooling website solution which makes access to DevOps tools within the corporate infrastructure easily accessible.
In this project you will implement a solution that consists of following components:
1. **Infrastructure**: AWS
2. **Webserver Linux**: Red Hat Enterprise Linux 8
3. **Database Server**: Ubuntu 22.04 + MySQL
4. **Storage Server**: Red Hat Enterprise Linux 8 + NFS Server
5. **Programming Language**: PHP
6. **Code Repository**: GitHub
On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage. Even though the NFS server might be located on a completely separate hardware – for Web Servers it look like a local file system from where they can serve the same files.

On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage. Even though the NFS server might be located on a completely separate hardware – for Web Servers it look like a local file system from where they can serve the same files.
![Screenshot (115)](https://user-images.githubusercontent.com/111396874/230899681-e40dd751-6bd5-43a5-9b45-7bb9e8b1acb6.png)
It is important to know what storage solution is suitable for what use cases, for this – you need to answer the following questions: what data will be stored, in what format, how this data will be accessed, by whom, from where, how frequently, etc. Based on this you will be able to choose the right storage system for your solution.

## STEP 1 – PREPARE NFS SERVER

1. Spin up a new EC2 instance with RHEL Linux 8 Operating System.
2. Based on your LVM experience from Project 6, Configure LVM on the Server.
* Instead of formatting the disks as ext4, you will have to format them as xfs
* Ensure there are 3 Logical Volumes. lv-opt lv-apps, and lv-logs
* Create mount points on /mnt directory for the logical volumes as follow:
  - Mount lv-apps on /mnt/apps – To be used by webservers
  - Mount lv-logs on /mnt/logs – To be used by webserver logs
  - Mount lv-opt on /mnt/opt – To be used by Jenkins server in Project 8
3. Install NFS server, configure it to start on reboot and make sure it is up and running
```
sudo yum -y update
sudo yum install nfs-utils -y
sudo systemctl start nfs-server.service
sudo systemctl enable nfs-server.service
sudo systemctl status nfs-server.service
```
4. Export the mounts for webservers’ subnet CIDR to connect as clients. For simplicity, you will install all three Web Servers inside the same subnet, but in the production set-up, you would probably want to separate each tier inside its own subnet for a higher level of security.
To check your subnet cidr – open your EC2 details in the AWS web console and locate the ‘Networking’ tab and open a Subnet link:



