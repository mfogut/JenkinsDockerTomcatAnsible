# DevOps Project

## AWS Services for Jenkins, Tomcat, Docker, Ansible Instances
- AMI : Amazon Linux 2 AMI (HVM) as operating system.
- Instance Type : t2.micro.
- VPC : AWS Default VPC.
- Subnet : AWS Default Subnet.
- Storage : 8 GiB, General Purpose SSD (gp2).
- Security Group created.
    - Inbound Rules for SSH port 22 Source Anywhere.
    - Inbound Rules for Jenkins port 8080 Source Anywhere.
- Used existing key pair(.pem file) to access to Jenkins server over SSH.

## Jenkins Server Configuration
- Logout ec2-user and login root user --> sudo su - 
- Change hostname to Jenkins --> hostnamectl set-hostname "jenkins"
- Install Java 1.8 (Jenkins prerequisite) --> yum install java-1.8* -y
- Add $JAVA_HOME variable to .bash_profile file --> vim .bash_profile paste /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.265.b01-1.amzn2.0.1.x86_64
- Downloand Jenkins from internet --> wget jenkins_downland_file_url
- Start Jenkins service --> service start jenkins
- Install Git to Jenkins server --> yum install git -y
- Install GitHub plugin to Jenkins GUI --> Manage Jenkins -->Manage Plugins -->Available --> GitHub
- Install Maven to Jenkins server in /opt/ directory in order to make code exectuble. Configure the maven
    - vim .bash_profile
    - M2_HOME=/opt/maven
    - M2=$M2_HOME/bin
- Install Maven plugin to Jenkins GUI --> Manage Jenkins -->Manage Plugins -->Available -->Maven Integration, Maven Invoker
- Install Deploy to container Plugin to Jenkins GUI to deploy war to container --> Manage Jenkins -->Manage Plugins -->Available --> Deploy to container.
- Install Publish over SSH to Jenkins GUI to Send build artifacts over SSH (integration Docker to Jenkins)--> Manage Jenkins -->Manage Plugins -->Available --> Publish over SSH
- 

## Tomcat Server Configuration
- Spin up EC2 instance with following same step of Jenkins Server.
- Logout ec2-user and login root user --> sudo su -
- Change hostname to Jenkins --> hostnamectl set-hostname "tomcat"
- Install Java 1.8 (Jenkins prerequisite) --> yum install java-1.8* -y
- Add $JAVA_HOME variable to .bash_profile file --> vim .bash_profile paste /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.265.b01-1.amzn2.0.1.x86_64
- Open Tomcat server to internet (default open only localhost) --> find / -name context.xml, take last 2 context files and comment out loopback addresses.
- Create Username and Password for Tomcat Server.
    - cd /opt/tomcat/conf
    - vim tomcat-user.xml (Create role and user)
- Deploy war file to Tomcat server via Jenkins as a Maven Project.
- Test tomcat server exit criteria hit tomcat server ip_address:port_number on web browser.

## Docker Server Configuration
- Spin up EC2 instance with following same step of Jenkins Server.
- Logout ec2-user and login root user --> sudo su -
- Change hostname to Jenkins --> hostnamectl set-hostname "docker"
- Install Docker --> yum install docker -y
- Start docker service --> service docker start
- Download specific docker images from Docker Hub to docker server --> docker pull tomcat:8.0
- Create container from Docker images --> docker run -d --name Container-Name -p VM-Port:Container-Port Images-Name:tag
- Create Docker user in Docker Server --> useradd username , passwd username password
- Add created user to docker group --> usermod -aG docker username
- Enable Password Authentication --> vim /etc/ssh/sshd_config (comment out Passowrd Authentication No)
- Restart sshd service to in order to activate password authentication --> service sshd restart
- Deploy war to Docker Container.
- Test docker container exit criteria hit docker container ip_address:port_number on web browser.

## Ansible Server Configuration
- Spin up EC2 instance with following same step of Jenkins Server.
- Logout ec2-user and login root user --> sudo su -
- Change hostname to Jenkins --> hostnamectl set-hostname "ansible-controller"
- Install python pip(python package manager) --> yum install python-pip -y
- Create ansible user in Ansible Server --> useradd ansadmin , psswd username password
- Add ansadmin user to sudoers file --> visudo
    - ansaddmin ALL=(ALL)   NOPASSWD:ALL
- Add created user to docker group --> usermod -aG docker username
- Create ansible user in Docker Server --> useradd ansadmin , psswd username password
- Copy Ansible Server ssh id to Docker server --> ssh-copy-id Docker-Server-Private-IP
- Insert ansadmin user password.
