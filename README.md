# AWS VPC Traffic Flow and Security Project

This project demonstrates how to control traffic flow and secure resources inside a Virtual Private Cloud (VPC) on AWS. It covers **routing**, **subnet security**, and **resource-level access control**, offering a layered understanding of networking in the cloud.

---

## 🖼️ Project Architecture Diagram
<img width="877" height="470" alt="VPC Diagram" src="https://github.com/user-attachments/assets/da8f0df1-bb95-4eeb-b6cd-d9efb62c980f" />



> This diagram shows the complete flow of user traffic from the **Internet Gateway** into the AWS **VPC**, through **Route Tables**, **Network ACLs**, and finally into the **Public Subnet** where the EC2 instance is hosted. Each layer contributes to managing and securing the flow of data.



## 📌 Objectives

- Set up secure inbound/outbound traffic in AWS.
- Distinguish between **stateful (Security Groups)** and **stateless (Network ACLs)** traffic filters.
- Understand how Internet Gateways, Route Tables, and access control layers work together.

### 🔧 Create a VPC

- **Log in** to your AWS Management Console.
- Navigate to the **VPC Dashboard**.
- Click **Create VPC** and configure the following:
  - **Name tag**: `NextWork VPC`
  - **IPv4 CIDR block**: `10.0.0.0/16`
- ✅ Click **Create VPC**.



### 🌐 Create a Public Subnet

- Go to **Subnets** and click **Create Subnet**.
  - **VPC ID**: `NextWork VPC`
  - **Subnet Name**: `Public 1`
  - **Availability Zone**: (Select the first AZ in your region)
  - **IPv4 CIDR Block**: `10.0.0.0/24`
- Enable **Auto-assign Public IPv4 address** for this subnet.
- ✅ Click **Create Subnet**.


### 🌍 Create and Attach an Internet Gateway

- Navigate to **Internet Gateways**, click **Create Internet Gateway**:
  - **Name tag**: `NextWork IG`
- ✅ Click **Create** and then **Attach** it to your `NextWork VPC`.
  <img width="1170" height="355" alt="Screenshot AWS VPC " src="https://github.com/user-attachments/assets/9890d764-f768-4a43-a53c-4d7064e49ffc" />




### 🛣️ Create and Update Route Table

- In the VPC console, go to **Route Tables** and locate the one associated with your VPC.
- Rename it to: `NextWork Route Table`
- Add a new route:
  - **Destination**: `0.0.0.0/0`
  - **Target**: The Internet Gateway (`NextWork IG`)
- Associate this route table with the `Public 1` subnet.

---

### 🔐 Create a Security Group

- Navigate to **Security Groups** and click **Create Security Group**:
  - **Name**: `NextWork Security Group`
  - **Description**: A Security Group for the NextWork VPC.
  - **VPC**: `NextWork VPC`
- Add an **Inbound Rule**:
  - **Type**: HTTP
  - **Protocol**: TCP
  - **Port Range**: 80
  - **Source**: Anywhere (IPv4) — `0.0.0.0/0`
  <img width="1187" height="630" alt="Screenshot AWX VPC security Group" src="https://github.com/user-attachments/assets/e566cafb-8e84-4941-ad28-22f73d2ea64d" />

- ✅ Save the rule.

---

### 🚪 Create a Custom Network ACL

> While AWS provides a default Network ACL, here we’ll create one from scratch to reinforce subnet-level security.

- Go to **Network ACLs**, click **Create Network ACL**:
  - **Name**: `NextWork Network ACL`
  - **VPC**: `NextWork VPC`
- Add an **Inbound Rule**:
  - **Rule Number**: `100`
  - **Type**: All Traffic
  - **Protocol**: ALL
  - **Port Range**: ALL
  - **Source**: `0.0.0.0/0`
  - **Allow/Deny**: Allow
- Associate this Network ACL with the `Public 1` subnet.
  
<img width="932" height="198" alt="Screenshot AWS ACL In" src="https://github.com/user-attachments/assets/1126e29d-c42a-4b98-8b13-4d77d10d68f5" />
<img width="987" height="256" alt="Screenshot AWS ACL out" src="https://github.com/user-attachments/assets/f912a58d-9bd7-4b08-9671-6a6602045633" />

## 🧱 Key Components Configured

### 1. **Virtual Private Cloud (VPC)**
- CIDR Block: `10.0.0.0/16`
- Public Subnet: `10.0.1.0/24`
- Optional: Reserved CIDR block for future private subnet use.



### 2. **Internet Gateway & Route Tables**
- Internet Gateway attached to VPC.
- Custom Route Table created:
  - Associated with the Public Subnet.
  - Route added:  
    `0.0.0.0/0 → Internet Gateway`



### 3. **Network ACLs (Subnet-Level Filtering)**
- Custom NACL created and linked to the public subnet.
- **Inbound Rules:**
  - Allow HTTP (port 80)
  - Allow SSH (port 22)

- **Outbound Rules:**
  - Allow ephemeral ports (`1024-65535`)


> 🔁 Stateless: Inbound and outbound rules must be explicitly defined.

-

### 4. **Security Groups (Instance-Level Filtering)**

- Attached to the EC2 instance.
- **Inbound Rules:**
  - HTTP: `port 80` (for web traffic)
  - SSH: `port 22` (for secure shell access)
- **Outbound Rules:**
  - Allow all (default behavior)

> 🧠 Stateful: Return traffic is automatically allowed.



### 5. **EC2 Instance Deployment**
- Deployed a test EC2 instance in the public subnet.
- Attached the custom security group and verified:
  - SSH access via terminal
  - HTTP access via browser



## 🔎 Testing & Validation

- Verified connectivity and access rules using browser and SSH terminal.
- Inspected AWS logs and traffic flow using built-in monitoring tools.
- Confirmed layered behavior between NACLs and Security Groups.


## ✅ Outcomes

- Learned to manage traffic at different network layers in AWS.
- Understood the difference between **stateless** and **stateful** security controls.
- Gained practical skills on configuring secure AWS infrastructure.

