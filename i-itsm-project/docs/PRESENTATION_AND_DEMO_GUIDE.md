# 15–20 Minute Video Presentation & Live Demo Guide
**File Naming Convention (Per PDF Page 17):** `<GroupNo>_OOP_S1_2025-2026_Project.mp4` (or `2026-2027` as instructed by your section)  
**Hard Deadline:** On or before **22.11.2026**  
**Rule:** Every team member must introduce themselves on camera/voice and explain the functioning of their respective module(s) and OOP design decisions.

---

## Part 1: Time-Boxed Presentation Structure (18 Minutes Total)

| Time | Speaker | Segment | What to Show on Screen |
| :--- | :--- | :--- | :--- |
| **00:00 – 03:30** | **Student 1** | **Introduction, Problem Analysis, Architecture & Employee Portal** | Architecture diagram in `PROJECT_BLUEPRINT_AND_REPORT.md`, Registration of a new Employee, Login, and **Workflow 1 (Part A)**: Reporting the Payment Application outage. |
| **03:30 – 09:30** | **Student 2** | **OOP Domain Model, 5 Design Patterns & Technician Workspace** | UML Class Diagram + `backend/src/domain/` & `backend/src/patterns/` code walkthrough (State, Strategy, Observer, Factory), followed by **Workflow 1 (Part B)** & **Workflow 2**: Technician acknowledging, investigating, requesting info, and resolving the incident. |
| **09:30 – 15:30** | **Student 3** | **Intelligent Features, SLA Escalation, Admin Governance & Automated Tests** | **Workflow 3**: Duplicate detection (`INC101`/`INC102`/`INC103`), Service correlation, SLA violation simulation & 4-tier escalation (`Technician -> Team Lead -> System Administrator -> IT Manager`), Strategy switching at runtime, Audit Trail, Analytics Dashboard, and running `npm test` (18 test cases passing). |
| **15:30 – 18:00** | **All 3 Students** | **Technology Justification & Q&A Defense Summary** | Summary of how OOP principles (Encapsulation, Inheritance, Polymorphism, Abstraction, Exception Handling) made the system extensible and maintainable. |

---

## Part 2: Step-by-Step Live Demonstration of the 3 Mandatory Workflows (PDF Page 15)

### Workflow 1: Employee Reports → System Prioritizes → Technician Assigned → Notification Generated
1. **Register / Login as Employee:**
   - Open `http://localhost:5173`. Click **"Register New Employee"** to demonstrate self-registration (or click the quick demo button for **Employee: Ananya (`emp.ananya@bits-itsm.edu`)**).
2. **Smart Incident Reporting:**
   - In the **Report New Incident** form, type Title: `"Payment application unavailable"` and Description: `"The organization's payment application is unavailable and users are unable to complete transactions."`
   - Click **"Auto-Analyze Description (AI Assistant)"** — point out how `IntelligenceService` automatically suggests Category = `Application`, checks for duplicates, and previews historical resolutions.
   - Select Affected Service = **Enterprise Payment Gateway (Criticality: 5)**, Impact = **3 (High - Organization-wide)**, Urgency = **3 (High - Immediate)**.
   - Submit the incident.
3. **Show Automated OOP Pipeline Execution:**
   - Point out the newly created incident card:
     - **State Pattern:** Automatically progressed `NEW → CLASSIFIED → ASSIGNED`.
     - **Priority Strategy:** Calculated Priority Score = `3 × 3 × 5 = 45` → **`P1 - Critical`**.
     - **SLA Management:** Assigned **P1 SLA** (Response Target: **5 mins**, Resolution Target: **30 mins**).
     - **Assignment Strategy:** Automatically assigned to **Arjun Mehta (`tech.arjun@bits-itsm.edu`)** based on his `Application` skill and availability.
     - **Observer Pattern:** Open the **Notifications Drawer** (top right) to show both the **In-App Notification** and **Simulated Email Notification** generated for the reporter and technician.

---

### Workflow 2: Technician Investigates → SLA Monitored → Incident Resolved → User Notified & Verified
1. **Login as Technician:**
   - Switch to **Technician: Arjun (`tech.arjun@bits-itsm.edu`)**.
   - Show the **Current Workload Indicator** (`2 / 5 active incidents`) and the live **SLA Countdown Timer** on the assigned P1 incident.
