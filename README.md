# Projects
## Deploying multipule servers using vargrant with unique private Ip addresses as well as with different OS.

**1. Clone the git file**
```
git clone git@github.com:pdops/Projects.git
```
**2. Install the dependencies (for linux)**
```
sudo apt-get install vagrant 
sudo apt install virtualbox 
```
Now that you have the necessities, create an empty folder and initialize the vagrant file using the command:
```
vagrant init bento/ubuntu-18.04
```
And once the vagrant file has been created, open the file with a editor of your choice and delete all of the uncessery parts. Once that has been done, you can start defining all of the hosts using the variable "name" so that they can be used as a reference later. Each host should have it's own hostname , box with the image associated with it ,unique ip address and ssh ports so that the machines can be connected with ssh on the different stations. This is an example of the defined host and one of the configuartions for the hosts:
```
servers =[
  {
    :hostname => "Ubuntu",
    :box => "bento/ubuntu-18.04",
    :ip => "172.16.1.50",
    :ssh_port => '2200'
  },
```
Once the hosts have been set create a loop for all of the host options that were set up so that vagrant can reference and loop trough them.
To do that use servers.each to reference the configuartion group that was named *"servers"* , from there on out define a variable *"name"* , the name that was used for the project was *"machine"* , once it has been completed a loop needs to be be done for the rest of the configurations which can be created using the **config.vm.define** command which creates a loop and you can use the machine variable which will allow you to reference each of the host and after that define it as node.
```
servers.each do |machine|
  config.vm.define machine [:hostname] do |node|
    node.vm.box = machine[:box]
    node.vm.hostname = machine[:hostname]
    node.vm.network :private_network, ip: machine[:ip]
    node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id:"ssh"
    node.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.cpus = "3"
  end
end
```
Once the loops and configurations have been finished save the Vagrantfile and open up the terminal so that you can test if the machine is working , first check for any syntax errors by using the **vagrant validate** command and and if everything is ok , the command will return a successful status. Finally boot up the machine by using the **vagrant up** command and after that you will see all of the machines booting up 1 by 1 and once finished , you will be able to connect to any of them by using the **vagrant ssh** and any of the host names associated with the machine. 

```
vagrant validate
vagrant up 
vagrant ssh Ubuntu/Mint/Centos
```
This is the entire guide to the Project , hopefully it was helpful.
