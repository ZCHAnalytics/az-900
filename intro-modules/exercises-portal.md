
## Create VM
no public inbound ports

Task 1: Create a Linux virtual machine and install Nginx
Use the following Azure CLI commands to create a Linux VM and install Nginx. After your VM is created, you'll use the Custom Script Extension to install Nginx. The Custom Script Extension is an easy way to download and run scripts on your Azure VMs. It's just one of the many ways you can configure the system after your VM is up and running.

From Cloud Shell, run the following az vm create command to create a Linux VM:

az vm create --resource-group "learn-189bedbf-6c21-448b-ada4-81d2f187e96f" --name my-vm --public-ip-sku Standard --image Ubuntu2204 --admin-username azureuser --generate-ssh-keys

Run the following az vm extension set command to configure Nginx on your VM:

Azure CLI

Copy
az vm extension set \
  --resource-group "learn-189bedbf-6c21-448b-ada4-81d2f187e96f" \
  --vm-name my-vm \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --version 2.1 \
  --settings '{"fileUris":["https://raw.githubusercontent.com/MicrosoftDocs/mslearn-welcome-to-azure/master/configure-nginx.sh"]}' \
  --protected-settings '{"commandToExecute": "./configure-nginx.sh"}'    
  This command uses the Custom Script Extension to run a Bash script on your VM. The script is stored on GitHub. While the command runs, you can choose to examine the Bash script from a separate browser tab. To summarize, the script:

Runs apt-get update to download the latest package information from the internet. This step helps ensure that the next command can locate the latest version of the Nginx package.
Installs Nginx.
Sets the home page, /var/www/html/index.html, to print a welcome message that includes your VM's host name.

## Exercise - Configure network access

az vm list
Task 1: Access your web server
In this procedure, you get the IP address for your VM and attempt to access your web server's home page.

Run the following az vm list-ip-addresses command to get your VM's IP address and store the result as a Bash variable:
```
IPADDRESS="$(az vm list-ip-addresses --resource-group "learn-4fb36716-3e5d-4f11-ac0c-4589be8e40f8" --name my-VM --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"
```

curl --connect-timeout 5 http://$IPADDRESS

echo $IPADDRESS

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/187ad96e-c89b-4ca5-9dd2-6fc74f659e4b)


Task 2: List the current network security group rules

az network nsg list --resource-group "learn-4fb36716-3e5d-4f11-ac0c-4589be8e40f8" --query '[].name' --output tsv
Every VM on Azure is associated with at least one network security group. In this case, Azure created an NSG for you called my-vmNSG.
You see a large block of text in JSON format in the output. In the next step, you'll run a similar command that makes this output easier to read.

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/23b1755b-79a5-432f-9d20-5ccf14386704)

az network nsg rule list --resource-group "learn-189bedbf-6c21-448b-ada4-81d2f187e96f" --nsg-name my-vmNSG --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' --output table

You see the default rule, default-allow-ssh. This rule allows inbound connections over port 22 (SSH). SSH (Secure Shell) is a protocol that's used on Linux to allow administrators to access the system remotely. The priority of this rule is 1000. Rules are processed in priority order, with lower numbers processed before higher numbers.

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/e5d1523f-434c-4d0d-a87d-7d9341cc94db)

By default, a Linux VM's NSG allows network access only on port 22. This enables administrators to access the system. You need to also allow inbound connections on port 80, which allows access over HTTP.

## Task 3: Create the network security rule
Here, you create a network security rule that allows inbound access on port 80 (HTTP).
Run the following az network nsg rule create command to create a rule called allow-http that allows inbound access on port 80:

az network nsg rule create \
  --resource-group "learn-189bedbf-6c21-448b-ada4-81d2f187e96f" \
  --nsg-name my-vmNSG \
  --name allow-http \
  --protocol tcp \
  --priority 100 \
  --destination-port-range 80 \
  --access Allow    

For learning purposes, here you set the priority to 100. In this case, the priority doesn't matter. You would need to consider the priority if you had overlapping port ranges.

To verify the configuration, run az network nsg rule list to see the updated list of rules:

az network nsg rule list \
  --resource-group "learn-189bedbf-6c21-448b-ada4-81d2f187e96f" \
  --nsg-name my-vmNSG \
  --query '[].{Name:name, Priority:priority, Port:destinationPortRange, Access:access}' \
  --output table    

You see this both the default-allow-ssh rule and your new rule, allow-http:
![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/d6964fa4-7ff6-47b7-ac37-3ec6f67758cf)

curl --connect-timeout 5 http://$IPADDRESS

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/a861ac16-fb77-41fa-8f0d-82dc7e19d2db)

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/b7f6c501-d595-45d1-895a-8d0e8181bbc6)

