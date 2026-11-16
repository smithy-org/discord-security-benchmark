# Discord Server Security Configuration Benchmark

Oct 28, 2026 · Peter Derouen

This benchmark gives a secure baseline for a Discord server and, for each recommendation, a check to run and a fix to apply, so an owner can audit and configure a server without guessing.

**Version 0.1.0 (draft).** Versioning follows semantic versioning; see Appendix F.

## Overview

The layout follows two familiar formats. From CIS Benchmarks it takes profile levels (L1 and L2) and the per-recommendation fields Description, Rationale, Impact, Audit, and Remediation. From DISA STIGs it takes a severity category (CAT I to III), a check and fix pairing, and a NIST 800-53 mapping.

No DISA STIG or CIS Benchmark for Discord was found during research. The content therefore comes from Discord's own safety and moderation pages, the Security Alliance (SEAL) Discord security guide, and general CISA and NIST guidance on multifactor authentication, account access, and incident response. A government-hosted identity protection toolkit (S12) supplied some account settings. Where a control is the author's own addition, it is marked Judgment (see Appendix E).

## Intended audience

- **Server owners and administrators**, who configure the server.
- **Moderators**, who need to know which of their own account settings matter.
- **Reviewers**, who audit a server against this baseline and record Pass, Fail, or Not applicable for each control.

## Scope

Covered: server settings, safety and raid controls, AutoMod, roles and permissions, channels and private areas, apps and webhooks, invites, onboarding and rules, logging, owner recovery, and the personal account settings of the people who run the server.

Not covered: Discord's own platform security, the security of any bot's code or hosting, server monetization and subscriptions, and device or network hardening beyond the basics in section 1.

Some controls require Community to be enabled on the server (for example AutoMod rule editing and Server Insights). Those controls say so in their Audit or Impact field.

## How to use this benchmark

1. Rate the server's impact level (see Conventions), then choose the profile: L1 for every server, L1 and L2 for a server rated Moderate or High.
2. Work through sections 1 to 10 in order. For each control run the Audit step and record Pass, Fail, or Not applicable.
3. Fix each failure using its Remediation step, starting with CAT I.
4. Re-run the Audit step to confirm the fix, then record the date.
5. Repeat on the schedule in control 9.7.

Controls marked Automated can be checked through the Discord API (Appendix C), which is how the companion audit skill works. Controls marked Manual need someone to look in the Discord app.

## Limitations

- Menu paths come from several guides written at different times and may differ in your version of Discord. Where two sources disagree, both paths are given.
- Discord's developer documentation could not be retrieved during research. The permission reference (Appendix A) and API field map (Appendix C) are written from the author's knowledge and must be checked against the current developer docs before being relied on.
- This document is not endorsed by Discord, DISA, CIS, or NIST. The NIST 800-53 mappings are the author's judgment, and a few are loose fits.

## Conventions

Each recommendation has an ID of the form section.number (for example 3.1) and the same fields in the same order, so a reviewer can scan or script them.

### Profile levels

| Profile | Meaning |
| --- | --- |
| L1 | The minimum baseline for any server. Low cost, little effect on members. Aimed at the NIST 800-53B Low baseline. |
| L2 | Stronger controls for a server rated Moderate or High impact, where a compromise or leak would cause real harm. Add them to L1. May add friction for members or staff. |

### Server impact level

Rate the server before choosing a profile. The rating follows the idea in FIPS 199: it is the harm if the server's confidentiality, integrity, or availability is lost, and the highest of the three sets the level. These definitions are adapted for Discord by the author and are not NIST's.

| Level | Harm if the server is compromised, leaked, or lost | Typical servers | Profile |
| --- | --- | --- | --- |
| Low | Limited. A cleanup and some embarrassment. Nothing in the server is confidential | Open public community, hobby or interest server | L1 |
| Moderate | Serious for people or the organization. Staff-only or members-only channels or member information exposed, loss of the organization's main community channel, or financial or reputational damage | An organization's main community, a paid or membership community, a server with staff-only channels | L1 and L2 |
| High | Severe or hard to reverse. Confidential business information, credentials, or sensitive personal data exposed, or harm to people's safety | A server where confidential work, credentials, or sensitive personal data are shared | L1 and L2, plus a review for needs beyond this benchmark |

Record the rating and the reason in the audit record (control 9.8). Re-rate the server when its purpose changes.

### Severity

Severity uses the STIG category names. The definitions below are adapted for Discord and are not DISA's.

| Category | Meaning if the control fails |
| --- | --- |
| CAT I | An unauthorized person could gain control of the server or reach restricted content, or one compromised account could take over the server. |
| CAT II | Protection or detection is weakened enough that compromise or abuse becomes likely. |
| CAT III | Defense in depth or hygiene is reduced. |

### Assessment

- **Automated:** can be checked by reading the server through the Discord API with a read-only bot (Appendix C).
- **Manual:** must be checked in the Discord app or by asking people.

### Applies to

Owner, Admin, Moderator, Member, Bot, or Server (a setting of the server itself).

### Fields in each recommendation

- **Profile, Severity, Assessment, Applies to, NIST 800-53:** the metadata line.
- **Description:** what the setting is.
- **Rationale:** why it matters.
- **Impact:** what changes for members or staff when it is applied.
- **Audit:** how to check it, including the menu path and, where possible, the API field.
- **Remediation:** how to set it.
- **Default / current value:** Discord's default is given only where a source states it. Otherwise record the current value during the audit.
- **References:** source keys from Appendix E. Judgment means the author's own addition.

Menu paths use the form Server Settings > Safety Setup. Discord moves items between menus, so if a path does not match, look for the setting's name.

## Summary table of recommendations

| ID | Recommendation | Profile | Severity | Assessment |
| --- | --- | --- | --- | --- |
| 1.1 | Enable MFA with an authenticator app or security key | L1 | CAT I | Manual |
| 1.2 | Remove SMS as a recovery or backup method | L1 | CAT II | Manual |
| 1.3 | Store backup codes offline | L1 | CAT II | Manual |
| 1.4 | Use a unique, strong password | L1 | CAT II | Manual |
| 1.5 | Protect the email account tied to Discord | L1 | CAT I | Manual |
| 1.6 | Review logged-in devices | L1 | CAT II | Manual |
| 1.7 | Review authorized apps | L1 | CAT II | Manual |
| 1.8 | Remove unneeded account connections | L2 | CAT III | Manual |
| 1.9 | Limit direct messages from server members | L1 | CAT II | Manual |
| 1.10 | Set the DM spam filter to maximum | L1 | CAT III | Manual |
| 1.11 | Brief staff on phishing and QR-code scams | L1 | CAT II | Manual |
| 1.12 | Secure the devices used for staff accounts | L1 | CAT II | Manual |
| 1.13 | Keep confidential discussion off Discord | L1 | CAT III | Manual |
| 1.14 | Use Streamer Mode when streaming | L2 | CAT III | Manual |
| 1.15 | Close unused staff accounts | L2 | CAT III | Manual |
| 2.1 | Set the verification level to Medium or higher | L1 | CAT II | Automated |
| 2.2 | Set the verification level to Highest on Moderate and High impact servers | L2 | CAT II | Automated |
| 2.3 | Scan media from all members with the explicit media filter | L1 | CAT III | Automated |
| 2.4 | Make invite links expire and limit their uses | L1 | CAT II | Automated |
| 2.5 | Review invites regularly | L1 | CAT III | Automated |
| 2.6 | Remove Create Invite from the default role on private servers | L2 | CAT II | Automated |
| 2.7 | Do not use a vanity URL on a private server | L2 | CAT III | Automated |
| 2.8 | Keep the server out of Discovery unless public listing is intended | L1 | CAT II | Automated |
| 2.9 | Disable the server widget | L1 | CAT III | Automated |
| 2.10 | Disable direct messages between members through the server | L2 | CAT III | Manual |
| 3.1 | Require 2FA for moderation actions | L1 | CAT I | Automated |
| 3.2 | Remove risky permissions from @everyone | L1 | CAT I | Automated |
| 3.3 | Restrict member pruning to administrators | L1 | CAT II | Manual |
| 3.4 | Enable Activity Alerts | L1 | CAT II | Manual |
| 3.5 | Send alerts to a private staff channel | L1 | CAT III | Automated |
| 3.6 | Require CAPTCHA for suspicious accounts before they join | L2 | CAT II | Manual |
| 3.7 | Require CAPTCHA for everyone during a suspected raid | L2 | CAT II | Manual |
| 3.8 | Hide DMs from suspicious users | L1 | CAT III | Manual |
| 3.9 | Filter DMs from unknown users | L2 | CAT III | Manual |
| 3.10 | Warn members before they open outbound links | L1 | CAT III | Manual |
| 3.11 | Hide and delete messages from suspected spammers | L1 | CAT III | Manual |
| 3.12 | Document and rehearse Security Actions | L1 | CAT III | Manual |
| 3.13 | Review and resolve alerts | L2 | CAT III | Manual |
| 3.14 | Default notifications to Only @mentions | L1 | CAT III | Automated |
| 4.1 | Create a Block Spam Content rule | L1 | CAT II | Manual |
| 4.2 | Create a Block Mention Spam rule with a low limit | L1 | CAT III | Manual |
| 4.3 | Block harmful links | L1 | CAT II | Manual |
| 4.4 | Block staff and bot impersonation in member names | L1 | CAT II | Manual |
| 4.5 | Enable the commonly flagged words lists | L2 | CAT III | Manual |
| 4.6 | Add custom keyword rules for scams and impersonation | L2 | CAT III | Manual |
| 4.7 | Send AutoMod alerts to a private staff channel | L1 | CAT III | Manual |
| 4.8 | Use timeouts for repeat or severe violations | L2 | CAT III | Manual |
| 4.9 | Keep AutoMod exemptions minimal and documented | L2 | CAT III | Manual |
| 4.10 | Review AutoMod rules and make sure staff know them | L2 | CAT III | Manual |
| 5.1 | Give @everyone baseline permissions only | L1 | CAT I | Automated |
| 5.2 | Limit Administrator to one role with very few members | L1 | CAT I | Automated |
| 5.3 | Restrict Manage Server, Manage Roles, Manage Channels, and Manage Webhooks | L1 | CAT I | Automated |
| 5.4 | Grant Kick and Ban only to fully trusted people | L1 | CAT II | Automated |
| 5.5 | Use the recommended moderator permission set | L1 | CAT II | Automated |
| 5.6 | Use the recommended member permission set | L1 | CAT II | Automated |
| 5.7 | Order roles by privilege | L1 | CAT II | Automated |
| 5.8 | Do daily work from a non-admin role | L1 | CAT II | Manual |
| 5.9 | Review roles and permissions monthly and record changes | L1 | CAT III | Manual |
| 5.10 | Remove unused, duplicate, and overbroad roles | L1 | CAT III | Automated |
| 5.11 | Remove staff roles promptly when someone leaves | L1 | CAT II | Manual |
| 5.12 | Do not let anyone mention powerful roles | L2 | CAT III | Automated |
| 5.13 | Phase in permissions with account leveling | L2 | CAT III | Manual |
| 5.14 | Check access with View Server as Role | L1 | CAT III | Manual |
| 6.1 | Make restricted categories private | L1 | CAT I | Automated |
| 6.2 | Keep every channel in a private category synced to it | L1 | CAT I | Automated |
| 6.3 | Do not open private areas through a broad role | L1 | CAT I | Automated |
| 6.4 | Keep channel overrides few and consistent | L2 | CAT III | Manual |
| 6.5 | Restrict staff, log, and alert channels to staff | L1 | CAT II | Automated |
| 6.6 | Mark mature channels age-restricted | L1 | CAT III | Automated |
| 6.7 | Set slowmode on busy public channels | L2 | CAT III | Automated |
| 6.8 | Configure thread, event, and stage permissions | L2 | CAT III | Automated |
| 6.9 | Restrict voice moderation permissions | L2 | CAT III | Automated |
| 6.10 | Review followed channels | L2 | CAT III | Manual |
| 6.11 | Organize channels into categories | L2 | CAT III | Manual |
| 6.12 | Keep an access list for each private area | L1 | CAT II | Manual |
| 6.13 | Archive or delete inactive channels | L2 | CAT III | Manual |
| 7.1 | Keep an inventory of apps, each with an owner and purpose | L1 | CAT II | Manual |
| 7.2 | Allow only verified apps | L1 | CAT II | Manual |
| 7.3 | Give each bot only the permissions it uses | L1 | CAT I | Automated |
| 7.4 | Keep each bot role to its bot and below human admin roles | L2 | CAT III | Manual |
| 7.5 | Restrict who can run sensitive bot commands | L2 | CAT II | Manual |
| 7.6 | Remove inactive or unneeded apps | L1 | CAT II | Manual |
| 7.7 | Control who can add apps | L1 | CAT II | Manual |
| 7.8 | Review and prune webhooks | L1 | CAT II | Automated |
| 7.9 | Treat tokens and webhook URLs as secrets | L1 | CAT II | Manual |
| 7.10 | Stop role-granting bots from handing out privileged roles | L2 | CAT II | Manual |
| 7.11 | Use logging and protection bots from reputable sources | L2 | CAT III | Manual |
| 7.12 | Review linked platform integrations | L2 | CAT III | Manual |
| 8.1 | Enable Community with its required safety settings | L1 | CAT III | Automated |
| 8.2 | Publish rules that cover behavior, content, moderation, and appeals | L1 | CAT III | Manual |
| 8.3 | Require new members to accept the rules | L2 | CAT III | Manual |
| 8.4 | Make sure onboarding cannot assign privileged or private-area roles | L2 | CAT II | Automated |
| 8.5 | Gate new members with a verification step | L2 | CAT II | Manual |
| 8.6 | Enable Server Insights and watch for anomalies | L2 | CAT III | Manual |
| 8.7 | Keep the Community updates channel staff-only | L1 | CAT III | Automated |
| 9.1 | Limit who can view the audit log | L1 | CAT II | Automated |
| 9.2 | Review the audit log monthly | L1 | CAT II | Manual |
| 9.3 | Create a private logging channel | L1 | CAT III | Manual |
| 9.4 | Keep logs longer than 45 days | L2 | CAT II | Manual |
| 9.5 | Alert on high-risk changes | L2 | CAT II | Manual |
| 9.6 | Keep an incident response plan | L1 | CAT II | Manual |
| 9.7 | Re-check the baseline on a schedule | L1 | CAT III | Manual |
| 9.8 | Record audit results | L2 | CAT III | Manual |
| 9.9 | Report abusive messages before deleting them | L2 | CAT III | Manual |
| 10.1 | Create a dedicated cold admin account | L2 | CAT II | Manual |
| 10.2 | Introduce the cold admin to the server gradually | L2 | CAT II | Manual |
| 10.3 | Transfer ownership with checks | L2 | CAT I | Manual |
| 10.4 | Use the cold admin only for owner-only work, and log in monthly | L2 | CAT III | Manual |
| 10.5 | Back up the server configuration off-platform | L2 | CAT III | Manual |

