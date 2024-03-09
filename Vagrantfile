#Parameter setting
host_list = [
  { :name => "k8s1",
    :eth1 => "192.168.20.10",
    :cpus => 4,
    :memory => 8192,
    :disk => "70GB",
  },
  {
    :name => "k8s2",
    :eth1 => "192.168.20.11",
    :cpus => 2,
    :memory => 4096,
    :disk => "20GB",
  },
  {
    :name => "k8s3",
    :eth1 => "192.168.20.13",
    :cpus => 2,
    :memory => 4096,
    :disk => "20GB",
  }


]

## install VirtualBox  disk size plugin
required_plugins = %w( vagrant-vbguest vagrant-disksize )
_retry = false
required_plugins.each do |plugin|
    unless Vagrant.has_plugin? plugin
        system "vagrant plugin install #{plugin}"
        _retry=true
    end
end

if (_retry)
    exec "vagrant " + ARGV.join(' ')
end 


#Vagrant host boot
Vagrant.configure(2) do |config|
  config.vm.box = "bento/rockylinux-9"
  config.vm.synced_folder "./", "/home/vagrant"

  #loop
  host_list.each do |item|

    config.vm.define  item[:name] do |host|
      host.vm.provider "virtualbox" do |vb|
        vb.cpus = item[:cpus]
        vb.memory = item[:memory]   
    end

      host.vm.hostname = item[:name]
      host.vm.network "private_network", ip: item[:eth1]
      

      ## default 64gb  can not lower than default (Condition)
      disk_size = item[:disk].to_i  ## just retun int 

      if disk_size > 64
      host.disksize.size = item[:disk]
      end 
    end
  end
end