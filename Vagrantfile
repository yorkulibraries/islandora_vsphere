# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.
  config.vm.provider "virtualbox" do |v|
    v.name = "Islandora 7.x-1.x Base VM"
  end
  config.vm.hostname = "islandora"

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "ubuntu/trusty64"

  config.vm.network :forwarded_port, guest: 8080, host: 8080 # Tomcat
  config.vm.network :forwarded_port, guest: 3306, host: 3306 # MySQL
  config.vm.network :forwarded_port, guest: 8000, host: 8000 # Apache

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", '3000']
    vb.customize ["modifyvm", :id, "--cpus", "2"]   
  end

  config.vm.provider "vmware_fusion" do |v, override|
    override.vm.box = "phusion/ubuntu-14.04-amd64"
    override.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"

    v.vmx["memsize"] = "3000"
    v.vmx["numvcpus"] = "2"
  end

  config.vm.provider :vsphere do |vsphere, override|
    override.vm.box = 'dzirkler/vsphere-dummy'
    override.vm.box_url = "https://atlas.hashicorp.com/dzirkler/boxes/vsphere-dummy"
    #config.ssh.username = "ubuntu"
    # The host we're going to connect to
    vsphere.host = '10.0.0.8'
    # The host for the new VM
    vsphere.compute_resource_name = '10.0.0.2'
    # The resource pool for the new VM
    vsphere.resource_pool_name = 'vagrant-yudl'
    # The template we're going to clone
    vsphere.template_name = 'template_xenial' #<--You'll need a 14.04 Ubuntu template
    # The name of the new machine
    #vsphere.name = "dummy"
    # vSphere login
    vsphere.user = 'administrator@vsphere.local'
    # vSphere password - leave commented out and vagrant will prompt for it
    # If you don't have SSL configured correctly, set this to 'true'
    vsphere.insecure = true
    # datastore - pick one of the 4 available SSD drives: esxi2_ssd1, esxi2_ssd2, esxi2_ssd3, esxi2_ssd4
    vsphere.data_store_name = 'esxi2_ssd1'
    # memory and CPU
    vsphere.memory_mb = '4096'
    vsphere.cpu_count = '6'
    vsphere.customization_spec_name = 'vagrant-yudl'
    vsphere.linked_clone = true
  end 
 
  shared_dir = "/vagrant"

  config.vm.provision :shell, inline: "sudo sed -i '/tty/!s/mesg n/tty -s \\&\\& mesg n/' /root/.profile", :privileged =>false
  config.vm.provision :shell, path: "./scripts/bootstrap.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/devtools.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/fits.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/fcrepo.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/djatoka.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/solr.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/gsearch.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/drupal.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/tesseract.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/ffmpeg.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/warctools.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/sleuthkit.sh", :args => shared_dir
  config.vm.provision :shell, path: "./scripts/islandora_modules.sh", :args => shared_dir, :privileged => false
  config.vm.provision :shell, path: "./scripts/islandora_libraries.sh", :args => shared_dir, :privileged => false
  config.vm.provision :shell, path: "./scripts/post.sh"

  if File.exist?("./scripts/custom.sh") then
    config.vm.provision :shell, path: "./scripts/custom.sh", :args => shared_dir
  end
end
