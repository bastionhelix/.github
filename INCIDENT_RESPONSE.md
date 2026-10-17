# Security incident response plan

**Bastion Helix** · Version 1.0 · Adopted 17 October 2026 · Next review: April 2027

This plan sets out what we do if a security incident affects a Bastion Helix app or the data our customers trust it with. It follows Atlassian's [App security incident management guidelines](https://developer.atlassian.com/platform/marketplace/app-security-incident-management-guidelines/) and [Preparing for a security incident](https://developer.atlassian.com/platform/marketplace/preparing-for-a-security-incident/).

## 1. Scope

- **Bastion Helix for Jira** (Atlassian Forge, Runs on Atlassian). Customer data is stored only in Atlassian's Forge hosted storage. We run no servers of our own for this app.
- The same steps apply to our other products (browser extensions, website, APIs), with each platform's own reporting channel used in step 4.

## 2. Who is responsible

| Role | Who | How to reach |
|---|---|---|
| Incident lead and security contact | Tej Thimmaiah, founder | support@bastionhelix.com (subject "Security") |
| Atlassian | Ecosystem security | [P1 security incident ticket](https://ecosystem.atlassian.net/servicedesk/customer/portal/34/group/3534/create/4180); if the portal is unreachable, security@atlassian.com |

Bastion Helix is a one-person company, so the incident lead handles every step below. The Security Incidents contact in the Atlassian Marketplace Partner Portal is kept current, and updated the same day if it changes.

## 3. What counts as a security incident

We use Atlassian's definition:

1. Any actual or suspected unauthorized access, acquisition, use, disclosure, modification or destruction of end user data in our possession or control.
2. A security compromise of our app, including stolen or leaked credentials, tokens or secrets, or a compromised dependency.
3. Any issue in our app that materially degrades Atlassian systems or networks.

When in doubt, we treat it as an incident and report it.

A vulnerability report with no sign of compromise is handled under our [security policy](SECURITY.md) and Atlassian's [security bug fix policy](https://developer.atlassian.com/platform/marketplace/security-bugfix-policy/), not this plan.

## 4. Severity

| Severity | Examples | Response starts |
|---|---|---|
| **Critical** | Customer data exposed or changed by someone who shouldn't have access; a leaked credential that can reach customer data; app code compromised | Immediately |
| **High** | A credential or token leaked without evidence of use; a flaw being actively exploited somewhere else that affects our app | Within 4 hours |
| **Low** | A suspicious event that hasn't been confirmed; a leaked credential that can't reach customer data | Within 24 hours |

## 5. Response steps

**Step 1. Record.** Open an incident log. Write down when the issue was first observed, when it was detected, who reported it and what is known so far. Every later action is added with its time (IST and UTC).

**Step 2. Assess.** Decide the severity (section 4). Work out which app versions, customers, data types and time period are affected. Keep any evidence: Forge logs, screenshots, report emails.

**Step 3. Contain**, as fast as possible. Depending on the cause:
- revoke and replace the affected credential: Atlassian API token, GitHub, Cloudflare or email provider keys, or the app's own web trigger tokens (each has a "New token" control);
- turn off or remove the affected feature and deploy the fix, or roll back to the last safe version;
- remove access for any account that should not have it;
- ask Atlassian to restrict access to the app (for example, delisting it) while the fix is made.

**Step 4. Notify Atlassian within 24 hours.** For any Critical or High incident, and any confirmed incident in section 3, raise a [P1 security incident ticket](https://ecosystem.atlassian.net/servicedesk/customer/portal/34/group/3534/create/4180) promptly and no later than 24 hours after we become aware. We share what we know at the time and add details as we learn them. The ticket covers:
- incident category and scope, including duration and customer impact;
- whether end user data was affected, whether affected users will be notified, and the likely number of customers affected;
- the types of data involved;
- the expected resolution time and why;
- containment taken or planned;
- our plan for telling customers;
- the root cause, if known;
- the technical and organisational fixes;
- contact details: email and phone number.

**Step 5. Keep Atlassian updated** on the ticket at least every 6 hours until the incident is contained and fixed. Each update says what was done since the last one.

**Step 6. Tell affected customers**, when possible within 72 hours of identifying the incident. We use Atlassian's [security incident communication template](https://developer.atlassian.com/platform/marketplace/app-security-incident-communication-template/): what happened and when, how we found it, what we did, the real impact, any steps customers need to take, and how to reach us. We say plainly what we know and what we don't, and we don't hide information.

**Step 7. Fix and verify.** Write an automated test that reproduces the problem, fix it, confirm the test passes along with the full test suite and `forge lint`, then deploy. Confirm in production that the problem is gone.

**Step 8. Review**, within 5 business days of closing the incident:
- Are any indicators of compromise still present?
- What caused it, and how do we stop it, or anything like it, happening again?
- Do our logging, tests or this plan need to change?

The findings go to Atlassian on the same ticket. We confirm to Atlassian, if asked, that the incident is resolved and customers have been told.

## 6. Handling evidence and shared data

Logs and data shared with us during an incident, including by Atlassian, are kept confidential. They are stored only in encrypted, access-controlled storage and used only to resolve the incident. They are deleted within 90 days of the incident closing unless the law requires us to keep them. If any of it is accidentally exposed, we tell Atlassian immediately.

## 7. Staying ready

- Secrets are never put in source code. Credentials are rotated as soon as they may have been exposed.
- App logs never contain credentials, tokens, passwords or unnecessary personal data.
- Automated tests check permissions and data visibility for several user roles before every deploy.
- The security contact in the Atlassian Partner Portal and in this plan is kept current.
- This plan is reviewed every 6 months and after every incident.

## Change history

| Version | Date | Change |
|---|---|---|
| 1.0 | 17 October 2026 | First version |
