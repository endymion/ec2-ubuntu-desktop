# -*- mode: ruby -*-
# vi: set ft=ruby :
# This is a Vagrant configuration file. It can be used to set up and manage
# virtual machines on your local system or in the cloud. See http://downloads.vagrantup.com/
# for downloads and installation instructions, and see http://docs.vagrantup.com/v2/
# for more information and configuring and using Vagrant.

hostname = 'vnc'

Vagrant.configure("2") do |config|

  # Dummy box, because we're using EC2.
  config.vm.box = "dummy"
  config.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"

  config.vm.hostname = hostname

  config.vm.provider "virtualbox" do |v|
    v.name = hostname
    v.customize ["modifyvm", :id, "--cpuexecutioncap", "90"]
    v.customize ["modifyvm", :id, "--memory", "4096"]
    v.customize ["modifyvm", :id, "--cpus", 4]
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # port 3000 on the virtual machine is forwarded to port 7777 on the host.
  # This will allow the virtual machine to communicate of the common proxy port 7777.
  # config.vm.network :forwarded_port, guest: 3000, host: 7782, auto_correct: true

  # This port will be used to see/debug background tasks
  # config.vm.network :forwarded_port, guest: 5678, host: 7770

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network :private_network, ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # View the documentation for the provider you're using for more
  # information on available options.
  # config.omnibus.chef_version = :latest

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.

  # Un-comment this to update portage during provisioning.  Good to run this at least once.  Slow.
  # config.vm.provision :shell, :inline => "apt-get update --fix-missing"

  config.vm.provider :aws do |aws, override|
    override.vm.box = "dummy"
    override.vm.box_url = "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
    aws.ami = "ami-df6a8b9b"
    aws.access_key_id = ENV['AWS_KEY']
    aws.secret_access_key = ENV['AWS_SECRET']
    aws.keypair_name = ENV['AWS_KEYPAIR_NAME']
    override.ssh.private_key_path = ENV['AWS_KEYPAIR_PATH']
    override.ssh.username = "ubuntu"
    aws.instance_ready_timeout = 300

    aws.region = "us-west-1"
    aws.instance_type = "t2.medium"
    aws.block_device_mapping = [{ 'DeviceName' => '/dev/sda1', 'Ebs.VolumeSize' => 100 }]
  end
#
#   # User: vnc  Password: password
#   config.vm.provision :shell, :inline => "adduser --disabled-login --gecos '' vnc"
#   config.vm.provision :shell, :inline => "echo 'vnc:password'| chpasswd"
#
#   config.vm.provision :shell, :inline =>
#     "apt-get install -y --no-install-recommends lubuntu-desktop tightvncserver"
#
# $script = <<SCRIPT
# cat > /etc/lightdm/lightdm.conf <<EOF
# [VNCServer]
# enabled=true
# port=5900
# width=1366
# height=768
# depth=24
# [SeatDefaults]
# greeter-hide-users=true
# allow-guest=false
# greeter-show-manual-login=true
# EOF
# SCRIPT
#   config.vm.provision :shell, :inline => $script

  config.vm.provision :shell, :privileged => false, :inline =>
    "sudo /etc/init.d/lightdm stop; true"
  config.vm.provision :shell, :privileged => false, :inline =>
    "sudo /etc/init.d/lightdm start >/dev/null 2>&1 &"

end