## 1 Account security

These controls protect the personal Discord accounts of the owner, admins, and moderators. A server is only as safe as the weakest account that holds a powerful role.

### 1.1 Enable MFA with an authenticator app or security key

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** IA-2(1), IA-2(2)

**Description:** Multi-factor authentication (MFA, also called 2FA) requires a second proof of identity at login. Use an authenticator app or a security key.

**Rationale:** A compromised moderator or admin account can ban members and delete channels, messages, and roles. MFA stops most password-only takeovers.

**Impact:** Staff must keep the second factor available. Losing it without backup codes locks them out.

**Audit:** Each staff member opens User Settings > My Account (SEAL: User Settings > Account > Password & Security > Multi-Factor Authentication) and confirms MFA is on. Discord does not show a reviewer who has MFA, so collect a confirmation from each person. Control 3.1 enforces this for moderation actions.

**Remediation:** In the same menu, enable MFA with an authenticator app and/or security key.

**References:** S9, S10, S11, S12, C2, C1, N5

### 1.2 Remove SMS as a recovery or backup method

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** IA-2(1), IA-5

**Description:** Text messages can be intercepted and phone numbers can be taken over. Staff accounts should not depend on SMS.

**Rationale:** Discord advises against SMS 2FA, and SEAL says managed MFA beats SMS, which it lists as a common failure point.

**Impact:** An account with no verified phone cannot join servers that set verification to Highest. Check that staff do not need that.

**Audit:** User Settings > Account > Account Info shows no phone number, and SMS Backup Authentication is off.

**Remediation:** Disable SMS Backup Authentication and remove the phone number from the account.

**References:** S10, S11, S9, C2, C3, C1

### 1.3 Store backup codes offline

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** IA-5, CP-2

**Description:** Backup codes let staff regain access if the authenticator is lost.

**Rationale:** Codes saved in email or cloud notes can be stolen with the account they protect.

**Impact:** None beyond keeping a paper copy safe.

**Audit:** Each staff member confirms they have codes stored offline (for example, on paper) and not in an online document.

**Remediation:** After enabling MFA, choose View Backup Codes, write them down, and store them securely.

**References:** S10

### 1.4 Use a unique, strong password

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** IA-5(1)

**Description:** The Discord password is long, random, and used nowhere else.

**Rationale:** Reused passwords turn any breach elsewhere into a Discord breach. Discord recommends a password manager.

**Impact:** Staff need a password manager.

**Audit:** Each staff member confirms the password is unique and generated, and was not used on other sites.

