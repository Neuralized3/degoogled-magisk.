# Degoogled Root Android Setup

A comprehensive guide and list of applications, modules, and modifications for a privacy-respecting, de-googled, and rooted Android experience.

---

## 1. Base OS & Framework

* **ROM:** [LineageOS (De-googled)](#)
* **Google Services Alternative:** [microG Services & microG Companion](#) *(Split setup for microG base services: one for root, one for ReVanced)*
* **App Store:** [Aurora Store](#) *(best UI and works nice)*

## 2. System Modifications (Root)

### Magisk Modules
* **De-bloater** *(v1.0 by sunilpaulmathew)*: Removes stock AOSP bloatware like recorder, fmradio.
* **Frost IOS Emoji & Font** *(v2.0 by @Xv2lce)*: System-wide iOS typography.
* **Iconify** *(v7.2.0 by @DrDisagree)*: UI theming (Plumpy icons, iOS battery, hidden navbar/keyboard space).
* **Play Integrity Fix** *(v4.4-inject-s by chiteroman)*: API compliance and spoofing.
* **Systemless Hosts** *(v1.0 by Magisk)*: System-wide ad blocking.
* **Zygisk - LSPosed** *(v1.10.2)* & **Vector** *(v2.0)*: Frameworks for hooking and module support.

### LSPosed Modules
* **FakeGapps:** Enables signature spoofing for microG.
* **Iconify:** Hook for advanced launcher/UI customization.
* **Pixel Launcher Enhanced:** Homescreen behavior modifications.
* **Wa Enhancer:** WhatsApp feature unlocks and channel bloat removal.

## 3. Core System & Utilities

* **Browser:** [Brave](#) or any hardened Chromium/Firefox-based browser. *(Primary)*
* **File Manager:** [ZArchiver](#) *(Replaces debloated AOSP Files)*
* **Keyboard:** [Gboard](#)
* **Launcher:** [Trebuchet](#) *(LineageOS Default)*
* **Stock Google Apps:** Calculator, Clock, Maps, Phone, Contacts, Messages, Settings, SIM Toolkit
* **Security/Email:** [Proton Mail](#) *(Primary E2E)* / [Gmail](#) *(For custom domains)*
* **Digital Wellbeing:** [StayFree](#)

## 4. Customization & UI

* **Widget Engine:** [Kustom Widget (KWGT)](#) *(AOSP version for no premium)*
* **Widget Packs:** [Nothing 2.0 KWGT (Adaptive)](#), [ThinkPro KWGT](#), & Nothing GitHub widgets repo *(search it you'll find it)*.

---

## Extra Applications for Workflow

### Productivity & Development
* **Obsidian:** Note-taking and vault management.
* **Autosync for Google Drive:** Automated cloud syncing for Obsidian.
* **Google Drive:** For PDFs.
* **Google Maps:** For navigation or use OpenMaps for better privacy. *(Note: you can enable and disable location in Lineage or Android 15 in general).*
* **GitHub:** Repository workflow management.
* **NotebookLM:** AI-driven research and fast studying.
* **Gemini & Google App:** Primary AI assistant dependencies. *(Google is needed for Gemini setup).*

### Media & Photography
* **Camera:** [LMC](#) *(GCam port, no Google services needed, most natural colors).*
* **Gallery/Backup:** [Google Photos](#) *(Modded for unlimited backup using Magisk or ReVanced).*
* **Photo Editor:** [Lightroom for Samsung](#).
* **Video:** [YouTube](#) *(ReVanced Extended - Ad-free, Shorts hidden).*
* **Music:** [YouTube Music](#) *(ReVanced Extended - Ad-free Spotify alternative).*

### Social & Networking
* **WhatsApp:** Primary communication *(Modded via Wa Enhancer).*
* **Telegram:** Mod repositories, support channels, and communication.
* **MyInsta:** Instagram mod *(Ghost mode enabled; Reels disabled via Panavision nav3 settings: go into metaconfigs in developer options, search `panavision nav3`, in its config rewrite `tab1` and `tab3` to `home`, save the config in metaconfig and restart).*
* **LinkedIn:** Professional networking and resume management.
* **Pinterest:** Visual inspiration.
* **Letterboxd:** Movie reviews and logging.
* **Strava:** Walking and activity tracker.
