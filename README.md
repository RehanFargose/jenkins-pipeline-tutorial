# 20 commands of linux
```
compgen -c
```

# git fetch push pull merge
```
git init
git clone url
cd 
git remote add origin url
git config --global user.name "your_username"
git config --global user.email "your_email_address@example.com"
git pull
git push
git fetch 
git status
git merge
```


# java jenkins
### without parameters
```
make java file with name Simple.java:
class Simple{  
    public static void main(String args[]){  
     System.out.println("Hello World");  
    }  
}  

build execute windows batch command
D:
javac Simple.java
java Simple 
```
### with parameters
```
make java file for parameters
This project is parameterized
parameter name:
Name
Surname

code:
class StringArguments
{
    public static void main(String args[])
    {
        System.out.println("My name is "+args[0]);
        System.out.println("My surname is "+args[1]);
    }
}

build execute windows batch command
D:
cd D:\jenkinsviva
javac Yourfilename.java
java Yourfilename %Name% %Surname%
```

# batch jenkins
### for parameters code
```
Here u must tick on parametrized project and then create 4 parameters given below and their corresponding types
name,sur=string, city=choice, student=boolean(yes/no)
echo "Your name is %name%"
echo "Your Surname is %sur%"
echo "Your Favorite City is %city%"
echo "Are you a student %student%"
```

# maven and ant
### maven
```
create Maven project
SCM: GIT
enter git link for maven
goals and options: clean compile test package
output directory : C:\ProgramData\Jenkins\.jenkins\workspace\maventry\target
```
### ant
```
create freestyle project
SCM: GIT
enter git link for ant
build: invoke ant
ant version :ant
targets: clean compile test package war
```

# pipeline with stages and parameters
```
pipeline {
  agent any
  stages {
    stage('Form') {
      input {
        message "Please fill this form"
        parameters {
          string(name: 'PERSON', defaultValue: '', description: 'Your Name: ')
          booleanParam(name: 'STUDENT', defaultValue: true, description: 'Student')
          choice(name: 'DIVISION', choices: ['A', 'B'], description: 'Pick division')
          text(name: 'PID', defaultValue: '', description: 'Enter your PID')
        }
      }
      steps {
        echo "Hello, ${params.PERSON}"
        echo "Toggle: ${params.STUDENT}"
        echo "Choice: ${params.DIVISION}"
        echo "PID: ${params.PID}"
      }
    }
    stage('info') {
            steps {
                echo 'This was your information'
            }
        }
    stage('Bye') {
            steps {
                echo 'Bye'
            }
        }
  }
  post {
    failure {
      echo 'The build was unsuccessful, try again later.'
    }
  }
}
```

# webhook
```
ngrok http 8080
webhook create
webhook ngrok http link paste /github-webhook/
webhook applicaton json
build triggers : github hook trigger for gitscm polling
pipeline: defn: pipeline script from scm
scm git
repo url
script path: hello-world/Jenkinsfile
build manually once
FInally update github repo
```

# Puppet
### Commands to run on puppet Master
```
Step 1: Perform till sudo nano
sudo apt-get update
wget https://apt.puppetlabs.com/puppet-release-bionic.deb
sudo dpkg -i puppet-release-bionic.deb
sudo apt-get install puppetmaster
apt policy puppetmaster
sudo systemctl status puppet-master.service
sudo nano /etc/default/puppet-master
Add this line in the puppet master file: JAVA_ARGS="-Xms512m -Xmx512m"
sudo systemctl restart puppet-master.service
sudo ufw allow 8140/tcp
sudo nano /etc/hosts
add line here "master-ip puppet"

Step 4: Perform these 2 commands to sign these cert req from slave
Run these two commands at the end(after every slave command is executed)
sudo puppet cert list
sudo puppet cert sign --all
```

### Commands to run on slave node/ puppet agent
```
Step 2: Perform till sudo nano
sudo apt-get update
wget https://apt.puppetlabs.com/puppet-release-bionic.deb
sudo dpkg -i puppet-release-bionic.deb
sudo apt-get install puppet
sudo nano /etc/hosts
add line here "master-ip puppet"

Step 3: Pefrom these 2 commands to send request to master
sudo systemctl start puppet
sudo systemctl enable puppet
sudo puppet agent --test
```

# Puppet Manifest
### On Master terminal
```
sudo mkdir -p /etc/puppet/code/environments/production/manifests/
cd /etc/puppet/code/environments/production/manifests/
sudo nano site.pp
```
### Following manifests to be written in site.pp
### Manifest 1
```
file {'/tmp/hi.txt':
ensure => present,
mode =>  '0644',
content => "it works on ${ipaddress_eth0}! \n",
}
```
### Manifest 2
```
node default{
package {'nginx':
ensure => installed,
}
file {'/tmp/status.txt':
content => 'nginx installed',
mode =>  '0644',
}
}
```
### Manifest 3
```
node default {
exec { 'apt-update': 
# exec resource named 'apt-update'
command => '/usr/bin/apt-get update' 
# command this resource will run
}
# install apache2 package
package { 'apache2':
require => Exec['apt-update'], 
# require 'apt-update' before install$
ensure => installed,
}
}
```
### On slave terminal
```
After writing every manifest, sudo puppet agent --test
```

### After every manifest
##### For 1st manifest - On slave
```
cd
cd /tmp
cat hi.txt
```

##### For 2nd manifest - On browser
```
go to Slave's Ip address
nginx webpage shows
```

##### For 3rd manifest - On browser
```
go to Slave's Ip address
Apache2 webpage shows
```
