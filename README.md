# ServiceNow ITSM Hardware & Access Fulfillment Lab

An end-to-end IT Service Management (ITSM) workflow built in a ServiceNow Personal Developer Instance (PDI). This project simulates a corporate onboarding and hardware request process, from employee self-service submission and form validation to manager approval routing and IT Service Desk task fulfillment.

Alex Chen (requester), Jordan Taylor (manager), and Sam Rivera (IT Service Desk technician) are test users created for this lab.

## Architecture & Workflow Overview

1. **Employee Request (Service Portal):** An end user (**Alex Chen**) submits a *New Hire Hardware & Access Bundle* request through the Service Portal (`/sp`).
2. **Form Validation (Catalog UI Policy):** Selecting high-cost hardware (*High-Performance MacBook Pro*) or elevated permissions (*Remote VPN & Admin Access*) makes the business justification field mandatory.
3. **Approval Routing (Flow Designer):** The flow looks up the requester's manager (**Jordan Taylor**), sends an approval request, and waits for a decision before fulfillment continues.
4. **Fulfillment & Queue Assignment:**
   - **If approved:** The flow creates a Catalog Task (`SCTASK`) with the form variables and assigns it to the **IT Service Desk** group, where a technician with the `itil` role (**Sam Rivera**) provisions the assets and closes the task.
   - **If rejected:** The flow is configured to update the parent Requested Item (`RITM`) to *Closed Incomplete*.

**Tested path:** approval and fulfillment (`REQ0010001`, `SCTASK0010001`).

---

## Key Components Built

- **Role-Based Access Control & User Administration:** Set up Requester, Manager, and IT Fulfiller test users, and assigned the `itil` role at the group level (`IT Service Desk`) instead of to individual users.
- **Service Catalog Item & Variables:** Created a catalog item with `Select Box` and `Multi Line Text` variables mapped to the flow's data pills.
- **Catalog UI Policy:** Added conditional logic (`OR` conditions) that requires a business justification for certain selections.
- **Flow Designer Automation:** Built a low-code flow running as `System User` with `Get Catalog Variables`, `Ask for Approval`, `If/Else` branching, `Create Catalog Task`, and `Update Record` actions.

---

## Project Walkthrough & Screenshots

### 1. RBAC & IT Service Desk Group Configuration
Configured the **IT Service Desk** assignment group managed by **Jordan Taylor**, gave it the `itil` role so members can work tickets, and assigned **Sam Rivera** as the Tier 1 fulfillment technician.

<img width="1467" height="619" alt="01-it-service-desk-rbac-setup part 1" src="https://github.com/user-attachments/assets/f70b3a19-7185-4fe5-b4a9-0ff96aeb0acd" />
<img width="1467" height="619" alt="01-it-service-desk-rbac-setup part 2" src="https://github.com/user-attachments/assets/72bc012a-7a3f-4d1f-92c6-d07427aeecca" />


### 2. Service Catalog Form & UI Policy
Built the **New Hire Hardware & Access Bundle** form, with conditional validation that requires a **Business Justification** when requesting a MacBook Pro or Remote VPN access.

<img width="1467" height="828" alt="02-service-catalog-form-ui-policy" src="https://github.com/user-attachments/assets/ffb77c62-c738-4d4a-b0a8-f7d3cb904e5a" />


### 3. Flow Designer Approval & Task Automation
Built the flow that pulls the form variables, routes the approval to the requester's manager, and then either creates an IT Service Desk task or closes a rejected request.

<img width="1467" height="828" alt="03-flow-designer-approval-workflow" src="https://github.com/user-attachments/assets/ecc06b72-27bc-45f3-8e1e-fe33b7bbe131" />


### 4. End-User Portal Submission
Tested the employee experience by impersonating **Alex Chen** in the Service Portal (`/sp`) and submitting request `REQ0010001`.

<img width="1470" height="837" alt="04-portal-request-submission-alex" src="https://github.com/user-attachments/assets/43413f5b-f5b0-44f2-a131-4f03f7bdc54b" />


### 5. IT Fulfillment & Closed Catalog Task
Tested technician queue routing and ticket resolution by impersonating **Sam Rivera**, recording provisioning details in the work notes, and closing `SCTASK0010001`.

<img width="1469" height="837" alt="05-closed-catalog-task-activity-log" src="https://github.com/user-attachments/assets/ac68d0dd-dae4-48cc-a2af-c573768bd50d" />
<img width="1469" height="389" alt="05-closed-catalog-task-activity-log part 2" src="https://github.com/user-attachments/assets/68ed5230-9a76-4a78-8e53-973e9edf8f61" />


