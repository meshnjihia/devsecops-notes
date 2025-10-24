x📝 File name:

linux_day1_full_session.md

# 🧭 DevSecOps Bootcamp — Day 1 Full Session Summary

## 🗓️ Date
**Friday, October 24, 2025**

---

## 🧩 Overview
**Theme:** Linux Fundamentals, Git Workflow, and Defensive Operations  
Today’s session focused on foundational DevSecOps skills using real-world command-line practice.  
Topics included file navigation, permissions, Git branching, troubleshooting, and automation scripting.

---

## ⚙️ PART A — Linux Navigation & Search

### 🔹 Commands Practiced
```bash
cd ~/devsecops_basics
mkdir -p practice/audit && cd practice

# Create sample files and folders
printf "alpha\nbeta\ngamma\n" > words.txt
printf "INFO boot\nWARN disk\nERROR perms\n" > logs.txt
mkdir audit && touch audit/permissions.txt audit/users.txt

# Quick search drills
grep -n "ERROR" logs.txt
grep -i "warn" logs.txt
find . -type f -name "*.txt"

🧠 What I Learned

cd moves between directories.

mkdir -p creates nested directories safely.

grep searches inside files; find locates files across directories.

. = current directory, .. = parent directory.

Using structured directories (practice/audit/) helps organize work.

🧩 Fix / Observation

The command mkdir audit gave an error (“File exists”) because the audit folder was already created by mkdir -p practice/audit. It was harmless — just redundant.

🔐 PART B — File Permissions Quick Reps
🔹 Commands Practiced
touch secret.txt
ls -l secret.txt
chmod 600 secret.txt
sudo chown $USER:$USER secret.txt
ls -l secret.txt

🧠 What I Learned
Command	Purpose
touch	creates empty file
ls -l	shows permission, ownership, size, and timestamp
chmod 600	allows only owner read/write access (rw-------)
chown $USER:$USER	ensures correct file ownership

chmod 600 is commonly used for sensitive files like SSH keys.

No output from chown means success.

🧾 Log Entry
echo "$(date) — practiced grep/find/chmod (600)" >> ../linux_day1_notes.txt


Used ../ to go up one level (to the devsecops_basics directory) and append the log entry.

📍 Current Directory

At this point, still in:

~/devsecops_basics/practice

🌿 PART C — Git Branching, Rebase, and Workflow
🔹 Commands Practiced
cd ~/devsecops_basics
git status
git pull --rebase origin main
git checkout -b feature/linux-permissions-notes

🧠 What I Learned

git pull --rebase keeps history linear and clean.

git checkout -b creates and switches to a new branch instantly.

🪲 BUG FIX: “Unstaged Changes” Error
🧩 Error Message
error: cannot pull with rebase: You have unstaged changes.
error: please commit or stash them.

🧰 Fix

Committed my local changes before pulling:

git add .
git commit -m "Save local edits before pulling"
git pull --rebase origin main

🪲 BUG FIX: Merge Conflict During Rebase
🧩 Error Message
CONFLICT (content): Merge conflict in linux_day1_notes.txt

🧰 Fix

Opened the conflicted file:

nano linux_day1_notes.txt


Removed conflict markers:

<<<<<<< HEAD
(content from origin/main)
=======
(my local content)
>>>>>>> save local edits before pulling


Kept both relevant notes, deleted markers.

Resolved rebase:

git add linux_day1_notes.txt
git rebase --continue


Verified clean state:

git status


✅ Lesson: Merge conflicts aren’t errors — they’re opportunities to integrate multiple changes carefully.

🔹 Updated Notes
echo -e "\n## Permissions cheatsheet\n- chmod 600 file  # owner read/write\n- chmod 400 file  # owner read only\n- chown user:group file" >> linux_day1_notes.txt

🔹 Committed & Pushed
git add linux_day1_notes.txt README.md
git commit -m "Add permissions cheatsheet and initial README"
git push -u origin feature/linux-permissions-notes

🔹 Merged & Cleaned Up
git checkout main
git pull --rebase origin main
git branch -d feature/linux-permissions-notes

👥 PART D — Users, Groups & umask
🔹 Commands Practiced
whoami
id
getent passwd | head
getent group | head
umask
umask 027
mkdir -p ~/devsecops_basics/umask_demo && cd $_
touch a.txt && mkdir bdir && ls -l

🧠 What I Learned

whoami → shows the current user.

id → shows UID, GID, and groups.

getent → displays system user/group info.

umask → sets default permission mask for new files.

Default umask was 0022 → files get 644 permissions.
Changing to 027 → removes “others” access → safer default.

🗑️ PART E — Safer Deletes (Recycle Alias)
🔹 Purpose

Protect files from accidental deletion during development.

🔹 Commands Practiced
cd ~/devsecops_basics
mkdir -p ~/.trash
echo 'alias rm="mv -t ~/.trash"' >> ~/.bashrc
source ~/.bashrc
touch will_remove.txt
rm will_remove.txt
ls ~/.trash


✅ Verified that deleted files moved to ~/.trash instead of being destroyed.

🧩 Improved Version (Added to ~/.bashrc)
export TRASH_DIR="$HOME/.trash"
recycle() {
  mkdir -p "$TRASH_DIR/$(date +%F)"
  mv -- "$@" "$TRASH_DIR/$(date +%F)/"
}
restore() {
  local item="$1"
  [ -z "$item" ] && { echo "Usage: restore <filename>"; return 1; }
  local found
  found=$(ls -1dt "$TRASH_DIR"/*/"$item" 2>/dev/null | head -n1)
  [ -z "$found" ] && { echo "Not found in trash: $item"; return 1; }
  mv -- "$found" .
}
case $- in *i*) alias rm='recycle' ;; esac

💡 Interview Takeaway

"In my DevSecOps lab, I implemented a safer rm alias that redirects deletions to a local recycle folder.
It reduces risk and teaches defensive habits. In production, I’d rely on backups or tools like trash-cli instead."

⚙️ PART F — Mini Script: List Top Big Files
🔹 Script Creation
cat > top_big_files.sh << 'EOF'
#!/usr/bin/env bash
# List top 10 largest files under current dir
du -ah . 2>/dev/null | sort -hr | head -n 10
EOF

chmod +x top_big_files.sh
./top_big_files.sh > bigfiles.txt

🔹 What It Does

Scans the current directory.

Lists files by size, largest first.

Captures results into bigfiles.txt.

🔹 Commit & Push
git add top_big_files.sh bigfiles.txt linux_day1_notes.txt
git commit -m "Add big files helper script and update notes"
git push


✅ Result: reusable utility script for quickly identifying disk usage patterns — useful for DevOps auditing and cleanup automation.

🪲 PART G — Troubleshooting Highlights
1️⃣ Git Error: “Password Authentication Not Supported”

Fix:
Set up SSH key authentication:

ssh-keygen -t ed25519 -C "meshworks7@gmail.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub   # added to GitHub
ssh -T git@github.com       # confirmed success
git remote set-url origin git@github.com:meshnjihia/devsecops-notes.git

2️⃣ Git Error: “Unstaged Changes During Rebase”

Fix: Committed changes before pulling.

git add .
git commit -m "Save local edits before pulling"
git pull --rebase origin main

3️⃣ Git Error: “Merge Conflict in linux_day1_notes.txt”

Fix: Manually edited, removed conflict markers, continued rebase.

🧠 Reflection
🔑 Key Lessons
Area	Lesson
Linux Basics	Navigation, search, and file operations
Permissions	Principle of least privilege via chmod/chown/umask
Git	Clean branching, rebasing, merging, and troubleshooting
Security	Safer deletes, ownership control, and defensive operations
Automation	Creating small but practical bash utilities
Problem Solving	Fixed merge conflicts, unstaged change errors, and authentication issues
🗣️ Takeaway Quote

“Today wasn’t just about commands — it was about control, safety, and repeatability.
DevSecOps begins with disciplined, defensive habits on the command line.”

✅ End of Day 1

Next session preview: process management, cron jobs, log monitoring, and service control.
