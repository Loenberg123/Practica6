PROJECT_NAME = "PrestaShop"
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_url = "https://vagrantcloud.com/ubuntu/boxes/xenial64"

  config.vm.hostname = 'prestashop.dev'
  config.vm.network :private_network, ip: "192.168.33.103"
  config.vm.network :forwarded_port, guest: 80, host: 8080

  config.vm.provider :virtualbox do |v|
    v.name = PROJECT_NAME + "." + Time.now.strftime("%y%m%d%H%M")
    v.customize ["modifyvm", :id, "--memory", 2048]
  end

  config.vm.synced_folder "www", "/var/www", :owner => "www-data", mount_options: ["dmode=775,fmode=664"], type: "rsync", rsync__exclude: "cache", disabled: false

  if Vagrant.has_plugin?("vagrant-gatling-rsync")
    config.gatling.latency = 0.7
    config.gatling.rsync_on_startup = false
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
    ansible.inventory_path = "ansible/hosts"
    ansible.verbose = "v"
    ansible.limit = "all"
  end

end
