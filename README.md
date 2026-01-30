# AI-Powered-Physiotherapy-System-n8n-

Project Overview

This project implements an AI-powered appointment scheduling agent for Dr. Strange (Physiotherapist) using n8n.
The workflow automates patient appointment booking, availability checks, conflict prevention, calendar event creation, and confirmation notifications.

The system integrates:

Google Sheets as the patient and appointment database

Google Calendar for scheduling and conflict detection

AI/NLP (optional) for parsing free-text booking requests

Features

Automated appointment booking

Availability check before scheduling

Conflict and overlap prevention

30-minute buffer between appointments

Appointment confirmation via email

Rescheduling and cancellation support

Business rule enforcement (working hours & weekdays)

Business Rules Implemented

Working Days: Monday to Friday

Working Hours: 9:00 AM – 5:00 PM

Timezone: Asia/Singapore

Appointment Duration: 30 minutes (default)

Buffer Time: 30 minutes after every appointment

No Lunch Break

No Overlapping or Conflicting Appointments

If a preferred slot is unavailable:

Suggest the next available slot on the same day

If none available, suggest slots within the next 7 working days

Workflow Architecture (n8n Nodes)

Webhook Trigger
Receives appointment requests (JSON or form data)

AI / NLP Node (Optional)
Parses free-text appointment requests into structured data

Google Sheets – Read
Checks existing patient records and appointments

Function / If Node
Applies business rules and validates:

Working hours

Weekdays

Buffer time

Input format

Google Calendar – Search Events
Checks for conflicting appointments (including buffer time)

Google Calendar – Create / Update / Delete Event

Creates appointment

Blocks buffer time

Updates or cancels events when needed

Google Sheets – Append / Update
Stores or updates patient and appointment data

Email Node (SMTP / Gmail)
Sends booking, reschedule, or cancellation confirmations

Webhook Response
Returns booking status and suggestions

Input Format (Example)
{
  "action": "book",
  "patient_name": "John Doe",
  "email": "john@example.com",
  "phone": "+65XXXXXXXX",
  "preferred_date": "2026-02-02",
  "preferred_time": "10:30",
  "duration_minutes": 30
}

Output Responses
Successful Booking
{
  "status": "confirmed",
  "calendar_start": "2026-02-02T10:30+08:00",
  "calendar_end": "2026-02-02T11:00+08:00",
  "event_link": "Google Calendar Event URL"
}

Conflict Detected
{
  "status": "conflict",
  "suggestions": [
    "2026-02-02T11:30+08:00",
    "2026-02-02T12:30+08:00",
    "2026-02-02T15:00+08:00"
  ]
}

Google Sheets Structure
Sheet Name: Patients
id	patient_name	email	phone	notes
Sheet Name: Appointments

| appointment_id | patient_email | start_time | end_time | status |

Setup Instructions

Import the workflow JSON into n8n

Configure credentials:

Google Sheets OAuth

Google Calendar OAuth

Email (SMTP or Gmail)

Set Google Calendar ID for Dr. Strange

Ensure timezone is set to Asia/Singapore

Share Google Sheet access with the connected Google account

Activate the workflow

Supported Actions

book – Schedule a new appointment

reschedule – Update an existing appointment

cancel – Cancel an appointment and free the slot

Testing Scenarios

Valid Booking

Book a 10:00 AM slot → Appointment confirmed

Buffer Conflict

Book at 10:15 AM → Conflict returned with suggestions

Outside Working Hours

Book at 7:00 PM → Rejected with next working slot suggestion

Reschedule Appointment

Existing booking moved to a new available slot

Simultaneous Requests

Two users request same slot → Only one confirmed, other gets conflict

Deliverables Included

n8n Workflow JSON

This README

Sample Google Sheets structure

Test case examples

Conclusion

This workflow demonstrates a real-world AI automation use case combining scheduling logic, external services, and constraint handling — fully aligned with the assignment requirements.
