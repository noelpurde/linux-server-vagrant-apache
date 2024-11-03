# Linux Server Project - Vagrant and Apache Configuration

This project provides a streamlined setup for running a Linux-based web server using Vagrant and VirtualBox, ideal for web application deployment and testing. This guide covers configuring Apache to serve a Python WSGI application and integrating PostgreSQL as the database backend.

---

## Prerequisites

1. **VirtualBox**
2. **Vagrant**

---

## Setup Instructions

### 1. Initialize Vagrant
   - Create a vagrant folder for the project, open Git Bash in that directory, and run:
     ```bash
     vagrant init ubuntu/trusty64
     ```
   - Start the virtual machine:
     ```bash
     vagrant up
     ```

### 2. Configure Port Forwarding
   - In the `Vagrantfile`, uncomment and update the line to enable port forwarding from your host’s port `8080` to the VM's port `80`:

   - Restart the VM:
     ```bash
     vagrant reload
     ```

### 3. Install Apache and Enable Port Access
   - SSH into the virtual machine:
     ```bash
     vagrant ssh
     ```
   - Install Apache:
     ```bash
     sudo apt-get install apache2
     ```

### 4. Set Up WSGI Application
   - Install `mod_wsgi` for Apache:
     ```bash
     sudo apt-get install libapache2-mod-wsgi
     ```
   - Edit the Apache configuration:
     ```bash
     sudo nano /etc/apache2/sites-enabled/000-default.conf
     ```
   - Add this line within the `<VirtualHost *:80>` block:
     ```apache
     WSGIScriptAlias / /var/www/html/myapp.wsgi
     ```
   - Create the `myapp.wsgi` file:
     ```bash
     sudo nano /var/www/html/myapp.wsgi
     ```
   - Add the following code in Python for a simple response:
     ```python
     def application(environ, start_response):
         status = '200 OK'
         output = 'A linux server!'
         response_headers = [('Content-type', 'text/plain'), ('Content-Length', str(len(output)))]
         start_response(status, response_headers)
         return [output]
     ```
   - Restart Apache:
     ```bash
     sudo apache2ctl restart
     ```

### 5. Install PostgreSQL
   - Install the PostgreSQL database server:
     ```bash
     sudo apt-get install postgresql
     ```
     Create a table and start testing for it to show up on localhost.

---

## Testing

- Test the web server by navigating to [http://localhost:8080](http://localhost:8080).
