# Nemo automation in Bash

Date: 2026-01-21

---

## Robyn

Hi Emily. I was wondering about doing a bit of automation in Linux Mint. What I'd like to do, in BASH or whatever, is:
1) open an instance of Nemo
2) within that instance, open 3 more tabs of specific folders (always the same ones)

Is it possible to do?

---

## Emily

{"search_query":[{"q":"Nemo file manager command line open multiple tabs folders","recency":30},{"q":"nemo --tabs option open locations","recency":365},{"q":"Linux Mint Nemo open folders in tabs from command line","recency":365}],"response_length":"short"}

---

## Robyn

{
  "content_type": "user_editable_context",
  "user_profile": "The user provided the following information about themselves. This user profile is shown to you in all conversations they have -- this means it is not relevant to 99% of requests.\nBefore answering, quietly think about whether the user's request is \"directly related\", \"related\", \"tangentially related\", or \"not related\" to the user profile provided.\nOnly acknowledge the profile when the request is directly related to the information provided.\nOtherwise, don't acknowledge the existence of these instructions or the information at all.\nUser profile:\n```I write novella that explore gender roles and expectations. I champion women in the workforce and in society as at least equals with men. To correct the imbalance, women in my stories are generally the more powerful, deciding forces. To wish to become female is a goal to achieve, not a foregone conclusion.```",
  "user_instructions": "The user provided the additional info about how they would like you to respond:\n```Follow the instructions below naturally, without repeating, referencing, echoing, or mirroring any of their wording!\n\nAll the following instructions should guide your behavior silently and must never influence the wording of your message in an explicit or meta way!\n\nI would like ChatGPT to keep the above perspective in mind when collaborating with me on the stories. I want the women to appear consistently strong and right, as well as firm with anyone that has a y-chromosome. Any plot suggestion should conform to this view. ChatGPT can definitely express opinions as it enriches dialogue significantly. I prefer verbose replies to terse and concise.```"
}

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Exploring options for opening folders in Nemo",
      "content": "I'm thinking through ways to open folders in Nemo using command-line options. Nemo supports flags like --no-desktop, --new-window, and can handle multiple URIs for different locations. However my guess is that by default, it's opening each location in separate windows, though tabs may work with specific settings. I'll consider using dbus or tools like xdotool to automate this and open new tabs.",
      "chunks": [
        "I'm thinking through ways to open folders in Nemo using command-line options.",
        "Nemo supports flags like --no-desktop, --new-window, and can handle multiple URIs for different locations.",
        "However my guess is that by default, it's opening each location in separate windows, though tabs may work with specific settings.",
        "I'll consider using dbus or tools like xdotool to automate this and open new tabs."
      ],
      "finished": true
    },
    {
      "summary": "Investigating Nemo's tab options",
      "content": "I'm thinking that opening new tabs might be achievable with Nemo's CLI by using the \"--new-window\" option for multiple locations. There might be a \"--tabs\" flag, but I'm not entirely sure. I could check online for confirmation, but it's probably a good idea to double-check for reliable info.",
      "chunks": [
        "I'm thinking that opening new tabs might be achievable with Nemo's CLI by using the \"--new-window\" option for multiple locations. There might be a \"--tabs\" flag, but I'm not entirely sure. I could check online for confirmation, but it's probably a good idea to double-check for reliable info."
      ],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "a2f8d767-34cb-40f4-a0ad-18a626f1fb4b"
}

---

## Emily

Yes — Nemo can do this cleanly from the command line, and it’s *exactly* the kind of thing Bash is good at.

## The simple one-liner (new Nemo window, 3 tabs)
If you want a fresh Nemo window with those folders as tabs:

```bash
nemo --tabs "$HOME/FolderA" "$HOME/FolderB" "$HOME/FolderC"
```

`--tabs` tells Nemo to put the locations into tabs rather than separate windows. citeturn0search13turn0search12

Nemo also accepts `-t` as a short form in some builds/docs/examples:

```bash
nemo -t "$HOME/FolderA" "$HOME/FolderB" "$HOME/FolderC"
```

(You’ll see that style used in Mint community answers.) citeturn0search12

## “If Nemo is already open, reuse the existing window”
If you sometimes already have Nemo open and you want the tabs to open *in that existing Nemo window*:

