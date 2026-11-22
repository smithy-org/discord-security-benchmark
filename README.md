# Discord Server Security Configuration Benchmark

A secure baseline for a Discord server. Every recommendation comes with a check to run and a fix to apply, so an owner can audit and configure a server without guessing.

**Version 0.1.3 (draft)** · 109 controls in 10 sections · by Peter Derouen · published by [Smithy](https://github.com/smithy-org)

## What's in it

- **Two profiles.** Level 1 (L1) is the baseline for every server. Level 2 (L2) is added for servers where a compromise would do real harm.
- **Severity.** CAT I, CAT II, CAT III, adapted for Discord from DISA STIG categories.
- **Per-control fields.** Description, Rationale, Impact, Audit, Remediation, Default value and References, in the same order every time so reviewers can scan or script them.
- **Assessment.** 36 controls can be checked through the Discord API with a read-only bot. 73 need someone to look in the app.
- **Mapping.** NIST SP 800-53 Rev. 5 crosswalk (the author's judgment).
- **Appendices.** Permission reference, reference role model, audit data map, NIST crosswalk, sources, version history.

The layout borrows from CIS Benchmarks and DISA STIGs. No CIS Benchmark or STIG exists for Discord, and this document has not been reviewed or endorsed by CIS, DISA, NIST or Discord.

## Read it

| File | Use it for |
| --- | --- |
| [`benchmark/discord-security-benchmark.html`](benchmark/discord-security-benchmark.html) | The typeset edition. Download it and open it in a browser. It loads fonts from Google Fonts and nothing else. |
| [`benchmark/discord-security-benchmark.md`](benchmark/discord-security-benchmark.md) | Plain Markdown. Readable on GitHub, easy to diff, and the one to edit. |

The two files hold the same content.

## Status and known gaps

This is a draft. Before 1.0 the open items in Appendix F need closing:

- Check the permission list (Appendix A) and API field names (Appendix C) against the current Discord developer documentation.
- Check every menu path and setting name in the Discord app. Discord renames settings often.
- Have a second person review the severity ratings and NIST mappings.
- Run an audit against a real server and note any control the data does not support.

Corrections are welcome. Open an issue or a pull request, and name the control ID (for example `3.2`).

## Licence

[Creative Commons Attribution-ShareAlike 4.0 International](LICENSE). Share and adapt it, including commercially, as long as you credit the author, say what you changed, and license adaptations under the same terms. Some recommendations are adapted from SEAL's Discord guides, which are also CC BY-SA 4.0; Appendix E lists them.
