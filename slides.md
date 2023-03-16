

# Kubernetes avec Vagrant

<br>
Objectifs :

* activer le ssh hors vagrant

* créer une boucle pour créer 2 nodes

<br>
* passer une commande shell dans le Vagrantfile

```
    config.vm.provision "shell", inline: <<-SHELL
      sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
      service ssh restart
    SHELL
```

--------------------------------------------------------------------------------------------------

# Utilisation d'une boucle


<br>
* rappel : ruby

<br>

```
  numberSrv=2
  # slave server
  (1..numberSrv).each do |i|
    config.vm.define "knode#{i}" do |knode|
      knode.vm.box = "ubuntu/bionic64"
      knode.vm.hostname = "knode#{i}"
      knode.vm.network "private_network", ip: "192.168.100.1#{i}"
      knode.vm.provider "virtualbox" do |v|
        v.name = "knode#{i}"
        v.memory = 1024
        v.cpus = 1
      end
      config.vm.provision "shell", inline: <<-SHELL
        sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config
        service ssh restart
      SHELL
    end
  end
```
