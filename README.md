# PropConnect | AI Real Estate Voice Assistant

**A voice-first real estate assistant that helps people find the right next step, without pretending to know what it cannot verify.**

PropConnect's configured assistant is named **Estaro**. It is designed to speak with prospective buyers, sellers, tenants, landlords, property owners, existing customers, and other callers. The workflow focuses on useful conversations, verified information, customer choice, and a clear handoff when a human should take over.

> **Project status:** This repository currently contains a Retell agent workflow export, not a standalone application. There is no source-code runtime, dependency manifest, or local server command. The agent is imported, configured, tested, and operated through the Retell platform and its connected telephony/integration services.

## Contents

- [What's included](#whats-included)
- [Capabilities](#capabilities)
- [Requirements](#requirements)
- [Setup and installation](#setup-and-installation)
- [Run and test the agent](#run-and-test-the-agent)
- [Current configuration and integration gaps](#current-configuration-and-integration-gaps)
- [Workflow terminology](#workflow-terminology)
- [Privacy and safe operation](#privacy-and-safe-operation)
- [Troubleshooting](#troubleshooting)
- [Repository structure](#repository-structure)

## What's included

| File | Purpose |
| --- | --- |
| [`Agent Workflow File.json`](Agent%20Workflow%20File.json) | Exported Retell voice-agent configuration, including the agent prompt, model and voice settings, and configured call tools. |
| [`LICENSE`](LICENSE) | License information for this repository. |

The JSON file is a platform configuration export. It is not a Python, Node.js, or web application entry point and should be imported into a compatible Retell workspace rather than run directly.

## Capabilities

The agent prompt describes five supported call purposes:

1. **Lead follow-up:** Reconnect with a person whose inquiry is verified, confirm whether their requirements have changed, and establish an appropriate next step.
2. **Cold calling:** Introduce PropConnect transparently, check whether the topic is relevant, and respect campaign eligibility and contact preferences.
3. **Appointments and consultations:** Collect appointment requirements, check real availability, confirm details, and report a booking only after the booking system succeeds.
4. **Customer support and property inquiries:** Answer from approved records, handle supported questions, and route unresolved or sensitive matters to a human when available.
5. **Prospect qualification:** Understand a caller's stated role, needs, readiness, and requested next step without pressuring them or making unsupported judgments.

Across these flows, Estaro is instructed to identify itself as an AI assistant, ask one primary question at a time, verify important facts, avoid unsupported promises, respect opt-outs, and never claim that a system action succeeded unless the relevant tool confirms it.

## Requirements

### Required to configure and operate

- A Retell account/workspace with permission to create or import an agent configuration.
- A compatible voice-agent setup and an authorized phone number or telephony connection for live calls.
- Any credentials required by Retell or connected providers, entered in their secure configuration interfaces. **Do not commit API keys, passwords, access tokens, or private credentials to this repository.**
- An approved human transfer destination and a team prepared to receive transferred calls.
- Approved PropConnect business information and property records before answering factual business or listing questions.
- A verified calling purpose, contact source, consent/legal basis, calling-time policy, and suppression-list process before initiating outbound or cold calls.

### Optional integrations for the full described workflow

Connect and test these only if the corresponding feature is enabled for your deployment:

- **CRM/contact system:** authorized contact lookup, lead-source verification, call outcomes, requirement updates, callback preferences, support records, and opt-out recording.
- **Scheduling/booking system:** availability lookup, appointment creation, booking-reference retrieval, and authorized rescheduling or cancellation.
- **Knowledge base:** approved company information, service descriptions, service areas, property records, policies, and other verified answers.
- **Transfer and messaging services:** configured human handoff and any approved customer communication channels.

### Dependencies and installation

There are **no local software dependencies to install** for the files currently in this repository. No `pip install`, `npm install`, or local build step is applicable. The runtime dependencies are the Retell platform and whichever telephony, CRM, scheduling, or knowledge-base integrations you configure outside this repository.

## Setup and installation

1. **Get the project files.** Clone or download this repository, then locate [`Agent Workflow File.json`](Agent%20Workflow%20File.json).
2. **Review the export before import.** Confirm the agent name, language, voice, timezone, data-storage settings, tool destinations, prompt, and any organization-specific settings. The current export contains a predefined transfer destination; verify that it is authorized and appropriate for your environment before publishing or sharing the configuration.
3. **Import the workflow.** In your Retell workspace, use the platform's agent/workflow import option and select `Agent Workflow File.json`. Retell's interface may change; use its current import documentation if the menu names differ.
4. **Resolve platform validation.** The export has an empty `agent_id`. Let the target workspace assign or associate the correct agent identifier during import, then confirm the imported agent appears in that workspace.
5. **Configure business integrations.** Add only the CRM, booking, knowledge-base, transfer, and messaging tools that actually exist in your account. Map the prompt's integration placeholders to real tools and verify tool inputs, outputs, permissions, and failure handling.
6. **Add approved knowledge.** Attach a current, authorized knowledge base and property source. Check that the agent can distinguish a verified answer from an unavailable or stale listing detail.
7. **Review calling and privacy settings.** Confirm lawful calling eligibility, consent and suppression workflows, recording disclosures, data retention, and any required AI disclosure for the places where calls will be made or received.
8. **Set the correct locale and schedule settings.** Align the configured language and timezone with the intended callers and business operations. The current export uses `en-IN` and `America/Los_Angeles`; verify both rather than assuming they match.
9. **Connect a test phone number or telephony route.** Keep production routing disabled until the transfer destination and call flows have been verified.

## Run and test the agent

There is no local `run` command. To run the agent:

1. Open the imported agent in the Retell workspace.
2. Confirm the platform has accepted the configuration and that the agent is associated with the intended workspace and phone/telephony setup.
3. Use the platform's test-call or simulator feature to test each relevant flow before enabling live traffic.
4. Verify integrations against test records and test appointment slots. Confirm that failed or ambiguous tool responses never produce a false success message.
5. Check inbound and outbound routing separately if both are enabled. Do not start outbound campaigns until eligibility, consent/legal basis, call timing, and suppression handling have been reviewed.
6. Enable the approved production route only after the deployment checklist below passes.

### Suggested test scenarios

- Correct recipient, identity confirmation, and permission to continue.
- Wrong recipient, busy recipient, unavailable contact, and request to end the call.
- Clear refusal and explicit request for no further calls.
- Missing or conflicting CRM details and an unverified contact source.
- Property questions where the record is current, unavailable, or stale.
- Appointment slot available, slot unavailable, booking success, booking failure, and ambiguous timeout.
- Human transfer success and transfer failure.
- Caller asks whether Estaro is AI, where their number came from, or how their data is handled.
- Sensitive legal, financial, payment, safety, or complaint scenario that should be routed rather than guessed at.

## Current configuration and integration gaps

The workflow export currently records these notable settings:

| Area | Exported setting | Operational note |
| --- | --- | --- |
| Agent identity | Agent name `PEC ISE2`; prompt identity `Estaro` | Confirm the display name and spoken identity are intentional and consistent. |
| Channel and language | Voice; `en-IN` | Confirm the language/voice are suitable for the intended callers. |
| Voice | `retell-Nico` | Confirm this voice is available and appropriate in the target workspace. |
| Model | `gpt-5.6-terra` | Confirm model availability and account entitlement in the target environment. |
| Timezone | `America/Los_Angeles` | Align this with the business and the caller's appointment timezone. |
| Opening behavior | `begin_message` is empty; `start_speaker` is `user` | Test the initial turn and configure an opening message if required by the intended call experience. |
| Knowledge base | No knowledge-base IDs are attached | Listing and business answers need an approved source before they can be verified. |
| Configured tools | `end_call` and `transfer_call` | These are the only general tools currently present in the export. Verify their behavior in the target workspace. |
| CRM and booking | Prompt references `crm_tool` and `booking_tool` as integration placeholders | Neither tool is configured in the exported tool list. Record updates, opt-out processing, availability lookup, and booking are not operational until real integrations are mapped and tested. |
| Contact memory | Read enabled; update disabled | Confirm this matches the intended data-access and update policy. |
| Data storage | `everything` | Review retention and privacy implications in the platform before using real customer data. |
| AI disclosure | Enabled in handbook settings | Preserve the configured disclosure and verify any additional legal disclosures required for your deployment. |
| Agent identifier | Empty `agent_id` in the export | Import into the target workspace and verify its assigned identity. |

The prompt describes more integrations than the export currently configures. Until those integrations are installed and successfully tested, do not tell callers that a CRM update, callback request, opt-out record, knowledge-base lookup, appointment, or booking has been completed.

## Workflow terminology

### Call outcomes and tools

- **`end_call`**: Ends the call when it is complete, the caller declines, or no further conversation is appropriate.
- **`transfer_call`**: Attempts to transfer the caller to the configured human destination. A transfer should be described as complete only when the platform confirms it.
- **`crm_tool`**: Prompt placeholder for an authorized CRM lookup/update integration. It is not configured in the current export.
- **`booking_tool`**: Prompt placeholder for an appointment availability and booking integration. It is not configured in the current export.
- **Knowledge base (`knowledge_base_ids`)**: The approved source for verified business and property information. The exported list is empty.
- **Suppression workflow**: The authorized process for recording and honoring a request not to receive further calls or other contact.
- **Cold transfer**: The configured transfer mode attempts a direct handoff; confirm routing and failure behavior with the telephony setup.

### Prompt variables

The prompt uses double-braced variables such as `{{name}}`. They are values expected from the calling platform, CRM, lead form, scheduling system, or knowledge base. They are not automatically populated just because they appear in the prompt. Map each variable to a trusted source, and do not speak an unresolved variable name to a caller.

| Group | Variables used by the prompt |
| --- | --- |
| Contact | `{{name}}`, `{{phone}}`, `{{email}}`, `{{preferred_language}}`, `{{contact_source}}`, `{{consent_status}}`, `{{do_not_contact_status}}` |
| Call and lead | `{{call_purpose}}`, `{{lead_source}}`, `{{inquiry_date}}`, `{{previous_interaction}}`, `{{assigned_agent}}`, `{{callback_preference}}` |
| Property needs | `{{property_type}}`, `{{transaction_type}}`, `{{preferred_location}}`, `{{budget_min}}`, `{{budget_max}}`, `{{budget_currency}}`, `{{bedroom_requirement}}`, `{{property_size}}`, `{{move_in_date}}`, `{{purchase_timeline}}`, `{{property_reference}}`, `{{property_status}}`, `{{additional_requirements}}` |
| Scheduling | `{{preferred_date}}`, `{{preferred_time}}`, `{{available_slots}}`, `{{appointment_type}}`, `{{appointment_location}}`, `{{appointment_confirmation_id}}`, `{{timezone}}`, `{{callback_date}}`, `{{callback_time}}` |
| Business and integrations | `{{business_hours}}`, `{{service_areas}}`, `{{official_services}}`, `{{knowledge_base}}`, `{{booking_tool}}`, `{{crm_tool}}`, `{{transfer_destination}}`, `{{team_contact_details}}`, `{{callback_policy}}` |
| Verified response values | `{{verified_answer}}`, `{{verified_service_or_property_option}}` |

## Privacy and safe operation

- Verify the recipient before revealing inquiry details or other personal information.
- Do not invent property availability, prices, business policies, legal status, appointment slots, or service guarantees.
- Do not request passwords, one-time codes, payment-card information, or unnecessary identity documents.
- Respect a request to stop, pause, or change contact preferences, and use the configured authorized process. Do not bypass a restriction by switching channels.
- Use contact and property information only for an authorized purpose. Do not disclose internal notes or another customer's information.
- Follow applicable privacy, marketing, telecommunications, recording, and AI-disclosure requirements for each deployment location. The project configuration is not legal advice or proof of compliance.
- Escalate sensitive, disputed, or unverified matters to the designated team rather than giving legal, tax, financial, or investment advice.
- Review the Retell `data_storage_setting` and contact-memory configuration before processing real customer calls.

## Troubleshooting

| Symptom | What to check |
| --- | --- |
| Import fails or agent is not selectable | Confirm the JSON export is intact, the target workspace supports its configuration version/model, and the workspace assigns an agent identifier. |
| Agent cannot answer property questions | Attach and test an approved knowledge base or property lookup integration; no knowledge-base IDs are included in this export. |
| Agent cannot schedule or update a lead | Configure and map real booking/CRM tools. The prompt mentions these integrations, but the export does not define them. |
| Human handoff does not connect | Verify the transfer destination, number format, telephony permissions, routing availability, and failure path in the target workspace. |
| Spoken greeting or locale is wrong | Review `begin_message`, `start_speaker`, language, voice, timezone, and the spoken agent identity. |
| Agent claims an action succeeded when it did not | Inspect the tool result mapping and test timeout/error cases. The agent should only confirm actions after a successful tool response. |
| Calls run at an unexpected local time | Check the configured timezone and the timezone supplied for the appointment or call. |

## Repository structure

```text
PropConnect-AI-Powered-Real-Estate-Assistant-System/
|-- Agent Workflow File.json   # Retell voice-agent workflow export
|-- LICENSE                    # Repository license
`-- README.md                  # Setup, configuration, and operating guide
```

## Deployment checklist

- [ ] Imported the workflow into the correct Retell workspace and verified the assigned agent identity.
- [ ] Reviewed the prompt, spoken name, opening behavior, language, voice, model, and timezone.
- [ ] Verified the human transfer destination and tested transfer failure handling.
- [ ] Attached an approved knowledge base and configured only integrations that are actually available.
- [ ] Tested CRM, booking, opt-out, and callback workflows end to end where enabled.
- [ ] Reviewed contact consent, suppression, call recording, AI disclosure, retention, and access controls.
- [ ] Tested refusal, wrong-recipient, missing-data, tool-error, and sensitive-topic scenarios.
- [ ] Enabled production calling only after the appropriate business and compliance reviews.

---

**PropConnect: clearer conversations, verified details, and a human handoff when it matters.**
