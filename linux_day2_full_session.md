📝 File name:

linux_day2_template.md

# 🧭 DevSecOps Bootcamp — Day 2 Session Summary

## 🗓️ Date
**[Enter date here]**

---

## 🧩 Overview
**Theme:** [Briefly describe today’s focus — e.g., Process Management, Scheduling, Logging, or Networking Fundamentals]  

Today’s session expanded on Linux and DevSecOps foundations with a focus on [insert main topic].  
You’ll continue building hands-on fluency and secure operational habits.

---

## ⚙️ PART A — Main Concepts / Practice Area

### 🔹 Commands Practiced
```bash
# Paste commands used in this section

🧠 What I Learned

[Explain what each command or set of commands did and why it matters]

[Note any relationships between today’s and previous lessons]

[Include optional output examples or screenshots]

🪲 Issues or Fixes

[Describe any errors encountered and how you fixed them]

Example:

error: permission denied
fix: used sudo or adjusted chmod to grant execution rights

🧾 Log Entry
echo "$(date) — practiced [topic]" >> ../linux_day2_notes.txt

🌿 PART B — Git Workflow Practice
🔹 Steps
git status
git pull --rebase origin main
git checkout -b feature/[short-topic-name]
# ... edits, additions ...
git add .
git commit -m "Add [description of change]"
git push -u origin feature/[short-topic-name]

💡 Pull Request

On GitHub:

Open PR from feature/[branch-name] → main

Title: “[Day 2]: [Summary of work]”

Description: 1–2 lines explaining what changed

🔹 Merge & Cleanup
git checkout main
git pull --rebase origin main
git branch -d feature/[branch-name]

🪲 Git Issues & Fixes
Issue	Cause	Solution
Merge conflict	File edited both locally and remotely	Open file, resolve conflict markers, git add, git rebase --continue
Unstaged changes	Edited file not yet added	git add . then commit
Authentication issues	Missing SSH key	Confirm ssh -T git@github.com works
👥 PART C — Secondary Focus / Lab Exercise
🔹 Example Topics

Process management (ps, top, kill)

Job scheduling (cron, at)

Logging (tail, less, /var/log/)

Networking basics (ping, netstat, ss, curl)

🧰 Commands Used
# Add your command list here

📘 Notes

[Summarize how you verified processes, managed jobs, or inspected logs]

[Mention security implications — e.g., avoiding privilege misuse or monitoring attacks]

🧑‍💻 PART D — Mini Automation Script
🔹 Script
cat > [script_name].sh << 'EOF'
#!/usr/bin/env bash
# [Describe what this script does]
EOF

chmod +x [script_name].sh
./[script_name].sh

🔹 Commit & Push
git add [script_name].sh [any_output_files]
git commit -m "Add [description] mini script and update notes"
git push

🧩 PART E — Troubleshooting Log
Issue	Command/Error	Root Cause	Resolution	Verification
				

Example:
| File permission error | bash: ./script.sh: Permission denied | Missing execute bit | chmod +x script.sh | Script runs successfully |

🧠 Reflection
🔑 Key Lessons
Area	Lesson
Linux	[Core concept learned]
Security	[Security insight or safer practice learned]
Git	[New branching/merging skill learned]
Automation	[What you scripted or automated]
Problem Solving	[A bug you diagnosed/fixed]
🗣️ Takeaway Quote

“Write a one-line reflection here — something you realized or mastered today.”

✅ End of Day 2

Next session preview: [Briefly describe upcoming topic or goal — e.g., “Intro to Cloud CLI tools and environment hardening.”]
