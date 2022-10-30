# Common variables
PROVIDER        = "libvirt"
PROVIDER_DRIVER = "kvm"
BOOTSTRAP_PATH  = "bootstrap"

# Network variables
NETWORK = "private_network"
SUBNET  = "192.168.60"

# Zabbix Server variables
ZABBIX_SERVER_NODES      = 1
ZABBIX_SERVER_CPU        = "2"
ZABBIX_SERVER_MEMORY     = "1024"
ZABBIX_SERVER_IMAGE_NAME = "generic/ubuntu2004"
ZABBIX_SERVER_PREFIX     = "zabbix-server"
ZABBIX_SERVER_RANGE      = 10
ZABBIX_SERVER_IP         = "192.168.60.11"

# Zabbix Proxy variables
ZABBIX_PROXY_NODES       = 1
ZABBIX_PROXY_CPU         = "1"
ZABBIX_PROXY_MEMORY      = "512"
ZABBIX_PROXY_IMAGE_NAME  = "generic/ubuntu2004"
ZABBIX_PROXY_PREFIX      = "zabbix-proxy"
ZABBIX_PROXY_RANGE       = 20
ZABBIX_PROXY_IP          = "192.168.60.21"

# Zabbix Linux Client variables
ZABBIX_CLIENT_LINUX_NODES      = 1
ZABBIX_CLIENT_LINUX_CPU        = "1"
ZABBIX_CLIENT_LINUX_MEMORY     = "384"
ZABBIX_CLIENT_LINUX_IMAGE_NAME = "generic/ubuntu2004"
ZABBIX_CLIENT_LINUX_PREFIX     = "client-linux"
ZABBIX_CLIENT_LINUX_RANGE      = 30

# Zabbix Windows Client variables
ZABBIX_CLIENT_WIN_NODES      = 1
ZABBIX_CLIENT_WIN_CPU        = "4"
ZABBIX_CLIENT_WIN_MEMORY     = "384"
ZABBIX_CLIENT_WIN_IMAGE_NAME = "jborean93/WindowsServer2019"
ZABBIX_CLIENT_WIN_PREFIX     = "client-win"
ZABBIX_CLIENT_WIN_RANGE      = 40

# Kubernetes variables
K8S_NODES        = 1
K8S_CPU          = "4"
K8S_MEMORY       = "1536"
K8S_IMAGE_NAME   = "generic/ubuntu2004"
K8S_PREFIX       = "k8s"
K8S_POD_CIDR     = "10.244.0.0/16"
K8S_SVC_CIDR     = "10.96.0.0/16"
K8S_VERSION_KUBE = "1.23.4-00"
K8S_VERSION_CRI  = "1.5.9-0ubuntu1~20.04.4"
K8S_RANGE        = 50
        
