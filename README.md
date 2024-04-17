# az-100

## Core Architectural Components 
exercise: Task 1: Create a virtual machine, no public inbound ports, password authentication

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/da7617f7-bef1-47a2-b31a-89a06e271d41)

## Exercise - Configure network access

az vm list
Task 1: Access your web server
In this procedure, you get the IP address for your VM and attempt to access your web server's home page.

Run the following az vm list-ip-addresses command to get your VM's IP address and store the result as a Bash variable:
```
IPADDRESS="$(az vm list-ip-addresses --resource-group "learn-4fb36716-3e5d-4f11-ac0c-4589be8e40f8" --name my-vm --query "[].virtualMachine.network.publicIpAddresses[*].ipAddress" --output tsv)"
```

curl --connect-timeout 5 http://$IPADDRESS

echo $IPADDRESS

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/4458b162-bbc3-4fa5-87cc-3f877c2f2633)


![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/5a7f0ebb-e13b-4ed6-9101-0b7bc3160ce4)

Task 2: List the current network security group rules

az network nsg list --resource-group "learn-4fb36716-3e5d-4f11-ac0c-4589be8e40f8" --query '[].name' --output tsv
Every VM on Azure is associated with at least one network security group. In this case, Azure created an NSG for you called my-vmNSG.

![image](https://github.com/ZCHAnalytics/az-100/assets/146954022/626e6d6c-e957-4f4c-83db-5d7c09ac5ed1)

