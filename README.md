# 🧱 VProfile Application Stack (Manual Setup)

A hands-on DevOps project to simulate a **multi-tier Java web application** stack using:

**Nginx · Apache Tomcat · RabbitMQ · Memcache · MySQL · Maven · Vagrant**

> All components are manually configured to reflect real-world deployment patterns. Also includes Vagrant-based automation for local provisioning.

---

## 📊 Stack Architecture Overview

The goal is to deploy a **Java-based web application** (`vProfile`) across a modular stack with clearly defined responsibilities:

| Layer              | Component    | Role                            |
| ------------------ | ------------ | ------------------------------- |
| Load Balancing     | **Nginx**    | Routes user requests to backend |
| Application Server | **Tomcat**   | Hosts the Java WAR app          |
| Messaging          | **RabbitMQ** | Asynchronous message brokering  |
| Caching            | **Memcache** | Speeds up frequent DB queries   |
| Database           | **MySQL**    | Relational data store           |
| Build Tool         | **Maven**    | WAR packaging of the Java app   |

> 📷 Screenshots included in the `screenshots/` folder.

---

## ⚙️ Stack Flow

1. **User → Nginx (LB)**
   IP-based access triggers Nginx to forward requests to Tomcat.

2. **Tomcat → App Logic**
   Tomcat runs the `vprofile.war` file (packaged via Maven) and serves dynamic Java content.

3. **Tomcat ↔ Memcache**
   Checks Memcache for query results. If a miss, it queries MySQL and updates the cache.

4. **MySQL → Data Layer**
   Stores application state, logs, and user data.

5. **RabbitMQ (Planned)**
   Message queue integration planned for async processing.

---

## 🛠️ Configuration Highlights

### `application.properties`

Located at: `src/main/resources/application.properties`

Contains all key service configs:

* MySQL DB host, port, creds
* RabbitMQ settings
* Memcache endpoints
* App-specific variables

---

### Maven Build

Build WAR file using:

```bash
/usr/local/maven3.9/bin/mvn install
```

This generates the deployable `.war` file for Tomcat.

---

## 🚧 Challenges & Fixes

| Issue                | Solution                                                  |
| -------------------- | --------------------------------------------------------- |
| Nginx not proxying   | Fine-tuned `nginx.conf` to reverse-proxy to Tomcat        |
| RabbitMQ dummy state | Set up service, full integration pending                  |
| Memcache staleness   | Handled by setting eviction policies + TTLs               |
| Localhost IP limits  | Updated services to bind to `0.0.0.0` for external access |

---

## 📦 Vagrant-Based Local Automation

To simulate the stack in local VMs:

### 🪜 Steps

```bash
# (Optional) Remove existing base box
vagrant box remove ubuntu/jammy64 --force

# Spin up all VMs
vagrant up

# SSH into any VM
vagrant ssh <vm-name>
```

> Each VM auto-installs its required services (Nginx, Tomcat, RabbitMQ, etc.)

---

## ✅ Outcome

* Fully functioning multi-tier Java web app stack.
* Manual provisioning emphasizes understanding over abstraction.
* RabbitMQ integration & NFS storage planned for future expansion.


---

## 🚀 What's Next?

* [ ] Fully integrate RabbitMQ and simulate service queues
* [ ] Add shared storage via NFS
* [ ] Containerize with Docker (WIP)

---

## 👋 Author

**Harshit Chauhan**
[LinkedIn](https://www.linkedin.com/in/harshit-chauhan-tentinqu) | [Medium](https://medium.com/@harshitchauhan2233) | [GitHub](https://github.com/tentinqu)

---

Let me know if you want this formatted as a Notion-compatible markdown table or want a copy ready for pasting into your GitHub repo.
