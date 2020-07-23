Cài đặt Prometheus

Launching F5 BIGIP Virtual Edition
----------------------------------

Login to the VNG Cloud Portal

.. image:: _static/general/vng-portal-login.png

From the landing page, click on vMarketplace

.. image:: _static/general/vng-portal-landingpage.png

In the search text box of Marketplace, type bigip then select "F5-BIGIP Virtual Edtion - BYOL".

BYOL means Bring Your Own Licence - You need to have your own license to utilize the instance. Please have the license key with you before proceeding to the next steps.
If you want to get the trial license, please go here https://www.f5.com/trials/big-ip-virtual-edition

.. image:: _static/general/vng-portal-marketplace-search-bigip.png

Click on "LAUNCH ON COMPUTE ENGINE" from the product page of F5-BIGIP Virtual Edition - BYOL

.. image:: _static/general/vng-portal-launch-bigip.png

Choose the instance flavor as you want. It's recommended that you should start with 16GB RAM and 4 vCPU, with at least 80GB storage as root volume

.. image:: _static/general/vng-portal-bigip-instance-config.png

Below is the summary page of what will be created and the cost

.. image:: _static/general/vng-portal-bigip-launch-summary.png

Prepare to checkout..

.. image:: _static/general/vng-portal-checkout.png

Applying any coupon..

.. image:: _static/general/vng-bigip-checkout2.png

Make the payment..

.. image:: _static/general/vng-bigip-cloud-checkout3.png

Payment confirmation

.. image:: _static/general/vng-big-ip-checkout-done.png

Wait for about 5 to 10 minutes, your instance will be ready as shown in the example below

.. image:: _static/general/vng-bigip-instance-detail.png

Accessing F5 BIGIP Instance Management Interface
------------------------------------------------

Check your inbox, you will have an email from VNG Cloud team with detail of how to access your instance.

Example as the one below:

.. image:: _static/general/vng-bigip-logindetail.png

Adjusting your Security Group (inbound rules) to allow accessing ports 22/tcp, 8443/tcp for management interfaces of the instance itself. Other ports such as 80, 443 should be opened for your application later on.

.. image:: _static/general/vng-securitygroup.png


Licensing your F5 BIGIP instance
--------------------------------

Access to the Web-based management interface of the F5 BIGIP instance as mentioned in the email from VNG Cloud team.

Typically, it is https://<wan_IP>:8443.
Click on "Activate"

.. image:: _static/general/vng-bigip-license.png

Key in the license key string. If your instance can reach the internet directly, you can choose Automatic as Activation Method. If not, click on Manual and follow the next steps.

.. image:: _static/general/vng-bigip-license-key.png


Manual activation:

* Step 1: select and copy to clipboard all the text in "Dossier".
* Step 2: Click on "Click here to access F5 Licensing Server".
* Step 3: Paste the Dossier text in https://activate.f5.com/license/dossier.jsp.

.. image:: _static/general/license-activate1.png

* Step 4: Accept User Legal Agreement

.. image:: _static/general/license-activate2.png

* Step 5: Copy the content or download the license file

.. image:: _static/general/license-activate3.png


* Step 6: Paste the content of the license file into "License" text field of your F5 BIGIP instance. Then click Next

.. image:: _static/general/license-activate4.png

Your instance will be restarting some services and ready after few minutes

Re-configure the network
------------------------

Access the instance via SSH by user root, then launch TMSH to re-configure the network settings

.. code-block:: console

    [root@bigip1:Active:Standalone] ~ # tmsh

Disable DHCP on management interface

.. code-block:: console

    modify sys db dhclient.mgmt value disable

Re-configure the self IP and adding a default route.
(10.4.222.3/24 and 10.4.222.1 are the ip address and default gateway assigned by DHCP on VNG Cloud on instance start)

.. code-block:: console

    create net self self1_nic address 10.4.222.3/24 vlan internal
    create net route defaultroute network 0.0.0.0/0 gw 10.4.222.1

Save the configuration

.. code-block:: console

    save sys config


Provisioning modules
--------------------

Depend on your license and usage, you should go to System --> Resource Provisioning to turn on/off the modules.

Below is an example screenshot of activating Advanced Web Application Firewall and Application Visibility and Reporting modules.

.. image:: _static/general/vng-bigip-provisioning.png

Changing the password
---------------------

Before starting to configure anything further, REMEMBER TO CHANGE THE PASSSWORD of admin user.

Goto System --> Users --> User List --> Select admin user --> Change the password.

You can give "admin" the access to SSH by selecting "Advanced Shell" or "tmsh".
If you open SSH to public, REMEMBER to change the ROOT password as well. Make it very difficult or disable root login completely.

.. image:: _static/general/change-password.png

You can continue with other tasks such as configuring NTP, timezone, hostname, DNS, remote syslog.. but they are optional sometimes. It's up to you.
You've just finished the basic setup of F5 BIGIP instance in VNG Cloud.

Congratulation! and do not forget to check out `F5 Networks official support page <https://support.f5.com/>`_
