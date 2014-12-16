ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

# BUILD ALL WITH: vagrant up --no-parallel

Vagrant.configure("2") do |config|

  # BUILD WITH: vagrant up composer --provider=docker
  config.vm.define "composer" do |v|
    # Share the laravel folder wth the container
    v.vm.synced_folder "laravel/", "/opt/laravel", create: true
    v.vm.provider "docker" do |d|
      # Needed to force the Proxy VM in a Linux host with docker
      #d.force_host_vm = true
      d.vagrant_vagrantfile = "./Vagrantfile.proxy"
      d.build_dir = "./docker/composer"
      d.remains_running = false
      d.name = "composer"
    end
  end

  # BUILD WITH: vagrant up phpfpm --provider=docker
  config.vm.define "phpfpm" do |v|
    # we sync the app folder to the php-fpm container b/c php-fpm needs to read files too
    v.vm.synced_folder "laravel/", "/opt/webapp", create: true
    v.vm.provider "docker" do |d|
      # Needed to force the Proxy VM in a Linux host with docker
      #d.force_host_vm = true
      d.vagrant_vagrantfile = "./Vagrantfile.proxy"
      d.build_dir = "./docker/phpfpm"
      d.remains_running = true
      d.name = "phpfpm"
    end
  end

  # BUILD WITH: vagrant up nginx --provider=docker
  config.vm.define "nginx" do |v|
    v.vm.provider "docker" do |d|
      # Needed to force the Proxy VM in a Linux host with docker
      #d.force_host_vm = true
      d.vagrant_vagrantfile = "./Vagrantfile.proxy"
      d.build_dir = "./docker/nginx"
      d.remains_running = true
      d.name = "nginx"
      # maps the container's 80 port, to the host machine's 80 port
      d.ports = ["80:80","443:443"]
      # this adds an phpfpm host in the /etc/hosts on this container with the dynamic internal ip that phpfpm gets
      d.link("phpfpm:phpfpm")
      # same location as the phpfpm box
      d.volumes = ["/opt/webapp"]
      # tell this container to mount volumes from the phpfpm box
      d.create_args = ["--volumes-from=phpfpm"]
    end
  end

end
