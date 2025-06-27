# Ansible Role: WordPress Deployment

[![Ansible Galaxy](https://img.shields.io/badge/galaxy-Ymed95.wordpress-blue.svg)](https://galaxy.ansible.com/Ymed95/wordpress)

## 📦 Role Name

**Ymed95.wordpress**

## 🧠 Description

This Ansible role automates the installation and configuration of a full WordPress stack on both Debian-based and RedHat-based systems (Ubuntu, Debian, Rocky Linux, etc.).

It installs and configures:
- Apache (or httpd)
- PHP and required modules
- MariaDB (with secured root password and a new database/user for WordPress)
- WordPress (download, unarchive, setup config)
- Apache VirtualHost for WordPress

## 📁 Role Structure

```bash
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
```

## 📋 Requirements

- Python on the target system
- SSH access to target machine(s)
- `community.mysql` collection installed:
```bash
ansible-galaxy collection install community.mysql
```

## ⚙️ Role Variables

Defined in `vars/main.yml` (and override if needed):

```yaml
wordpress_db_name: wordpress
wordpress_db_user: example
wordpress_db_password: examplePW
mariadb_root_password: examplerootPW

wordpress_url: https://wordpress.org/latest.zip
document_root: /var/www/html
apache_config_file: wordpress.conf

wordpress_owner: www-data
wordpress_group: www-data

apache_package: "{ 'apache2' if ansible_os_family == 'Debian' else 'httpd' }"
apache_service: "{ 'apache2' if ansible_os_family == 'Debian' else 'httpd' }"
apache_conf_dir: "{ '/etc/apache2/sites-available' if ansible_os_family == 'Debian' else '/etc/httpd/conf.d' }"
php_package: php
php_mysql_package: "{ 'php-mysql' if ansible_os_family == 'Debian' else 'php-mysqlnd' }"
mariadb_package: mariadb-server
mariadb_service: mariadb
```

## 🧾 Example Playbook

```yaml
- hosts: all
  become: true
  roles:
    - Ymed95.wordpress
```

## 🔧 Handlers

- Reload Apache when needed

```yaml
- name: Reload Apache
  service:
    name: "{ apache_service }"
    state: restarted
```

## 💡 Compatibility

- ✅ Debian 10/11/12
- ✅ Ubuntu 20.04+
- ✅ Rocky Linux 8/9

## 🔗 Install this Role

From Ansible Galaxy:

```bash
ansible-galaxy role install Ymed95.wordpress
```

Or add to your `requirements.yml`:

```yaml
- src: Ymed95.wordpress
```

Then run:

```bash
ansible-galaxy install -r requirements.yml
```

## 🧪 Test Locally with Docker

Use Ubuntu or Rocky SSH containers and test with:

```bash
ansible-playbook -i inventory.ini test.yml
```

## 📅 Last Updated
