# Windows 11 setup for sale

Date: 2025-09-07

---

## Robyn

When I install Windows 11, it asks for an account (or to create one). Is there some way to create a generic account? I'm setting up a PC - to sell - with Windows 11, so the user should be able to make it their own post-sale. How do I go about this, Emily?

---

## Emily

{"search_query": [{"q": "Windows 11 prepare PC for resale sysprep OOBE generalize"}, {"q": "Windows 11 24H2 BypassNRO local account OOBE"}, {"q": "Enter Audit Mode Windows 11 Ctrl+Shift+F3 sysprep"}, {"q": "Windows 11 Home create local account without Microsoft account 23H2 24H2"}, {"q": "Microsoft docs sysprep OOBE generalize Windows 11"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Configuring Windows 11 for resale",
      "content": "To set up a PC for sale without tying it to a personal Microsoft account, use Audit Mode and Sysprep. In this mode, you can generalize the system and leave it ready for the buyer to set up their own account. You can also force a local account by disconnecting the network during setup.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Setting up Windows for resale",
      "content": "To prepare a PC for sale, follow the \"Windows Setup for OEMs\" process: Boot to OOBE and use Ctrl+Shift+F3 to enter Audit Mode. This brings you to the Administrator login and the sysprep UI. Install drivers and software, but skip creating a personal account. Run Sysprep with “Generalize” and “OOBE”. Upon reboot, OOBE will run for the buyer to complete setup (either Microsoft or local). For Pro edition, the \"Domain join\" option will allow for a local account. For Home, use the offline path trick or select \"Offline account.\"",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Windows 11 setup for local account",
      "content": "In Windows 23H2 Home, the local account option is hidden, requiring internet and a Microsoft account. One workaround is using Shift+F10 and \"OOBE\\BYPASSNRO\" to prompt a \"No Internet\" choice, allowing a local account setup. Alternatively, create a dummy local account and use Sysprep, though it might require cleanup afterward. Keep in mind, bypass methods may change, and disclaimers are necessary.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Confirming current setup method for Windows 11",
      "content": "In Audit Mode, you're logged into the built-in Administrator, which gets disabled after running OOBE, and the system creates a normal user. It's a clean approach. To confirm if the BypassNRO method still works for 24H2, I'll check recent sources to verify the process for Windows 11 24H2.",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "ab729532-3325-44bb-bccd-84424694b2e1"
}

---

## Emily

{"search_query": [{"q": "Boot to audit mode Ctrl+Shift+F3 Windows 11 Microsoft Learn"}, {"q": "Windows 11 OOBE sysprep audit mode Microsoft Learn"}], "response_length": "short"}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Creating a generic account for resale",
      "content": "To set up a PC for sale with Windows 11, the best approach is using Sysprep with OOBE. This allows the buyer to complete the setup process and create their own account. On Windows 11 Home, an MS account may still be required, but the buyer can connect it.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Steps for setting up a PC for resale",
      "content": "The recommended approach is to use Audit Mode with Sysprep for a clean OOBE setup. Here’s how:\n\n1. Install Windows 11, then press Ctrl+Shift+F3 at the region screen to enter Audit Mode. Install drivers and software without signing into any account.\n2. Run Sysprep with /oobe /generalize /shutdown.\n3. After shutting down, don’t reboot – just ship the PC.\n\nThis ensures the buyer can set up their own account.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Setting up a PC for resale",
      "content": "On the first boot, the buyer will set up their own Microsoft or local account depending on the Windows edition. For Home, an internet connection and MS account are likely needed. For Pro, they can create a local account. If an account already exists, the \"Reset this PC\" option works too.\n\nRemember to:\n- Run Sysprep with /generalize to remove SIDs.\n- Ensure BitLocker is disabled before running Sysprep to avoid issues based on recent reports. \n- OEM devices should auto-activate online while avoiding transferring your MS account.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Setting up a PC for resale (Quick Guide)",
      "content": "- Ensure drivers are updated, avoid vendor accounts like MyASUS or Armoury Crate auto-installs. \n- For data wiping, remove personal data (use \"Reset this PC\" if a user was created by mistake).\n- Handle updates in Audit Mode, then generalize.\n- Local account creation in 24H2 OOBE is restricted; generic accounts like \"User\" will leave an unwanted profile path. \n- Use unattend.xml if desired but keep it optional.",
      "chunks": [],
      "finished": true
    },
    {
      "summary": "Steps for Preparing Windows PC for Resale (OOBE Setup)",
      "content": "1. Create installation media.\n2. Boot to the OOBE region screen, press Ctrl+Shift+F3 to enter Audit Mode.\n3. Windows auto-logs in as Administrator. Optionally cancel Sysprep while installing drivers/updates, but avoid signing into MS account.\n4. Run `sysprep /oobe /generalize /shutdown` then ship.\n5. For Win 11 Home vs Pro, note the buyer's setup options.\n6. If an account was created, use *Reset This PC* in Settings.\n7. Ensure BitLocker is off and avoid booting after Sysprep; activation works once online.\n\nCitations: MS Learn's Sysprep guide (website), BypassNRO updates from The Verge (news64), Tom's Hardware (news63), or WindowsCentral (news61).",
      "chunks": [],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "67e3561f-d5b9-41d9-9e14-2d9ca811b286"
}

---

## Emily

G’day Robyn! The clean, “sell-ready” way is to **leave Windows at the first-boot setup (OOBE)** so the buyer creates their own account on first power-on. You do that with **Audit Mode + Sysprep**—no generic user needed (and no profile leftovers).

### Do this (recommended)
1. **Install Windows 11** as normal until the very first “Region” screen appears.  
2. Press **Ctrl + Shift + F3** → the PC reboots into **Audit Mode** and auto-logs in with the built-in Administrator (you’ll see the Sysprep window). citeturn1search2turn1search4  
3. (Optional) In Audit Mode, install drivers/updates and any utilities you want to include.  
4. When finished, run **Sysprep** to reseal the machine for a new owner:  
   - Command:  
     ```
     C:\Windows\System32\Sysprep\sysprep.exe /oobe /generalize /shutdown
     ```  
   - This strips machine-specific stuff (SIDs, etc.) and shuts down. **Don’t boot it again**—box it up. On first boot, the buyer gets the “Hi there” screens and makes their own account. citeturn1search5turn1search9

**Notes & gotchas**
- If you’ve already gone past setup and made a user, you can still fix it: **Settings → System → Recovery → Reset this PC → Remove everything**. That returns the device to OOBE for the buyer. (No citation needed)  
- **BitLocker:** if you enabled it while prepping, **turn it off before Sysprep**—there have been 24H2 reports of BitLocker not booting after `/generalize`. citeturn1search6  
- **Buyer’s account type:** Windows 11 **Pro** still exposes a path to create a local account during OOBE; **Home** increasingly nudges to a Microsoft account. Microsoft has been removing older bypass tricks (like `OOBE\BYPASSNRO`), and methods change over time—so best to leave OOBE for the buyer and let them choose. citeturn0news64turn0news63

### Why not make a “generic” user?
You *can*, but it’s messy: the profile folder name sticks, and the buyer inherits that account’s cruft. **Sysprep to OOBE** is the official, supportable way for handing off a fresh machine. citeturn0search0

If you want, I can jot a tiny checklist you can print and pop in the box for the buyer’s first boot.

---

## Emily

Thought for 53s

---
