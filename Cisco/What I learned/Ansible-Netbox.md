# 📌 Topic  
Deploying NetBox using Ansible Automation

---

## 🎯 Why This Matters  
NetBox is widely used for network documentation and automation.  
Using Ansible to deploy it:
- Saves time
- Ensures consistency
- Scales easily across environments
- Removes mundane task 


---

## 🧠 Key Concepts  

### 🔹 Infrastructure as Code (IaC)  
- Infrastructure can be defined and deployed using code  
- Ansible automates repetitive setup tasks  

### 🔹 Idempotency  
- Running the playbook multiple times does not break the system  
- Only changes what is needed  

### 🔹 Service Dependencies  
- NetBox depends on:
  - PostgreSQL (database)
  - Redis (cache & queue)
  - Python environment  

---

## ⚙️ How It Works  

- Ansible connects to the target machine via SSH  
- Tasks are executed in order:
  1. Install system packages  
  2. Configure PostgreSQL  
  3. Create NetBox database and user  
  4. Install NetBox  
  5. Configure services (Redis, Gunicorn, etc.)  

---

## 🔄 Before vs After Understanding  

### ❌ Before  
- Installed applications manually  
- Didn’t fully understand ansible syntax 
- Setup was inconsistent
- Didnt't know you could use become for sudo but also use in a task to become a root use of an DB
- Didn't know how to ready ansible error codes

### ✅ After  
- Can automate full application deployment  
- Understand backend components required for NetBox  
- Can troubleshoot failures during automation 
- learn how to use variables instead of just hardcoding everything into the script
- everything you can do manually in linux, ansible allows you to do it at scale.
 

---

## 🧪 Real Example  

### Error code

```
ERROR]: Task failed: Module failed: value of state must be one of: reloaded, restarted, started, stopped, got: restart
Origin: /home/wlouis/Templates/playbook.yml:375:7

373         line: "    server_name {{ ansible_host }};"
374
375     - name: restart Nginx
          ^ column 7

fatal: [default]: FAILED! => {"changed": false, "msg": "value of state must be one of: reloaded, restarted, started, stopped, got: restart"}
```


### Create Database with Ansible  
```
- - name: Create Netbox DB Progres
      become: true
      become_user: postgres
      community.postgresql.postgresql_db:
        name: netbox
    
```

- Notice how can use become the root user for the DB