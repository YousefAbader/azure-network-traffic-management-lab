# Deployment Notes

## Project Scope

This project implements Azure network traffic management using two services:

- Azure Load Balancer (Layer 4)
- Azure Application Gateway (Layer 7)

The environment was deployed in:

- **Region:** Switzerland North

---

## Base Infrastructure Deployment

The initial infrastructure was provisioned using an ARM template.

### Template files used

- `az104-06-vms-template.json`
- `az104-06-vms-parameters.json`

### Base deployment outcome

The ARM deployment provisioned:

- 1 virtual network
- 1 network security group
- 3 virtual machines

### Resource group

- `az104-rg6`

---

## Load Balancer Deployment

A public Standard Load Balancer was created to distribute HTTP traffic across two backend virtual machines.

### Load Balancer configuration

- **Name:** `az104-lb`
- **SKU:** Standard
- **Type:** Public
- **Tier:** Regional
- **Frontend IP configuration:** `az104-fe`
- **Public IP:** `az104-lbpip`
- **Backend pool:** `az104-be`

### Backend targets

- `az104-06-vm0`
- `az104-06-vm1`

### Load balancing rule

- **Rule name:** `az104-lbrule`
- **Protocol:** TCP
- **Frontend port:** 80
- **Backend port:** 80

### Health probe

- **Name:** `az104-hp`
- **Protocol:** TCP
- **Port:** 80
- **Interval:** 5 seconds

---

## Application Gateway Deployment

A Standard_v2 Application Gateway was deployed to provide Layer 7 routing and path-based traffic distribution.

### Virtual network

- **VNet:** `az104-06-vnet1`

### Dedicated subnet

- **Subnet name:** `subnet-appgw`
- **Subnet range:** `10.60.3.224/27`

### Application Gateway settings

- **Name:** `az104-appgw`
- **Tier:** Standard_v2
- **Autoscaling:** Disabled
- **Instance count:** 2
- **HTTP2:** Disabled
- **Public IP:** `az104-gwpip`

### Backend pools

- `az104-appgwbe`
- `az104-imagebe`
- `az104-videobe`

### Listener and rule

- **Listener:** `az104-listener`
- **Routing rule:** `az104-gwrule`
- **Protocol:** HTTP
- **Port:** 80

### Path-based routing

- `/image/*` → `az104-imagebe`
- `/video/*` → `az104-videobe`

---

## Deployment Notes

- The original lab steps were written for East US, but this implementation was documented and prepared for **Switzerland North**.
- The deployment was organized to show both transport-layer and application-layer traffic management in the same lab environment.
- The Application Gateway was deployed in the same virtual network as the Load Balancer for lab and learning purposes.

---

## Deployment Status

- Base infrastructure: Completed
- Load Balancer: Completed
- Application Gateway: Completed
- Path-based routing: Completed
- Validation testing: Completed
