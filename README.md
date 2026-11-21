# ServiceNow ITSM Hardware & Access Fulfillment Lab

An end-to-end IT Service Management (ITSM) workflow built in a ServiceNow Personal Developer Instance (PDI). This project simulates a real-world corporate onboarding and hardware request lifecycle—from employee self-service submission and dynamic form validation to automated manager approval routing and IT Service Desk task fulfillment.

## Architecture & Workflow Overview

1. **Employee Request (Service Portal):** An end user (**Alex Chen**) submits a *New Hire Hardware & Access Bundle* request via the Service Portal (`/sp`).
2. **Dynamic Validation (Catalog UI Policy):** Selecting high-cost hardware (*High-Performance MacBook Pro*) or elevated permissions (*Remote VPN & Admin Access*) dynamically enforces a mandatory business justification field.
3. **Automated Approval Routing (Flow Designer):** The workflow inspects the requester's user profile, routes an approval request to their direct manager (**Jordan Taylor**), and pauses fulfillment until a decision is recorded.
4. **Conditional Fulfillment & Queue Assignment:**
   * **If Approved:** Automatically generates a Catalog Task (`SCTASK`) containing all form variables and assigns it to the **IT Service Desk** queue, where an ITIL-licensed technician (**Sam Rivera**) provisions the assets and closes the ticket.
   * **If Rejected:** Automatically updates the parent Requested Item (`RITM`) state to *Closed Incomplete*.

---

## Key Components Built

* **Role-Based Access Control (RBAC) & User Administration:** Configured distinct personas across *Requester*, *Manager*, and *IT Fulfiller* roles, assigning the `itil` role at the group level (`IT Service Desk`) rather than directly to individual users.
* **Service Catalog Item & Variables:** Created a custom catalog item with structured `Select Box` and `Multi Line Text` variables mapped cleanly to backend workflow data pills.
* **Catalog UI Policies:** Implemented client-side conditional logic (`OR` conditions) to enforce business justification requirements dynamically.
* **Flow Designer Automation:** Built a low-code workflow running as `System User` with `Get Catalog Variables`, `Ask for Approval`, `If/Else` branching logic, `Create Catalog Task`, and `Update Record` actions.

---

## Project Walkthrough & Screenshots

### 1. RBAC & IT Service Desk Group Configuration
Configured the **IT Service Desk** assignment group managed by **Jordan Taylor**, granted ticket-handling permissions via the `itil` role, and assigned **Sam Rivera** as the Tier 1 fulfillment technician.
<img width="1467" height="619" alt="01-it-service-desk-rbac-setup part 1" src="https://github.com/user-attachments/assets/1f46e368-3864-4de5-b067-d956bb3032a9" />
<img width="1467" height="619" alt="01-it-service-desk-rbac-setup part 2" src="https://github.com/user-attachments/assets/3e8c0a5d-da18-44dc-b932-ed724983b4ab" />


### 2. Dynamic Service Catalog Form & UI Policy
Built the **New Hire Hardware & Access Bundle** form with conditional validation that dynamically requires a **Business Justification** when requesting a MacBook Pro or Remote VPN access.
<img width="1467" height="828" alt="02-service-catalog-form-ui-policy" src="https://github.com/user-attachments/assets/901fbf01-3b3e-4b96-a178-be27de9f9260" />


### 3. Flow Designer Approval & Task Automation
Automated the backend lifecycle to pull form variables, route approvals dynamically to the requester's manager, and branch between creating an IT Service Desk task or closing rejected requests.
<img width="1467" height="828" alt="03-flow-designer-approval-workflow" src="https://github.com/user-attachments/assets/40b73379-d1b1-478b-80cc-0b57c4216a3e" />


### 4. End-User Portal Submission
Validated the employee experience by impersonating **Alex Chen** in the Service Portal (`/sp`) to submit request `REQ0010001`.
<img width="1470" height="837" alt="04-portal-request-submission-alex" src="https://github.com/user-attachments/assets/2554f8d5-3eaf-40a3-9370-deca2de3cf96" />


### 5. IT Fulfillment & Closed Catalog Task
Validated technician queue routing and ticket resolution by impersonating **Sam Rivera**, documenting provisioning details in the work notes, and closing `SCTASK0010001`.
<img width="1469" height="837" alt="05-closed-catalog-task-activity-log" src="https://github.com/user-attachments/assets/bce12c18-21e0-4a39-8147-2cad4755bf05" />
<img width="1469" height="389" alt="05-closed-catalog-task-activity-log part 2" src="https://github.com/user-attachments/assets/1066b9be-cd0a-44c3-b84e-2ef1be6307b8" />


