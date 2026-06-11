# Azure DevOps Import Guide

This guide explains how to import the MedWatch backlog CSV files into Azure DevOps as Epics, User Stories, and Tasks.

## Files

| File | Work Item Type | Rows | Description |
|------|---------------|------|-------------|
| `epics.csv` | Epic | 21 | High-level capability areas |
| `user-stories.csv` | User Story | 63 | 3 stories per epic (Governance, Setup, Control Policies) |
| `tasks.csv` | Task | 63 | 1 task per user story (1:1 mapping for MVP) |

---

## Import Order

> **Important**: Import in this exact order to preserve parent-child relationships.

1. **Step 1** – Import `epics.csv`
2. **Step 2** – Import `user-stories.csv`
3. **Step 3** – Import `tasks.csv`

---

## Column Mapping

### epics.csv (3 columns)

| CSV Column | Azure DevOps Field |
|------------|-------------------|
| `Title` | Title |
| `Description` | Description |
| `Area Path` | Area Path |

### user-stories.csv (4 columns)

| CSV Column | Azure DevOps Field |
|------------|-------------------|
| `Epic` | Parent (link to Epic by title) |
| `Title` | Title |
| `Description` | Description |
| `Area Path` | Area Path |

### tasks.csv (5 columns)

| CSV Column | Azure DevOps Field |
|------------|-------------------|
| `Epic` | Grandparent Epic (reference) |
| `User Story` | Parent (link to User Story by title) |
| `Title` | Title |
| `Description` | Description |
| `Area Path` | Area Path |

---

## Area Path Structure

```
MedWatch
├── Foundation         (Sprint 1)
├── Core Platform      (Sprints 2–3)
├── Operational        (Sprint 4)
├── Automation         (Sprint 5)
└── Presentation       (Sprint 6)
```

Before importing, ensure these Area Paths exist in your Azure DevOps project under **Project Settings → Project configuration → Areas**.

---

## Hierarchy Visualization

```
Epic (21)
└── User Story (63, 3 per epic)
    └── Task (63, 1 per user story)
```

**Example**:
```
Secure Access Governance  [Epic]
├── Deliver Secure and Accountable Monitoring Access Governance  [User Story]
│   └── Establish Secure and Accountable Monitoring Access Framework  [Task]
├── Enable Secure and Accountable Monitoring Access Setup  [User Story]
│   └── Configure Secure and Accountable Monitoring Access Components  [Task]
└── Provide Secure and Accountable Monitoring Access Control Policies  [User Story]
    └── Implement Secure and Accountable Monitoring Access Decision Rules  [Task]
```

---

## Step-by-Step Import Instructions

### Prerequisites

1. Open your Azure DevOps project.
2. Confirm the Area Paths exist: `MedWatch\Foundation`, `MedWatch\Core Platform`, `MedWatch\Operational`, `MedWatch\Automation`, `MedWatch\Presentation`.
3. Confirm the work item process template supports **Epic**, **User Story**, and **Task** types (Agile or Scrum process).

### Importing epics.csv

1. Navigate to **Boards → Backlogs**.
2. Click the **⋮ (More actions)** menu → **Import work items**.
3. Select `epics.csv`.
4. Map columns:
   - `Title` → **Title**
   - `Description` → **Description**
   - `Area Path` → **Area Path**
5. Set **Work Item Type** to **Epic**.
6. Click **Import** and review any errors.

### Importing user-stories.csv

1. Navigate to **Boards → Backlogs**.
2. Click the **⋮ (More actions)** menu → **Import work items**.
3. Select `user-stories.csv`.
4. Map columns:
   - `Title` → **Title**
   - `Description` → **Description**
   - `Area Path` → **Area Path**
5. Set **Work Item Type** to **User Story** (or **Product Backlog Item** in Scrum).
6. Click **Import**.
7. After import, link each User Story to its parent Epic manually or via bulk edit using the `Epic` column as a reference.

### Importing tasks.csv