2. **Demonstrate State Pattern Guardrails:**
   - Explain that `IncidentState.js` blocks illegal jumps (e.g., a technician cannot jump directly from `ASSIGNED` to `CLOSED` or `RESOLVED` without acknowledging and investigating first).
   - Click **"1. Acknowledge Incident"** → Status transitions `ASSIGNED → ACKNOWLEDGED` and records the exact response timestamp within the 5-minute P1 target.
   - Click **"2. Start Investigation"** → Status transitions `ACKNOWLEDGED → IN_PROGRESS`.
3. **Add Investigation Note & Request Info from Employee:**
   - Add an investigation note: `"Checked payment gateway logs; connection pool exhausted on primary node. Requesting merchant transaction ID from reporter."` and check **"Request additional info from employee"**.
   - Switch briefly to **Employee: Ananya**, show the prompt requesting info, and submit a reply: `"Transaction batch #TX-9982 failed at 10:04 AM."`
4. **Resolve the Incident:**
   - Switch back to **Technician: Arjun**, enter Root Cause: `"Database connection pool leak in payment microservice"` and Resolution Summary: `"Restarted payment worker pods and increased connection pool limit to 200."`
   - Click **"3. Mark Resolved"** → Status transitions `IN_PROGRESS → RESOLVED`, technician workload decrements automatically, and the employee receives a resolution notification.
5. **Employee Feedback & Verification/Closure:**
   - Switch to **Employee: Ananya** (or **Admin**), submit a **5-Star Feedback Rating** (`"Payment gateway restored quickly!"`), verify the fix (`RESOLVED → VERIFIED`), and close the incident (`VERIFIED → CLOSED`).

---

### Workflow 3: Multiple Incidents Reported → System Identifies Duplicates/Related Incidents → Incidents Correlated/Escalated
1. **Report Correlated / Duplicate Incidents:**
   - Log in as **Employee: Vikram (`emp.vikram@bits-itsm.edu`)**.
   - Report a second payment incident: `"Payment transaction failing for customer checkout"` on **Enterprise Payment Gateway**.
   - Point out how the system immediately flags **Duplicate Detection** (matching the existing Payment incident) and links them under **Correlated Service Incidents**.
2. **Demonstrate Runtime Strategy Switching & SLA Escalation (Admin Console):**
   - Log in as **Administrator (`admin@bits-itsm.edu`)**.
   - Go to **Strategy & Governance Config**:
     - Switch Technician Assignment Strategy from `SKILL_BASED` to `LOAD_BASED` or `ROUND_ROBIN`, and Priority Strategy from `RULE_BASED` to `WEIGHTED_SCORING`.
   - Go to **SLA Monitor & Escalation**:
     - Click **"Simulate SLA Breach / Run Escalation Check"** on an unacknowledged critical incident.
     - Show how `SLAMonitorService` detects the response/resolution deadline breach, flags `SLA VIOLATED`, and triggers **Automatic Escalation** up the hierarchy: `Technician → Team Lead → System Administrator → IT Manager`.
3. **Show Complete Audit Trail & Analytics Dashboard:**
   - Open the **Audit Trail** tab to show timestamped, immutable logs (`10:05 - INC1001 created`, `10:05 - Classified as APPLICATION`, `10:05 - Priority P1`, `10:05 - Assigned to Tech`, etc.).
   - Open **Reports & Search Dashboard** to demonstrate the 8 search/filter criteria and the 3 required charts/tables: **Incident Overview**, **Technician Workload**, and **Service Analysis**.

---

## Part 3: Viva / Evaluation Defense Cheat Sheet (OOP Questions)

- **Q: Where is Encapsulation used?**
  - *Answer:* In `Domain/Incident.js` and `Domain/User.js`, internal fields (`#passwordHash`, state transitions, workload counters) are protected. State cannot be mutated via `incident.status = 'CLOSED'`; it must pass through `incident.transitionState(targetState, actor)` which validates state rules and permissions.
- **Q: Where is Inheritance and Polymorphism used?**
  - *Answer:* `Employee`, `Technician`, and `Administrator` inherit from the abstract `User` class and polymorphically override `getPermissions()` and `canTransitionTo()`. Similarly, all 4 `AssignmentStrategy` classes and both `PriorityStrategy` classes implement common strategy contracts and are invoked polymorphically by `IncidentService`.
- **Q: How does your design satisfy the Open/Closed Principle (OCP)?**
  - *Answer:* Introducing a 5th technician assignment algorithm or a new notification channel (e.g., Slack/Webhook) only requires adding a new class implementing `AssignmentStrategy` or `IncidentObserver` and registering it—zero lines of `IncidentService` orchestration code need to change.
