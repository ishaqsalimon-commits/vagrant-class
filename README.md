# Creating a Virtual Machine Using Vagrant

## 1. Introduction

Vagrant is a tool used to create and manage virtual machines. It works with providers such as Oracle VirtualBox to create reproducible development environments.

In this project, we will create an Ubuntu virtual machine using Vagrant and VirtualBox.

## 2. Prerequisites

Install the following software:

- [Vagrant](https://developer.hashicorp.com/vagrant/install)
- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## 3. Create a Project Directory

Open Windows PowerShell and run:

```powershell
mkdir vagrant-class
cd vagrant-class
```

## 4. Initialize Vagrant

Run:

```powershell
vagrant init ubuntu/jammy64
```

This creates a file called `Vagrantfile`.

The Vagrantfile contains the configuration for the virtual machine.

## 5. Configure the Vagrantfile

Open the file:

```powershell
notepad Vagrantfile
```

Replace its contents with:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end
end
```

Save the file.

### Configuration explained

- `config.vm.box`: Specifies the Ubuntu operating system image.
- `vb.memory`: Allocates 2 GB of RAM.
- `vb.cpus`: Allocates 2 virtual CPUs.
- `virtualbox`: Uses VirtualBox as the virtualization provider.

## 6. Validate the Configuration

Run:

```powershell
vagrant validate
```

Expected output:

```text
Vagrantfile validated successfully.
```

## 7. Create and Start the Virtual Machine

Run:

```powershell
vagrant up
```

Vagrant downloads the Ubuntu box if necessary and creates the virtual machine in VirtualBox.

## 8. Check the Virtual Machine Status

Run:

```powershell
vagrant status
```

Expected output:

```text
default running (virtualbox)
```

## 9. Connect to the Virtual Machine

Run this command in PowerShell:

```powershell
vagrant ssh
```

You should see an Ubuntu terminal.

## 10. Run Linux Commands

Inside Ubuntu, try:

```bash
whoami
```

```bash
pwd
```

```bash
ls -la
```

```bash
cat /etc/os-release
```

These commands display your username, current directory, files, and Ubuntu version.

## 11. Stop the Virtual Machine

To shut down the VM:

```powershell
vagrant halt
```

To start it again:

```powershell
vagrant up
```

## 12. Useful Vagrant Commands

| Command | Purpose |
|---|---|
| `vagrant init` | Create a Vagrantfile |
| `vagrant validate` | Validate configuration |
| `vagrant up` | Create and start VM |
| `vagrant ssh` | Connect to VM |
| `vagrant status` | Check VM status |
| `vagrant halt` | Shut down VM |
| `vagrant reload` | Restart VM |
| `vagrant suspend` | Pause VM |
| `vagrant resume` | Resume VM |
| `vagrant destroy` | Remove VM |

## 13. Conclusion

Successfully created an Ubuntu virtual machine using Vagrant and Oracle VirtualBox.

This project demonstrates the basics of virtualization and infrastructure automation.

Vagrant makes it easier to create, configure, and manage virtual machines using a simple configuration file.

# Vagrant Apache Web Server

## Project Overview

This project demonstrates how to create and configure an Apache web server using **Vagrant**, **VirtualBox**, and **Ubuntu 24.04 LTS**.

The goal is to set up a virtual machine, install Apache, and host a simple custom webpage.

## Technologies Used

- Vagrant
- Oracle VirtualBox
- Ubuntu 24.04.5 LTS
- Apache2
- PowerShell
- Markdown

## Project Setup

### 1. Create the Project Directory

In Windows PowerShell:

```powershell
cd C:\Users\hp\OneDrive\Documents\Workspace
mkdir vagrant-apache
cd vagrant-apache
```

### 2. Initialize Vagrant

```powershell
vagrant init ubuntu/jammy64
```

### 3. Configure the Vagrantfile

The `Vagrantfile` contains the virtual machine configuration.

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end
end
```

### 4. Start the Virtual Machine

```powershell
vagrant up
```

### 5. Connect to Ubuntu

```powershell
vagrant ssh
```

## Apache Installation

Inside the Ubuntu virtual machine, update the package list:

```bash
sudo apt update
```

Install Apache:

```bash
sudo apt install -y apache2
```

Enable and start Apache:

```bash
sudo systemctl enable --now apache2
```

Check Apache status:

```bash
sudo systemctl status apache2
```

Expected result:

```text
Active: active (running)
```

## Create a Custom Webpage

Create a custom homepage:

```bash
echo '<h1>Welcome to my Vagrant Apache Server!</h1>' | sudo tee /var/www/html/index.html
```

## Access the Website

Open your browser on Windows and visit:

**http://localhost:8080**

The browser should display:

> Welcome to my Vagrant Apache Server!

## Useful Vagrant Commands

| Command | Description |
|---|---|
| `vagrant up` | Start the virtual machine |
| `vagrant ssh` | Connect to Ubuntu |
| `vagrant status` | Check VM status |
| `vagrant halt` | Stop the virtual machine |
| `vagrant reload` | Restart the virtual machine |
| `vagrant provision` | Run provisioning again |
| `vagrant destroy` | Delete the virtual machine |

## Conclusion

Successfully created an Ubuntu virtual machine using Vagrant and VirtualBox, installed and configured Apache2, and hosted a custom HTML webpage.

This project demonstrates the fundamentals of virtualization, Linux server administration, web server configuration, and infrastructure automation.

# Vagrant Nginx Web Server

## 1. Project Overview

This project demonstrates how to create and configure an Nginx web server using Vagrant, Oracle VirtualBox, and Ubuntu.

Vagrant is used to create and manage the virtual machine, while Nginx is installed on Ubuntu to host a custom website.

## 2. Technologies Used

- Vagrant
- Oracle VirtualBox
- Ubuntu 22.04 / 24.04 LTS
- Nginx
- PowerShell
- Linux
- Markdown

## 3. Prerequisites

Install the following software:

- [Vagrant](https://developer.hashicorp.com/vagrant/install)
- [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## 4. Create the Project Directory

Open Windows PowerShell and run:

```powershell
cd C:\Users\hp\OneDrive\Documents\Workspace

mkdir vagrant-nginx

cd vagrant-nginx
```

## 5. Initialize Vagrant

Run:

```powershell
vagrant init ubuntu/jammy64
```

This creates a `Vagrantfile` containing the virtual machine configuration.

## 6. Configure the Vagrantfile

Open the Vagrantfile:

```powershell
notepad Vagrantfile
```

Replace its contents with:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64"

  # Forward host port 8081 to guest port 80
  config.vm.network "forwarded_port", guest: 80, host: 8081

  config.vm.boot_timeout = 600

  # Install and configure Nginx
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y nginx

    echo "<h1>Welcome to my Vagrant Nginx Server!</h1>" > /var/www/html/index.html

    systemctl enable nginx
    systemctl restart nginx
  SHELL

  # VirtualBox resources
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end
end
```

Save and close the file.

## 7. Validate the Configuration

Run:

```powershell
vagrant validate
```

Expected output:

```text
Vagrantfile validated successfully.
```

## 8. Create and Start the Virtual Machine

Run:

```powershell
vagrant up
```

Vagrant creates the Ubuntu virtual machine and runs the provisioning commands to install Nginx.

## 9. Connect to Ubuntu

Run in PowerShell:

```powershell
vagrant ssh
```

You should now be connected to the Ubuntu virtual machine.

## 10. Check Nginx Status

Inside Ubuntu, run:

```bash
sudo systemctl status nginx
```

Expected result:

```text
Active: active (running)
```

This confirms that Nginx is running.

## 11. Create a Custom Webpage

Inside Ubuntu, run:

```bash
echo '<h1>Welcome to my Vagrant Nginx Server!</h1>' | sudo tee /var/www/html/index.html
```

This creates a custom homepage for the Nginx web server.

## 12. Access the Nginx Website

Open your web browser on Windows and visit:

**http://localhost:8081**

You should see:

```text
Welcome to my Vagrant Nginx Server!
```

Port forwarding connects Windows port `8081` to Ubuntu port `80`.

## 13. Useful Vagrant Commands

| Command | Description |
|---|---|
| `vagrant init` | Initialize a Vagrant project |
| `vagrant validate` | Validate the Vagrantfile |
| `vagrant up` | Start the virtual machine |
| `vagrant ssh` | Connect to Ubuntu |
| `vagrant status` | Check VM status |
| `vagrant halt` | Shut down the VM |
| `vagrant reload` | Restart the VM |
| `vagrant provision` | Run provisioning again |
| `vagrant destroy` | Delete the VM |

## 14. Useful Nginx Commands

| Command | Description |
|---|---|
| `sudo systemctl start nginx` | Start Nginx |
| `sudo systemctl stop nginx` | Stop Nginx |
| `sudo systemctl restart nginx` | Restart Nginx |
| `sudo systemctl status nginx` | Check Nginx status |
| `sudo systemctl enable nginx` | Enable Nginx at startup |
| `sudo nginx -t` | Test Nginx configuration |

## 15. Project Folder Structure

```text
vagrant-nginx/
│
├── Vagrantfile
└── README.md
```

## 16. Conclusion

Successfully created an Ubuntu virtual machine using Vagrant and Oracle VirtualBox, installed and configured Nginx, and hosted a custom HTML webpage.

This project demonstrates:

- Virtual machine creation and management.
- Linux server administration.
- Nginx installation and configuration.
- Port forwarding between host and guest machines.
- Infrastructure automation using Vagrant.

The Nginx website is accessible through:

**http://localhost:8081**

# Multiple Vagrant Virtual Machines

## Project Overview

This project demonstrates how to create and manage multiple Ubuntu virtual machines using Vagrant and VirtualBox.

Two virtual machines are configured:

1. **Apache VM** – Runs an Apache web server.
2. **Nginx VM** – Runs an Nginx web server.

Each VM uses a separate port to allow both websites to run simultaneously.

## Technologies Used

- Vagrant
- Oracle VirtualBox
- Ubuntu 22.04 LTS
- Apache2
- Nginx
- Windows PowerShell

## Project Structure

```text
multiple-vms/
│
├── Vagrantfile
└── README.md
```

## Virtual Machine Configuration

| VM Name | Operating System | Web Server | Host Port |
|---|---|---|---|
| apache | Ubuntu 22.04 | Apache2 | 8082 |
| nginx | Ubuntu 22.04 | Nginx | 8083 |

## Installation and Setup

### 1. Navigate to the project directory

```powershell
cd C:\Users\hp\OneDrive\Documents\Workspace\multiple-vms
```

### 2. Validate the Vagrantfile

```powershell
vagrant validate
```

### 3. Start the virtual machines

```powershell
vagrant up
```

### 4. Start an individual virtual machine

Start Apache:

```powershell
vagrant up apache
```

Start Nginx:

```powershell
vagrant up nginx
```

### 5. Check virtual machine status

```powershell
vagrant status
```

## Accessing the Web Servers

### Apache Web Server

Open the following address in your browser:

http://localhost:8082

Expected output:

**Welcome to Apache VM!**

### Nginx Web Server

Open the following address in your browser:

http://localhost:8083

Expected output:

**Welcome to Ngin