# Vagrant deployment
Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    # General VM settings
    #config.vm.provider :libvirt do |provider|
    config.vm.provider :"#{PROVIDER}" do |provider|
        provider.driver = "#{PROVIDER_DRIVER}"
    end
      
    # Zabbix Server Nodes
    (1..ZABBIX_SERVER_NODES).each do |i|
        config.vm.define "#{ZABBIX_SERVER_PREFIX}-#{i}" do |zabbix|
            zabbix.vm.box = ZABBIX_SERVER_IMAGE_NAME
            zabbix.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + ZABBIX_SERVER_RANGE}"
            zabbix.vm.hostname = "#{ZABBIX_SERVER_PREFIX}-#{i}"
            zabbix.vm.provider :"#{PROVIDER}" do |provider|
                provider.memory = ZABBIX_SERVER_MEMORY
                provider.cpus = ZABBIX_SERVER_CPU
                end
            zabbix.vm.provision "ansible" do |ansible|
                ansible.playbook = "#{BOOTSTRAP_PATH}/#{ZABBIX_SERVER_PREFIX}.yml"
                ansible.extra_vars = {
                    node_count: "#{i}",
                    node_prefix: "#{ZABBIX_SERVER_PREFIX}",
                    zabbix_server_ip: "#{ZABBIX_SERVER_IP}",
                    zabbix_proxy_ip: "#{ZABBIX_PROXY_IP}",
                }
            end
        end
    end

    # Zabbix Proxy Nodes
    (1..ZABBIX_PROXY_NODES).each do |i|
        config.vm.define "#{ZABBIX_PROXY_PREFIX}-#{i}" do |zabbix|
            zabbix.vm.box = ZABBIX_PROXY_IMAGE_NAME
            zabbix.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + ZABBIX_PROXY_RANGE}"
            zabbix.vm.hostname = "#{ZABBIX_PROXY_PREFIX}-#{i}"
            zabbix.vm.provider :"#{PROVIDER}" do |provider|
                provider.memory = ZABBIX_PROXY_MEMORY
                provider.cpus = ZABBIX_PROXY_CPU
                end
            zabbix.vm.provision "ansible" do |ansible|
                ansible.playbook = "#{BOOTSTRAP_PATH}/#{ZABBIX_PROXY_PREFIX}.yml"
                ansible.extra_vars = {
                    node_count: "#{i}",
                    node_prefix: "#{ZABBIX_PROXY_PREFIX}",
                    zabbix_server_ip: "#{ZABBIX_SERVER_IP}",
                    zabbix_proxy_ip: "#{ZABBIX_PROXY_IP}",
                }
            end
        end
    end

    # Zabbix Linux Clients
    (1..ZABBIX_CLIENT_LINUX_NODES).each do |i|
        config.vm.define "#{ZABBIX_CLIENT_LINUX_PREFIX}-#{i}" do |zabbix|
            zabbix.vm.box = ZABBIX_CLIENT_LINUX_IMAGE_NAME
            zabbix.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + ZABBIX_CLIENT_LINUX_RANGE}"
            zabbix.vm.hostname = "#{ZABBIX_CLIENT_LINUX_PREFIX}-#{i}"
            zabbix.vm.provider :"#{PROVIDER}" do |provider|
                provider.memory = ZABBIX_CLIENT_LINUX_MEMORY
                provider.cpus = ZABBIX_CLIENT_LINUX_CPU
                end
            zabbix.vm.provision "ansible" do |ansible|
                ansible.playbook = "#{BOOTSTRAP_PATH}/#{ZABBIX_CLIENT_LINUX_PREFIX}.yml"
                ansible.extra_vars = {
                    node_count: "#{i}",
                    node_prefix: "#{ZABBIX_CLIENT_LINUX_PREFIX}",
                    zabbix_server_ip: "#{ZABBIX_SERVER_IP}",
                    zabbix_proxy_ip: "#{ZABBIX_PROXY_IP}",
                }
            end
        end
    end

    # Zabbix Windows Clients
    (1..ZABBIX_CLIENT_WIN_NODES).each do |i|
        config.vm.define "#{ZABBIX_CLIENT_WIN_PREFIX}-#{i}" do |zabbix|
            zabbix.vm.box = ZABBIX_CLIENT_WIN_IMAGE_NAME
            zabbix.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + ZABBIX_CLIENT_WIN_RANGE}"
            zabbix.vm.hostname = "#{ZABBIX_CLIENT_WIN_PREFIX}-#{i}"
            zabbix.vm.provider :"#{PROVIDER}" do |provider|
                provider.memory = ZABBIX_CLIENT_WIN_MEMORY
                provider.cpus = ZABBIX_CLIENT_WIN_CPU
                end
            zabbix.vm.provision "shell",
                inline: 'cmd.exe /c \'winrm set winrm/config/service @{AllowUnencrypted="true"}\''
            zabbix.vm.provision "ansible" do |ansible|
                ansible.playbook = "#{BOOTSTRAP_PATH}/#{ZABBIX_CLIENT_WIN_PREFIX}.yml"
                ansible.extra_vars = {
                    node_count: "#{i}",
                    node_prefix: "#{ZABBIX_CLIENT_WIN_PREFIX}",
                    zabbix_server_ip: "#{ZABBIX_SERVER_IP}",
                    zabbix_proxy_ip: "#{ZABBIX_PROXY_IP}",
                }
            end
        end
    end

    # Kubernetes
    (1..K8S_NODES).each do |i|
        config.vm.define "#{K8S_PREFIX}-#{i}" do |k8s|
            k8s.vm.box = K8S_IMAGE_NAME
            k8s.vm.network "#{NETWORK}", ip: "#{SUBNET}.#{i + K8S_RANGE}"
            k8s.vm.hostname = "#{K8S_PREFIX}-#{i}"
            k8s.vm.provider :"#{PROVIDER}" do |provider|
                provider.memory = K8S_MEMORY
                provider.cpus = K8S_CPU
                end
            k8s.vm.provision "ansible" do |ansible|
                ansible.playbook = "#{BOOTSTRAP_PATH}/#{K8S_PREFIX}.yml"
                ansible.extra_vars = {
                    node_count: "#{i}",
                    node_prefix: "#{K8S_PREFIX}",
                    node_ip: "#{SUBNET}.#{i + K8S_RANGE}",
                    pod_cidr: "#{K8S_POD_CIDR}",
                    svc_cidr: "#{K8S_SVC_CIDR}",
                    version_kube: "#{K8S_VERSION_KUBE}",
                    version_cri: "#{K8S_VERSION_CRI}",
                    zabbix_server_ip: "#{ZABBIX_SERVER_IP}",
                    zabbix_proxy_ip: "#{ZABBIX_PROXY_IP}",
                }
            end
        end
    end
end