**Remediation:** Generate a new password with a password manager. Change it at once if a QR code was scanned or a breach is suspected. Some sources advise periodic changes; current NIST guidance favors changing on evidence of compromise (author's note).

**References:** S11, S12, S9, C1, C3, N5

### 1.5 Protect the email account tied to Discord

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Manual · **Applies to:** Owner, Admin · **NIST 800-53:** IA-2(1), IA-5

**Description:** The email address on the account can reset the Discord password, so it needs the same protection.

**Rationale:** An attacker who controls the email can take the Discord account regardless of its own settings.

**Impact:** None beyond configuring the email account.

**Audit:** The email account has MFA from an authenticator app or security key, no SMS recovery, and a unique password.

**Remediation:** Enable MFA on the email account and remove phone-based recovery. For a cold admin account, use a new dedicated email address (control 10.1).

**References:** S10, Judgment, C1

### 1.6 Review logged-in devices

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** AC-12, AC-2

**Description:** The list of devices signed in to the account.

**Rationale:** An unknown device may be an attacker's session.

**Impact:** Removed devices must sign in again.

**Audit:** User Settings > Account > Password & Security > Logged-in Devices (SOCOM: User Settings > Devices). Every entry is recognized.

**Remediation:** Remove unknown devices. If one is unrecognized, log out of all known devices and change the password immediately. Repeat quarterly.

**References:** S10, S12

### 1.7 Review authorized apps

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** CM-7, AC-6

**Description:** Third-party apps that were granted access to the account.

**Rationale:** An authorized app can act with the permissions you gave it. Old or unknown apps are an unnecessary risk.

**Impact:** Some apps stop working until reauthorized.

**Audit:** User Settings > Connected Apps > Authorized Apps lists only apps still in use.

**Remediation:** Deauthorize the rest. Repeat quarterly.

**References:** S10, S9, C1

### 1.8 Remove unneeded account connections

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** CM-7

**Description:** Linked accounts such as other game or social services.

**Rationale:** Each link exposes information and adds another way in. SOCOM advises not linking accounts.

**Impact:** Features that rely on a link, such as showing game activity, stop working.

**Audit:** User Settings > Connected Apps > Connections lists only what is needed.

**Remediation:** Remove unneeded connections.

**References:** S10, S12

### 1.9 Limit direct messages from server members

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** SI-8

**Description:** Staff accounts are the main target for phishing DMs from people in the server.

**Rationale:** SEAL and SOCOM both recommend turning off DMs and message requests from server members.

**Impact:** Members cannot DM staff directly unless they are friends. Give members another way to reach staff, such as a ticket channel.

**Audit:** User Settings > Messaging Permissions > Direct Messages (SOCOM: Privacy and Safety > Server Privacy Defaults) shows Allow DMs from other server members off, and message requests from server members you may not know off.

**Remediation:** Turn both off. SEAL labels the second setting Filter messages from server members I may not know; confirm which toggle matches in your app version.

**References:** S10, S12

### 1.10 Set the DM spam filter to maximum

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator, Member · **NIST 800-53:** SI-8

**Description:** Discord can filter DMs it considers spam.

**Rationale:** Reduces the volume of phishing and scam messages reaching staff and members.

**Impact:** Some legitimate DMs from strangers may be filtered.

**Audit:** User Settings > Messaging Permissions > Spam Filters is set to Filter all spam (SOCOM, older layout: Privacy and Safety, filter direct messages from non-friends at minimum).

**Remediation:** Choose the strictest option.

**References:** S10, S12

### 1.11 Brief staff on phishing and QR-code scams

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** AT-2

**Description:** A short, repeated briefing on how Discord accounts are taken over.

**Rationale:** Discord warns that scanning a QR code sent by someone else can let an attacker into the account without the password or 2FA. Compromised trusted accounts are a main route for phishing.

**Impact:** Staff time only.

**Audit:** A record exists that each staff member has read the briefing in the last 12 months.

**Remediation:** Brief staff on: never scanning QR codes from others; typing discord.com instead of following a login link; checking for lookalike domains; treating unexpected urgency, rewards, or requests for sensitive actions with suspicion; and changing the password at once after a suspicious scan, which also logs out every device.

**References:** S11, S9, C1

### 1.12 Secure the devices used for staff accounts

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** AC-11

**Description:** Devices holding Discord logins and the authenticator app are locked and protected.

**Rationale:** Physical access to an unlocked device bypasses MFA.

**Impact:** None beyond screen locks.

**Audit:** Each staff member confirms a password or PIN, an automatic lock, and that shared or public computers are used only in a private browser window.

**Remediation:** Set the lock and keep the device holding the authenticator physically secure.

**References:** S11, C1

### 1.13 Keep confidential discussion off Discord

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** SC-8, SC-28

**Description:** Restricted channels and DMs on Discord are not end-to-end encrypted.

**Rationale:** Anyone with access to the server data, or a compromised admin, can read them.

**Impact:** Staff need another channel for secrets such as recovery codes and bot tokens.

**Audit:** Confirm that passwords, tokens, and backup codes are not posted in any channel or DM.

**Remediation:** Move such material to a password manager or other secrets store.

**References:** S10

### 1.14 Use Streamer Mode when streaming

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** AC-3 (loose fit)

**Description:** Streamer Mode hides personal details on screen while sharing a screen or streaming.

**Rationale:** SOCOM recommends it when streaming through Discord.

**Impact:** Some details are hidden on screen.

**Audit:** User Settings > App Settings shows Streamer Mode on, or set to enable automatically.

**Remediation:** Turn Streamer Mode on.

**References:** S12

### 1.15 Close unused staff accounts

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Owner, Admin, Moderator · **NIST 800-53:** AC-2(3)

**Description:** Old or alternate accounts that no longer serve a purpose.

**Rationale:** SOCOM advises disabling or deleting accounts that are no longer needed, since forgotten accounts are never watched.

**Impact:** Deleted accounts cannot be recovered.

**Audit:** The person can name every Discord account they hold and why.

**Remediation:** User Settings > My Account: disable or delete accounts that are no longer needed. Do not close the cold admin account (control 10.1).

**References:** S12, C1

## 2 Access control and exposure

These controls decide who can join the server and how visible it is to the public.

### 2.1 Set the verification level to Medium or higher

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-2, SC-5

**Description:** The verification level sets what a new account must satisfy before it can send messages or DM members.

| Level | Requirement (API value) |
| --- | --- |
| None (0) | Anyone can chat immediately. |
| Low (1) | A verified email on the account. |
| Medium (2) | Verified email, plus an account-age wait of about five minutes. |
| High (3) | Medium, plus ten minutes of server membership. |
| Highest (4) | High, plus a verified phone number on the account. |

**Rationale:** Throwaway and newly made accounts are the usual source of spam and raids. Discord recommends at least Low for any server that posts an invite link publicly; SEAL recommends at least Medium. This benchmark uses Medium as the L1 floor.

**Impact:** Brand-new accounts must wait before chatting. Members who hold any role, and moderators and administrators, are exempt, so granting a role bypasses this check.

**Audit:** Server Settings > Safety Setup > DM and Spam Protection > Verification Level (a non-Community server shows it under Safety Setup > Verification Level; mobile: Server Settings > Moderation). API: verification\_level is 2 or higher.

**Remediation:** Choose Medium or higher and save.

**Default / current value:** Record during audit.

**References:** S1, S3, S7, S9, S10

### 2.2 Set the verification level to Highest on Moderate and High impact servers

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-2, SC-5

**Description:** Highest requires a verified phone number on every new account.

**Rationale:** SOCOM recommends Highest. A phone number is hard to create in bulk, so it blocks most disposable accounts.

**Impact:** Legitimate new members must add a phone number to chat, which many will refuse. Consider High (3) if that cost is too large. Members who hold a role are exempt, so staff can still grant roles to approved people.

**Audit:** API: verification\_level is 4 (or 3 where Highest is judged too costly and the exception is recorded).

**Remediation:** Same menu as 2.1; choose Highest.

**References:** S12, S3

### 2.3 Scan media from all members with the explicit media filter

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** SI-3 (loose fit)

**Description:** Discord can scan images and delete media it judges explicit. The options are off, scan members without roles, or scan all members.

**Rationale:** Discord recommends turning it on, SOCOM recommends all members, and it is a requirement for Community servers. Age-restricted channels are exempt.

**Impact:** Some legitimate images may be removed. Channels with intentionally explicit content must be marked age-restricted.

**Audit:** Server Settings > Safety Setup > explicit image filter (SOCOM: Server Settings > Overview > Moderation). API: explicit\_content\_filter is 2.

**Remediation:** Select the option that scans media from all members.

**References:** S1, S7, S12

### 2.4 Make invite links expire and limit their uses

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-2, AC-3

**Description:** An invite that never expires and has no use limit lets anyone join at any time.

**Rationale:** SEAL suggests invites that expire after about 24 hours with a maximum of 50 to 100 uses. A permanent link posted anywhere stays valid for as long as it exists.

**Impact:** Old links stop working, so links on websites and in videos must be replaced.

**Audit:** Server Settings > Invites lists every link with its creator, uses, and expiry. API: GET guild invites (needs Manage Server); fail if any invite has max\_age 0 and max\_uses 0.

**Remediation:** Delete permanent links. When creating a new invite, set Expire After and Max Number of Uses.

**References:** S10, S2

### 2.5 Review invites regularly

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Admin · **NIST 800-53:** AC-2

**Description:** An inventory of active invites with a known creator and purpose.

**Rationale:** SEAL advises reviewing and deleting old invites regularly. Stale links and links made by former staff are untracked entry points.

**Impact:** Staff time.

**Audit:** Every active invite has a known creator who is still staff and a stated purpose. Review monthly (control 9.7).

**Remediation:** Delete invites that fail the test.

**References:** S10, S2

### 2.6 Remove Create Invite from the default role on private servers

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3, AC-6

**Description:** The Create Invite permission lets a member generate new invite links.

**Rationale:** On an invite-only server, members should not be able to bring in people staff have not approved.

**Impact:** Members cannot invite friends. Staff share invites on request.

**Audit:** Server Settings > Roles > @everyone: Create Invite is off. API: @everyone permissions lack CREATE\_INSTANT\_INVITE.

**Remediation:** Turn the permission off for @everyone and any broad member role.

**References:** Judgment

### 2.7 Do not use a vanity URL on a private server

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3

**Description:** A vanity URL is a short, permanent public join link where available.

**Rationale:** It cannot expire. Discord notes that pausing invites also covers vanity URLs, which confirms they are a join path.

**Impact:** The short link stops working.

**Audit:** Server Settings shows no vanity URL set. API: features does not include VANITY\_URL.

**Remediation:** Remove the vanity URL in the server's vanity URL settings, where available.

**References:** S5, Judgment

### 2.8 Keep the server out of Discovery unless public listing is intended

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-22

**Description:** Discovery lists a Community server in Discord's public server browser. Discord states it currently needs more than 1,000 members or Partnered or Verified status.

**Rationale:** A discoverable server lets strangers find and join it. A private or pre-release server should not be listed.

**Impact:** None for a private server. A public community loses a growth channel.

**Audit:** Server Settings > Discovery is off. API: features does not include DISCOVERABLE.

**Remediation:** Disable Discovery. Note that pausing invites also blocks joining through Discovery.

**References:** S7, S5

### 2.9 Disable the server widget

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-22

**Description:** The widget exposes server information publicly.

**Rationale:** Public information about members and channels is unneeded exposure on a private server.

**Impact:** Websites that embed the widget stop showing it.

**Audit:** Server Settings > Widget: Enable Server Widget is off. API: widget\_enabled is false.

**Remediation:** Turn it off.

**References:** Judgment

### 2.10 Disable direct messages between members through the server

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8

**Description:** A server privacy setting that stops members from DMing each other through the server.

**Rationale:** SEAL recommends it for admins. It removes the main path for scam DMs from other members.

**Impact:** Members cannot start a DM with another member from this server. Friends can still DM.

**Audit:** Server Settings > Privacy Settings: Direct Messages is disabled.

**Remediation:** Disable it. For a temporary block during an incident, use Pause DMs (control 3.12).

**References:** S10, S4

## 3 Safety Setup and incident controls

These controls live mostly under Server Settings > Safety Setup, which groups Moderation permissions, DM and Spam Protection, Raid Protection and CAPTCHA, and AutoMod (section 4). They protect the server against account takeover, spam, and raids, and give staff tools for an incident.

### 3.1 Require 2FA for moderation actions

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** IA-2(1)

**Description:** When on, moderators and administrators must have 2FA on their own accounts before Discord lets them use the most powerful permissions. Discord's Moderator Academy lists seven that are disabled for staff without 2FA: Administrator, Manage Server, Manage Channels, Manage Roles, Manage Messages, Kick Members, and Ban Members. Discord may have added others since; check the current list.

**Rationale:** It enforces control 1.1 instead of relying on staff to comply. The owner must have 2FA on their own account first.

**Impact:** Staff without 2FA lose those permissions until they enable it.

**Audit:** Server Settings > Safety Setup > Moderation > Permissions: Require 2FA for moderation is on (S1 calls it server-wide 2FA). API: mfa\_level is 1.

**Remediation:** The owner turns it on after enabling 2FA on the owner account.

**Default / current value:** Off unless the owner enables it.

**References:** S1, S2, S10, C1, C2

### 3.2 Remove risky permissions from @everyone

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6, CM-6

**Description:** A Safety Setup option that strips dangerous permissions from the default role every member has.

**Rationale:** Anything granted to @everyone is granted to every member, including accounts created minutes ago.

**Impact:** Members lose permissions they should not have had.

**Audit:** Server Settings > Safety Setup > Moderation > Permissions: Remove risky permissions for @everyone is on. Confirm with control 5.1.

**Remediation:** Turn it on, then verify @everyone under Server Settings > Roles.

**References:** S10

### 3.3 Restrict member pruning to administrators

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** AC-6

**Description:** Pruning removes inactive members in bulk. Discord's Moderator Academy notes that Kick Members also allows running a prune.

**Rationale:** A bulk removal by a compromised or careless moderator is costly to undo.

**Impact:** Moderators can still kick individuals but cannot prune.

**Audit:** Server Settings > Safety Setup > Moderation > Permissions: Restrict prune members to admin is on.

**Remediation:** Turn it on.

**References:** S10, S2

### 3.4 Enable Activity Alerts

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server, Admin, Moderator · **NIST 800-53:** SI-4

**Description:** Activity Alerts warn staff of unusual or heightened activity, such as abnormal join or DM volume. They appear as a banner when staff are online, a push notification when offline, and a post in the alert channel.

**Rationale:** Early warning gives staff time to pause invites or DMs before a raid does damage.

**Impact:** Staff receive notifications.

**Audit:** Server Settings > Safety Setup > Raid Protection and CAPTCHA: Activity Alerts is on. Each staff member also has User Settings > Notifications > Community Activity Alerts on. Alerts are visible to anyone with Administrator, Kick Members, Timeout Members, Ban Members, or Manage Server.

**Remediation:** Turn both on.

**References:** S4, S10, C1

### 3.5 Send alerts to a private staff channel

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AU-6, SI-4

**Description:** A designated channel that receives Activity Alert and AutoMod messages.

**Rationale:** Discord says moderators need access to the alert channel to see in-channel alerts, and the AutoMod guidance says to pick a channel only admins and moderators can reach. A public alert channel tells attackers what you detect.

**Impact:** None.

**Audit:** The alert channel exists, is not visible to @everyone, and is visible to moderators. API: the guild's safety alerts channel id is set (author's recollection of the field; verify) and the channel's overwrites deny View Channel to @everyone.

**Remediation:** Create a private staff channel and select it as the alert channel.

**References:** S4, S6

### 3.6 Require CAPTCHA for suspicious accounts before they join

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SC-5, SI-4

**Description:** Discord can challenge accounts it considers suspicious with a CAPTCHA at join.

**Rationale:** SEAL recommends it. It blocks automated accounts without affecting normal members.

**Impact:** A small number of members see a challenge.

**Audit:** Server Settings > Safety Setup > Raid Protection and CAPTCHA: CAPTCHA suspicious accounts before they join is on.

**Remediation:** Turn it on.

**References:** S10

### 3.7 Require CAPTCHA for everyone during a suspected raid

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SC-5, IR-4

**Description:** When a raid is detected, all new accounts must complete a CAPTCHA, according to SEAL for one hour after detection.

**Rationale:** Slows a raid while staff respond.

**Impact:** Legitimate joiners are challenged during the raid window only.

**Audit:** The matching option under Raid Protection and CAPTCHA is on.

**Remediation:** Turn it on.

**References:** S10

### 3.8 Hide DMs from suspicious users

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8

**Description:** A DM and Spam Protection option that hides direct messages from accounts Discord flags as suspicious.

**Rationale:** Cuts exposure to scam DMs for all members at no cost to normal use.

**Impact:** None expected.

**Audit:** Server Settings > Safety Setup > DM and Spam Protection: Hide DMs from suspicious users is on.

**Remediation:** Turn it on.

**References:** S10

### 3.9 Filter DMs from unknown users

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8

**Description:** Sends DMs from unknown users to a filtered inbox.

**Rationale:** SEAL recommends it as a further reduction in scam DMs.

**Impact:** Some legitimate first-contact DMs are filtered.

**Audit:** The option under DM and Spam Protection is on.

**Remediation:** Turn it on.

**References:** S10

### 3.10 Warn members before they open outbound links

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8, AT-2

**Description:** Shows a warning before a member follows a link out of Discord.

**Rationale:** Phishing links are the main route to account takeover. A warning adds a moment of doubt.

**Impact:** An extra click.

**Audit:** Warn members before they visit outbound links is on under DM and Spam Protection.

**Remediation:** Turn it on.

**References:** S10

### 3.11 Hide and delete messages from suspected spammers

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8

**Description:** Hides all messages from accounts judged to be spammers and deletes them.

**Rationale:** Removes spam content before members see it.

**Impact:** Rare false positives may hide a real member's messages.

**Audit:** The option under DM and Spam Protection is on.

**Remediation:** Turn it on.

**References:** S10

### 3.12 Document and rehearse Security Actions

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Owner, Admin · **NIST 800-53:** IR-1, IR-4

**Description:** Security Actions let staff pause invites and pause DMs between members who are not friends during an incident. They are reached from the server name menu, the Members tab, or an alert message. Staff choose the duration and save; a banner shows what is paused and when it ends. Pause Invites can also be set at Server Settings > Invites and needs Manage Server on desktop.

**Rationale:** Pausing stops new joins without deleting invite links. It covers invite links, embeds, vanity URLs, and Discovery. Users who tried while paused are not notified when invites return, and AutoMod cannot pause invites automatically.

**Impact:** While paused, new members cannot join.

**Audit:** A written runbook names who may use Security Actions and when, and the person responsible can open the Security Actions panel without help.

**Remediation:** Write the runbook, and do a trial pause and resume at a quiet time.

**References:** S4, S5, C1

### 3.13 Review and resolve alerts

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin, Moderator · **NIST 800-53:** IR-5, IR-4

**Description:** Each Activity Alert is reviewed, usually through the Members page, and marked Mark as Resolved with a note for other moderators.

**Rationale:** Notes make an alert's outcome visible to the next person.

**Impact:** Staff time.

**Audit:** Recent alerts show a resolved state and a note.

**Remediation:** Resolve alerts as they are handled.

**References:** S4

### 3.14 Default notifications to Only @mentions

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** CM-6, SC-5

**Description:** The default notification level for new members.

**Rationale:** Notifying on every message in a busy server makes mass pinging and spam more disruptive. SEAL recommends Only @mentions.

**Impact:** Members receive fewer notifications until they change their own setting.

**Audit:** Server Settings > Engagement > Default Notification Settings (SEAL path) is Only @mentions. API: default\_message\_notifications is 1.

**Remediation:** Select Only @mentions.

**References:** S10

## 4 AutoMod

AutoMod is Discord's built-in rule engine for blocking spam, scam content, and abusive names. Per Discord's support article, editing rules needs Manage Server or Administrator, works on desktop only, and requires Community to be enabled on the server (control 8.1). Users with Administrator or Manage Server are always exempt from AutoMod.

