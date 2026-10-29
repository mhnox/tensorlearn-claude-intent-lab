# Intent: Results-ready notification

Sources: approved PRD N-17 (`inputs/01-approved-prd.md`), discovery notes (`inputs/02-current-meeting.md`). The early request (`inputs/03-superseded-request.md`) is superseded by PRD N-17 and is used as history only; nothing in this intent is approved by it.

## Problem

- **I-1.** The fictional clinic receives avoidable calls from patients checking whether their results are ready. PRD: "The fictional clinic receives avoidable calls from patients checking whether results are ready."

## Proposed outcome

- **I-2.** The patient is notified when a result is ready for their review, and the notification directs them to the existing authenticated portal. PRD: "Notify the patient when a result is ready for their review in the existing authenticated portal."
- **I-3.** "Ready" means a doctor has reviewed and released the result. A finished laboratory upload alone does not make a result ready. PRD: "A doctor must review and release the result before any notification is eligible." Discovery (product owner): "'Ready' must not mean that the laboratory upload merely finished."
- **I-4. How success will be observed:**
  - Results-status calls, counted separately from clinical questions and compared against a measured baseline. Discovery (support lead): "count status-only calls separately from clinical questions; compare against a measured baseline."
  - Delivery failures. PRD: "Measure reduction in results-status calls and delivery failures."
  - The baseline and any numeric target are agreed during discovery. None is approved yet, so this intent sets no target. PRD: "The baseline and numerical target will be agreed during discovery; none is approved yet."

## Affected users and systems

- **I-5. Patients** are the only approved recipients of the notification.
- **I-6. Doctors** review and release results. Their review and release gate every notification.
- **I-7. Systems:**
  - The existing portal result route, with authentication in the existing portal. Engineering lead: "the existing portal path is adequate."
  - Duplicate prevention and delivery-failure handling are in scope.

## Out of scope

- **I-8.** Out of scope:
  - Portal redesign
  - Appointment scheduling
  - Clinical interpretation
  - Notifying third parties. PRD: "Portal redesign, appointment scheduling, clinical interpretation and notifying third parties are outside this approved scope."
- **I-9.** A new or rebuilt patient dashboard is excluded. Engineering lead: "A new patient dashboard is a separate project."
- **I-10.** Everything that only the superseded request asked for is excluded:
  - Sending as soon as the lab uploads a result
  - Putting the test result in the message
  - Copying the referring doctor
  - Rebuilding the dashboard
  - Its call-reduction target

## Constraints

- **C-1. Review before notification.** No notification is eligible until a doctor has reviewed and released the result.
- **C-2. No clinical details.** The message says only that an update is available and directs the patient to the existing authenticated portal. It must not contain a diagnosis, test name, test result, clinical measurement or treatment detail. The same applies to notification and operational logs. PRD: "No diagnosis, test result, clinical measurement or treatment detail goes in the message or notification logs." Privacy owner: "no sensitive content in messages or operational logs."
- **C-3. Authenticated access only.** Results are viewed after authentication in the existing portal. Results are never attached, and no public result link is introduced.
- **C-4. No duplicates.** There is at most one notification per released result version. Delivery failures are handled without sending the patient duplicate messages.
- **C-5. Patient only.** No other recipient is approved. Notifying anyone else needs the explicit decision in OPEN-2.

## Open questions and owners

- **OPEN-1. Channel.** Should the notification go by SMS, email or portal push? Owners: product owner and privacy owner, who decide after the consent and delivery review. PRD: "product owner and privacy owner must decide after consent/delivery review." Privacy owner: "Consent and the actual channel are still under review."
- **OPEN-2. Referring-doctor notification.** May the referring doctor be notified? Owners: clinical owner and privacy owner, who decide whether the recipient is authorized. The PRD does not approve it. Clinical owner: "do not infer approval for referring-doctor notifications from the old request."

```json
{
  "notifyWhen": ["doctor-reviewed", "released"],
  "messageMayInclude": ["update-available"],
  "inScope": ["existing-portal-route", "duplicate-prevention", "delivery-failure-handling"],
  "openDecisions": [
    {"topic": "channel", "owners": ["product", "privacy"]},
    {"topic": "referring-doctor-notification", "owners": ["clinical", "privacy"]}
  ],
  "successMeasures": ["status-only-calls", "delivery-failures"],
  "numericTarget": null
}
```
