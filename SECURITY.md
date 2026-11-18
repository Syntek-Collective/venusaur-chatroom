# Security Policy

## Reporting a Vulnerability

Please **do not** report security problems in public issues, discussions, or pull requests.

Report them privately through GitHub instead:

1. Go to the repository's [**Security** tab](https://github.com/Syntek-Collective/venusaur-chatroom/security).
2. Click **Report a vulnerability**.
3. Describe the problem, how to reproduce it, and what an attacker could do with it.

Only the maintainers can see your report. We'll reply as soon as we can, keep you updated while we work on a fix, and credit you when it's released unless you'd rather stay anonymous.

## Supported Versions

Only the latest code on the `main` branch receives security fixes.

## Scope

Areas we especially want to hear about:

- authentication and sessions (sign-up, login, cookies)
- access to the premium room and the Stripe webhook
- Socket.io events that let a user read or send messages in a room they shouldn't
- anything that exposes another user's data
