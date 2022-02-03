# Projects
## Deploying multipule servers using vargrant with unique private Ip addresses as well as with different OS.
Hello , this md file has been created to exlpain the project and the process it took me of creating it.
If you would like to clone the file and check it out for yourself you will be able to do so with these commands :

**1. Clone the git file**
```
git@github.com:pdops/Projects.git
```
**2. Install the depenencies (for linux)**
```
sudo apt-get install vagrant 
sudo apt install virtualbox 
```
Now that you have the file , let me start explaining how I created it.

To beging with I initiated a vagrant file in a empty folder , and from there on our I deleted the contents of the file copying the main settings to use as reference.
For the first step I used the command:
```
vagrant init bento/ubuntu-18.04
```
and once the machine had been initiated and the file created I started creating the configuration's for the multiple server by providing each server with a unique 
hostname , box , Ip and ssh port so that I can connect to each of the individually. As an example I have provided the configuartion for the first server which is
supposed to be a Ubuntu server: 
```
{
  :hostname => "Ubuntu",
  :box => "bento/ubuntu-18.04",
  :ip => "172.16.1.50",
  :ssh_port => '2200'
},
```
I also wanted to define what I was going to call the 3 configurations when I was referencing to them so I put them in square brackets and name the entire
configuartion servers.

Once the configuartions was set up I wanted to create a loop for all of the configuartions that I set up so that vagrant could create them one after another.
To do that I used servers.each to reference the configuartion group that I named.
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
Once everything has been completed , I saved the program and tried to see if the servers would run , so first I checked for syntax error's using vagrant validate
and once everything was ok , I used the vagrant up command and all the servers ran perfectly and I was able to connect to  them using ssh however I had to define 
which machine I was connecting to as just connecting with ssh would not do anything.

```
vagrant validate
vagrant up 
vagrant ssh Ubuntu/Mint/Centos
```
This is the entire guide to the Project , hopefully it was helpfull.
