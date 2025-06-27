🎯 Rôle Ansible : Déploiement de WordPress
Ansible Galaxy

📦 Nom du rôle
Ymed95.wordpress

🧠 Description
Ce rôle Ansible automatise l’installation et la configuration d’une stack WordPress complète sur des systèmes basés sur Debian et RedHat (Ubuntu, Debian, Rocky Linux, etc.).

Il installe et configure :
- Apache (ou httpd)
- PHP et ses modules nécessaires
- MariaDB (avec mot de passe root sécurisé + création d’une base et d’un utilisateur WordPress)
- WordPress (téléchargement, extraction, configuration)
- VirtualHost Apache pour WordPress

📁 Structure du rôle
ansible-role-wordpress/
├── defaults/
│   └── main.yml
├── handlers/
│   └── main.yml
├── tasks/
│   └── main.yml
├── templates/
│   ├── wp-config.php.j2
│   └── wordpress.conf.j2
├── vars/
│   └── main.yml
├── meta/
│   └── main.yml
└── README.md

📋 Pré-requis
- Python installé sur la machine cible
- Accès SSH aux machines cibles
- Collection community.mysql installée :
  ansible-galaxy collection install community.mysql

⚙️ Variables du rôle
Définies dans vars/main.yml (modifiable si besoin) :

wordpress_db_name: wordpress
wordpress_db_user: example
wordpress_db_password: examplePW
mariadb_root_password: examplerootPW

wordpress_url: https://wordpress.org/latest.zip
document_root: /var/www/html
apache_config_file: wordpress.conf

wordpress_owner: www-data
wordpress_group: www-data

apache_package: "{ 'apache2' si ansible_os_family == 'Debian' sinon 'httpd' }"
apache_service: "{ 'apache2' si ansible_os_family == 'Debian' sinon 'httpd' }"
apache_conf_dir: "{ '/etc/apache2/sites-available' si ansible_os_family == 'Debian' sinon '/etc/httpd/conf.d' }"
php_package: php
php_mysql_package: "{ 'php-mysql' si ansible_os_family == 'Debian' sinon 'php-mysqlnd' }"
mariadb_package: mariadb-server
mariadb_service: mariadb

🧾 Playbook d’exemple
- hosts: all
  become: true
  roles:
    - Ymed95.wordpress

🔧 Handlers
Redémarrage d’Apache si nécessaire :
- name: Reload Apache
  service:
    name: "{{ apache_service }}"
    state: restarted

💡 Compatibilité
✅ Debian 10/11/12
✅ Ubuntu 20.04+
✅ Rocky Linux 8/9

🔗 Installation de ce rôle
Depuis Ansible Galaxy :
ansible-galaxy role install Ymed95.wordpress

Ou via requirements.yml :
- src: Ymed95.wordpress

Puis :
ansible-galaxy install -r requirements.yml

🧪 Tester localement avec Docker
Utilisez des conteneurs SSH Ubuntu ou Rocky et testez avec :
ansible-playbook -i inventory.ini test.yml
