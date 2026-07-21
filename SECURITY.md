# SHEESEE™ Security Policy

Security, privacy, and responsible handling of user information are mandatory requirements of this project.

## Supported Versions

Only the latest production version from the default branch is supported with security updates.

| Version | Supported |
| ------- | --------- |
| Latest production release | ✅ |
| Development and preview builds | ❌ |
| Older versions | ❌ |

## Private Security Reporting

Do not report vulnerabilities through public GitHub Issues, Discussions, pull requests, social media, comments, or public messages.

Security vulnerabilities must be reported privately using GitHub Private Vulnerability Reporting:

1. Open this repository.
2. Select **Security**.
3. Select **Advisories**.
4. Select **Report a vulnerability**.

If private reporting is unavailable, do not publish technical details publicly. Contact the repository owner through an established private business channel.

## Required Report Information

A useful security report should include:

- A clear description of the vulnerability
- The affected file, page, API route, component, or version
- Safe and minimal reproduction steps
- The expected behavior and actual behavior
- The potential security or privacy impact
- Relevant screenshots or sanitized logs
- Any suggested mitigation, if available

Remove API keys, tokens, passwords, cookies, personal conversations, customer information, and other sensitive information before submitting evidence.

## Prohibited Testing

Security research must not:

- Access, copy, alter, expose, or delete another person’s data
- Attempt to obtain passwords, tokens, API keys, session cookies, or credentials
- Perform denial-of-service, traffic flooding, spam, or resource exhaustion
- Use social engineering, phishing, impersonation, or deception
- Upload malware or attempt persistence
- Access production systems beyond what is strictly necessary to demonstrate an issue
- Disrupt the service or interfere with other users
- Publicly disclose an unresolved vulnerability
- Demand payment, threaten disclosure, or retain sensitive information

Stop testing immediately if personal data, credentials, private messages, or unauthorized access are encountered.

## Sensitive Information

The following must never be committed to this repository:

- API keys, access tokens, refresh tokens, or passwords
- `.env` files or production environment variables
- Authentication cookies, session identifiers, or private keys
- Database credentials, connection strings, or backups
- Customer information or private conversations
- Screenshots containing personal or confidential information
- Internal system prompts containing confidential instructions
- Production logs containing personal data
- Cloud credentials, deployment keys, certificates, or signing secrets
- Payment information
- Recovery codes or two-factor authentication secrets

Placeholder values must be clearly fake and must not resemble working credentials.

## Exposed Credential Response

If a secret may have been exposed:

1. Revoke or disable it immediately.
2. Generate a new credential.
3. Update the protected deployment environment.
4. Remove the credential from the repository and its history where necessary.
5. Review access logs and recent activity.
6. Determine what systems and data the credential could access.
7. Document the incident privately.
8. Do not reuse the exposed credential.

Deleting a secret from the latest file is not sufficient. The credential must still be considered compromised and rotated.

## Security Requirements

All contributions and deployments must follow these requirements:

- Secrets are stored only in protected server-side environment variables
- Secrets must never be included in browser bundles or client-side code
- Authentication and authorization are enforced server-side
- Access follows least privilege and deny-by-default principles
- Administrative functions require explicit authorization
- User input is validated, normalized, and size-limited
- Uploaded files are restricted by type and size
- API endpoints use rate limits, timeouts, and abuse controls
- Sensitive responses are not cached publicly
- Error messages do not reveal secrets or internal system details
- Logs are minimized and sanitized
- Personal data is not logged unless strictly necessary
- Production and development environments remain separated
- Dependencies are pinned, reviewed, and regularly updated
- Security scanning must run before production deployment
- Changes to authentication, authorization, payments, privacy, or data storage require additional review
- Debug mode must remain disabled in production
- Default credentials are prohibited
- HTTPS is mandatory in production
- Security headers must be configured at the application or hosting layer
- Backups, where used, must be protected and access-controlled

## Artificial Intelligence and User Content

User messages, screenshots, and analysis requests must be treated as potentially sensitive.

The application must:

- Send only the minimum necessary information to external services
- Avoid storing user content by default unless storage is explicitly required
- Clearly disclose when information is stored
- Prevent secrets from being included in prompts or logs
- Treat model output as untrusted data
- Validate model output before using it in privileged actions
- Never allow model output to directly authorize access, payments, account changes, or destructive operations
- Apply server-side authorization independently of any AI response
- Defend against prompt injection and malicious uploaded content
- Avoid exposing internal prompts, credentials, configuration, or hidden application data

## Vulnerability Handling

Reports will be evaluated based on reproducibility, impact, affected users, and exploitation risk.

The repository owner may:

- Request additional information
- Reject reports that cannot be reproduced
- Reject reports involving only unsupported versions
- Reject automated scanner output without demonstrated impact
- Coordinate remediation privately
- Publish a security advisory after a fix is available

No reward, bounty, payment, or compensation is promised unless a separate written program explicitly states otherwise.

## Responsible Disclosure

Reporters must allow reasonable time for investigation, remediation, testing, and deployment before public disclosure.

Public disclosure must not include:

- Working credentials
- Personal information
- Private user content
- Exploit code that creates unnecessary risk
- Information that would expose users before a fix is available

## Security Is a Shared Responsibility

A security policy does not guarantee that software is vulnerability-free. It establishes the required process for reporting and handling suspected vulnerabilities responsibly.
