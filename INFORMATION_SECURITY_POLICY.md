# Bastion Helix Information Security Policy

**Version:** 1.1
**Adopted:** 19 October 2026 (version 1.1: 20 October 2026, adds section 20 on AI)
**Approved by:** the Owner of Bastion Helix
**Next review due:** October 2027 (see section 18)

Bastion Helix is a one-person software business. This policy sets out how we protect our products, our customers' data and our own systems. It is written for a business of our size: it describes what we actually do, and it only commits us to things we can keep doing.

Related documents:
- [Security policy and vulnerability reporting](SECURITY.md)
- [Incident Response Plan](INCIDENT_RESPONSE.md)
- [Privacy Policy](https://bastionhelix.com/privacy)
- [Data Processing Addendum](https://bastionhelix.com/dpa)
- [Agent Lens acceptable use policy](https://bastionhelix.com/lens/acceptable-use)

---

## 1. Scope

This policy covers:
- every Bastion Helix product: the Jira Cloud app, the browser extensions, the Agent Lens API and the bastionhelix.com website;
- every company account, repository, domain and device listed in Appendix B;
- all data we handle: customer data, personal data, source code and secrets.

## 2. Roles and responsibilities

- **The Owner** is responsible and accountable for all information security, privacy, risk and compliance work. This includes security operations, incident response, key management and supplier management.
- There are no employees. If anyone is ever given access to company systems (for example, a contractor), they must accept this policy and sign a confidentiality agreement first, get only the access they need, and lose that access on the day their work ends (section 7).
- Bastion Helix work is kept separate from any other employment. It is never done on another employer's equipment, network or accounts, and it never uses another employer's data or confidential information.

## 3. Risk management

- Security risks are recorded in the risk register (Appendix A). Each risk has a likelihood, an impact, a treatment (reduce, accept, transfer or avoid) and the Owner as its owner.
- The register is reviewed at least once a year, and also when a product, supplier or the way we handle data changes significantly.
- A risk may only be accepted by the Owner, and the reason is written down in the register.

## 4. Legal, regulatory and contractual requirements

The requirements that apply to us are:
- **EU and UK GDPR:** we act as a processor for customer data and as a controller for licensing and support data.
- **California CCPA:** we act as a service provider.
- **India's Digital Personal Data Protection Act, 2023:** as an Indian business.
- **Atlassian Marketplace Partner Agreement and Atlassian's security requirements for cloud apps:** including personal data reporting, data residency and reporting security incidents.
- **Chrome Web Store and Microsoft Edge Add-ons developer policies.**
- **India's CERT-In Directions (April 2022) on reporting cyber security incidents**, where they apply to us, including their 6-hour reporting time limit.

These requirements are reviewed at least once a year, and whenever we enter a new marketplace or sell to a new kind of customer.

## 5. Asset management

- Every company account, repository, domain, cloud service and device is listed in the inventory (Appendix B), together with its purpose and the most sensitive class of data it holds.
- Nothing new is created or connected (an account, service, repository, domain, device or third-party app) unless the Owner approves it and adds it to the inventory first. Anything that is no longer used is closed and removed from the inventory.
- The inventory is reviewed at least once a year.

## 6. Data classification and handling

| Class | Examples | Handling |
|---|---|---|
| **Public** | Website, store listings, published docs, open repositories | Can be shared freely. Must be accurate. |
| **Internal** | Plans, drafts, private source code, business records | Kept in company accounts with two-factor authentication. Not shared outside without a reason. |
| **Confidential** | Customer data in our apps, personal data, secrets (API tokens, keys, passwords) | See the rules below. |

Rules for Confidential data:
- Customer data stays on the platform it came from (for the Jira app, Atlassian Forge). It is never copied to our own computers or to other services.
- Secrets are never placed in code, repositories, chat, email, screenshots or tickets. They are kept only in an encrypted location on the work laptop, and they are entered into tools without being displayed on screen where possible.
- A secret that has been exposed, or might have been, is revoked and replaced straight away (section 9).
- Personal data and customer content are never written to logs (section 10).

## 7. Access control

- **Passwords:** every company account has its own strong password that is not used anywhere else. Passwords are never shared or written into code, chat or screenshots.
- **Two-factor authentication** is required on every company account that offers it. The GitHub organisation enforces it for all members. Any exception is recorded in the risk register with a plan to close it.
- **Least privilege:** apps request only the platform permissions (scopes) they need. API tokens are created for one purpose, with the narrowest permissions available.
- **Granting access:** access to company systems is approved by the Owner, recorded in the inventory, and limited to what the task needs.
- **Removing access:** access is removed on the day it is no longer needed. Tokens that are no longer used are deleted.
- **Review:** all accounts, members, tokens and app permissions are reviewed at least once a year and after any security incident.
- **Separation of duties:** not possible with one person. To compensate, all changes are recorded (Git history, platform audit logs), and production deploys follow the change process in section 8.

## 8. Secure development and change management

### 8.1 Security baseline for every product
- Request the smallest set of platform permissions that makes the feature work.
- Check the user's permissions on the server for every request for data. Never trust the browser to enforce access.
- Inbound endpoints must require a secret token or a signature, and must reject anything else.
- Make no network calls that are not declared and needed. The Jira app makes none outside Atlassian.
- Keep personal data and customer content out of logs.
- Never include secrets in code or in shipped packages.
- Never use customer data in development or testing. Development uses separate environments and test data only.
- Every claim we publish about a product must match what the product actually does.

### 8.2 Development lifecycle
1. **Design:** consider the data involved, the permissions needed and how the feature could be misused.
2. **Build:** follow the baseline in 8.1, and use maintained dependencies only.
3. **Test:** automated tests cover the change, and a test for an action must check that the action really happened.
4. **Check:** platform checks must pass (for example, `forge lint`). Dependency alerts are reviewed.
5. **Release:** deploy to the development environment and check there first, then deploy to production. Store and marketplace releases go through the store's own review.
6. **Operate:** watch errors, alerts and reports, and fix them under section 11.

### 8.3 Configuration baseline
The baseline for each product is defined by:
- its app manifest (permissions, triggers, network access);
- its dependency lock files;
- the account security settings listed in Appendix B.

Any change to these goes through 8.2 and is recorded in Git. The baseline is reviewed at least once a year.

The dependency lock files, together with GitHub's dependency graph, are the software bill of materials (SBOM) for each product. An SBOM can be exported from GitHub on request.

### 8.4 Rollback
Every release is a Git commit and a numbered version. If a release causes errors or a security problem, the previous known-good version is redeployed.

### 8.5 Emergency changes
An urgent security fix may skip the development-environment check, but only if the automated tests and platform checks still pass. It is recorded in Git as an emergency change and reviewed within 5 business days. Any other departure from this process is handled as an exception (section 19).

## 9. Cryptography and key management

- **Encryption:** data at rest and in transit is encrypted by the hosting platform, using industry-standard algorithms (Atlassian, Cloudflare, GitHub). All traffic uses HTTPS/TLS. Signatures we check ourselves use HMAC-SHA256.
- **Key generation:** keys, tokens and secrets are generated only with cryptographically secure random generators (the platform's or Node.js's built-in ones), never with general-purpose random functions.
- **Rotation:** platform and API tokens are rotated at least every 12 months, and immediately when they might have been exposed.
- **Revocation:** a key is revoked as soon as it is replaced or might be compromised, or when the service it was for stops being used.
- **Destruction:** a key or secret that is no longer needed is deleted from the platform and from where it was stored.
- **Changes:** any change to how we use cryptography follows the change process in section 8.
- **Records:** the Owner keeps a list of active tokens (name, purpose, permissions, date created).

## 10. Logging and monitoring

- Logs must never contain personal data, customer content, secrets or full error responses from other services. Error details are reduced to the error type, code and status.
- App logs are only accessible through the platform's developer consoles, which are protected by two-factor authentication.
- The Owner reviews platform alerts and error reports as they arrive, and checks the developer consoles at least once a month.

## 11. Vulnerability and malware management

- **Detection:** GitHub Dependabot alerts are switched on for every code repository. The work laptop runs antivirus with automatic updates.
- **Prioritisation:** vulnerabilities are ranked by their CVSS severity and by whether the vulnerable code actually ships or can be reached.
- **Fix times:** critical and high severity issues are fixed or mitigated within 14 days. Medium and low severity issues are fixed within 60 days or at the next release, whichever comes first.
- **Records:** every alert is either fixed or dismissed with a written reason, in GitHub.
- **Reports:** vulnerability reports from others are handled under SECURITY.md. If a vulnerability affected customers' data, they are told under the Incident Response Plan.

## 12. Incident management

- Incidents are handled under the [Incident Response Plan](INCIDENT_RESPONSE.md). The plan covers severity levels, raising it with the platform (for example, Atlassian), notifying customers, and the review afterwards.
- The plan is tested at least once a year with a short walk-through exercise, and it is updated after every real incident. The results are recorded.
- Contacts for platforms and authorities are listed in Appendix D and reviewed once a year.

## 13. Business continuity and disaster recovery

### 13.1 Impact analysis

| Service | If unavailable | Most likely cause | Recovery |
|---|---|---|---|
| Jira app | Customers can't use it | Atlassian outage | Atlassian restores the service. We tell customers when the outage is caused by our release. |
| Browser extensions | Keep working on users' computers | Store outage | No action needed. |
| Website and Agent Lens API | Site or API down | Cloudflare outage, or a faulty deploy | Roll back the deploy, or wait for Cloudflare. |
| Development | No new releases | Laptop lost or broken | Work from the cloud development environment (GitHub Codespaces) on another device. |
| Company accounts | Locked out | Lost two-factor device or a compromised account | Use recovery codes and recovery email. Follow the Incident Response Plan if compromised. |
| The Owner | No one to respond | Illness or absence | Customers can still use the products. Support replies are delayed. |

### 13.2 Continuity measures
- There are no servers of our own: products run on Atlassian, Cloudflare and the browser stores.
- All source code, including build tools and tests, is kept in private GitHub repositories. Work done anywhere else (a temporary workspace, a laptop folder) is committed to GitHub before the work session ends.
- Recovery codes for company accounts are stored offline.
- Customer data in the Jira app is backed up by Atlassian.

### 13.3 Reviews
This analysis and plan are reviewed at least once a year, and whenever a new product or supplier is added.

## 14. Suppliers and shared responsibility

- Suppliers are listed in Appendix B. A supplier that would process customer personal data (a sub-processor) is added only after checking its security commitments, and after giving customers the notice set out in the DPA.
- Each supplier's security documents and status are checked at least once a year.
- The shared responsibility split between the platform, Bastion Helix and the customer is set out in Appendix C. For each product, it is also set out in that product's documentation and in its CAIQ-Lite answers.

## 15. Workplace and remote working

- Work is done from a private home workspace. The screen is locked whenever the laptop is left, and confidential information is not left visible.
- Work on public networks only over HTTPS. Do not show secrets on screen in public places.
- No company data is kept on paper. The only exception is account recovery codes, which are kept offline in a private, safe place.
- Company work is never done on another employer's devices or networks.

## 16. Endpoints

- The only device that holds company data is the work laptop, which is listed in Appendix B. Its required settings are:
  - full-disk encryption on;
  - password sign-in, with biometrics allowed;
  - automatic lock after inactivity;
  - automatic operating system updates;
  - antivirus on;
  - firewall on.
- Only these may be installed on it: software from the operating system's store or from the vendor's official website, plus the development tools listed in Appendix B.
- The laptop's settings are checked against this list at least once a year.
- If the laptop is lost or stolen, all company sessions and tokens are revoked from another device, and all passwords and tokens are rotated.
- When the laptop is retired or repaired by others, company secrets are removed first. A retired laptop's drive is fully wiped (Windows "Remove everything" with "Clean the drive") or physically destroyed.

## 17. Security awareness

At least once a year, the Owner refreshes their security knowledge, and records the date. This covers:
- current platform security requirements (Atlassian, Chrome Web Store, Microsoft Edge);
- the OWASP Top 10;
- one recent incident in our industry.

Security advisories from our platforms and from GitHub are followed throughout the year.

## 18. Yearly security checklist (every October)

- [ ] Review this policy and the documents it links to; update the version and date.
- [ ] Review the risk register (Appendix A).
- [ ] Review the inventory and suppliers (Appendix B), and the shared responsibility split (Appendix C).
- [ ] Review all accounts, members, tokens and app permissions; remove what is unused.
- [ ] Rotate platform and API tokens that are 12 months old.
- [ ] Check the laptop settings (section 16).
- [ ] Run the incident response walk-through and record the result.
- [ ] Exercise one continuity scenario (for example, deploy a release from a cloud development environment on another device) and record the result.
- [ ] Review the business continuity analysis (section 13).
- [ ] Review the legal requirements (section 4), the contacts in Appendix D and the API security of each product.
- [ ] Do the security knowledge refresh (section 17).
- [ ] Review section 20 (AI) and the Agent Lens acceptable use policy against new AI risks, laws and platform rules.
- [ ] Update the CAIQ and AI-CAIQ answers.

## 19. Exceptions

Any departure from this policy is recorded in the risk register with a reason and a plan to close it. Open exceptions at adoption:
- **Two-factor authentication on the company Google account** is not yet on. Google's own verification step currently blocks it. We will retry until it is on.
- **Separation of duties** isn't possible with one person. This is compensated by the controls in section 7.

## 20. AI: how we use it and how our products serve it

### 20.1 Using AI tools in our own work
We use AI assistants (for example, for coding, writing and research). When we do:
- **Never share secrets or protected data** with an AI tool: no passwords, API keys, tokens or recovery codes; no customer data or personal data; and no information from any other employer.
- **A human is responsible for every result.** The Owner reviews AI-written code, text and answers, and code only ships after the automated tests and checks in section 8.2 pass.
- **Every claim is checked.** Anything an AI tool states about our products, laws, prices or other facts is checked against the product or a primary source before it is published (section 8.1).
- **Least access for AI agents.** An AI agent working for us gets only the access its task needs. It never holds production secrets, and deploys and other changes outside its workspace are approved by the Owner.
- **Reputable tools only.** We use AI tools from established providers under their published terms, and prefer settings that do not use our inputs for training where the provider offers them.

### 20.2 Products used by AI agents
Some of our products, such as the Agent Lens API, are built to be called by AI agents. For these products:
- The service enforces guardrails in code: it obeys robots.txt, refuses pages whose Content Signals disallow AI input, never bypasses bot or human checks, never signs in, and is read-only.
- Customers and their agents must follow the [Agent Lens acceptable use policy](https://bastionhelix.com/lens/acceptable-use). We may disable an API key that breaks it.
- Content returned from web pages is untrusted. Our documentation tells agents to treat it as data, never as instructions.
- Reports of misuse go to support@bastionhelix.com and are handled within 5 business days. Serious misuse is handled as a security incident (section 12).
- We do not train, fine-tune or host AI models. If that changes, this section will be extended before launch.

### 20.3 Review
This section and the acceptable use policy are reviewed at least once a year (section 18), and whenever a new AI product, law or platform rule affects them.

---

## Appendix A: Risk register (October 2026)

| # | Risk | Likelihood | Impact | Treatment |
|---|---|---|---|---|
| R1 | A company account is taken over | Low | High | Reduce: two-factor authentication everywhere, unique passwords, recovery codes stored offline. Google 2SV still open (section 19). |
| R2 | A token or secret leaks (for example, in a screenshot) | Medium | Medium | Reduce: secrets handling rules (section 6), rotation (section 9). Done after the October 2026 exposures. |
| R3 | A vulnerable dependency ships | Medium | Medium | Reduce: Dependabot alerts, fix times (section 11). |
| R4 | A bug lets one user see data they shouldn't | Low | High | Reduce: permission checks on the server, tests for each viewer type, development before production. |
| R5 | Customer data appears in logs | Low | Medium | Reduce: safe logging since October 2026, with tests (section 10). |
| R6 | The laptop is lost or stolen | Low | Medium | Reduce: full-disk encryption, auto-lock, remote token revocation (section 16). |
| R7 | The Owner is unavailable | Medium | Medium | Accept: products keep running on their platforms; support replies are delayed. |
| R8 | A platform outage (Atlassian or Cloudflare) | Low | Medium | Transfer: covered by the platforms' own resilience (section 13). |
| R9 | We publish a claim that isn't true | Low | Medium | Reduce: every public claim is checked against the product before publishing (section 8.1). |

## Appendix B: Inventory (October 2026)

| Asset | Purpose | Highest data class |
|---|---|---|
| Work laptop (Windows; owned by the Owner, used for Bastion Helix) | Development and administration | Confidential (secrets) |
| GitHub organisation `bastionhelix` (app, site, extension, community and policy repositories) and GitHub Codespaces | Source code, issues, development environment | Internal (Confidential in private repositories) |
| Atlassian developer console and Marketplace partner account | Jira app hosting and listing | Confidential (customer data stays on Atlassian) |
| Cloudflare account (bastionhelix.com DNS, email routing, website, relay and API workers, D1 database) | Website, email routing, Tables relay, Agent Lens API | Confidential (API keys, Agent Lens records) |
| Google account (company email) | Company email and identity for stores and YouTube | Internal |
| Chrome Web Store developer account | Extension publishing | Internal |
| Microsoft Partner Center (Edge Add-ons) | Extension publishing | Internal |
| Resend account | Website waitlist and transactional email | Confidential (waitlist email addresses) |
| YouTube and X accounts | Product videos and updates | Public |

**Sub-processors for the Jira app:** Atlassian only (see the DPA).

## Appendix C: Shared responsibility (summary)

| Area | Platform (Atlassian, Cloudflare, stores) | Bastion Helix | Customer |
|---|---|---|---|
| Hosting, physical, network, backups | Yes | | |
| Encryption at rest and in transit | Yes | Uses platform encryption; signs inbound requests | |
| App code, permissions, logging | | Yes | |
| Who may use the app and see what | | Enforces Jira permissions | Sets Jira permissions and app settings |
| Data residency location | Provides it | Supports it | Chooses it |
| Data subject requests | | Helps, as processor | Handles them, as controller |
| Incident response | For the platform | For the app (Incident Response Plan) | For their own users |

## Appendix D: Contacts for incidents

| Who | When | How |
|---|---|---|
| Atlassian (Marketplace partner support) | Any security incident affecting the Jira app or its customers | Raise a ticket in the Atlassian Ecosystem support portal, as set out in the Incident Response Plan |
| Google (Chrome Web Store), Microsoft (Edge Add-ons) | An incident affecting a browser extension | The developer dashboards' support contacts |
| Cloudflare | An incident affecting the website, relay or Agent Lens API | Cloudflare dashboard support |
| CERT-In (Indian Computer Emergency Response Team) | Reportable cyber security incidents, where the CERT-In Directions apply | The reporting channels published at cert-in.org.in (email, phone 1800-11-4949 or fax 1800-11-6969). Check the current details before use. |
| Police (cyber crime, India) | Fraud or crime affecting the business | National Cyber Crime Reporting Portal, cybercrime.gov.in, or helpline 1930 |
| Affected customers | Under the Incident Response Plan and the DPA | Their admin or contact email; support@bastionhelix.com for replies |

