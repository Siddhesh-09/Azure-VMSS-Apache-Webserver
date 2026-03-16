# Azure VM Scale Set – Apache Web Server Demo

This project demonstrates how to deploy a Virtual Machine Scale Set (VMSS) in Microsoft Azure and host a simple static webpage using the Apache2 web server on multiple virtual machine instances.

The goal of this exercise was to understand how Azure automatically manages multiple VM instances, networking components, and load balancing to achieve scalability and high availability.

## Architecture Overview

The setup includes:

Virtual Machine Scale Set (VMSS) with 2 VM instances

Instances deployed across Availability Zone 1 and Zone 3

Azure Load Balancer automatically created during deployment

Network Interface (NIC) automatically assigned to each VM

- Inbound security rules configured for SSH and HTTP access
- Apache2 Web Server running on both instances
- Custom index.html pages to identify which VM instance serves the request

## Resources Created

During the VMSS deployment, Azure automatically created several components:

- Virtual Machine Scale Set
- Load Balancer
- Backend Pool
- Network Interfaces (NIC)
- Public IP Address
- Virtual Network (VNet)
- Subnet

## Prerequisites

- Azure subscription
- SSH client installed on your local machine
- Basic knowledge of Linux commands

## ## Configuration Steps

### 1. Deploy VM Scale Set

- Created a VM Scale Set with 2 instances
- Region: Central India
- Availability Zones: Zone 1 and Zone 3

### 2. Configure Network Security Rules

By default, some ports were not open, so inbound rules were added for:

- **Port 22 (SSH)** – to access the virtual machines
- **Port 80 (HTTP)** – to allow web traffic

### 3. Connect to VM Instances

Connected to both VM instances using SSH.

Example command:

```bash
ssh username@public-ip
```

### 4. Update the Virtual Machine

```bash
sudo apt update
sudo apt upgrade -y
```

Updating the system packages ensures the server is running the latest updates.

### 5. Install Apache Web Server

```bash
sudo apt install apache2 -y
```

### 6. Start and Enable Apache

```bash
sudo systemctl start apache2
sudo systemctl enable apache2
```
### 7. Deploy the Webpage

The webpage files were placed inside the Apache web root directory:

```
/var/www/html
```

Each VM instance was given a slightly different webpage to identify which instance is serving the request through the load balancer. Copy the respective HTML files to each VM:

- **VM Instance 1:** Use `index-vm1.html` as the landing page
- **VM Instance 2:** Use `index-vm2.html` as the landing page

## Result

Once the setup was complete:

- The Load Balancer distributed traffic between both VM instances
- Refreshing the browser showed responses coming from different VMs
- This demonstrates basic load distribution and high availability

## ## Project Purpose

This project was created as part of my Azure Cloud Learning Journey to understand:

- Virtual Machine Scale Sets
- Load Balancing
- High Availability Architecture
- Web Server Deployment on Azure
- Network Security Configuration

## ## Repository Contents

- **index-vm1.html** – Custom landing page for VM Instance 1, identifies which instance served the request
- **index-vm2.html** – Custom landing page for VM Instance 2, identifies which instance served the request
- **README.md** – Project documentation and setup instructions

## Future Improvements

Next steps to expand this project:

- Perform the same deployment using **Azure CLI** for scripted deployments
- Implement Infrastructure as Code (IaC) with **Terraform**
- Explore **ARM Templates** for Azure-native IaC
- Add auto-scaling policies based on CPU load
- Implement health monitoring and alerts

## Troubleshooting

**Unable to connect via SSH:**
- Verify the Network Security Group (NSG) allows inbound traffic on port 22
- Check that you're using the correct public IP address
- Ensure your SSH key has the correct permissions (chmod 600)

**Web server not responding:**
- SSH into the VM and verify Apache is running: `sudo systemctl status apache2`
- Check that the HTML files are in `/var/www/html/`
- Verify the Load Balancer health probes are configured correctly

**Load balancer not distributing traffic:**
- Check the backend pool status in the Azure portal
- Verify both VM instances are in a healthy state
- Review the Load Balancer rules and backend pool configuration

---

**Author:** Siddhesh Khanorkar  
**Created:** March 2026  
**Status:** Completed

*For questions or improvements, please refer to the Azure documentation or the project comments.*