### 4.1 Create a Block Spam Content rule

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8

**Description:** A machine-learning rule that detects spammy text such as unsolicited ads and invite spam. Per Discord it is English only.

**Rationale:** SEAL lists spam as the first AutoMod rule to create.

**Impact:** Some borderline messages are blocked.

**Audit:** Server Settings > AutoMod shows an enabled spam rule with the Block and Alert actions.

**Remediation:** Create the rule, choose Block message and Send alert, and select the private alert channel (control 4.7).

**References:** S6, S10

### 4.2 Create a Block Mention Spam rule with a low limit

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8, SC-5

**Description:** Limits the number of user or role mentions in one message. Discord allows a limit of up to 50.

**Rationale:** Mass mentions are a common raid and harassment tool.

**Impact:** A message with more mentions than the limit is blocked.

**Audit:** The rule exists and its limit is low. This benchmark suggests about 5 to 10 mentions, which is the author's judgment and should be set to what normal use needs.

**Remediation:** Create the rule with Block, Alert, and Timeout member.

**References:** S6, S10, Judgment

### 4.3 Block harmful links

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8

**Description:** A rule that blocks links to known harmful sites.

**Rationale:** SEAL lists Harmful Links among the rules to create. Phishing links are the main route to account takeover.

**Impact:** Legitimate links to flagged sites are blocked.

**Audit:** A harmful-links rule is enabled. Discord's AutoMod article does not list this rule type, so if your app does not show it, record that and use control 4.6 for known scam domains.

**Remediation:** Create the rule with Block and Alert.

**References:** S10, S6

### 4.4 Block staff and bot impersonation in member names

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** IA-4, SI-4

**Description:** Blocks chosen words in usernames and server nicknames. A member who matches must change their nickname, or username if they cannot edit nicknames, before they can talk.

**Rationale:** SEAL suggests blocking words such as Support, Bot, Admin, Tech, and Helpdesk, which impersonators use to scam members.

**Impact:** Members whose names contain a blocked word must rename.

**Audit:** Server Settings > Safety Setup > AutoMod > Edit > Members shows a rule with the blocked words, plus the names of your own staff and organization where impersonation is a risk.

**Remediation:** Add the rule and review matches monthly.

**References:** S6, S10

### 4.5 Enable the commonly flagged words lists

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8 (loose fit)

**Description:** Three ready-made lists: Insults and Slurs, Sexual Content, and Severe Profanity. Discord notes they are English only.

**Rationale:** Reduces abusive content without maintaining your own list.

**Impact:** Some legitimate uses of listed words are blocked. Add exempt words as needed.

**Audit:** The lists are enabled in Server Settings > AutoMod.

**Remediation:** Enable the lists and set the Block and Alert actions.

**References:** S6

### 4.6 Add custom keyword rules for scams and impersonation

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-8

**Description:** Custom keyword rules accept up to 1,000 terms each, up to 60 characters per term, with wildcards (an asterisk matches a prefix, suffix, or anywhere). They can block with a custom message of up to 150 characters, alert, and time out.

**Rationale:** Scam phrases and known bad domains seen in your server can be blocked before staff see them.

**Impact:** Over-broad terms block normal chat.

**Audit:** At least one custom rule covers scam phrases and links seen in your server.

**Remediation:** Create the rule from your moderation log and review it quarterly.

**References:** S6, Judgment

### 4.7 Send AutoMod alerts to a private staff channel

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** AU-6, SI-4

**Description:** Every rule's alert action posts to a channel you choose.

**Rationale:** SEAL recommends a private channel visible only to mods, team, and admins, with each rule's logs sent there. Discord says to use a channel only admins and moderators can access.

**Impact:** None.

**Audit:** Each rule has Send alert on, pointing at a channel that denies View Channel to @everyone.

**Remediation:** Create the channel (control 3.5) and set it on every rule.

**References:** S6, S10

### 4.8 Use timeouts for repeat or severe violations

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** SI-4, IR-4

**Description:** Custom keyword and mention spam rules can time out the offending member automatically.

**Rationale:** Stops a spammer from continuing while staff are away.

**Impact:** A false positive silences a member temporarily.

**Audit:** At least the mention spam rule has Timeout member on.

**Remediation:** Turn it on for the rules where blocking alone is not enough.

**References:** S6

### 4.9 Keep AutoMod exemptions minimal and documented

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** CM-6, AC-6

**Description:** Each rule can exempt roles and channels. Exempting a channel also exempts its threads and voice-channel text.

**Rationale:** Exemptions are blind spots. An exempt role that attackers can obtain defeats the rule.

**Impact:** Exempt roles are not filtered.

**Audit:** Every exemption has a recorded reason and no broad member role is exempt.

**Remediation:** Remove exemptions that cannot be justified.

**References:** S6, S10

### 4.10 Review AutoMod rules and make sure staff know them

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin, Moderator · **NIST 800-53:** AU-6, AT-2

**Description:** Staff know which rules exist, what each blocks, and which roles are exempt.

**Rationale:** SEAL asks team members to know the AutoMod rules so that they can explain blocks and spot gaps.

**Impact:** Staff time.

**Audit:** Each moderator can list the rules and exemptions. Rule changes are reviewed monthly (control 9.7).

**Remediation:** Brief new moderators and review rule logs for false positives.

**References:** S10

## 5 Roles and permissions

Roles decide what each person can do. Appendix A lists every permission with a risk tier, and Appendix B gives a reference role layout. Discord's Moderator Academy notes that a role can only edit roles below its highest role and cannot grant permissions it does not hold.

### 5.1 Give @everyone baseline permissions only

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6, CM-6

**Description:** @everyone is the role every member has, including a brand-new account.

**Rationale:** Anything granted here is granted to everyone. SEAL asks reviewers to watch for Manage Channels, Manage Roles, Manage Webhooks, Manage Server, and Administrator on user roles.

**Impact:** None for members who only chat.

**Audit:** Server Settings > Roles > @everyone shows none of: Administrator, Manage Server, Manage Roles, Manage Channels, Manage Webhooks, Manage Expressions, Manage Events, Manage Threads, Manage Messages, Manage Nicknames, Kick Members, Ban Members, Time Out Members, View Audit Log, View Server Insights, Mention @everyone, @here and All Roles. API: the @everyone role's permissions do not intersect that set.

**Remediation:** Turn each off. See also control 3.2.

**References:** S10, S2

### 5.2 Limit Administrator to one role with very few members

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server, Admin · **NIST 800-53:** AC-6(5)

**Description:** Administrator grants every permission and bypasses every channel lock.

**Rationale:** Discord calls it the most dangerous permission. SEAL advises one admin role with two or three members at most, and rarely any bot.

**Impact:** Staff who need only some admin powers must get specific permissions instead.

**Audit:** Server Settings > Roles: exactly one role, or a documented few, has Administrator. API: count the roles with the Administrator permission. Read role membership from the Members tab, because the audit bot cannot list members without a privileged setting.

**Remediation:** Remove Administrator from every other role and grant specific permissions.

**References:** S2, S10, C1

### 5.3 Restrict Manage Server, Manage Roles, Manage Channels, and Manage Webhooks

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6

**Description:** Each of these lets the holder change how the server is protected: settings, who has access, which channels exist and who can see them, and which webhooks can post.

**Rationale:** SEAL says to limit these to the fewest roles needed. Manage Roles can hand out access, and Manage Channels can open a private channel.

**Impact:** Fewer staff can change structure.

**Audit:** For each role, review these four. Only the admin role and documented exceptions hold them. API: list roles with these bits set.

**Remediation:** Remove the permissions from other roles.

**References:** S10, S2, C1

### 5.4 Grant Kick and Ban only to fully trusted people

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6

**Description:** Kick removes a member, who can rejoin by invite. Ban blocks them from rejoining.

**Rationale:** SEAL says to give these only to fully trusted people. A hijacked account with Ban can empty the server.

**Impact:** Fewer staff can remove members.

**Audit:** Only moderator and admin roles hold Kick Members and Ban Members.

**Remediation:** Remove them from other roles.

**References:** S10, S2

### 5.5 Use the recommended moderator permission set

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Moderator · **NIST 800-53:** AC-6

**Description:** SEAL's moderator set is: View Channels, View Audit Log, View Server Insights, Kick Members, Ban Members, Timeout Members, Send Messages and Create Posts, Embed Links, Attach Files, Add Reactions, Add External Emoji, Use External Stickers, Manage Messages, Bypass Slowmode, Read Message History, and Request to Speak.

**Rationale:** It lets moderators moderate without being able to change server structure.

**Impact:** Moderators cannot manage roles, channels, webhooks, or server settings.

**Audit:** The moderator role's permissions are a subset of the set above. API: compare against the list.

**Remediation:** Remove anything extra. If this set includes Kick and Ban for someone you do not fully trust, drop those two (control 5.4).

**References:** S10

### 5.6 Use the recommended member permission set

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Member · **NIST 800-53:** AC-6

**Description:** SEAL's set for ordinary members is: View Channels, Send Messages and Create Posts, Embed Links, Add Reactions, Read Message History, and Request to Speak. Discord's Moderator Academy lists others that are generally safe, such as Attach Files, Connect, and Speak.

**Rationale:** A short list is easy to review and limits what a compromised member account can do.

**Impact:** Members may lack extras such as external emoji until you add them deliberately.

**Audit:** The member or verified role's permissions are a subset of the set plus documented additions from Appendix A tier Low.

**Remediation:** Remove anything extra.

**References:** S10, S2

### 5.7 Order roles by privilege

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6, AC-3

**Description:** Higher roles override lower ones, and a role can only manage roles below it.

**Rationale:** SEAL orders roles Cold Admin, Team, Moderator, Verified from top to bottom. A bot or moderator role placed above an admin role can act on it.

**Impact:** None.

**Audit:** Server Settings > Roles shows the order in Appendix B. API: role positions follow that order, and no bot role sits above a human admin role.

**Remediation:** Drag roles into order.

**References:** S10, S2

### 5.8 Do daily work from a non-admin role

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** AC-6(2)

**Description:** People with Administrator use a lower role for everyday moderation and the admin role only for changes that need it.

**Rationale:** SEAL tells moderators not to use admin accounts for everyday moderation. It limits what a hijacked session can do.

**Impact:** Staff switch roles or accounts for admin tasks.

**Audit:** Ask each admin which account or role they use daily.

**Remediation:** Create a moderator role or account for daily work.

**References:** S9

### 5.9 Review roles and permissions monthly and record changes

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** AC-6(7), CM-3

**Description:** A monthly review of every role's permissions and members, with each change and its reason in a log.

**Rationale:** SEAL advises monthly review with a spreadsheet of changes and justifications, and reviewing all of a role's permissions.

**Impact:** Staff time.

**Audit:** A change log exists with an entry for every role change since the last review, and the last review is under a month old.

**Remediation:** Start the log. Use Appendix A as the checklist.

**References:** S10, C1

### 5.10 Remove unused, duplicate, and overbroad roles

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Admin · **NIST 800-53:** AC-2

**Description:** Roles that nobody needs, share a name with another role, or give more than their purpose requires.

**Rationale:** Extra roles hide excess privilege. SEAL asks to remove lingering or overly broad permissions and roles with unintended members.

**Impact:** None if the role is truly unused.

**Audit:** API: roles with no permissions, no channel overwrite, and no color, and roles with duplicate names, are candidates. Confirm member counts in the Members tab.

**Remediation:** Delete or merge them. Review twice a year.

**References:** S10

### 5.11 Remove staff roles promptly when someone leaves

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** PS-4, AC-2(3)

**Description:** A person who stops moderating or developing loses the role the same day.

**Rationale:** A former staff member's account keeps its powers and is no longer watched.

**Impact:** None.

**Audit:** Compare the staff roster with the members of each privileged role.

**Remediation:** Remove the roles, and rotate any shared secrets they knew.

**References:** Judgment, C1

### 5.12 Do not let anyone mention powerful roles

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6

**Description:** A role setting allows any member to @mention it.

**Rationale:** A mentionable admin or moderator role makes staff impersonation and mass pings easy.

**Impact:** Staff roles cannot be pinged by members.

**Audit:** Server Settings > Roles > Edit role: Allow anyone to @mention this role is off for staff roles. API: the role's mentionable flag is false.

**Remediation:** Turn it off.

**References:** Judgment

### 5.13 Phase in permissions with account leveling

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** AC-2, AC-6

**Description:** New members start with few permissions and gain more over time or by approval.

**Rationale:** SEAL recommends leveling so a newly joined account cannot do much.

**Impact:** Members wait or must be approved.

