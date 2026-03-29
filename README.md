
📱 Degoogled Root Android Setup
A comprehensive guide to a privacy-focused, high-performance Android environment using LineageOS, microG, and Magisk.
🏗️ Base OS & Framework
 * ROM: LineageOS (De-googled)
 * Services: microG Services & microG Companion
   * Note: Split setup for base services; dedicated instances for Root and ReVanced.
 * App Store: Aurora Store (Best UI/UX for Play Store access).
🛠️ System Modifications (Root)
Magisk Modules
| Module | Version | Purpose |
|---|---|---|
| De-bloater | v1.0 | Removes AOSP bloat (Recorder, FM Radio, etc.) |
| Frost IOS Emoji & Font | v2.0 | System-wide iOS typography and emojis |
| Iconify | v7.2.0 | UI theming: Plumpy icons, iOS battery, removes navbar/KB space |
| Play Integrity Fix | v4.4-inject-s | Fixes API compliance and device spoofing |
| Systemless Hosts | v1.0 | Native, system-wide ad blocking |
| Zygisk - LSPosed | v1.10.2 | Core framework for hooking and module support |
| Vector | v2.0 | LSPosed framework enhancement |
LSPosed Modules
 * FakeGapps: Enables signature spoofing for microG.
 * Iconify: Advanced hook for launcher and UI customization.
 * Pixel Launcher Enhanced: Homescreen behavior and layout modifications.
 * Wa Enhancer: WhatsApp feature unlocks and channel bloat removal.
📂 Core System & Utilities
 * Browser: Brave (Or any hardened Chromium/Firefox fork).
 * File Manager: ZArchiver (Replaces stock AOSP Files).
 * Keyboard: Gboard.
 * Launcher: Trebuchet (LineageOS Default).
 * Essential Suite: Calculator, Clock, Maps, Phone, Contacts, Messages, Settings, SIM Toolkit.
 * Email: Proton Mail (E2E Primary) & Gmail (For custom domains).
 * Wellbeing: StayFree (Screen time tracking).
🎨 Customization & UI
 * Widget Engine: KWGT (AOSP version for ad-free experience).
 * Widget Packs: * Nothing 2.0 KWGT (Adaptive)
   * ThinkPro KWGT
   * Nothing GitHub Widgets Repo
🚀 Workflow & Productivity
Development & Research
 * Obsidian: Primary note-taking and knowledge base.
 * Autosync for Google Drive: Real-time cloud syncing for Obsidian vaults.
 * GitHub: Repository and workflow management.
 * NotebookLM: AI-driven research and study tool.
 * Gemini & Google App: AI assistant dependencies.
Media & Photography
 * Camera: LMC 8.8 (GCam port - natural colors, no Google Services required).
 * Gallery: Google Photos (Modded for unlimited backup via Magisk/ReVanced).
 * Editing: Lightroom for Samsung.
 * Video: YouTube ReVanced Extended (Ad-free, Shorts hidden).
 * Music: YouTube Music ReVanced (Spotify alternative).
Social & Networking
 * WhatsApp: Modded via Wa Enhancer.
 * Telegram: Accessing mod repositories and support channels.
 * MyInsta: Instagram Mod.
   * Config: Ghost mode enabled. Reels disabled via Panavision nav3 settings (MetaConfig developer options: rewrite tab1 and tab3 to home).
 * LinkedIn: Resume and professional networking.
 * Pinterest: Visual inspiration.
 * Letterboxd: Movie reviews and logging.
 * Strava: Activity and walking tracker.