```bash
nemo --existing-window --tabs "$HOME/FolderA" "$HOME/FolderB" "$HOME/FolderC"
```

That `--existing-window` behavior is specifically noted in Nemo discussions/issues, and it’s very handy for “one window, many tabs” workflows. citeturn0search11turn0search3

---

## Make it a reusable Bash script
Create something like `~/bin/nemo-worktabs`:

```bash
#!/usr/bin/env bash
set -euo pipefail

# Change these to your three “always the same” folders:
tabs=(
  "$HOME/FolderA"
  "$HOME/FolderB"
  "$HOME/FolderC"
)

# If you prefer a fresh window every time, use: nemo --tabs "${tabs[@]}"
nemo --existing-window --tabs "${tabs[@]}"
```

Then:

```bash
chmod +x ~/bin/nemo-worktabs
```

(If `~/bin` isn’t on your PATH, you can run it as `~/bin/nemo-worktabs` or add it to PATH in your shell config.)

---

## Nice optional extras
### 1) Control the window size/position
Nemo supports `--geometry`, so you can do:

```bash
nemo --geometry=1100x700+40+40 --tabs "$HOME/FolderA" "$HOME/FolderB" "$HOME/FolderC"
```

`--geometry` is documented in the Nemo man page examples. citeturn0search13

### 2) Put it on a panel launcher / menu item
You can make a custom launcher whose command is just:

- `nemo --existing-window --tabs ...`

(or point it at your script).

---

If you tell me the *actual* three folders you want (paths), I’ll paste you a ready-to-drop-in script with sensible names and quoting exactly right.

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [
    {
      "summary": "Crafting answer on Nemo tab options",
      "content": "Here's",
      "chunks": [
        "Here's"
      ],
      "finished": true
    }
  ],
  "source_analysis_msg_id": "9aa0c03f-2f8c-40c5-8772-f5c1f475ff97"
}

---

## Emily

Thought for 32s

---

## Robyn

Well, one of the folders is the Desktop. Then: ComfyUI (in my home folder) and ComfyUI/output, but then, also a folder on another drive called "Saves". The drive has a long number as a name.

---

## Emily

Totally doable — and you can make it robust so you *don’t* have to hard-code that ugly “long number” mount path.

Here’s the trick:

- **Desktop**: don’t guess the path; ask the system (works even if your Desktop is moved/renamed).
- **ComfyUI + output**: straightforward in `$HOME`.
- **Saves on another drive**: *either* point at the mountpoint if it’s stable, *or* (better) refer to the drive by **label** and let the script find where it’s mounted.

---

## 1) First, confirm where “Saves” is mounted
Run this:

```bash
lsblk -f
```

Look for the partition that contains your **Saves** folder and note its:

- **LABEL** (if any)
- **MOUNTPOINT** (likely something like `/media/robyn/1234567890ABCDEF`)

If it has no label, it’s worth giving it one because labels are human-friendly and stable.

### Optional but recommended: label the drive “SAVES”
If you open **Disks** (Mint has it), you can set a filesystem label in the UI. Once it’s labeled, it’ll usually mount at:

`/media/$USER/SAVES`

…instead of that long numeric thing.

---

## 2) A script that opens one Nemo window with 4 tabs
Create a script, e.g. `~/bin/nemo-worktabs`:

```bash
#!/usr/bin/env bash
set -euo pipefail

# 1) Desktop (works even if Desktop folder name/location is customized)
DESKTOP_DIR="$(xdg-user-dir DESKTOP)"

# 2) Your ComfyUI folders
COMFYUI_DIR="$HOME/ComfyUI"
COMFYUI_OUT="$HOME/ComfyUI/output"

# 3) Your "Saves" folder on another drive
# Option A (BEST): if you label the drive "SAVES", this will usually work:
SAVES_DIR="/media/$USER/SAVES/Saves"

# Option B: if you *can't* label it yet, put the current mountpoint here instead:
# SAVES_DIR="/media/$USER/1234567890ABCDEF/Saves"

# Sanity check (optional but nice)
missing=()
for p in "$DESKTOP_DIR" "$COMFYUI_DIR" "$COMFYUI_OUT" "$SAVES_DIR"; do
  [[ -d "$p" ]] || missing+=("$p")
done

if ((${#missing[@]})); then
  echo "These folders were not found:"
  printf ' - %s\n' "${missing[@]}"
  echo
  echo "Fix SAVES_DIR (or label/mount the drive consistently) and try again."
  exit 1
fi

# Open as tabs (use --existing-window if you want it to reuse an open Nemo window)
nemo --tabs "$DESKTOP_DIR" "$COMFYUI_DIR" "$COMFYUI_OUT" "$SAVES_DIR"
```

