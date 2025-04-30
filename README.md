# Prerequisites
#
- JDK 17 or 21
- Maven 3.9
- MySQL 8

# Technologies 
- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- Tomcat
- MySQL
- Memcached
- Rabbitmq
- ElasticSearch
# Database
Here,we used Mysql DB 
sql dump file:
- /src/main/resources/db_backup.sql
- db_backup.sql file is a mysql dump file.we have to import this dump to mysql db server
- > mysql -u <user_name> -p accounts < db_backup.sql

# vProfile Project – Manual Setup with Vagrant Automation

This project is a manual setup of the **vProfile** web application stack, intended to replicate a real-world deployment environment without relying on cloud infrastructure. It highlights the traditional approach of provisioning, configuring, and linking multiple services, with an added **automated setup using Vagrant** for reproducibility and efficiency.

---

## Stack Components

The application is a **Java-based web app** deployed using:

- **Nginx**: Load Balancer (LB01)
- **Apache Tomcat**: Application Server (APP01)
- **MySQL**: Database (DB01)
- **RabbitMQ**: Message Broker (RMQ01)
- **Memcached**: Cache Layer (MC01)

Optional suggestion: **NFS** can be used as shared storage if external file storage is required (not implemented in this setup).

---

## Flow of the Stack

1. **User** accesses the application via the IP of the load balancer (Nginx).
2. **Nginx** forwards traffic to **Apache Tomcat**, which hosts the Java `.war` artifact.
3. Before Tomcat hits the **MySQL** server, it checks **Memcached** to serve cached queries.
4. **RabbitMQ** is also integrated as a messaging queue (though non-functional in this demo).
5. All service IPs and ports are configured in `application.properties` under: src/main/resources/application.properties
   
Example:
```properties
db01.url=jdbc:mysql://db01:3306/accounts
db01.username=admin
db01.password=admin123
```
## Manual Setup Instructions
1. Install Maven 3.9, Java 8+, MySQL, RabbitMQ, Memcached, and Tomcat on respective servers.

2. Build the app:
```
/usr/local/maven3.9/bin/mvn install
```
3. Copy the WAR file to Tomcat:

```
cp target/vprofile-v2.war /opt/tomcat/webapps/ROOT.war
```
4. Start and enable services:
```
sudo systemctl enable --now tomcat nginx mysql memcached rabbitmq-server
```

5. Ensure proper IP binding for inter-VM communication (e.g., 0.0.0.0 instead of 127.0.0.1).

## Vagrant Automated Setup
This project includes a Vagrantfile for spinning up pre-configured local VMs.

Instructions:
(Optional) Remove previous boxes if needed:

```
vagrant box remove ubuntu/jammy64 --force
```

Provision the environment:
```
vagrant up
```
Access each VM (e.g.):
```
vagrant ssh app01
```

### Application accessible via Nginx IP once provisioning completes.

Check out the original code from https://github.com/hkhcoder/vprofile-project.git