**Audit:** A defined path exists from new member to full member, enforced by a role or bot.

**Remediation:** Create the intermediate roles and a way to promote.

**References:** S10

### 5.14 Check access with View Server as Role

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin, Moderator · **NIST 800-53:** CA-2

**Description:** Discord can show the server as it appears to a given role.

**Rationale:** It is the quickest way to confirm a private area is really hidden and a role sees what it should.

**Impact:** None.

**Audit:** For @everyone, member, and each role that unlocks a restricted area, use Server Settings > Roles > View Server as Role and confirm the visible channels match Appendix B.

**Remediation:** Fix any mismatch using section 6.

**References:** S10

## 6 Channels, categories, and private areas

This section covers how access is applied to channels. It is where a restricted area, such as a staff-only category, is actually locked down or accidentally left open.

### 6.1 Make restricted categories private

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3, AC-6

**Description:** A private category hides itself and its channels from everyone except roles you name.

**Rationale:** If @everyone can view a restricted category, anyone who joins can read it. Worked example: a Staff category that denies View Channel to @everyone and allows it only to the Moderator and Admin roles.

**Impact:** People without an allowed role see nothing in that category.

**Audit:** Right-click the category > Edit Category > Permissions: Private Category is on, @everyone has View Channel denied, and the allowed roles match Appendix B. API: the category's @everyone overwrite denies View Channel (the companion audit skill computes this).

**Remediation:** Turn Private Category on, then add only the roles that need access.

**References:** S2, Judgment

### 6.2 Keep every channel in a private category synced to it

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3, AC-6

**Description:** A channel can carry its own permission settings that differ from its category's.

**Rationale:** A channel that allows @everyone to view, inside a private category, is open to the public. This is the most common leak.

**Impact:** The channel loses any custom permissions it had.

**Audit:** For each channel in the category: Edit Channel > Permissions shows it is synced. API: no channel in a private category lets @everyone view it.

**Remediation:** Choose Sync now, or remove the @everyone override.

**References:** S2, Judgment

### 6.3 Do not open private areas through a broad role

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3, AC-6

**Description:** Any role that every member receives, such as Member, Verified, or an onboarding role, must not have View Channel on a private category.

**Rationale:** Granting access to a role everyone gets is the same as granting it to everyone.

**Impact:** None.

**Audit:** Use View Server as Role for each broad role and confirm the private area is hidden. API: for each role that onboarding or a bot assigns automatically, the effective visibility of the private category is hidden.

**Remediation:** Remove the overwrite, and grant access only through roles staff assign by hand.

**References:** S2, Judgment

### 6.4 Keep channel overrides few and consistent

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** AC-3

**Description:** Channel overrides add exceptions to role permissions. Discord notes that when a member has several roles with conflicting overrides, the permission is allowed.

**Rationale:** Discord advises using as few overrides as possible. A Muted role that denies Send Messages does not stop a member whose other role allows it.

**Impact:** Staff must manage fewer, broader rules.

**Audit:** Review overrides on each private channel. Muting works as intended for members who hold other roles.

**Remediation:** Prefer Timeout Members for muting. If a Muted role is used, also deny View Channel or add a member-level override.

**References:** S2

### 6.5 Restrict staff, log, and alert channels to staff

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3, AU-9

**Description:** Channels used for moderation chat, audit logs, AutoMod alerts, and Activity Alerts.

**Rationale:** They reveal how you detect abuse and contain member data.

**Impact:** None.

**Audit:** Each such channel is hidden from @everyone and from member roles, and visible to staff only.

**Remediation:** Make the category private (control 6.1).

**References:** S4, S6, S10

### 6.6 Mark mature channels age-restricted

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3

**Description:** An age-restricted channel requires members to confirm they are adults.

**Rationale:** Discord requires channels with explicit content to carry the label, and age-restricted channels are exempt from the explicit media filter.

**Impact:** Members must confirm their age.

**Audit:** Channel Settings > Overview: Age-Restricted is on wherever mature content is allowed. API: the channel's nsfw flag.

**Remediation:** Turn it on.

**References:** S7, S10, S1

### 6.7 Set slowmode on busy public channels

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** SC-5

**Description:** Slowmode limits how often one member can post.

**Rationale:** SEAL suggests about 5 to 30 seconds on busy channels to blunt flooding.

**Impact:** Members wait between messages. Moderators with Bypass Slowmode are unaffected.

**Audit:** Channel Settings > Overview > Slow Mode is set on busy public channels. API: the channel's rate limit per user is above zero.

**Remediation:** Set a value in the range above.

**References:** S10

### 6.8 Configure thread, event, and stage permissions

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6

**Description:** Who can create public or private threads, send messages in threads, manage threads, create or manage events, and request to speak on stage.

**Rationale:** SEAL asks admins to set these deliberately, for example who can start or join a thread.

**Impact:** Members may need a role to start threads or events.

**Audit:** Create Public Threads, Create Private Threads, Manage Threads, Create Events, Manage Events, and stage permissions are reviewed per role against Appendix A.

**Remediation:** Remove management permissions from member roles.

**References:** S10

### 6.9 Restrict voice moderation permissions

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6

**Description:** Mute Members, Deafen Members, Move Members, and Priority Speaker affect other people in voice channels.

**Rationale:** Discord's Moderator Academy lists them as for moderators and trusted users. Connect, Speak, Video, and Use Voice Activity are generally safe for everyone.

**Impact:** Members cannot mute or move others.

**Audit:** Only moderator and admin roles hold those four permissions.

**Remediation:** Remove them from other roles.

**References:** S2

### 6.10 Review followed channels

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** AC-4

**Description:** Followed announcement channels from other servers post into yours.

**Rationale:** Discord's Moderator Academy lists followed channels under Integrations. Unknown follows can bring in unwanted content.

**Impact:** None.

**Audit:** Server Settings > Integrations lists only follows you expect.

**Remediation:** Remove the rest.

**References:** S2

### 6.11 Organize channels into categories

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** CM-6

**Description:** SEAL suggests grouping channels, for example Information, General, Voice Channels, and Topic-Specific.

**Rationale:** Permissions are set per category, so a clear structure makes them easier to review and keeps private channels from being scattered.

**Impact:** None.

**Audit:** Every channel belongs to a category.

**Remediation:** Group channels and apply permissions at the category.

**References:** S10

### 6.12 Keep an access list for each private area

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** AC-2, AC-6(7)

**Description:** A short record of which roles may enter each private category and why.

**Rationale:** Without it, nobody can tell whether a role should still have access.

**Impact:** Staff time.

**Audit:** The record exists and matches the actual category permissions. Review monthly.

**Remediation:** Write it, using Appendix B as the template.

**References:** Judgment, C1

### 6.13 Archive or delete inactive channels

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CM-8

**Description:** Channels no longer in use.

**Rationale:** SEAL recommends removing or archiving inactive channels twice a year. Old channels keep old permissions.

**Impact:** Old content becomes unavailable.

**Audit:** No channel has been inactive for six months without a stated reason.

**Remediation:** Archive the content elsewhere, then delete the channel.

**References:** S10

## 7 Integrations, bots, and webhooks

Apps, bots, and webhooks act with their own permissions and are common points of compromise. Everything in this section is managed under Server Settings > Integrations.

### 7.1 Keep an inventory of apps, each with an owner and purpose

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CM-8, SA-9

**Description:** A list of every bot and app installed, who is responsible for it, and why it is there.

**Rationale:** SEAL advises regularly reviewing Integrations for new apps and link shorteners. You cannot review what you have not listed.

**Impact:** Staff time.

**Audit:** Server Settings > Integrations lists nothing that is not on the inventory. Review monthly.

**Remediation:** Build the list and remove anything without an owner.

**References:** S10, C1

### 7.2 Allow only verified apps

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** SA-9

**Description:** Verified apps have been checked by Discord.

**Rationale:** SEAL says to confirm that all bots and apps are verified, and Discord's Moderator Academy says to treat a bot's developers as trusted parties.

**Impact:** Some useful community bots are unverified. Record an exception and apply controls 7.3 to 7.5 strictly.

**Audit:** Each installed app shows as verified, or has a recorded exception.

**Remediation:** Replace or remove unverified apps.

**References:** S10, S2, C1

### 7.3 Give each bot only the permissions it uses

**Profile:** L1 · **Severity:** CAT I · **Assessment:** Automated · **Applies to:** Bot · **NIST 800-53:** AC-6, CM-7

**Description:** Apply least privilege to the bot's role and to its channel overrides.

**Rationale:** SEAL says bots should rarely need Administrator. A compromised token or developer account becomes full control of the server. If a bot does hold it, monitor the audit log closely or set alerts for admin actions.

**Impact:** Some bot features may stop until you add the specific permission they need.

**Audit:** Server Settings > Integrations > Manage (the app) > Roles and Members, and Channels, show only needed permissions. API: no role flagged as managed by an integration has Administrator without a recorded exception.

**Remediation:** Remove Administrator and unused permissions.

**References:** S10, S2

### 7.4 Keep each bot role to its bot and below human admin roles

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Bot · **NIST 800-53:** AC-6

**Description:** SEAL says the bot role should contain only the bot user.

**Rationale:** A human added to a bot role inherits its permissions. A bot role above an admin role can act on it.

**Impact:** None.

**Audit:** Server Settings > Roles: each bot role has one member, the bot, and sits below the admin role (control 5.7).

**Remediation:** Remove extra members and reorder.

**References:** S10

### 7.5 Restrict who can run sensitive bot commands

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Bot · **NIST 800-53:** AC-3

**Description:** Command permissions can be limited by role, member, and channel.

**Rationale:** SEAL says to restrict command permissions under Manage > Roles and Members, Channels, and Command Overrides.

**Impact:** Members cannot run restricted commands.

**Audit:** Administrative and moderation commands are limited to staff roles and staff channels.

**Remediation:** Set the overrides in the app's Manage screen.

**References:** S10

### 7.6 Remove inactive or unneeded apps

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CM-7

**Description:** Apps nobody uses.

**Rationale:** An unused app still holds permissions and can still be compromised.

**Impact:** None.

**Audit:** Every app on the inventory has been used recently.

**Remediation:** Uninstall it.

**References:** S10, C1

### 7.7 Control who can add apps

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CM-3, CM-5

**Description:** Per Discord's Moderator Academy, adding a bot requires Administrator or Manage Server.

**Rationale:** That makes control 5.3 the gate. Staff with those permissions need a rule that new apps are approved first.

**Impact:** Staff request approval before installing.

**Audit:** A written approval step exists, and recent additions in the audit log have a matching approval.

**Remediation:** Adopt the rule and check additions during the monthly review.

**References:** S2, Judgment, C1

### 7.8 Review and prune webhooks

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Admin · **NIST 800-53:** CM-7, AC-6

**Description:** Webhooks let outside services post into a channel.

**Rationale:** SEAL says to review and remove unnecessary webhooks and to reassess any with excessive reach. A leaked webhook URL lets anyone post as it.

**Impact:** Services that post through a removed webhook stop working.

**Audit:** Server Settings > Integrations > Webhooks lists only webhooks with a known owner. API: GET guild webhooks (needs Manage Webhooks) matches the inventory.

**Remediation:** Delete the rest, and keep Manage Webhooks to as few roles as possible (control 5.3).

**References:** S10

### 7.9 Treat tokens and webhook URLs as secrets

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin, Bot · **NIST 800-53:** IA-5

**Description:** A bot token or webhook URL is a credential.

**Rationale:** Anyone holding it can act as the bot or webhook. Discord messages and repositories are common places for them to leak.

**Impact:** Rotation requires updating the service that uses it.

**Audit:** Secrets are in a secrets store, not in chat or code. They were rotated when staff with access left.

**Remediation:** Reset the token or recreate the webhook if exposed, and on staff changes.

**References:** Judgment, C1

### 7.10 Stop role-granting bots from handing out privileged roles

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Bot · **NIST 800-53:** AC-6, AC-3

**Description:** Reaction-role and verification bots assign roles on request.

**Rationale:** A bot with Manage Roles can assign any role below its own. If its role is above a privileged role, a member can reach it by clicking.

**Impact:** None.

**Audit:** The bot's role sits below every role that grants private access or staff powers. Test with a normal account.

**Remediation:** Move the bot role down (control 5.7).

**References:** S10, Judgment

### 7.11 Use logging and protection bots from reputable sources

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** SI-4, AU-2

**Description:** SEAL describes categories: anti-impersonation, anti-raid, anti-nuke (watching channel, role, ban, and webhook changes), elevated-permission and link allowlisting, and general moderation and logging.

**Rationale:** They add detection Discord does not provide on its own, such as alerts on bulk admin actions.

