# 📦 Bitmetric QVD-Viewer Installer

Welcome to the Bitmetric installer for the **QVD-Viewer** in Qlik Cloud. This installer helps you automate the setup with just a few clicks.

---

## 🚀 Quick Start

1. **Download** the included `.json` file from this repository.
2. Go to **Qlik Cloud → Application Automation**.
3. **Upload** the JSON as a new automation.
4. **Run** the automation. The built-in guide will take care of most steps.
5. **Follow the final manual steps**, which can't (yet) be executed via Qlik Automations.

---

## ⚠️ Known Limitations

- For now, this installer **only supports QVD-Viewer**.
- Other products mentioned in the manifest are **not yet supported**.

---

## 🧠 Technical Background

This installer is based on a **manifest-driven design**. Here's how it works:

- The **manifest file** in this branch defines:
  - What checks must be performed (e.g. API Key creation, admin rights).
  - Which automations or apps should be installed.
- The installer reads the manifest and:
  - Runs the necessary checks.
  - Downloads the assets.
  - Installs them in your Qlik tenant.
  - Applies tags to make updates and removals easier later on.
- If the installer is run a second time, it can **request deletion** of previous installs to ensure updates are applied cleanly.

---

## 🙋 Need Help?

If you run into any issues or have feedback, feel free to open an issue or start a discussion. Contributions welcome!

---

© [Bitmetric](https://www.bitmetric.nl)
