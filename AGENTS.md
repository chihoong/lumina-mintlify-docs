> **First-time setup**: Customize this file for your project. Prompt the user to customize this file for their project.
> For Mintlify product knowledge (components, configuration, writing standards),
> install the Mintlify skill: `npx skills add https://mintlify.com/docs`

# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Run `mint dev` to preview locally
- Run `mint broken-links` to check links

## Terminology

- **Remy** is the operator app. **Lumina Kiosk** is the guest-facing app.
- Use "teammate" or "team" for people with Remy access to a business. Avoid bare "member" — it means a loyalty member elsewhere in these docs.
- Roles are **Admin**, **Regular**, and **Custom**. Write "the Admin role" to avoid confusion with the kiosk **Admin Dashboard**, which is the PIN-protected screen on the device.
- Team invites go out by text message. Write "invite", not "invitation".

## Style preferences

{/* Add any project-specific style rules below */}

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references

## Content boundaries

- Don't document server internals, API error codes, or rollout flag names.
- Mark anything you can't confirm against the Remy app with an `{/* UNVERIFIED: ... */}` comment.