**Impact:** Each bot is another app to trust, so controls 7.1 to 7.3 apply to it. SEAL advises reviewing a bot's permissions after significant updates.

**Audit:** At least logging and anti-nuke coverage exists, or the gap is recorded.

**Remediation:** Choose bots with frequent updates and a good track record, then configure them with least privilege.

**References:** S10

### 7.12 Review linked platform integrations

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CM-7

**Description:** Discord's Moderator Academy lists platform integrations such as Twitch and YouTube under Integrations.

**Rationale:** Linked services can post or grant roles on their own rules.

**Impact:** Linked features stop.

**Audit:** Every linked platform integration is in use and has a known owner.

**Remediation:** Remove the rest.

**References:** S2

## 8 Community features, onboarding, and rules

Community is an optional server mode that unlocks AutoMod rule editing, Server Insights, onboarding, and announcement channels. These controls set it up safely and make sure the way new members are welcomed does not hand out access.

### 8.1 Enable Community with its required safety settings

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** PL-4, CM-6

**Description:** Discord requires a Community server to have a verification level that needs a verified email, the explicit media filter scanning all members, a rules or guidelines channel, and a Community updates channel for moderators.

**Rationale:** Several controls in this benchmark (AutoMod rules, Insights, onboarding) depend on Community. Discord notes that once Community is on, the Welcome Screen can no longer be edited and onboarding is preferred.

**Impact:** The server must follow Discord's Community Guidelines and keep those settings on. Discovery becomes possible, so also apply control 2.8.

**Audit:** Server Settings > Overview shows Community on, with the rules channel and updates channel chosen. API: features includes COMMUNITY and the rules and public updates channel ids are set.

**Remediation:** Server Settings > Enable Community > Get Started > Finish Setup, then choose the channels under Overview.

**References:** S7

### 8.2 Publish rules that cover behavior, content, moderation, and appeals

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** PL-4, AT-2

**Description:** A rules channel stating what is expected and what staff may do.

**Rationale:** SEAL recommends rules covering Behavior, Content, Moderation Actions, and an Appeals Process. Clear rules make moderation defensible and consistent.

**Impact:** None.

**Audit:** The rules channel exists, is readable by everyone, and covers the four topics.

**Remediation:** Write and pin the rules. Where members can see non-public information, add confidentiality terms.

**References:** S10, S7, C1

### 8.3 Require new members to accept the rules

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** PL-4

**Description:** Rules screening makes members agree to the rules before they can take part.

**Rationale:** SEAL recommends using Discord's rules screening. It records that members were told the terms.

**Impact:** One extra step on joining.

**Audit:** Joining the server with a test account shows a rules acceptance step before chat is available.

**Remediation:** Enable rules screening in the server's Community settings.

**References:** S10

### 8.4 Make sure onboarding cannot assign privileged or private-area roles

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-6, AC-3

**Description:** Community Onboarding lets new members choose roles and channels by answering questions.

**Rationale:** Anything a member can pick for themselves is a role anyone can get. A restricted-area or staff role offered as an answer defeats control 6.1.

**Impact:** Access to restricted areas must be granted by staff.

**Audit:** Server Settings > Onboarding: no answer assigns a role with elevated permissions or access to a private category. API: GET guild onboarding, then compare each option's role ids with Appendix B.

**Remediation:** Remove those options, and grant restricted-area roles by hand after approval.

**References:** S7, Judgment

### 8.5 Gate new members with a verification step

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** AC-2, SI-4

**Description:** A verification bot or Discord's built-in CAPTCHA that new members must complete before seeing channels. SEAL describes filtering by account age and profile picture, and timing out members who do not finish.

**Rationale:** Adds a check that automated accounts fail.

**Impact:** New members take an extra step. The verification bot becomes a trusted app, so controls 7.1 to 7.3 apply.

**Audit:** A test account cannot see channels until it completes the step.

**Remediation:** Configure the bot or built-in tool, and grant the member role only on completion.

**References:** S10

### 8.6 Enable Server Insights and watch for anomalies

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** SI-4, AU-6

**Description:** Insights show joins, activity, and retention.

**Rationale:** SEAL recommends enabling Insights and using the data to guide moderation. Sudden join spikes are often the first sign of a raid.

**Impact:** Requires Community.

**Audit:** Server Settings > Server Insights is available, and someone with View Server Insights reviews it monthly.

**Remediation:** Enable Community, then review Insights during the monthly check.

**References:** S10, S7

### 8.7 Keep the Community updates channel staff-only

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AC-3

**Description:** Discord posts server updates for admins and moderators in this channel.

**Rationale:** Discord describes it as a moderator-only channel. It can include safety and policy notices not meant for members.

**Impact:** None.

**Audit:** The channel is hidden from @everyone and visible to staff roles.

**Remediation:** Set permissions as in control 6.5.

**References:** S7

## 9 Logging, monitoring, and periodic review

These controls make sure changes are recorded, someone looks at the record, and the whole baseline is re-checked on a schedule.

### 9.1 Limit who can view the audit log

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Automated · **Applies to:** Server · **NIST 800-53:** AU-9, AC-6

**Description:** The View Audit Log permission shows who changed what on the server. Entries cannot be edited or deleted by members.

**Rationale:** SEAL gives it to admin and moderator roles only. The log reveals staff actions and the structure of your defenses.

**Impact:** Only staff can read the log.

**Audit:** Server Settings > Roles: View Audit Log is on for staff roles and off for @everyone and member roles. API: check the role permission bit.

**Remediation:** Remove it from other roles.

**References:** S10, S2

### 9.2 Review the audit log monthly

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** AU-6, AU-11

**Description:** Discord keeps audit log entries for 45 days. It records administrative actions including server updates, channel and permission changes, bans and kicks, role changes, invites, webhooks, app additions, message deletions, AutoMod changes, and onboarding changes.

**Rationale:** Role grants, new webhooks, new apps, and permission changes are the footprints of an attacker or a careless admin. After 45 days they are gone, so the review must be more frequent than that.

**Impact:** Staff time.

**Audit:** Server Settings > Audit Log: the reviewer can show the last review date and notes. Filter by action type for role updates, channel overwrites, webhook creation, bot additions, and bans.

**Remediation:** Add the review to the monthly schedule (control 9.7).

**References:** S8, S2, S10, C1

### 9.3 Create a private logging channel

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Server · **NIST 800-53:** AU-2, AU-6

**Description:** A channel visible only to admins and moderators that receives log output.

**Rationale:** SEAL recommends a private logging channel. It gives staff one place to watch.

**Impact:** None.

**Audit:** The channel exists and is hidden from members (control 6.5).

**Remediation:** Create it in a private category.

**References:** S10

### 9.4 Keep logs longer than 45 days

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** AU-4, AU-11, AU-9

**Description:** A logging bot copies events to the private channel or to storage outside Discord.

**Rationale:** The built-in audit log expires after 45 days. SEAL names logging bots such as Logger and Dyno as examples.

**Impact:** The logging bot is a trusted app, so controls 7.1 to 7.3 apply. Logs may contain member data.

**Audit:** Entries older than 45 days can be produced for a past change.

**Remediation:** Configure a logging bot and export on a schedule.

**References:** S8, S10

### 9.5 Alert on high-risk changes

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** SI-4, AU-6(1)

**Description:** Immediate notice when an Administrator grant, a role permission change, a new webhook, a new app, a channel permission change, or a mass ban happens.

**Rationale:** SEAL says that if a bot holds Administrator, monitor the audit log closely or set alerts in a private channel for admin actions. Anti-nuke bots watch channel and role creation and deletion, bans and kicks, and webhook changes.

**Impact:** Alert noise until tuned.

**Audit:** Change a test role permission and confirm an alert arrives in the private channel.

**Remediation:** Configure a logging or anti-nuke bot, or review the log daily.

**References:** S10

### 9.6 Keep an incident response plan

**Profile:** L1 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner, Admin · **NIST 800-53:** IR-1, IR-4, IR-6

**Description:** A written list of who does what when the server is attacked or an account is compromised.

**Rationale:** Speed matters, and people act badly under pressure without a list.

**Impact:** Staff time to write and rehearse.

**Audit:** The plan names the people allowed to act and covers: pausing invites and DMs (control 3.12); removing a compromised account's roles; resetting bot tokens and webhooks; reviewing the audit log; recovering control through the cold admin account (section 10); and reporting scams and abuse to Discord Trust and Safety.

**Remediation:** Write it, store a copy outside Discord, and rehearse it once a year.

**References:** S4, S10, S11, C1

### 9.7 Re-check the baseline on a schedule

**Profile:** L1 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CA-7, AC-6(7)

**Description:** SEAL gives a review cadence: monthly for roles and permissions, quarterly for rules, and twice a year for channels and roles. This benchmark extends it to the rest.

| Frequency | Review | Controls |
| --- | --- | --- |
| Monthly | Role permissions and members, invites, audit log, apps and webhooks, AutoMod rules, private-area access lists | 5.9, 2.5, 9.2, 7.1, 7.8, 4.10, 6.12 |
| Quarterly | Rules and announcement of changes, staff devices and authorized apps | 8.2, 1.6, 1.7 |
| Twice a year | Inactive channels, unused roles | 6.13, 5.10 |
| Yearly | Staff phishing briefing, incident plan rehearsal | 1.11, 9.6 |

**Rationale:** Settings drift. Without a schedule, a baseline decays.

**Impact:** Staff time.

**Audit:** Each review has a dated record.

**Remediation:** Put the dates in a shared calendar.

**References:** S10, Judgment

### 9.8 Record audit results

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CA-2, CA-7

**Description:** For each audit, a record of the result of every control (Pass, Fail, Not applicable), the date, the reviewer, and the fix date for failures.

**Rationale:** It shows the baseline is applied and lets the next reviewer see what changed.

**Impact:** Staff time.

**Audit:** The last audit record exists and every Fail has an owner.

**Remediation:** Keep the record with the incident plan, outside Discord.

**References:** Judgment

### 9.9 Report abusive messages before deleting them

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Moderator · **NIST 800-53:** IR-4, AU-11

**Description:** Discord's Moderator Academy says deleted messages cannot be recovered, even by Discord, and cannot serve as evidence of a terms violation unless reported first.

**Rationale:** A scam or abuse report needs the original message.

**Impact:** A few extra seconds per incident.

**Audit:** Moderators can describe the report-then-delete step.

**Remediation:** Brief moderators and add it to the incident plan.

**References:** S2

## 10 Owner continuity and recovery

These controls follow SEAL's cold admin approach: the server owner is a dedicated account that stays out of daily use and exists to regain control after an incident. All five are L2.

### 10.1 Create a dedicated cold admin account

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner · **NIST 800-53:** AC-6(5), CP-2

**Description:** A separate Discord account, created on a factory-reset device with no other use, that holds ownership of the server.

**Rationale:** If every day-to-day admin account is compromised, the cold account can still remove them and recover. Using it rarely keeps it from being phished.

**Impact:** One extra device and two accounts to maintain.

**Audit:** The account and its email exist and meet SEAL's setup: a new email address created in a private browser window without a VPN, MFA turned on immediately with an authenticator app or security key, no phone number, and Skip password when possible turned off. Backup codes are written on paper. The Discord account uses a username unrelated to the server or organization and a profile picture, has DMs from server members off, spam filter at maximum, status Invisible, and MFA on. All credentials are recorded offline.

**Remediation:** Create the accounts in that order and record the details offline.

**References:** S10, S9

### 10.2 Introduce the cold admin to the server gradually

**Profile:** L2 · **Severity:** CAT II · **Assessment:** Manual · **Applies to:** Owner · **NIST 800-53:** AC-2, CP-2

**Description:** Before ownership moves, the cold admin joins and acts like a normal staff account.

**Rationale:** SEAL does it in steps so that mistakes are found while the current owner can still fix them.

**Impact:** The process takes at least a day.

**Audit:** The cold admin joined through a friend request and the normal verification, holds the highest admin role, and has posted in a private team chat on two separate days.

**Remediation:** Send a friend request from your personal account, have the cold admin join and complete verification, assign the highest admin role, post a few messages, wait about 24 hours, post a few more, then proceed to control 10.3.

**References:** S10

### 10.3 Transfer ownership with checks

**Profile:** L2 · **Severity:** CAT I · **Assessment:** Manual · **Applies to:** Owner · **NIST 800-53:** AC-2, CP-2

**Description:** Ownership transfer is done from the server's member list and needs the current owner's 2FA code.

**Rationale:** SEAL warns that a transfer to the wrong account is very hard to recover from.

**Impact:** The previous owner becomes an ordinary admin.

