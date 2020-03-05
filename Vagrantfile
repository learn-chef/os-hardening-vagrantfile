$chefinstallscript = <<-'SCRIPT'
curl -O https://packages.chef.io/files/stable/chef-workstation/0.16.31/el/7/chef-workstation-0.16.31-1.el7.x86_64.rpm
yum -y install chef-workstation-0.16.31-1.el7.x86_64.rpm
yum -y install tree wget nano vim-minimal emacs-nox
yum -y install git
echo "host target" >> .ssh/config
echo "    User vagrant" >> .ssh/config
echo "    Hostname 192.168.50.11" >> .ssh/config
chmod 600 .ssh/config
chown vagrant .ssh/config
echo "eval \`ssh-agent\`" >> .bashrc
echo "ssh-add ~/.ssh/id_rsa" >> .bashrc
SCRIPT
Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.define "target" do |target|
    target.vm.network "private_network", ip: "192.168.50.11"
    target.vm.hostname = "targetnode"
  end
  config.vm.define "workstation", primary: true do |ws|
    ws.vm.network "private_network", ip: "192.168.50.10"
    ws.vm.hostname = "workstation"
    ws.vm.provision "file", source: ".vagrant/machines/target/virtualbox/private_key", destination: "$HOME/.ssh/id_rsa"
    ws.vm.provision "shell", inline: $chefinstallscript
  end
  #config.vm.synced_folder "src/", "/var/www/html"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
    vb.cpus = "2"
  end
end
