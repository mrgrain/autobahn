# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    # box settings
    config.vm.box = "scotch/box"
    config.vm.box_version = ">= 2.5"

    # network
    config.vm.network "private_network", ip: "192.168.200.19"
    # You may safely use this domain, we control it and guarantee it won't be used otherwise
    config.vm.hostname = "my.autobahn.rocks"

    # synced directory
    config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=766"] }

    # hostmanager
    if Vagrant.has_plugin?("vagrant-hostmanager")
        config.hostmanager.enabled = true
        config.hostmanager.manage_host = true
    end

    # provisioning
    config.vm.provision "shell", inline: <<-SHELL
        cd /var/www
        composer install
        cp -n .env.example .env
        sudo -u vagrant ./vendor/bin/autobahn keys:generate -q
        sudo -u vagrant ./vendor/bin/wp core install --url="http://my.autobahn.rocks" --skip-email
        sudo -u vagrant ./vendor/bin/wp theme activate twentysixteen
    SHELL
end
