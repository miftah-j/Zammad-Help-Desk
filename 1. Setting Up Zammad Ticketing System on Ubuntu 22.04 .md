
Zammad Ticketing System is a powerful, open-source solution for managing customer communication. With its web-based interface and support for various channels like email, chat, telephone, Twitter, and Facebook, Zammad offers a comprehensive platform for streamlining customer support. In this guide, we will walk you through the step-by-step process of installing Zammad on your Ubuntu 22.04 server.

## Requirements

Before we begin, make sure you have the following:

-   A server running Ubuntu 22.04
-   Root access to the server

## Update the System

The first step is to update and upgrade all the system packages on your Ubuntu server. Open a terminal and run the following commands:

    apt update -y
    apt upgrade -y

This will ensure that you have the latest versions of all the packages installed on your system.

## Install Java JDK

Zammad requires Java OpenJDK to run. Install it by running the following command:

    apt install openjdk-17-jdk -y

Once the installation is complete, verify the Java version by running:

    java -version

You should see the installed Java version in the output.

## Install ElasticSearch

Zammad relies on ElasticSearch for search functionality. To install ElasticSearch, you need to add the ElasticSearch repository to your server. Follow the steps below:

1.  Install the required dependencies:

```
apt install gnupg2 curl -y
```

2.  Add the ElasticSearch GPG key and repository to APT:

```
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | gpg --dearmor -o /usr/share/keyrings/elastic.gpg
echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | tee -a /etc/apt/sources.list.d/elastic-7.x.list
```

3.  Update the repository cache:
```
apt update -y
```
4.  Install ElasticSearch:
```
apt install elasticsearch -y
```
5.  Start and enable the ElasticSearch service:
```
systemctl enable elasticsearch.service
systemctl start elasticsearch.service
```
6.  Verify the installation by running:
```
curl -X GET'http://localhost:9200'
```
If everything is set up correctly, you should see the output with information about your ElasticSearch installation.

## Install Apache and Other Dependencies

Zammad requires the Apache web server and some additional dependencies to run. Install Apache by running:
```
apt install apache2 -y
```
Next, install libssl on your server. Add the Focal security repository and install the libssl package:
```
echo "deb http://security.ubuntu.com/ubuntu focal-security main" | tee /etc/apt/sources.list.d/focal-security.list
apt update -y
apt install libssl1.1
```
## Install Zammad Ticketing System

To install Zammad, you need to add the Zammad repository to your server. Follow the steps below:

1.  Add the Zammad GPG key:
```
curl -fsSL https://dl.packager.io/srv/zammad/zammad/key | gpg --dearmor | tee /etc/apt/trusted.gpg.d/pkgr-zammad.gpg > /dev/null
```
2.  Add the Zammad repository to APT:
```
echo "deb [signed-by=/etc/apt/trusted.gpg.d/pkgr-zammad.gpg] https://dl.packager.io/srv/deb/zammad/zammad/stable/ubuntu 22.04 main"| tee /etc/apt/sources.list.d/zammad.list
```
3.  Update the repository cache:
```
apt update -y
```
4.  Install Zammad:
```
apt install zammad -y
```
Once the installation is complete, you can proceed to configure Apache for Zammad.

## Configure Apache for Zammad

Zammad automatically creates an Apache configuration file. However, you need to make some modifications to the configuration. Open the file with a text editor:
```
nano /etc/apache2/sites-available/zammad.conf
```
In the file, locate the following lines:

> #ServerTokens Prod ServerName your-server-ip
> #RequestHeader unset X-Forwarded-User

Replace  `your-server-ip`  with the actual IP address of your server. Save the file and close the editor.

Next, disable the default Apache configuration file:

> a2dissite 000-default.conf

Finally, restart the Apache service:

    systemctl restart apache2

You can check the status of the Apache service with the following command:

    systemctl status apache2

If everything is set up correctly, you should see the status of the Apache service.

## Access Zammad Ticketing System

Now that Zammad is installed and configured, you can access it via the web interface. Open your web browser and enter the URL  `http://your-server-ip`. You should see the Zammad setup page.

Click on “Set up a new system” to proceed. You will be prompted to create an administrator account by providing your name, email, and password.

After creating the administrator account, you will be asked to define your company name and site URL.

Next, you will be prompted to select your email provider for email notifications.

Finally, you will see the Zammad dashboard, where you can start using the ticketing system to manage customer communication.

## Conclusion

Congratulations! You have successfully installed and configured the Zammad ticketing system on your Ubuntu 22.04 server. By following the steps outlined in this guide, you can now leverage the power of Zammad to streamline your customer support operations. Don’t hesitate to reach out if you have any questions or need further assistance.
