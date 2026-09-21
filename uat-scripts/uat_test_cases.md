# UAT Test Scripts — NovaPharma Vault QualityDocs

> **Document ID:** NP-VAULT-UAT-001 | **Version:** 1.0 | **Total Test Cases:** 20

These test scripts are also available in the Configuration Workbook (`config/NovaPharma_Vault_Configuration_Workbook.xlsx`, tab: "UAT Test Scripts") with columns for Actual Result, Pass/Fail, Tester, and Date.

---

## TC-001 — Author Creates a New SOP Document

**Category:** Document Creation  
**Preconditions:** User logged in with Author role; SOP Template available

| Step | Action |
|------|--------|
| 1 | Navigate to Library |
| 2 | Click Create Document |
| 3 | Select Type: SOP > Quality SOP |
| 4 | Fill mandatory metadata (Department, Owner, GxP Classification) |
| 5 | Upload source file |
| 6 | Click Save |

**Expected Result:** Document created in Draft state with correct metadata. Document number auto-generated per naming convention (SOP-QA-XXXX-v1.0).

---

## TC-002 — Mandatory Fields Validation

**Category:** Document Creation  
**Preconditions:** User logged in with Author role

| Step | Action |
|------|--------|
| 1 | Click Create Document |
| 2 | Select Type: SOP |
| 3 | Leave Department and Document Owner blank |
| 4 | Click Save |

**Expected Result:** System displays validation error. Document is NOT created.

---

## TC-003 — Submit Draft SOP for Review

**Category:** Lifecycle Transition  
**Preconditions:** Document exists in Draft state; all required fields populated

| Step | Action |
|------|--------|
| 1 | Open document |
| 2 | Click Actions > Submit for Review |
| 3 | Confirm submission |

**Expected Result:** Document moves to In Review. Review workflow (WF-001) initiates. Email notification sent to assigned reviewers.

---

## TC-004 — Reviewer Approves Document

**Category:** Workflow - Review  
**Preconditions:** Document in In Review; user logged in as SME Reviewer

| Step | Action |
|------|--------|
| 1 | Navigate to My Tasks |
| 2 | Open review task |
| 3 | Add annotation/comment |
| 4 | Click Approve |

**Expected Result:** Review task completed. If all parallel reviewers approved, workflow advances to QA Review.

---

## TC-005 — Reviewer Rejects Document

**Category:** Workflow - Review  
**Preconditions:** Document in In Review; review task assigned

| Step | Action |
|------|--------|
| 1 | Open review task |
| 2 | Add rejection comment |
| 3 | Click Request Changes |

**Expected Result:** Document returns to Draft. Author receives notification. All pending review tasks cancelled.

---

## TC-006 — E-Signature Approval (21 CFR Part 11)

**Category:** Workflow - Approval  
**Preconditions:** Document passed review; In Approval state; user = QA Head

| Step | Action |
|------|--------|
| 1 | Open approval task |
| 2 | Review document |
| 3 | Click Approve |
| 4 | Enter username and password for e-signature |
| 5 | Add meaning: "Approved for use" |

**Expected Result:** E-signature captured with username, timestamp, and meaning. Document moves to Approved. Audit trail records signature event.

---

## TC-007 — Make Document Effective

**Category:** Lifecycle Transition  
**Preconditions:** Document in Approved state

| Step | Action |
|------|--------|
| 1 | Open Approved document |
| 2 | Click Actions > Make Effective |
| 3 | Confirm |

**Expected Result:** Document moves to Effective. Training assignments generated (if Training Required = Yes). Previous version moves to Superseded.

---

## TC-008 — Read-Only User Cannot Edit

**Category:** Security  
**Preconditions:** Effective document; user = Read-Only

| Step | Action |
|------|--------|
| 1 | Navigate to document |
| 2 | Attempt to click Edit |
| 3 | Attempt lifecycle action |

**Expected Result:** Edit button not visible. No lifecycle actions available. View and Download only.

---

## TC-009 — External Auditor Access Restrictions

**Category:** Security  
**Preconditions:** Documents in various states; user = External Auditor

| Step | Action |
|------|--------|
| 1 | Search for documents across all states |
| 2 | Attempt to open Draft document |
| 3 | Open Effective document |

**Expected Result:** Draft/In Review/Approved documents not visible. Effective documents visible with read-only access.

---

## TC-010 — Create New Version

**Category:** Version Control  
**Preconditions:** Effective document; user = Author

| Step | Action |
|------|--------|
| 1 | Open Effective document |
| 2 | Click Actions > Create New Version |
| 3 | Modify content |
| 4 | Save |

**Expected Result:** New version (v2.0) created in Draft. Original remains Effective until new version reaches Effective.

---

## TC-011 — Custom Metadata Validation

**Category:** Metadata  
**Preconditions:** Document with all custom fields

**Expected Result:** All fields display correct values. Picklists show only configured options. Required fields enforced.

---

## TC-012 — Document Status Report

**Category:** Reporting  
**Preconditions:** Multiple documents across departments

**Expected Result:** Report displays count by lifecycle state for selected department. Exportable to CSV.

---

## TC-013 — Audit Trail Verification

**Category:** Audit Trail  
**Preconditions:** Document has gone through Draft → In Review → Approved → Effective

**Expected Result:** Audit trail shows all events (creation, metadata changes, state transitions, workflow actions, e-signatures) with timestamps and user details. Non-editable.

---

## TC-014 — Periodic Review Auto-Trigger

**Category:** Periodic Review  
**Preconditions:** Effective document; review cycle = 24 months elapsed

**Expected Result:** Review task auto-generated for Document Owner. Review date updated after completion.

---

## TC-015 — Retire Document to Obsolete

**Category:** Document Retirement  
**Preconditions:** Effective document; Change Control approved; user = QA Admin

**Expected Result:** Document moves to Obsolete. Read-only for QA Admin, not visible to others.

---

## TC-016 — Auto-Generated Document Number

**Category:** Naming Convention  
**Preconditions:** New Quality SOP created

**Expected Result:** Number follows SOP-QA-XXXX-v1.0 pattern with auto-incremented sequence.

---

## TC-017 — Create from Template

**Category:** Template Usage  
**Preconditions:** SOP Template in Effective state

**Expected Result:** New document pre-populated with template content. Template reference maintained.

---

## TC-018 — Vault Loader Bulk Update

**Category:** Bulk Operations  
**Preconditions:** 50 documents need Department update; CSV prepared; user = QA Admin

**Expected Result:** All 50 documents updated. Loader log shows success. Audit trail updated per document.

---

## TC-019 — Advanced Metadata Search

**Category:** Search  
**Preconditions:** Multiple documents with various metadata

**Expected Result:** Filter by Type + Department + Status returns only matching documents.

---

## TC-020 — 21 CFR Part 11 Compliance

**Category:** Electronic Signatures  
**Preconditions:** User = Approver

| Step | Action |
|------|--------|
| 1 | Attempt e-sign with wrong password |
| 2 | Attempt with correct password |
| 3 | Check audit trail |
| 4 | Verify signature manifest |

**Expected Result:** Failed attempt logged. Successful signature recorded with user, timestamp, meaning. Manifest shows all signatories.
