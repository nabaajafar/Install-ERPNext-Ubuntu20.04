# Steps to Install ERPNext in Ubuntu20.04
# First steps, you should make sure the system is up to date:
*    $ sudo apt update
*    $ sudo apt -y upgrade

#  Second reboot your system:
*    $ sudo reboot

#  Install Python Tools & wkhtmltopdf that help to convert html to pdf and making bills:
*    $ sudo apt -y install vim libffi-dev python3-pip python3-dev  python3-testresources libssl-dev wkhtmltopdf

# Install Curl, Redis and Node.js 
for transferring data using various network protocols name stands for "Client URL",and in-memory data structure store, used as a distributed in-memory key–value database, cache and message broker, 
and executes JavaScript code outside a web browser:
  *  $ sudo apt install curl
*    $ sudo curl --silent --location https://deb.nodesource.com/setup_14.x | sudo bash -
*    $ curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
*    $ echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
*    $ sudo apt-get update && sudo apt-get install yarn
*    $ sudo apt -y install gcc g++ make nodejs redis-server

#  Install Nginx web server and MariaDB Database server:
*    $ sudo apt -y install nginx
*    $ sudo apt install mariadb-server

** Change authentication plugin, do it line by line and write your own password: **

*    $ sudo mysql -u root
*      USE mysql;
*      UPDATE user SET plugin='mysql_native_password' WHERE User='root';
*      UPDATE user SET authentication_string=password('your_password') WHERE user='root';
*      FLUSH PRIVILEGES;
*      EXIT;

#  settings for mysqld and mysql client:
*     $ sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
** Replace the whole file with the file in repository named ["ERPNext_mariadb_configuration"](https://github.com/nabaajafar/Install-ERPNext-Ubuntu20.04/blob/main/ERPNext_mariadb_configuration) , safe the file by press Ctrl x then enter after that write the command below:**
*     $ sudo systemctl restart mariadb

#  Install Bench and ERPNext:
** A bench(the application): is a tool used to install and manage ERPNext and CLI tool for frappe deployments on your Ubuntu system. **
** Now you will create a user that will run the ERPNext system, then configure the system:**
*     $ sudo useradd -m -s /bin/bash erpnext
*     $ sudo passwd erpnext
*     $ sudo usermod -aG sudo erpnext

#  update your PATH line by line:
*     $ sudo su - erpnext
*     $ tee -a ~/.bashrc<<EOF
*     PATH=\$PATH:~/.local/bin/
*     EOF
*     $ source ~/.bashrc

#  Create a directory for ERPNext setup and give erpnext user read and write permissions to the directory:
*     $ sudo mkdir /opt/bench
*     $ sudo chown -R erpnext /opt/bench

#  switch to erpnext user and install the application:
*     $ sudo su - erpnext
*     $ cd /opt/bench

# Install frappe bench and git:
** It provides an easy interface to help you setup and manage multiple sites and apps based on Frappe Framework.**
*     $ sudo apt install git
*     $ sudo pip3 install frappe-bench

 #  Initialize the bench directory with frappe framework installed. Make sure you are still inside the /opt/bench directory:
*     $ bench init erpnext
    
#  Create a new Frappe site:
*     $ cd erpnext
*     $ bench new-site erp.mysite

#  Download the ERPNext application from frappe Github repository:
*     $ bench get-app branch version-13 erpnext https://github.com/frappe/erpnext

#  Install ERPNext App on our Site:
*     $ bench --site erp.mysite install-app erpnext

#  Start ERPNext and finish Installation
*     $ bench start

