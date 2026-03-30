# Degoogled Root Android Setup

A comprehensive guide and list of applications, modules, and modifications for a privacy-respecting, de-googled, and rooted Android experience.

---

## 1. Base OS & Framework

* **ROM:** LineageOS De-googled/Google Services by flashing [MindtheGapps](https://gitlab.com/MindTheGapps) (Download LineageOS either [from the official website](https://wiki.lineageos.org/devices/) or [XDA forum (Search your phone's model and go into the forum's development tab](https://xdaforums.com/)) 
* **Google Services Alternative:** microG Services & microG Companion *(Split setup for microG base services: [one for root](https://github.com/microg/GmsCore/wiki/Downloads), one for [ReVanced](https://github.com/TeamVanced/VancedMicroG/releases/tag/v0.2.24.220220-220220001))*
* **App Store:** [Aurora Store](https://www.apkmirror.com/apk/aurora-oss/aurora-store-fdroid-version/) *(best UI and works nice)*

## 2. System Modifications (Root)

### Magisk Modules
* **[De-bloater](https://github.com/sunilpaulmathew/De-Bloater)** *(v1.0 by sunilpaulmathew)*: Removes stock AOSP bloatware like recorder, fmradio.
* **[Frost IOS Emoji & Font](https://xdaforums.com/t/module-magisk-ksu-frost-ios-emojis-sf-font-v2-0.4759469/)** *(v2.0 by @Xv2lce)*: System-wide iOS typography.
* **[Play Integrity Fix]** *(v4.4-inject-s by chiteroman)*: API compliance and spoofing.
* **[Systemless Hosts]** *(v1.0 by Magisk)*: System-wide ad blocking.
* [**Zygisk - LSPosed** *(v1.10.2)* & **Vector**](https://github.com/JingMatrix/Vector) *(v2.0)*: Frameworks for hooking and module support.

### LSPosed Modules
* **FakeGapps:** Enables signature spoofing for microG.
* **[Iconify:](https://github.com/Mahmud0808/Iconify)** Hook for advanced launcher/UI customization.
* **[Pixel Launcher Enhanced:](https://github.com/Mahmud0808/PixelLauncherEnhanced)** Homescreen behavior modifications.
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

* **Widget Engine:** [Kustom Widget (KWGT)](https://docs.kustom.rocks/docs/downloads/download-kwgt/) *(AOSP version for no premium)*
* **Widget Packs:** [Nothing 2.0 KWGT (Adaptive)](https://9mod.com/nothing-2-0-for-kwgt.html), [ThinkPro KWGT](https://9mod.com/thinkpro-kwgt.html), & [Nothing GitHub Widgets Repositories](https://github.com/GXX0T/NotWidgets)

---

## Extra Applications for Workflow
#### Note - I'll link most of the applications but those which are to be patched by revanced, I'll tag them as rvx and those which are to be downloaded by aurora store I'll tag them as aurs, I'll also tag the revanced patched applications link.

### Productivity & Development
* **Obsidian:**(aurs) Note-taking and vault management.
* **Autosync for Google Drive:**(aurs) Automated cloud syncing for Obsidian.
* **Google Drive:**(aurs) For PDFs.
* **Google Maps:**(aurs) For navigation or use OpenMaps for better privacy. *(Note: you can enable and disable location in Lineage or Android 15 in general).*
* **GitHub:**(aurs) Repository workflow management.
* **NotebookLM:**(aurs) AI-driven research and fast studying.
* **Gemini & Google App:**(aurs) Primary AI assistant dependencies. *(Google is needed for Gemini setup).*

### Media & Photography
* **Camera:** [LMC](https://www.celsoazevedo.com/files/android/google-camera/) *(GCam port, no Google services needed, most natural colors).*
* **Gallery/Backup:** [Google Photos](https://github.com/thunderkex/revanced-extended/releases) *(Modded for unlimited backup using Magisk or ReVanced).*
* **Photo Editor:** [Lightroom for Samsung](https://github.com/thunderkex/revanced-extended/releases).
* **Video:** [YouTube](https://github.com/thunderkex/revanced-extended/releases) *(ReVanced Extended - Ad-free, Shorts hidden).*
* **Music:** [YouTube Music](https://github.com/thunderkex/revanced-extended/releases) *(ReVanced Extended - Ad-free Spotify alternative).*

### Social & Networking
* **WhatsApp:**(aur) Primary communication *(Modded via Wa Enhancer).*
* **Telegram:**(aur) Mod repositories, support channels, and communication.
* **MyInsta:**(Find your own link, I cannot figure out whether it is safe from the source I downloaded) Instagram mod *(Ghost mode enabled; Reels disabled via Panavision nav3 settings: go into metaconfigs in developer options, search `panavision nav3`, in its config rewrite `tab1` and `tab3` to `home`, save the config in metaconfig and restart, also in myInsta settings disable reel scroll to one and feed reels.).*
* **LinkedIn:**(aurs) Professional networking and resume management.
* **Pinterest:**(aurs) Visual inspiration.
* [**Letterboxd:**(Patch yourself using revanced extended manager)](https://github.com/thunderkex/revanced-extended/releases) Movie reviews and logging.
* [**Strava:**](https://github.com/thunderkex/revanced-extended/releases) Walking and activity tracker.