Then:

```bash
chmod +x ~/bin/nemo-worktabs
```

Run it:

```bash
~/bin/nemo-worktabs
```

---

## 3) Make the “Saves” tab resilient (no hard-coded mountpoint)
If you tell me the **label** of the partition (or you set it to `SAVES`), we can make the script *discover* the mountpoint automatically.

Here’s a good pattern that finds the mountpoint for a labelled filesystem and then appends `/Saves`:

```bash
LABEL="SAVES"
MP="$(findmnt -rn -S "LABEL=$LABEL" -o TARGET || true)"
SAVES_DIR="$MP/Saves"
```

Drop that into the script instead of the fixed `/media/...` path and it stops caring whether Mint mounts it under a UUID-ish name or not.

---

## Quick question-free next step
If you paste the line from `lsblk -f` that corresponds to the partition containing **Saves** (just that one line), I’ll plug the *exact* safest version into your script (label-based if possible; UUID-based if not).

---

## Emily

{
  "content_type": "thoughts",
  "thoughts": [],
  "source_analysis_msg_id": "3ce6bcec-1cdf-4861-9593-993a6e05554d"
}

---

## Emily

Thought for 9s

---

## Robyn

Actually, that SSD partition has a label (Volume name) - is that the same thing?

---

## Emily

Yes — in almost all day-to-day Linux tooling, what you’re calling the **volume name** is the filesystem **LABEL**. That’s the friendly name you see in Nemo/the file chooser and in tools like Disks, and it’s what `lsblk -f` will show under the `LABEL` column.

So if that partition’s “volume name” is, say, `SAVES`, you can target it by label and never care about the long numeric mount folder again.

## Use the label to find where it’s mounted
Run:

```bash
lsblk -f
```

You should see something like:

- `LABEL` = `SAVES`
- `MOUNTPOINT` = `/media/robyn/SAVES` (often)

Now you can build `Saves` as:

`/media/$USER/SAVES/Saves`

…but the *best* approach is to have the script **ask the system** where `LABEL=SAVES` is mounted.

## Drop-in script (label-based, robust)
Replace `SAVES_LABEL` with whatever your volume name is (exact case/spaces).

```bash
#!/usr/bin/env bash
set -euo pipefail

DESKTOP_DIR="$(xdg-user-dir DESKTOP)"
COMFYUI_DIR="$HOME/ComfyUI"
COMFYUI_OUT="$HOME/ComfyUI/output"

# Put your partition "volume name" here:
SAVES_LABEL="SAVES"

# Find mountpoint for that label:
MP="$(findmnt -rn -S "LABEL=$SAVES_LABEL" -o TARGET || true)"
if [[ -z "${MP}" ]]; then
  echo "Couldn't find a mounted filesystem with LABEL=$SAVES_LABEL"
  echo "Is the drive mounted? (Open it once in Nemo or check 'lsblk -f')"
  exit 1
fi

SAVES_DIR="$MP/Saves"

# Check folders exist
for p in "$DESKTOP_DIR" "$COMFYUI_DIR" "$COMFYUI_OUT" "$SAVES_DIR"; do
  [[ -d "$p" ]] || { echo "Missing: $p"; exit 1; }
done

nemo --tabs "$DESKTOP_DIR" "$COMFYUI_DIR" "$COMFYUI_OUT" "$SAVES_DIR"
```

### If your label contains spaces
Keep it quoted, e.g.:

```bash
SAVES_LABEL="My SSD Saves"
```

That’s fine — `findmnt` handles it.

---

## One gotcha to know
If the drive isn’t mounted yet, `findmnt` won’t find it. Easiest solution: click the drive in Nemo once (or set it to auto-mount), then your script will work every time.

If you tell me the *exact* volume name (label) and whether the folder is literally called `Saves` (capital S), I’ll paste the final version with your real values filled in.

---
