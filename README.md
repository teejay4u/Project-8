# Project-8
>># Load Balancer Solution with Apache

> ###  This project was continuation of project 7, implementing a distributed solution to the architecture by introducing a load balancer as seen in the diagram below.


![](Diagram%20of%20Project.png)

1. An EC2 machine was launched with linux ubuntu running in AWS console 

![](1.%20Created%20Load-Balancer.png)

2. Apache2 installed on the machine
    - sudo apt update
    - sudo apt install apache2 -y
    - sudo apt-get install libxml2-dev
![](2.%20Installing%20Apache%20on%20serverLB.png)

    #Enabling modules:
    - sudo a2enmod rewrite
    - sudo a2enmod proxy
    - sudo a2enmod proxy_balancer
    - sudo a2enmod proxy_http
    - sudo a2enmod headers
    - sudo a2enmod lbmethod_bytraffic

    #Restarting apache2 service
    - sudo systemctl restart apache2
    #Ensuring Apache2 is running 
    - sudo systemctl status apache2

![](3.%20Apache%20is%20running.png)

3. Configuring Load Balancer and Registering Webservers to its configuration files 
    - sudo vi /etc/apache2/sites-available/000-default.conf

    - #Adding the info to the configuration file 

<Proxy "balancer://mycluster">
               BalancerMember http://<WebServer1-Private-IP-Address>:80 loadfactor=5 timeout=1
               BalancerMember http://<WebServer2-Private-IP-Address>:80 loadfactor=5 timeout=1
               ProxySet lbmethod=bytraffic
               # ProxySet lbmethod=byrequests
        </Proxy>

        ProxyPreserveHost On
        ProxyPass / balancer://mycluster/
        ProxyPassReverse / balancer://mycluster/

    #Restart apache server

    - sudo systemctl restart apache2
![](4.%20Updating%20the%20sites_available%20config%20file%20to%20register%20webservers%20.png)

4. Getting the public DNS address of Load-balancer 
![](5.%20Copying%20Load-balancer%20dns%20address%20to%20see%20if%20the%20site%20can%20be%20reached.png)

5. Site successfully loading from load balancer with registered webservers 
![](6.%20We%20can%20access%20the%20site%20from%20the%20dns%20of%20load%20balancer.png)

