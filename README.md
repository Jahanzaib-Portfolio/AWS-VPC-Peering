# AWS Cross-VPC Peering Infrastructure 

## Architectural Workflow Blueprint
This project builds two completely separated virtual networks connected seamlessly through the AWS private global backplane. The architecture model below outlines the isolated pathing configuration:

![Cross-VPC Peering Structural Workflow Architecture Diagram](images/image0.png)

---

## Project Overview
This project demonstrates how to plan, implement, and verify a secure network-to-network bridge using an **AWS VPC Peering Connection**. Rather than configuring complex, latency-heavy VPN tunnels or piping data loops over the public internet, VPC Peering merges distinct cloud networks logically. Compute layers communicate directly using private endpoints as if they belonged to an identical local framework. 

This method isolates workloads, protects application-to-database transactions, and allows distributed systems to run on separate infrastructure management timelines.

---

## Core Networking Components
- **VPC-001 (Requester)** -> Primary isolated infrastructure tier running a standard internal subnet range (`10.0.0.0/16`).
- **VPC-002 (Accepter)** -> Secondary isolated network infrastructure layer running a separate, non-overlapping address block (`192.168.0.0/16`).
- **Internet Gateways** -> Deployed on both networks to establish early-stage connectivity and configuration loops.
- **Cross-VPC Peering Matrix** -> The bidirectional logical channel configured to handle secure cross-VPC traffic requests.
- **Explicit Route Tables** -> Enhanced with target records that explicitly push traffic destined for the opposite network range through the active peering interface.
- **Layered Security Rules** -> Security group configuration matrices updated to permit selective ICMP diagnostic testing paths based on source network identification criteria.

---

## Step-by-Step Practical Implementation

### 1. Provision Custom Networks
- Access your AWS Infrastructure dashboard to start setting up your custom network environments.

![image1](images/image1.png)

- Set up your primary container (`VPC-001`) with an address scope of `10.1.0.0/16`.

![image2](images/image2.png)

- Set up your secondary container (`VPC-002`) on a non-overlapping space such as `10.2.0.0/16`.

![image3](images/image3.png)

---

### 2. Configure Subnets
- Open the subnets workspace panel to split your network addresses.

![image4](images/image4.png)

- Allocate a subnet range for `VPC-001` to anchor your first set of compute instances.

![image5](images/image5.png)

- Build a matching entry point subnet within your secondary network boundary (`VPC-002`).

![image6](images/image6.png)

---

### 3. Deploy External Access Infrastructure (Internet Gateways)
- Initialize standalone gateway points so you can run management commands from external terminal prompts.

![image7](images/image7.png)
![image8](images/image8.png)

- Verify that your newly created components are initialised in a detached layout mode.

![image9](images/image9.png)

- Use the Actions menu to link each gateway directly to its corresponding network container.

![image10](images/image10.png)
![image11](images/image11.png)

---

### 4. Create and Map Route Tables
- Create distinct route tables for each VPC layer to govern infrastructure traffic directions.

![image12](images/image12.png)
![image13](images/image13.png)

- Confirm the default association structure definitions.

![image14](images/image14.png)

- Explicitly bind your configured subnets to their respective primary route tables.

![image15](images/image15.png)
![image16](images/image16.png)

---

### 5. Initialize the VPC Peering Handshake
- Go to the VPC Peering management dash grid.

![image17](images/image17.png)

- Launch the configuration tool, assigning `VPC-001` as your requester network and `VPC-002` as your accepter target destination.

![image18](images/image18.png)
![image19](images/image19.png)

- Note that the peering status shifts to a pending confirmation status.

![image20](images/image20.png)

- Select the active connection item, choose the Actions dropdown menu, and select **Accept Request** to activate the link.

![image21](images/image21.png)
![image22](images/image22.png)

---

### 6. Spin Up Test Instances
- Go to the EC2 Dashboard workspace to deploy testing virtual machines.

![image23](images/image23.png)

- Launch an instance inside `VPC-001` to serve as your connection source.

![image24](images/image24.png)

- Update your inbound firewall policy parameters to permit incoming ICMP ping packets specifically from the `10.2.0.0/16` network segment.

![image25](images/image25.png)

- Launch a matching instance inside `VPC-002` to act as your connection target.

![image26](images/image26.png)
![image27](images/image27.png)

- Configure its security filters to allow diagnostic traffic coming from your primary network range (`10.1.0.0/16`).

![image28](images/image28.png)

---

### 7. Update Routing Records
- Select your primary route table, select **Edit Routes**, and map all destination requests headed to `10.2.0.0/16` directly through your peering link (`pcx-xxxx`).

![image29](images/image29.png)

- Select your secondary route table, open **Edit Routes**, and map all destination requests headed to `10.1.0.0/16` through the exact same peering connection entry.

![image30](images/image30.png)

---

### 8. Verify Network Connectivity
- Establish concurrent SSH or EC2 Instance Connect links to both machine hosts.

![image31](images/image31.png)
![image32](images/image32.png)

- From your source machine shell prompt inside `VPC-001`, execute a ping test targeting the private internal address of the server in `VPC-002`.

```bash
ping -c 4 10.2.0.47
