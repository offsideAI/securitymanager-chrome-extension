# securitymanager-privacy

# SecurityManager privacy policy (v1.0.0)

*Effective with version 1.0.0. Plan v1, ROADMAP E5-S05; D-085, D-089, D-104, D-149.*

## The short version
- **SecurityManager collects nothing and sends nothing.** It has no account, no server, no analytics and no crash reporting, and it makes **no network requests** of any kind. Its content security policy allows connections only to the extension itself (`connect-src 'self'`).
- **Your logins are stored on this computer and may be backed up and exported to the file you choose .
- **Turn on full-disk encryption:** FileVault on macOS, BitLocker on Windows, LUKS on Linux.

## What SecurityManager stores, and where
| Data | Where | Secure? |
|---|---|---|
| Your logins: website, label, email or username, password | Chrome's private storage for the extension (an SQLite database in the Origin Private File System) on this device | **Yes** |
| The same logins | The sync file you pick, e.g. `~/SecurityManagerSyncDB.sqlite` | **Yes** |
| Automatic safety copies of the sync file (up to 20, taken before overwriting another device's changes) | The extension's private storage on this device | **Yes** |
| Settings, this device's name, when each login was last used on this device, sync status | The extension's private storage (IndexedDB) on this device | Yes |
| A check value for your master password (PBKDF2-SHA-256, 600,000 iterations, random salt) | In the vault and the sync file | It is a one-way hash. Your master password itself is never stored. |

**About the master password.** In v1.0 the master password unlocks the SecurityManager *interface*. "Reset password" needs no old password.

**About crashes during a save.** If Chrome is force-quit while it saves the sync file, the file itself stays intact. But Chrome may leave a temporary file such as `SecurityManagerSyncDB.sqlite.crswap` next to it, holding a partial copy. SecurityManager can't see or delete it. Delete any `.crswap` file you find next to your sync file.

## What SecurityManager reads on web pages
On pages served over HTTPS, and on `localhost` addresses, SecurityManager looks for login forms so that it can show its key icon, fill a login **when you click**, and offer to save a login after you sign in. To offer saving, it reads the email and password you just typed and keeps them **only in memory** for up to 10 minutes, until you choose Save, Not now or Never. Nothing from web pages is sent anywhere.

## Permissions
| Permission | Why |
|---|---|
| Access to web pages (HTTPS sites and `localhost`) | Detect login forms, fill on your click, offer to save after you sign in |
| `sidePanel` | SecurityManager's interface is Chrome's side panel |
| `storage` | Settings and short-lived page state, with no passwords |
| `offscreen` | Runs the vault database in the background, and clears the clipboard 30 s after you copy a password |
| `clipboardWrite` | Copy a username or password when you ask |
| `alarms` | A check every 5 minutes, while the panel is open, for changes to the sync file |
| `idle` | Lock the vault after the auto-lock time, or when your computer locks |
| File access (no permission listed; Chrome asks through its file dialog) | Read and write the one sync file you pick, and only while a SecurityManager panel is open |

## Your control
- Delete a login in SecurityManager and it is removed from this device and, at the next sync, from the sync file. A deletion marker without the email or password is kept, so other devices delete it too.
- Disconnect the sync file in Settings › Sync file. The file stays where it is, and you can delete it yourself.
- Removing the extension deletes its private storage on this device. Your sync file is a normal file and stays until you delete it.

## Contact
chatoffside@gmail.com

This policy changes only with a new version of SecurityManager, and the change is noted in its release notes.

