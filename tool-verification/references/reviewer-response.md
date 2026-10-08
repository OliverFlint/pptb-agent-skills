# Reviewer response guidance and templates

Use this reference after completing the assessment. Append the tailored response at the end of the report so the reviewer can act on the findings without composing a message from the checklist.

The [current maturity model](https://docs.powerplatformtoolbox.com/tool-development/maturity-model) governs the decision and request process. Confirm the live policy before using process wording below.

## Select and prepare the response

- **Not ready:** Draft a rejection identifying every confirmed failed required criterion and the action to resolve it. Where required checks are incomplete, put a reviewer-only note before the draft explaining that those checks must be completed and the findings updated before sending a final decision.
- **Ready to request verification:** Draft a proposed approval for a human reviewer to adopt after completing the official review. An agent readiness assessment is not an approval, and the template must not claim the badge has already been applied.
- **Needs reviewer judgment:** State the unresolved migration exception, bug-health flag, or usage waiver in a reviewer-only note. Leave it for the human reviewer to decide before choosing an approval/rejection draft; if no decision is available, use the incomplete-review template.
- **Incomplete assessment:** Provide a holding draft explaining that the assessment cannot establish an outcome yet. Label it as a draft for an incomplete review, not an official approval/rejection or an additional policy stage. Identify who should perform each remaining check without inventing a reviewer back-and-forth process.

Use the actual tool name and assessed published version. Replace all template placeholders and omit inapplicable paragraphs. Keep each issue bullet to one line where practical. Address the author respectfully and focus on criteria, evidence, and fixes; do not infer poor maintenance or lack of human testing from unavailable evidence.

Keep confirmed failures separate from reviewer follow-up. In particular:

- Lack of a live host session means the agent has not tested the theme; it does not establish a theme failure.
- An outdated validator or reviewer network failure is a tooling limitation, not a proven tool defect.
- An unverified owner's My Tools access is an administrative check, not a failed tool-quality criterion.
- Optional contrast/console findings may be included separately as improvements; they do not cause rejection.
- If usage thresholds are demonstrably unmet, state that finding and the applicable waiver possibility. Do not award the waiver or imply the author must wait for adoption when the live policy permits discretion.

## Rejection draft

**Subject: [Tool name] — verification outcome**

Thanks for submitting [Tool name] [published version] for verification. This version has not been approved because the following required criteria need addressing:

- **[Criterion]:** [Observed failure and concrete action needed.]

[If usage is unmet and a waiver has not been granted: State the observed metrics, which thresholds are unmet, and that a reviewer may consider the applicable waiver for a new tool when the other criteria meet a high standard. Do not promise a waiver.]

[If useful: Briefly acknowledge material checks that passed, naming only demonstrated results.]

Once the required issues are addressed, publish and test the corrected version and submit a new verification request. You can resubmit immediately after fixing the issues; the new request receives a full review.

## Proposed approval draft

**Subject: [Tool name] — verification outcome**

Thanks for submitting [Tool name] [published version] for verification. The review is complete, and this version is approved for Verified status.

[Only if a human reviewer explicitly granted an exception: State the recorded usage waiver or migration/bug-health decision accurately and concisely.]

[If applicable: List optional improvements separately and state that they do not affect this approval.]

Keep the tool aligned with the current maintenance requirements. Future dependency vulnerabilities, CSP additions, bug-health breaches, or unresolved PPTB breaking changes can affect the badge.

**Reviewer-only instruction:** This is proposed wording, not evidence of approval. Use it only after the authorized human reviewer has completed the review and made the approval decision. Do not state that the badge was applied unless that action is verified.

## Incomplete-review draft

**Subject: [Tool name] — verification assessment incomplete**

The assessment of [Tool name] [published version] is incomplete. No final verification decision has been made because the following required checks remain unresolved:

- **[Check]:** [Missing evidence or decision, who must resolve it, and the next action.]

[If any failures are already confirmed: List those separately as confirmed issues; do not obscure them behind the missing evidence.]

Complete these checks and record any discretionary decisions before issuing the final verification outcome. This draft does not grant Verified status or create a new stage in the official review process.
