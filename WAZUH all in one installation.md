*Note: I encountered some issues while installing WAZUH all-in-one the other day, so I don't have screenshots. However, you can follow these steps to install and troubleshoot the process.* 

Installing WAZUH on the Linux based systems can seem to be a challenging at first, but with the all-in-one installation, the process can be completed in under 20 minutes.


## Prerequesites: 
- Server having a static ip address
- Curl installed on server
- Turn off ufw or any active firewalls temporarily to avoid conflict with installtion

#### Step 1: Download the Wazuh installation assistant and the configuration file. ####
$curl -sO https://packages.wazuh.com/4.10/wazuh-install.sh
$curl -sO https://packages.wazuh.com/4.10/config.yml

----
#### Step 2: Open the config.yaml and set the indexer, manager and dashboard <ip-address> following the server static ip address 
----
#### Step 3: Run the Wazuh installation assistant with the option --generate-config-files to generate the Wazuh cluster key, certificates, and passwords necessary for installation. You can find these files in ./wazuh-install-files.tar. 
$bash wazuh-install.sh --generate-config-files

----
#### Step 4: install the WAZUH all in one installation which automatically installs the dashboard, manager and indexer. Enter the directory where the wazuh-install.sh is located and run the following line.
$sudo bash ./wazuh-install.sh -a

----
#### Step 5: After installation, the login credentials will be provided in order to login to the website. and also website ip address will be provided (usually the server ip address). 
----
#### Step 6: Try to login to the website and check if all components are correctly loaded,if yes then you are done!! However, if there any error occurs here are some troubleshooting steps.
----
#### 1. Run the following command to verify that Filebeat is successfully installed. 
$filebeat test output

----
#### If there are any errors, try the following
- Ensure filebeat is installed and is the same version as elasticsearch. *When i was installing WAZUH using the all in one, i had to manually install filebeat for it to work*
- Ensure that all 3 indexer, server and dashboard are running by using these commands
- systemctl start wazuh-indexer
- systemctl start wazuh-manager
----
#### If you face this error Error: No template found for the selected index-pattern "wazuh-alerts-*
You can  manually add the index by running the following command:

curl https://raw.githubusercontent.com/wazuh/wazuh/v4.7.4/extensions/elasticsearch/7.x/wazuh-template.json | curl -X PUT "https://localhost:9200/_template/wazuh" -H 'Content-Type: application/json' -d @- -u <user>:<password> -k 

----
### Here is the source of the reference link: https://groups.google.com/g/wazuh/c/2voSddp8ml8

And just like that WAZUH can be easily installed. 






