# Ansible Role: Update & Upgrade & Install Package

Bu Ansible playbook'u, Debian ve Red Hat tabanlı sunucuların güncellenmesini ve belirli paketlerin kurulmasını otomatikleştirir. Debian sistemlerinde `apt` ile, Red Hat sistemlerinde ise `yum` ile güncellemeleri gerçekleştirir. Ayrıca, kullanıcının belirlediği bir paketi tüm sunuculara yükler; paket zaten yüklü ise bu durumu bildirir, değilse yükleme işlemini tamamlar. Bu sayede, sunucuların düzenli güncellemelerinin ve gerekli yazılımların kolayca yönetilmesini sağlar.


## Önkoşullar
1. **SSH** erişimi olan ve sudo yetkisine sahip Linux tabanlı hedef sunucular.
````bash
ssh-copy-id -i ~/.ssh/mykey root@192.168.1.152
ssh-copy-id -i ~/.ssh/mykey root@192.168.1.153
ssh-copy-id -i ~/.ssh/mykey root@192.168.1.156
````
<br>

2. **Envanter Dosyasını Düzenle**: Projenin inventory dizininde bulunan `server_groups.yml` dosyasını kendi ortamınıza göre ayarlayın.

<br>

```
# Örnek yapılandırma:
all:
  children:
    debian:
      hosts:
        debian-server-1:
          ansible_host: 192.168.1.152
        debian-server-2:
          ansible_host: 192.168.1.153
        debian-server-3:
          ansible_host: 192.168.1.156
      vars:
        ansible_user: 'root'
        ansible_connection: 'ssh'
        package_name: "sl"
    
    redhat:
      hosts:
        redhat-server-1:
          ansible_host: 192.168.1.11
        redhat-server-2:
          ansible_host: 192.168.1.13
      vars:
        ansible_user: 'root'
        ansible_connection: 'ssh'
        package_name: "sl"

```
3. Update ve upgrade işlemi yanı sıra bir paket yükletmek isterseniz. inventory dizininde bulunan `server_groups.yml` dosyasında `package_name:` kısma ekleyin. 

    Eğer bir paket yükletmek istemezseniz `main.yml` dosyasından `Install Package` kısmını açıklama satırı haline getirebilirsiniz.

<br>

# Yapı

````bash
ansible-role-update/
├── ansible.cfg
├── collections
│   └── requirements.yml
├── hosts
├── inventory
│   └── server_groups.yml
├── LICENSE
├── playbooks
│   └── roles
│       └── distro-update
│           ├── files
│           ├── handlers
│           │   └── main.yml
│           ├── meta
│           ├── output
│           ├── tasks
│           │   ├── 01_debian_update.yml
│           │   ├── 02_redhat_update.yml
│           │   ├── 03_install_package.yml
│           │   └── main.yml
│           ├── templates
│           └── vars
│               └── main.yml
├── README.md
└── site.yml

12 directories, 13 files
````