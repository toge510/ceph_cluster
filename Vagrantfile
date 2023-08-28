$ip_addresses = ["192.168.11.31", "192.168.11.32", "192.168.11.33"]
$interface = "enp2s0"
$script = <<-'SCRIPT'
  sudo apt-get update
  sudo apt-get -y install apt-transport-https ca-certificates docker docker.io
  sudo systemctl --now enable docker
  echo -e "192.168.11.31 node1\n192.168.11.32 node2\n192.168.11.33 node3" | sudo tee -a /etc/hosts
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  
  $ip_addresses.each_with_index do |ip_address, index|
    config.vm.define "node#{index + 1}" do |node|
      node.vm.hostname = "node#{index + 1}"
      node.vm.network "public_network", ip: ip_address, bridge: $interface
      node.vm.provider "virtualbox" do |vb|
        vb.name = "node#{index + 1}"
        vb.memory = "2048"
        vb.cpus = "2"
      end
      node.vm.disk :disk, size: "10GB", name: "extra_storage"
      node.vm.provision "shell", inline: $script
      node.vm.provision "shell", inline: "timedatectl set-ntp on"
    end
  end
end