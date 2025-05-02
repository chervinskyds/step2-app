Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"

  # Jenkins Master
  config.vm.define "jenkins-master" do |master|
    master.vm.hostname = "jenkins-master"
    master.vm.network "private_network", ip: "192.168.56.10"
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end

    master.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y docker.io docker-compose
      usermod -aG docker vagrant

      docker run -d -p 8080:8080 -p 50000:50000 --name jenkins \
        -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
    SHELL
  end

  # Jenkins Worker
  config.vm.define "jenkins-worker" do |worker|
    worker.vm.hostname = "jenkins-worker"
    worker.vm.network "private_network", ip: "192.168.56.11"
    worker.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end

    worker.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y openjdk-17-jdk docker.io
      usermod -aG docker vagrant
    SHELL
  end
end