1. Navigate to **Boards → Backlogs**.
2. Click the **⋮ (More actions)** menu → **Import work items**.
3. Select `tasks.csv`.
4. Map columns:
   - `Title` → **Title**
   - `Description` → **Description**
   - `Area Path` → **Area Path**
5. Set **Work Item Type** to **Task**.
6. Click **Import**.
7. After import, link each Task to its parent User Story manually or via bulk edit using the `User Story` column as a reference.

---

## Common Import Issues and Solutions

### Issue: Area Path not found
**Solution**: Create the Area Path under **Project Settings → Project configuration → Areas** before importing. The required paths are:
- `MedWatch\Foundation`
- `MedWatch\Core Platform`
- `MedWatch\Operational`
- `MedWatch\Automation`
- `MedWatch\Presentation`

### Issue: Work Item Type not available
**Solution**: Verify your project uses the **Agile** or **Scrum** process template. Epic, User Story, and Task are standard types in both.

### Issue: Parent links not set automatically
**Solution**: Azure DevOps CSV import does not create parent-child links automatically. After import:
1. Go to **Boards → Backlogs** and switch to **Epic** view.
2. Drag and drop User Stories under their Epics, or use **Edit → Link to** to set parent relationships.
3. Repeat for Tasks under User Stories.

### Issue: Duplicate titles on re-import
**Solution**: Use the **Work Items** query editor to find and delete existing items before re-importing, or use the unique title to identify and update existing items.

### Issue: Encoding errors
**Solution**: All files use **UTF-8** encoding. If you see encoding issues, open the CSV in a text editor and save as UTF-8 before importing.

---

## Post-Import Configuration

After importing all work items, consider the following:

1. **Set Story Points**: Assign story points to User Stories in the backlog view.
2. **Assign Owners**: Assign team members to work items via bulk edit.
3. **Set Sprint/Iteration**: Map work items to sprints using the iteration paths:
   - Sprint 1 → Foundation epics
   - Sprints 2–3 → Core Platform epics
   - Sprint 4 → Operational epics
   - Sprint 5 → Automation epics
   - Sprint 6 → Presentation epics
4. **Set Priority**: Use the priority field to order items within sprints.
5. **Create Relations**: Add dependency links between tasks where noted in the source backlog.

---

## Epic-to-Sprint Reference

| Epic | Sprint | Area Path |
|------|--------|-----------|
| Secure Access Governance | Sprint 1 | MedWatch\Foundation |
| Trusted Asset Enrollment | Sprint 1 | MedWatch\Foundation |
| Consistent Data Intake | Sprint 1 | MedWatch\Foundation |
| Unified Situation Awareness | Sprint 1 | MedWatch\Foundation |
| Infrastructure Health | Sprint 1 | MedWatch\Foundation |
| Availability Transparency | Sprint 2 | MedWatch\Core Platform |
| Clinical Data Exchange Reliability | Sprint 2 | MedWatch\Core Platform |
| Workload Processing Flow | Sprint 2 | MedWatch\Core Platform |
| Application Service Health | Sprint 2 | MedWatch\Core Platform |
| Timely Critical Condition Identification | Sprint 3 | MedWatch\Core Platform |
| Structured Incident Ownership | Sprint 3 | MedWatch\Core Platform |
| Evidence-Based Investigation | Sprint 3 | MedWatch\Core Platform |
| Coordinated Response Communication | Sprint 3 | MedWatch\Core Platform |
| Reliable Automation Oversight | Sprint 4 | MedWatch\Operational |
| Planned Change Control | Sprint 4 | MedWatch\Operational |
| Policy-Driven Response Decisions | Sprint 4 | MedWatch\Operational |
| Safe Automated Issue Correction | Sprint 4 | MedWatch\Operational |
| Business Workflow Reliability | Sprint 5 | MedWatch\Automation |
| Historical Performance Insight | Sprint 5 | MedWatch\Automation |
| Practical AI Operations Guidance | Sprint 6 | MedWatch\Presentation |
| Continuous Monitoring Solution Currency | Sprint 6 | MedWatch\Presentation |