**Audit:** The cold admin account owns the server. Server Settings > Members shows it as Server Owner.

**Remediation:** Server Settings > Members, search for the cold admin, open the three-dot menu, choose Transfer Ownership, enter the 2FA code, and confirm. Check the recipient's name three times before confirming.

**References:** S10

### 10.4 Use the cold admin only for owner-only work, and log in monthly

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Owner · **NIST 800-53:** AC-2(3), CP-2

**Description:** The account is used to add or invite bots and for owner-only changes, and during an incident to regain control.

**Rationale:** SEAL tells owners to sign in to both the email and Discord at least monthly so the accounts are not disabled for inactivity and so problems are noticed.

**Impact:** A calendar reminder.

**Audit:** The last login to both accounts is within the last month, and the audit log shows no daily activity from the cold admin.

**Remediation:** Set a phone reminder for the monthly login.

**References:** S10

### 10.5 Back up the server configuration off-platform

**Profile:** L2 · **Severity:** CAT III · **Assessment:** Manual · **Applies to:** Admin · **NIST 800-53:** CP-9, CP-10

**Description:** A backup of channels, roles, and permissions that can rebuild the server if it is deleted or vandalized.

**Rationale:** SEAL recommends a backup bot and storing backups securely outside Discord.

**Impact:** The backup bot is a trusted app with broad permissions, so controls 7.1 to 7.3 apply. A configuration backup does not necessarily restore members or messages.

**Audit:** A backup less than a month old exists outside Discord, and a restore has been tried on a test server.

**Remediation:** Configure the backup, store it securely, and test a restore.

**References:** S10

## 11 Member account settings

These settings belong to each person's own Discord account, not to the server. The server owner cannot enforce them, so this section is a checklist to give to every owner, admin, and moderator (and, as advice, to general members). Settings are under User Settings. Menu names change between app versions, so confirm against the current app.

| ID | Setting (where) | Recommended value | Profile | Applies to | Source |
| --- | --- | --- | --- | --- | --- |
| 11.1 | Two-factor authentication (My Account > Authentication app) | On, using an authenticator app or security key. Save backup codes offline | L1 | Staff; advised for all | S11, S10 |
| 11.2 | Strong, unique password (My Account) | Password manager, not reused anywhere else | L1 | All | S11 |
| 11.3 | Direct messages from server members (Privacy & Safety > Server privacy defaults) | Off for staff and the cold admin; off for new servers for everyone else | L1 | Staff | S10, S12 |
| 11.4 | Message requests (Privacy & Safety) | On, so DMs from non-friends go to a separate inbox | L1 | All | S12, S11 |
| 11.5 | Safe direct messaging / explicit image filter (Privacy & Safety) | Filter all direct messages. Label the exact toggle as shown in the app; SEAL's wording about members "I may not know" is ambiguous | L1 | All | S10, S12 |
| 11.6 | Who can send friend requests (Privacy & Safety > Friend requests) | Friends of friends and server members at most; none for the cold admin | L2 | Staff | S12, S10 |
| 11.7 | Streamer mode (Streamer Mode) | On when streaming or recording, so email, invite links, and tokens are hidden | L2 | Owner, Admin | S12 |
| 11.8 | Data use settings (Privacy & Safety) | Turn off personalization and activity data sharing you do not need | L2 | All | S12 |
| 11.9 | Connections (Connections) | Remove linked accounts you do not use; do not display them on the profile | L2 | Staff | S12, S11 |
| 11.10 | Authorized apps (Authorized Apps) | Review and revoke apps you no longer use | L1 | All | S11, S10 |
| 11.11 | Active sessions and devices (Devices) | Log out devices you do not recognize; change the password if any look wrong | L1 | Staff | S11 |
| 11.12 | Family Center (Family Center) | Available to parents of teen accounts for activity reports; optional | L2 | Parents | S12 |
| 11.13 | Staff display and status (Profile, Activity Status) | Staff avoid sharing personal details, location, or real name in their server profile | L2 | Staff | S10, S12 |

**Note:** Rows 11.1, 11.3, 11.5, and 11.6 repeat or extend controls in Section 1. Where they differ, the Section 1 control is the one to audit. This section exists so staff can work through one checklist.

## Appendix A Permission reference

This appendix lists server permissions by risk tier, with the recommended holders. It is written from the author's knowledge of Discord and from S2; **verify the list against the current Discord developer documentation before relying on it**, because Discord adds and renames permissions. Permission names below follow the app's wording.

**Tier definitions**

- **Critical:** can take over or destroy the server, or bypass every other control.
- **High:** can remove people, change who has access, or alter the server's configuration.
- **Moderate:** can disrupt conversation or reach many people at once.
- **Low:** everyday participation.

| Permission | Tier | Recommended holders | Notes |
| --- | --- | --- | --- |
| Administrator | Critical | Owner and cold admin only; never bots, never @everyone | Bypasses all channel overrides. Basis of controls in Section 5 |
| Manage Server | Critical | Owner and Admin | Changes server settings, invites, and adds bots |
| Manage Roles | Critical | Owner and Admin | Can grant roles lower than its own, so check role order |
| Manage Channels | High | Admin | Can change channel overrides, which can expose private areas |
| Manage Webhooks | High | Admin | A webhook can post as anything; see Section 7 |
| Manage Expressions | Moderate | Admin, Moderator (optional) | Emoji, stickers, and soundboard |
| View Audit Log | High | Admin, Moderator (L2: Admin only) | Reading the log reveals staff actions |
| View Server Insights | Low | Admin | Analytics |
| Ban Members | High | Admin, senior Moderator | Not for general members |
| Kick Members | High | Moderator |  |
| Moderate Members (timeout) | Moderate | Moderator | Preferred over kick or ban for first offenses |
| Manage Messages | Moderate | Moderator | Can delete and pin others' messages |
| Manage Threads | Moderate | Moderator |  |
| Mention @everyone, @here, and All Roles | Moderate | Staff only | Should be off for @everyone; see controls 3.2 and 5.12 |
| Create Invite | Moderate | Staff, or trusted role only | Off for @everyone on a private server |
| Manage Nicknames | Low | Moderator |  |
| Change Nickname | Low | Members |  |
| Create Public/Private Threads | Low | Members, if wanted |  |
| Embed Links, Attach Files | Low to Moderate | Members, but see AutoMod in Section 4 | Common route for scam links and files |
| Use External Emoji, Stickers | Low | Members |  |
| Add Reactions | Low | Members |  |
| Send Messages, Read Message History, View Channel | Low | Members, per channel | The basis of private-area design, Section 6 |
| Connect, Speak, Video | Low | Members, per voice channel |  |
| Mute, Deafen, Move Members | Moderate | Moderator | Voice moderation |
| Priority Speaker | Low | Staff |  |
| Use Application Commands | Low | Members, restrict per channel if bots are noisy |  |
| Request to Speak, Create Events | Low | Members, staff |  |

**Permissions that require two-factor authentication.** Discord lets a server require 2FA for moderators, and when it is on, certain permissions do not work for roles whose holders lack 2FA. S2 lists seven; the list may be outdated. See control 3.1 for the setting and verify the current list in the app.

## Appendix B Reference role model

This is an example role layout that meets the L1 and L2 controls in Sections 5, 6, and 8. It includes one restricted area, a category that only approved people can see. The shape applies to any server: rename the roles to suit. Role order matters, because a role can only manage roles below it (control 5.7), so the list runs from highest to lowest.

**Role order, highest first**

| Order | Role | Purpose | Permissions (beyond @everyone) | Who |
| --- | --- | --- | --- | --- |
| 1 | Cold Admin | Owner account; recovery only (Section 10) | Owner, so all | One account |
| 2 | Admin | Runs the server | Manage Server, Manage Roles, Manage Channels, Manage Webhooks, View Audit Log, Ban Members. Prefer these over Administrator (controls 5.2, 5.3) | Two or three trusted people |
| 3 | Bots | Shared tier for installed apps, or one role per bot | Only what each bot uses (control 7.3) | Apps |
| 4 | Moderator | Day-to-day moderation | Per control 5.5: Moderate Members, Kick Members, Manage Messages, Manage Threads, View Audit Log (L2: remove) | Vetted volunteers or staff |
| 5 | Contributor | People who work in the restricted area | View and post in the restricted area; Manage Messages and Manage Threads inside that category only | People the organization approves |
| 6 | Guest | Limited access to selected channels in the restricted area | View, Send Messages, Attach Files, and Read History in the selected channels only | Invited people, ideally time-limited |
| 7 | Member (optional) | Verified community members | Per control 5.6 | Everyone who passes verification |
| 8 | @everyone | Baseline | Per control 5.1 | Everyone |

**Role and channel matrix**

| Area | @everyone | Member | Guest | Contributor | Moderator | Admin |
| --- | --- | --- | --- | --- | --- | --- |
| Rules, announcements | Read | Read | Read | Read | Manage | Manage |
| General chat | No (L2) or read and post | Read and post | Read and post | Read and post | Moderate | Manage |
| Restricted category (parent) | Hidden | Hidden | Hidden unless granted per channel | See all | Hidden unless needed | See all |
| Restricted area, shared channels | Hidden | Hidden | Read and post | Read and post | Moderate | Manage |
| Restricted area, working channels | Hidden | Hidden | Hidden | Read and post | Hidden unless needed | Manage |
| Staff, mod-log, alerts | Hidden | Hidden | Hidden | Hidden | Read and post | Manage |

**Notes**

- Hide the restricted category from @everyone and from Member, then allow View Channel only for Contributor and Guest (control 6.1). Keep every channel in the category synced to it (6.2) and never give a general role such as Member access (6.3). An allow on any role a person holds wins over a deny on another, per S2.
- Do not assign Contributor or Guest through onboarding or reaction roles (control 8.4). Staff grant them by hand and record the grant in the access list (6.12).
- Test with View Server as Role for @everyone, Member, and Guest after every change (5.14).
- A person who holds both Guest and Contributor gets Contributor's access. Remove Guest when someone becomes a Contributor.
- Size the restricted area to the server's impact level (see Conventions). A Low impact server may have none.

## Appendix C Audit data map

This table maps each Automated control to the Discord REST API data that answers it and the condition that passes. Field names come from the author's knowledge of the API and **have not been checked against the current developer documentation**; confirm before building tooling on them. The `discord-server-audit` skill reads these endpoints with a read-only bot: guild (`GET /guilds/{id}?with_counts=true`), roles, channels, and, if the bot has Manage Server, invites and onboarding. Role membership is not read, because that needs a privileged intent, so controls that depend on who holds a role (5.2, 5.8, 5.11) stay Manual in practice.

| Control | Data source | Pass condition |
| --- | --- | --- |
| 2.1 | guild.verification\_level | 2 (Medium) or higher |
| 2.2 | guild.verification\_level | 4 (Highest) |
| 2.3 | guild.explicit\_content\_filter | 2 (all members) |
| 2.4 | invites\[\].max\_age, max\_uses | No invite with max\_age 0 (never expires), unless intended; uses limited for one-off invites |
| 2.5 | invites\[\] | Every invite has a known creator and purpose; none unused or stale |
| 2.6 | roles\[@everyone\].permissions | Create Instant Invite bit (0x1) clear |
| 2.7 | guild.features | VANITY\_URL absent on a private server |
| 2.8 | guild.features | DISCOVERABLE absent unless public listing is intended |
| 2.9 | guild.widget\_enabled (or the widget endpoint) | false |
| 3.1 | guild.mfa\_level | 1 (2FA required for moderation) |
| 3.2 | roles\[@everyone\].permissions | None of Administrator, Manage Server/Roles/Channels/Webhooks/Messages, Kick, Ban, Mention Everyone set |
| 3.5 | guild.safety\_alerts\_channel\_id | Set, and that channel is not visible to @everyone |
| 3.14 | guild.default\_message\_notifications | 1 (only @mentions) |
| 5.1 | roles\[@everyone\].permissions | Only the baseline set in control 5.1 |
| 5.2 | roles\[\].permissions | Administrator on one role at most; no bot role; no @everyone |
| 5.3 | roles\[\].permissions | Manage Server/Roles/Channels/Webhooks only on Admin-tier roles |
| 5.4 | roles\[\].permissions | Kick and Ban only on Admin or Moderator roles |
| 5.5, 5.6 | roles\[\].permissions | Role permissions equal the recommended sets |
| 5.7 | roles\[\].position | Privileged roles sit above less privileged ones; bot roles below human admin roles |
| 5.10 | roles\[\] | No empty or duplicate roles; no unused role with permissions |
| 5.12 | roles\[\].mentionable and Mention Everyone | Powerful roles not mentionable by members |
| 6.1 | channels\[type 4\].permission\_overwrites | Restricted categories deny View Channel to @everyone |
| 6.2 | channels\[\].permission\_overwrites vs parent | Channels inside a private category match it; none opens View Channel to @everyone or a general role |
| 6.3 | channels\[\].permission\_overwrites | No broad role (Member, Verified, default) holds View Channel on private areas |
| 6.5 | channels\[\].permission\_overwrites | Staff, log, and alert channels hidden from non-staff roles |
| 6.6 | channels\[\].nsfw | true on mature channels |
| 6.7 | channels\[\].rate\_limit\_per\_user | Greater than 0 on busy public channels |
| 6.8, 6.9 | channels\[\].permission\_overwrites | Thread, event, stage, and voice moderation allowed to staff only |
| 7.3 | roles\[\] with managed = true | Bot roles hold only permissions the bot needs; none with Administrator |
| 7.8 | `GET /guilds/{id}/webhooks` (not read by the skill; needs Manage Webhooks) | Every webhook has a known owner and purpose |
| 8.1 | guild.features, rules\_channel\_id, public\_updates\_channel\_id | COMMUNITY present with a rules channel and updates channel set |
| 8.4 | onboarding.prompts\[\].options\[\].role\_ids | No option assigns a privileged or private-area role |
| 8.7 | channels\[public\_updates\_channel\_id\].permission\_overwrites | Hidden from @everyone, or intentionally public |
| 9.1 | roles\[\].permissions | View Audit Log held only by Admin-tier roles (and Moderator at L1) |

