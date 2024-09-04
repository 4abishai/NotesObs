[Earlier sections of the SRS remain unchanged]

## 4. System Features

### 4.1 Patient Management

#### 4.1.1 Description and Priority

The patient management feature allows for the registration, updating, and tracking of patient information. This feature is of High priority as it forms the core of the hospital management system.

#### 4.1.2 Stimulus/Response Sequences

- User navigates to the patient registration page and enters patient details.
- System validates the information and creates a new patient record.
- User can search for existing patients and update their information.
- System provides a unique patient identifier for each new registration.

#### 4.1.3 Functional Requirements

Req. 1:
- R.1.1:
  - Input: "search" option
  - Output: user prompted to enter the key words
- R.1.2:
  - Input: key words
  - Output: Details of all patients whose name or ID matches any of the key words
    - Details include: Patient Name, Patient ID, Date of Birth, Contact Information, Emergency Contact, Insurance Information
  - Processing: Search the patient database for the keywords

Req. 2:
- R.2.1:
  - Input: "register new patient" option
  - Output: form for entering new patient details
- R.2.2:
  - Input: patient details (name, date of birth, gender, contact details, emergency contact, insurance information)
  - Output: confirmation of patient registration with unique patient identifier
  - Processing: Validate input data and generate unique patient ID

Req. 3:
- R.3.1:
  - Input: "update patient information" option and patient ID
  - Output: form with current patient information for editing
- R.3.2:
  - Input: updated patient information
  - Output: confirmation of successful update
  - Processing: Validate input data and update patient record

### 4.2 Appointment Scheduling

#### 4.2.1 Description and Priority

The appointment scheduling feature allows staff to book, reschedule, and cancel appointments for patients with various hospital departments and physicians. This feature is of High priority.

#### 4.2.2 Stimulus/Response Sequences

- User selects the appointment scheduling function and chooses a department or physician.
- System displays available time slots based on the selection.
- User selects a time slot and enters patient details.
- System confirms the appointment and sends a notification to the patient.

#### 4.2.3 Functional Requirements

Req. 4:
- R.4.1:
  - Input: "schedule appointment" option
  - Output: form to select department, physician, date, and time
- R.4.2:
  - Input: department, physician, date, and time selection
  - Output: available time slots
  - Processing: Check availability and display open slots

Req. 5:
- R.5.1:
  - Input: selected time slot and patient information
  - Output: appointment confirmation
  - Processing: Book the appointment and update the schedule

Req. 6:
- R.6.1:
  - Input: "reschedule appointment" option and appointment ID
  - Output: current appointment details and available new time slots
- R.6.2:
  - Input: new time slot selection
  - Output: confirmation of rescheduled appointment
  - Processing: Update appointment record and notify relevant parties

### 4.3 Electronic Health Records (EHR)

#### 4.3.1 Description and Priority

The Electronic Health Records feature enables the creation, storage, and retrieval of digital patient health records. This feature is of High priority.

#### 4.3.2 Stimulus/Response Sequences

- Physician accesses a patient's EHR during a consultation.
- System displays the patient's medical history, allergies, medications, and recent test results.
- Physician updates the EHR with new information, diagnoses, or treatment plans.
- System saves the updated information and maintains version history.

#### 4.3.3 Functional Requirements

Req. 7:
- R.7.1:
  - Input: patient ID and "view EHR" option
  - Output: comprehensive view of patient's medical history
- R.7.2:
  - Input: specific section selection (e.g., allergies, medications)
  - Output: detailed information for the selected section
  - Processing: Retrieve and display relevant EHR data

Req. 8:
- R.8.1:
  - Input: "update EHR" option and new medical information
  - Output: confirmation of EHR update
- R.8.2:
  - Input: file attachment (e.g., images, PDFs)
  - Output: confirmation of file attachment to EHR
  - Processing: Validate input, update EHR, and maintain version history

Req. 9:
- R.9.1:
  - Input: "share EHR" option and recipient selection
  - Output: confirmation of EHR sharing
- R.9.2:
  - Input: specific sections to share
  - Output: shared EHR access for selected recipient
  - Processing: Verify authorization and grant access to specified EHR sections

[The SRS would continue with additional features such as Billing and Insurance, Pharmacy Management, etc., following the same format for 2-3 pages total in the System Features section.]

