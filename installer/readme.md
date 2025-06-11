# 📦 Bitmetric QVD-Viewer Installer

Welcome to the Bitmetric installer for the **QVD-Viewer** in Qlik Cloud. This installer helps you automate the setup with just a few clicks.

---

## 🚀 Quick Start

1. **Download** the included `.json` file from this repository.
2. Go to **Qlik Cloud → Application Automation**.
3. **Upload** the JSON as a new automation.
4. **Run** the automation. The built-in guide will take care of most steps.
5. **Follow the final manual steps**, which can't (yet) be executed via Qlik Automations.

✅ No scripting  
✅ All checks are automated  
✅ You only configure what's needed  

---
## 🧠 Technical Background

This installer follows a **manifest-driven design**, meaning all install logic is controlled by a `manifest.json` file. This design allows for flexible installation of one or more tools depending on the manifest content. Here's how it works in detail:

### 1. Start

- The automation begins by downloading the `manifest.json` from the repository.
- All defined "products" in the manifest are collected.
- These products are shown in a multi-select input so the user can choose what to install.
- The selected products are stored for further use.

### 2. User Settings

Performs checks related to the user running the automation:

- **Tenant Admin Check** (based on `"tenant-admin": true` in manifest):
  - ✅ Yes → continue.
  - ❌ No → automation stops; user must log out and log back in as a tenant admin.

- **Professional License Check** (always checked):
  - ✅ Yes → continue.
  - ❌ No → checks if professional license can be assigned:
    - ✅ Yes → stops and instructs user to log out and back in.
    - ❌ No → automation stops permanently.

- **Developer Role Check** (based on `"api-key_required": true`):
  - ✅ Role present → continue.
  - ❌ Role missing → role is assigned and automation stops with the instruction for the user to log out and back in.

### 3. Tenant Settings

Checks tenant-level configuration:

- **API Keys Enabled** (`"api-key_required": true`):
  - ✅ If enabled → continue.
  - ❌ If disabled: the user is shown a yes/no prompt.  
    - ✅ If "Yes": API Keys will be enabled automatically in the tenant.  
    - ❌ If "No": the automation will stop.

### 4. Space Settings

- Retrieves lists of both **Managed Spaces** and **Shared Spaces**.
- These are stored for user selection in the next step.

### 5. Space Chooser

- User selects a **Shared Space** where apps and connections will be created.
- Saves the selected spaceId in `oSettings.sharedSpaceId`.
- Roles (`producer`, `facilitator`, `codeveloper`) are verified for the user.
  - ❌ Missing roles are automatically assigned.

### 6. Connection Creator

Executed **only if** both `"api-key_required": true` and `"data-connection": true` are present in the manifest.

#### API Key Creation

- Checks for an existing API key named `Bitmetric_APIKEY`.
  - ✅ Exists → it is deleted.
  - ❌ Not found → asks user permission to create one.
    - ❌ Permission denied → automation stops.
- Checks if the user has quota to create a new key.
  - ✅ Yes → new API key is created (max duration, excluded from logs).
  - ❌ No → automation stops.

#### Data Connection Creation

- Deletes any existing connections named `Bitmetric_Qlik_POST` or `Bitmetric_Qlik_GET`.
- Creates new POST/GET data connections using the generated API key. These steps will be excluded from the logs in order to hide the generated API-KEY.

### 7. Tag Checker

- For every tag defined in the manifest:
  - ✅ If the tag exists → all items with the tags are stored in `oTag`.
  - ❌ If not → tag is created in the tenant.

### 8. Automation Installer and App Installer

The following input from the manifest is needed. We will loop through the list of files in the manifest. Each file, with `type : automation`, will be processed in this step. The following inputs are required from the list of files in the manifest. The other fields aren't required yet.
- `type`: can be app or automation
- `file name`: free field. Contains the name of the automation or app that should be installed.
- `source`: the location to download the file. For automation is this a JSON, for an app a QVF. The automation translates this to base 64.

For each file to be installed:
- Confirms whether an old version should be deleted:
  - Checks tag match, name match, and user approval.
- Downloads the source file from the given `source` URL.
- Installs the automation and/or app and applies the appropriate tags.

### 9. Output

Presents the final output block. This can be written to give the user the last instructions.


---

## 🙋 Need Help?

If you run into any issues or have feedback, feel free to open an issue or start a discussion. Contributions welcome!

---

© [Bitmetric](https://www.bitmetric.nl)
