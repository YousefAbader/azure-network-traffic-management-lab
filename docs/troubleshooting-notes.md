# Troubleshooting Notes

## Purpose

This document records common deployment and validation issues that may occur during the implementation of the Azure Load Balancer and Application Gateway lab, along with the corrective actions used or recommended.

---

## 1) Free Account VM Core Limitation

### Issue

When using a free Azure account or a constrained personal lab subscription, the available VM quota may be limited. In my case, I worked under a practical limit of **4 total cores** for all virtual machines combined.

### Why this mattered

This lab requires **3 virtual machines**, so I had to keep the virtual machine sizing lightweight to stay within quota.

### Decision made

- I planned the environment around **three 1-vCPU virtual machines**
- This allowed the lab to fit within the available total core limit
- I checked the deployment region carefully before starting, because VM size availability and quota can vary by region

### Practical note

If the preferred VM SKU is unavailable, the selected region should be reviewed first. Region selection directly affects:

- available VM SKUs
- quota compatibility
- successful deployment of all three VMs

---

## 2) VM Size Unavailable During ARM Deployment

### Issue

The ARM template deployment may fail if the selected VM size is not available in the target subscription or region.

### Likely cause

- Regional SKU limitation
- Subscription quota limitation
- Free account capacity restrictions

### Action taken

- Reviewed the region before deployment
- Selected a VM size compatible with my available quota
- Prioritized SKUs that allowed all three VMs to be deployed successfully in the chosen region

### Result

- Deployment could proceed successfully after adjusting sizing to match the available region and quota constraints

---

## 3) Load Balancer Public IP Does Not Return a Web Page

### Issue

Opening the Load Balancer public IP does not return the expected `Hello World` page.

### Checks performed

- Verified that the Load Balancer frontend IP was created correctly
- Verified the backend pool included:
  - `az104-06-vm0`
  - `az104-06-vm1`
- Verified the load balancing rule:
  - protocol: TCP
  - port: 80
- Verified the health probe:
  - protocol: TCP
  - port: 80

### Possible causes

- Backend VMs not associated correctly
- Health probe not healthy
- Web service not responding on port 80
- NSG or configuration issue affecting connectivity

### Corrective action

- Rechecked backend pool membership
- Reviewed rule and probe settings
- Re-tested using browser refresh and InPrivate mode

---

## 4) Load Balancer Response Does Not Rotate Between VMs

### Issue

The public page loads, but the response appears to come from only one VM.

### Checks performed

- Refreshed the browser multiple times
- Opened the public IP in a new InPrivate browser window
- Confirmed both VMs were still in the backend pool

### Likely cause

- Session behavior or browser reuse
- Delay in distribution visibility during quick testing

### Corrective action

- Used repeated refreshes
- Used a new private browser session for validation

---

## 5) Application Gateway Deployment Delayed or Slow

### Issue

The Application Gateway deployment may take longer than expected.

### Likely cause

- Application Gateway provisioning time is typically longer than many other Azure networking resources

### Action taken

- Waited for deployment completion before starting validation
- Confirmed resource status in the Azure portal before testing

---

## 6) Backend Health Not Healthy in Application Gateway

### Issue

The backend health page may show an unhealthy status.

### Checks performed

- Verified the correct backend targets were added
- Confirmed the gateway was deployed in the dedicated subnet
- Confirmed backend settings were attached correctly
- Verified the listener and routing rule were created correctly

### Possible causes

- Incorrect backend pool assignment
- Backend service not responding as expected
- Misconfiguration in backend settings or routing rule

### Corrective action

- Revalidated backend pool targets
- Rechecked backend settings
- Reviewed routing rule configuration

---

## 7) Path-Based Routing Does Not Route Correctly

### Issue

Requests to `/image/` or `/video/` do not reach the intended backend.

### Checks performed

- Verified that the routing rule `az104-gwrule` existed
- Confirmed path rules were configured for:
  - `/image/*`
  - `/video/*`
- Verified backend pool mapping:
  - `/image/*` → `az104-imagebe`
  - `/video/*` → `az104-videobe`

### Possible causes

- Path rule typo
- Incorrect backend target selection
- Rule not saved or not applied correctly

### Corrective action

- Reopened the Application Gateway rule configuration
- Revalidated the path definitions
- Re-tested using direct browser access

---

## 8) Region Mismatch Between Documentation and Deployment

### Issue

The original lab guidance references East US, while this project was documented and prepared for Switzerland North.

### Risk

- Template or deployment parameters may still contain the original region
- Screenshots and written documentation may become inconsistent
- VM SKU availability may differ between regions

### Corrective action

- Standardized the repository documentation to Switzerland North
- Reviewed the template and parameter values before deployment
- Treated region selection as an early validation step rather than a last-minute fix

---

## Final Notes

This project was validated as a lab and portfolio implementation.
The troubleshooting approach focused on:

- quota-aware VM sizing
- region-aware deployment planning
- backend verification
- health validation
- routing verification
- configuration consistency
