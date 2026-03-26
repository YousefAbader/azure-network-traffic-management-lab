# Validation Results

## Validation Objective

The purpose of validation was to confirm that:

- the ARM deployment completed successfully
- the Load Balancer distributed traffic across backend virtual machines
- the Application Gateway backend pools were healthy
- path-based routing worked correctly for `/image/*` and `/video/*`

---

## 1) ARM Template Deployment Validation

### Validation performed

- Reviewed the deployment status in Azure portal
- Confirmed the resources were created in the target resource group
- Checked that the virtual network, network security group, and virtual machines were present

### Result

- **Status:** Successful

---

## 2) Load Balancer Validation

### Validation performed

- Opened the public IP address associated with the Load Balancer
- Verified that the browser displayed:
  - `Hello World from az104-06-vm0`
  - or `Hello World from az104-06-vm1`
- Refreshed the browser multiple times

### Expected behavior

The response should rotate between the two backend virtual machines.

### Result

- **Status:** Successful
- **Observation:** Traffic was distributed across both backend virtual machines

---

## 3) Application Gateway Backend Health Validation

### Validation performed

- Opened the Application Gateway resource
- Navigated to **Backend health**
- Checked the status of the configured backend servers

### Expected behavior

Both backend servers should display **Healthy**.

### Result

- **Status:** Successful
- **Observation:** Backend health displayed Healthy for the configured backend targets

---

## 4) Path-Based Routing Validation

### Validation performed

Tested the following URLs using the Application Gateway public frontend IP:

- `http://<frontend-ip>/image/`
- `http://<frontend-ip>/video/`

### Expected behavior

- `/image/` should route to the image backend
- `/video/` should route to the video backend

### Result

- **Status:** Successful
- **Observation:** Each path was routed to the correct backend target

---

## 5) Overall Validation Summary

### Confirmed

- ARM-based deployment completed successfully
- Public Load Balancer accepted HTTP traffic
- Backend pool configuration worked as expected
- Health probe and load balancing rule functioned correctly
- Application Gateway was deployed successfully
- Backend health was healthy
- Path-based routing worked correctly

---

## Final Outcome

The environment successfully demonstrated both:

- **Layer 4 traffic distribution** using Azure Load Balancer
- **Layer 7 path-based routing** using Azure Application Gateway
