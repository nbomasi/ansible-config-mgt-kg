PREREQUISITE:

To flow with and enjoy project 13, 

1. You must have successfully done project 6-10
2. You must have successfully done project 11-12
3. You have jenkins and ansible installed and running
4. You have configured a webhook on your github to your jenkins server.
5. You have ssh-agent running on your local system.
6. You have nginx-LB(ubuntu), apache-LB(ubuntu), DB(ubuntu), 2 webservers (Redhat)

# NOTE: To ensure smooth run of the db role, note that the target db is ubuntu, I edited the following files under mysql role:
* tasks/user.yml
* defaults/main.yml

# NOTE: ON nginx role, tasks/main.yml, default/main.yml, template/nginx.conf.j2 were edited by me for load balancing configuration, not just to install nginx

* For apache, default/main.yml, template/vhosts.conf.j2 were edited for same purpose as nginx
