# Source: https://docs.onlumina.com/kiosk-operations

# Kiosk operations

Updated May 20, 2026

Once a Kiosk is registered, you manage it day-to-day through **profiles** in Remy and a few controls on the device itself. This guide covers working with profiles, setting the welcome screen, and locking the Kiosk.

## [Profiles](https://docs.onlumina.com/#profiles)

A **profile** is a saved Kiosk configuration. Profiles let you:

- **Apply the same setup to multiple Kiosks** so every device runs an identical, consistent experience.
- **Keep multiple configurations on hand** and switch a device between them as needed (e.g. a lunch profile and a dinner profile, or a slimmed-down patio profile).

### [What a profile contains](https://docs.onlumina.com/#what-a-profile-contains)

A profile bundles every device-level setting in one place:

- **Welcome Experience** — welcome screen media (image or video), overlays, and the optional welcome notice. See [Welcome Screen](https://docs.onlumina.com/#welcome-screen) below.
- **Menu** — the menu the device shows guests, including any [Collections](https://docs.onlumina.com/menu-management/menu-management#collections) you've added.
- **Check Out experience** — order types (e.g. dine-in, takeout), guest inputs (name, phone, etc.), and the rest of the checkout settings.
- **Security settings** — the 4-digit unlock PIN that exits kiosk mode (see [Locking](https://docs.onlumina.com/#locking)).

### [Working with profiles in Remy](https://docs.onlumina.com/#working-with-profiles-in-remy)

1. Create a profile with **Create a New Profile** and give it a name that makes its purpose obvious (e.g. "Front counter", "Patio lunch", "Dinner service").
2. Configure each section — Welcome Experience, Menu, Check Out, Security — for that profile.
3. Assign the profile to a device during registration via **Select Profile** (see [Quick start → Register the Kiosk](https://docs.onlumina.com/quick-start/quick-start#register-the-kiosk)). Switching a device to a different profile later applies that profile's configuration to the Kiosk.
4. Use **Rename Profile** to keep names clear as you add more.

If you run multiple stations or locations, build one profile per setup and assign it to every Kiosk that should run that configuration. Editing the profile updates every device using it — no need to reconfigure each Kiosk by hand.

## [Welcome Screen](https://docs.onlumina.com/#welcome-screen)

The welcome screen is what guests see before they tap to start an order. You configure it in Remy under **Welcome Experience**.

1. Open the profile's **Welcome Experience** screen.
2. Tap **Choose image or video for welcome screen** to upload your media. Remy's guidance: _"Recommended image size: 1366 x 1024px (4:3)."_ You can use a still image or a looping video.
3. Toggle the overlays that layer on top of the welcome screen:
 - **Order Here**
 - **Tap to Continue**
 - **Pay by Tap**
 - **No Cash**
 - **Wait Time**
4. Optionally add a **Welcome Notice** — a short message (e.g. "Check out our new items!").

Save, and the welcome screen updates on every device running that profile.

## [Locking](https://docs.onlumina.com/#locking)

Kiosk mode locks the iPad to the guest-facing ordering screen so customers can't get into your settings.

- When kiosk mode is **on**, the device stays on the welcome / ordering screen.
- To get back to operator controls, a staff member taps **Need Help?**, then **Go to Admin Dashboard**, and enters the **security PIN**: _"Enter the security PIN to access Admin Dashboard."_
- The PIN is the 4-digit unlock code set on the device's profile.

The lock is built into the Lumina Kiosk app — you don't need iOS Guided Access.

## [Related](https://docs.onlumina.com/#related)

- [Menu management](https://docs.onlumina.com/menu-management/menu-management) — manage the menu a profile shows.
- [Troubleshooting](https://docs.onlumina.com/troubleshooting/troubleshooting) — if a device goes offline or the reader disconnects.