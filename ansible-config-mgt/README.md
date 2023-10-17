PREREQUISITE:

To flow with and enjoy project 13, 

1. You must have successfully done project 6-10
2. You must have successfully done project 11-12
3. You have jenkins and ansible installed and running
4. You have configured a webhook on your github to your jenkins server.
5. You have ssh-agent running on your local system.
6. You have nginx-LB(ubuntu), apache-LB(ubuntu), DB(ubuntu), 2 webservers (Redhat)

# MYSQL ROLE

**NOTE: To ensure smooth run of the db role, note that the target db is ubuntu, I edited the following files under mysql role:**

* defaults/main.yml:
```markdown
  # Databases.
mysql_databases: 
   - name: tooling
     collation: utf8_general_ci
     encoding: utf8
     replicate: 1

# Users.
mysql_users: 
   - name: webaccess
     host: 172.31.23.108
     password: secret
     priv: '*.*:ALL,GRANT'
```

* task/main.yml (all ansible.builtin removed from include function)
* task/variable.yml (the first ansible.builtin removed from include function)
* task/configure.yml (the first ansible.builtin removed from command function)
* task/secure-installation.yml (all ansible.builtin removed from command and shell function)

## NGINX ROLE
**NOTE: ON nginx role, default/main.yml, were edited by me for load balancing configuration, not just to install nginx:**

In default/main.yml:

nginx_upstreams: 
 - name: myapp1
   strategy: "ip_hash" # "least_conn", etc.
   keepalive: 16 # optional
   servers:
     - "http://172.31.26.240:80 weight=3"
     - "http://172.31.21.172:80 weight=3"


## APACHE ROLE

**For apache, default/main.yml, was edited for same purpose as nginx:**

in default/main.yml:

```markdown
#webservers
loadbalancer_name: "myapp1"
ip-172-31-26-240.us-west-2.compute.internal: "172.31.26.240 weight=3"
ip-172-31-21-127.us-west-2.compute.internal: "172.31.21.172 weight=3"
```

Method 1: Build branch with jenkins
Method 2: Build create pull request and merge to main before jenkins build

power shell equivalent of tree: tree /f

## Community role

At this point I need to use remote ssh agent, so that it will be easy for me to edit remote files through ssh, since vim or nano will be very stressfull.

Pushing branch:dynamic-assignments

## MYSQL/NGINX/Apache Roles:
sudo ansible-galaxy install geerlingguy.mysql
sudo mv geerlingguy.mysql/ mysql
sudo ansible-galaxy install geerlingguy.nginx
sudo mv geerlingguy.nginx/ nginx-lb
sudo ansible-galaxy install geerlingguy.apache
sudo mv geerlingguy.apache/ apache-lb