**Combined effective-permission check.** For any control about who can see a channel, compute the result in this order: Administrator on any role grants everything; otherwise start with @everyone plus the member's roles, then apply the channel's @everyone overwrite, then role overwrites, then member overwrites. Allows win over denies among role overwrites (S2). `analyze.py` in the audit skill implements this.

## Appendix D NIST 800-53 crosswalk

NIST SP 800-53 Rev. 5 does not describe Discord, so these mappings are the author's judgment about which control each recommendation supports. They are not an official or assessor-approved mapping. Three are loose fits and are marked in the controls themselves: 1.14 (AC-3), 2.3 (SI-3), and 4.5 (SI-8). L1 is aimed at the 800-53B Low baseline; L2 adds Moderate-style controls such as AC-6(5), AU-9, and CP-9. The table below was generated from the metadata line on each control, so the two agree.

| NIST 800-53 control | Family | Controls in this benchmark |
| --- | --- | --- |
| AC-2 | Access Control | 1.6, 2.1, 2.2, 2.4, 2.5, 5.10, 5.13, 6.12, 8.5, 10.2, 10.3 |
| AC-2(3) | Access Control | 1.15, 5.11, 10.4 |
| AC-3 | Access Control | 1.14, 2.4, 2.6, 2.7, 5.7, 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 7.5, 7.10, 8.4, 8.7 |
| AC-4 | Access Control | 6.10 |
| AC-6 | Access Control | 1.7, 2.6, 3.2, 3.3, 4.9, 5.1, 5.3, 5.4, 5.5, 5.6, 5.7, 5.12, 5.13, 6.1, 6.2, 6.3, 6.8, 6.9, 7.3, 7.4, 7.8, 7.10, 8.4, 9.1 |
| AC-6(2) | Access Control | 5.8 |
| AC-6(5) | Access Control | 5.2, 10.1 |
| AC-6(7) | Access Control | 5.9, 6.12, 9.7 |
| AC-11 | Access Control | 1.12 |
| AC-12 | Access Control | 1.6 |
| AC-22 | Access Control | 2.8, 2.9 |
| AT-2 | Awareness and Training | 1.11, 3.10, 4.10, 8.2 |
| AU-2 | Audit and Accountability | 7.11, 9.3 |
| AU-4 | Audit and Accountability | 9.4 |
| AU-6 | Audit and Accountability | 3.5, 4.7, 4.10, 8.6, 9.2, 9.3 |
| AU-6(1) | Audit and Accountability | 9.5 |
| AU-9 | Audit and Accountability | 6.5, 9.1, 9.4 |
| AU-11 | Audit and Accountability | 9.2, 9.4, 9.9 |
| CA-2 | Assessment, Authorization, and Monitoring | 5.14, 9.8 |
| CA-7 | Assessment, Authorization, and Monitoring | 9.7, 9.8 |
| CM-3 | Configuration Management | 5.9, 7.7 |
| CM-5 | Configuration Management | 7.7 |
| CM-6 | Configuration Management | 3.2, 3.14, 4.9, 5.1, 6.11, 8.1 |
| CM-7 | Configuration Management | 1.7, 1.8, 7.3, 7.6, 7.8, 7.12 |
| CM-8 | Configuration Management | 6.13, 7.1 |
| CP-2 | Contingency Planning | 1.3, 10.1, 10.2, 10.3, 10.4 |
| CP-9 | Contingency Planning | 10.5 |
| CP-10 | Contingency Planning | 10.5 |
| IA-2(1) | Identification and Authentication | 1.1, 1.2, 1.5, 3.1 |
| IA-2(2) | Identification and Authentication | 1.1 |
| IA-4 | Identification and Authentication | 4.4 |
| IA-5 | Identification and Authentication | 1.2, 1.3, 1.5, 7.9 |
| IA-5(1) | Identification and Authentication | 1.4 |
| IR-1 | Incident Response | 3.12, 9.6 |
| IR-4 | Incident Response | 3.7, 3.12, 3.13, 4.8, 9.6, 9.9 |
| IR-5 | Incident Response | 3.13 |
| IR-6 | Incident Response | 9.6 |
| PL-4 | Planning | 8.1, 8.2, 8.3 |
| PS-4 | Personnel Security | 5.11 |
| SA-9 | System and Services Acquisition | 7.1, 7.2 |
| SC-5 | System and Communications Protection | 2.1, 2.2, 3.6, 3.7, 3.14, 4.2, 6.7 |
| SC-8 | System and Communications Protection | 1.13 |
| SC-28 | System and Communications Protection | 1.13 |
| SI-3 | System and Information Integrity | 2.3 |
| SI-4 | System and Information Integrity | 3.4, 3.5, 3.6, 4.4, 4.7, 4.8, 7.11, 8.5, 8.6, 9.5 |
| SI-8 | System and Information Integrity | 1.9, 1.10, 2.10, 3.8, 3.9, 3.10, 3.11, 4.1, 4.2, 4.3, 4.5, 4.6 |

## Appendix E Sources

Each control cites these keys in its References line. "Judgment" means the recommendation is the author's own and no source states it directly.

| Key | Source | Used for |
| --- | --- | --- |
| S1 | [Discord, Four steps to a super safe server](https://discord.com/safety/360043653152-Four-steps-to-a-super-safe-server) | Verification level, explicit media filter, 2FA for moderation, Safety Setup basics |
| S2 | [Discord Moderator Academy, Permissions on Discord](https://discord.com/moderation/1500000176222-201-permissions-on-discord) | Permission meanings, role order, override resolution, permissions gated by 2FA |
| S3 | [Discord Support, Verification levels](https://support.discord.com/hc/en-us/articles/216679607) | Verification level definitions and impact |
| S4 | [Discord Support, Activity alerts and Security Actions](https://support.discord.com/hc/en-us/articles/17439993574167) | Safety Setup alerts, raid and DM protection, CAPTCHA |
| S5 | [Discord Support, Pause Invites](https://support.discord.com/hc/en-us/articles/8458903738647) | Invite controls during incidents |
| S6 | [Discord Support, AutoMod](https://support.discord.com/hc/articles/4421269296535) | AutoMod rule types and limits |
| S7 | [Discord Support, Enabling Community](https://support.discord.com/hc/en-us/articles/360047132851) | Community requirements, rules and updates channels |
| S8 | [Discord developer documentation, Audit Log](https://docs.discord.com/developers/resources/audit-log) | Audit log content and 45-day retention |
| S9 | [SEAL, Community management: Discord](https://frameworks.securityalliance.org/community-management/discord.html) | Server hardening, role and bot guidance |
| S10 | [SEAL, Discord account management guide](https://frameworks.securityalliance.org/guides/account-management/discord) | Cold admin procedure, account settings |
| S11 | [Discord, Securing your Discord account](https://discord.com/moderation/241-securing-your-discord-account) | MFA, password, devices, authorized apps |
| S12 | [SAFEGUARD Digital Identity Protection Toolkit, Discord (Feb 2025), hosted by U.S. Special Operations Command (soc.mil); issuing office not stated](https://www.soc.mil/IdM/publications/docs/socialMedia/Discord.pdf) | Member privacy and safety settings |
| S13 | Format models: [Chainguard, What are CIS Benchmarks](https://www.chainguard.dev/supply-chain-security-101/what-are-cis-benchmarks), [Cromwell, STIG compliance](https://cromwell-intl.com/open-source/stig-compliance/), [How to Harden Slack](https://howtoharden.com/guides/slack/) | Document structure: profile levels, CAT severity, check and fix, NIST mapping |
| Judgment | Author | Severity ratings, NIST mappings, level assignments, and recommendations with no direct source |

**U.S. government guidance.** CISA and NIST publish nothing specific to Discord. These documents support the controls that cite them where the same principle applies to any online account or platform. Each was read on October 28, 2026.

| Key | Source | Used for |
| --- | --- | --- |
| C1 | [CISA, Capacity Enhancement Guide: Social Media Account Protection (Dec 2021)](https://www.cisa.gov/sites/default/files/publications/CISA_CEG_Social_Media_Account_Protection_508.pdf) | Limiting and reviewing account access, MFA on administrator accounts, vetting third-party apps, securing tokens, monitoring alerts, incident response plan |
| C2 | [CISA, Implementing Phishing-Resistant MFA (Oct 2022)](https://www.cisa.gov/sites/default/files/publications/fact-sheet-implementing-phishing-resistant-mfa-508c.pdf) | Ranking of MFA methods; SMS and voice codes as last resort; prioritizing administrators |
| C3 | [CISA, Mobile Communications Best Practice Guidance (Dec 2024)](https://cisa.gov/sites/default/files/2024-12/guidance-mobile-communications-best-practices.pdf) | Dropping SMS as a second factor, password managers |
| N5 | [NIST SP 800-63B-4, Digital Identity Guidelines: Authentication and Authenticator Management (Jul 2025)](https://csrc.nist.gov/pubs/sp/800/63/b/4/final) | General authenticator and MFA requirements |
| N1 | [NIST SP 800-53 Rev. 5](https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final) | Control catalog used for the mappings in Appendix D |
| N2 | [NIST SP 800-53B](https://csrc.nist.gov/pubs/sp/800/53/b/upd1/final) | Low baseline that Level 1 is aimed at |
| N3 | [DISA STIGs](https://public.cyber.mil/stigs/) | Format model: CAT severity, check and fix |
| N4 | [CIS Benchmarks](https://www.cisecurity.org/cis-benchmarks) | Format model: profile levels, per-recommendation fields |

**What no source covers.** No CIS Benchmark or DISA STIG exists for Discord, and none of these sources claims alignment with them. This benchmark borrows their format only. It has not been reviewed or endorsed by CIS, DISA, NIST, or Discord.

## Appendix F Version history and open items

| Version | Date | Change |
| --- | --- | --- |
| 0.0.1 | 2026-10-28 | Initial document: summary tables of recommended settings |
| 0.1.0 (draft) | 2026-10-28 | Rebuilt in the CIS Benchmark and DISA STIG style: profile levels, CAT severity, per-control fields (Description, Rationale, Impact, Audit, Remediation, References), NIST 800-53 mapping, 109 controls in 10 sections, member checklist, and appendices A to F |

**Counts in this version.** 109 controls: 66 L1 and 43 L2; 12 CAT I, 45 CAT II, and 52 CAT III; 36 Automated and 73 Manual.

**Open items before 1.0**

- Verify Appendix A (permission list and the permissions that require 2FA) and Appendix C (API field names) against the current Discord developer documentation, which could not be fetched while writing.
- Check every menu path and setting name in the Discord app. Discord renames settings often.
- Confirm the SEAL "Moderate" versus Medium verification-level wording and the "members I may not know" DM filter label (control 1.9).
- Have a second person review the severity ratings and NIST mappings. Both are the author's judgment.
- Run the audit skill against a real server and note any control the data does not support.
- Decide whether to map each control ID to a finding in the `discord-server-audit` skill.

**Versioning.** Patch versions (0.1.x) fix wording or facts. Minor versions (0.x.0) add or change controls. Version 1.0.0 is reached once the open items are closed.
