# Updating sw_os - Explained Simply

**Last Updated:** January 29, 2026

This guide explains how to get new features and improvements for sw_os, written for people who've never used GitHub or command-line tools before.

---

## The Big Picture: Why Updates Matter

**What are updates?**

Think of sw_os like an app on your phone. Every few weeks, there are improvements:
- New features that make your work easier
- Bugs get fixed
- Things run smoother

The difference: sw_os lives on your computer (not in an app store), so updating works differently.

**What makes sw_os updates safe:**

Your data is **completely separate** from the sw_os application. Updates only touch the application - never your notes, tasks, or projects.

It's like updating Microsoft Word. The app gets better, but your documents stay exactly as they are.

**Your customizations are preserved too:**
- **CLAUDE.md personal notes:** Anything between `USER_EXTENSIONS_START/END` is preserved during updates
- **Custom MCP servers:** Name them with `user-` or `custom-` (e.g., `user-gmail`) so updates always keep them

---

## What You Need to Know First

### You Have Two Versions of sw_os

When you use sw_os, there are actually TWO versions:

1. **Your copy** - Lives on your computer
   - Your notes, tasks, projects
   - Your customizations
   - Your daily work

2. **The main version** - Lives on GitHub
   - New features being added
   - Bug fixes
   - Improvements from other users

**Updating means:** Getting the improvements from the main version and adding them to your copy, without touching your data.

### What is GitHub?

**GitHub is like Dropbox for code.**

Just like Dropbox stores your files in the cloud, GitHub stores sw_os's files. But GitHub has special features that help:
- Track every change made to sw_os
- Let you download improvements
- Keep your version and the main version in sync

**Do you need a GitHub account?**

Not for basic updates! You can:
- ✓ Check for updates (no account needed)
- ✓ Download updates (no account needed)
- ✓ Use all of sw_os (no account needed)

**When you DO need an account:**
- If you want to save your copy of sw_os on GitHub (like a backup)
- If you want to share improvements you make with others
- If you want to report problems

**Creating a GitHub account is free** and takes 2 minutes: [github.com/signup](https://github.com/signup)

But again - **not required for updating sw_os**.

### What is Git?

**Git is the tool that handles updates.**

Think of it like this:
- **GitHub** = The website (like Dropbox.com)
- **Git** = The tool on your computer that talks to GitHub (like the Dropbox app)

**Do you have Git?**

Probably yes! If you followed the setup, you already have it.

To check, you'll type one command later. Don't worry - we'll tell you exactly what to do.

---

## Updating sw_os: The Simple Way

### Step 1: Run the Update Command

**In Cursor, in the chat where you talk to sw_os, type:**

```
/dex-update
```

**Press Enter.**

**What happens:**

sw_os connects to GitHub, checks if there's a newer version, and shows you what's available.

You'll see one of two things:

**Option A: You're up to date**
```
✅ You're already on the latest version (v1.2.0)

No update needed!
```

Great! You're done. Come back in a few weeks.

**Option B: An update is available**
```
🎁 sw_os v1.3.0 is available

You're on: v1.2.0
Latest: v1.3.0

What's new:
- Career coach improvements
- Task deduplication fix
- Meeting intelligence enhancement

[View full release notes]
[Update now]
[Cancel]
```

**What this means:**
- Someone (probably me) made sw_os better
- Version 1.3.0 has new stuff
- You can get it by clicking [Update now]

**Your choices:**

1. **[Update now]** - Proceeds with the update (recommended)
2. **[View full release notes]** - Opens GitHub to see all details
3. **[Cancel]** - Exit without updating (you can run `/dex-update` again later)

**Should you update?**

Usually yes! Updates make things better. But you can cancel if:
- You're in the middle of something urgent
- You want to read the full release notes first
- You want to wait and see if others have problems

**Note about "breaking changes":**

Sometimes you'll see: ⚠️ **BREAKING CHANGES**

This means the update changes how something works in a big way. These are rare and come with extra instructions. Don't worry - the update process will guide you.

---

### Step 2: Confirm and Update

**If you clicked [Update now], here's what happens next:**

```
/dex-update
```

**Press Enter.**

**What happens next:**

sw_os will walk you through the update. Here's what to expect:

---

#### A. sw_os Checks If You Have Git

**You'll see:**
```
✓ Git detected
```

**If you see this instead:**
```
❌ Git not detected

sw_os updates require Git. Here's how to install...
```

Don't panic! Follow the simple instructions. It's a one-time thing:

**On Mac:**
1. Open Terminal (press Cmd+Space, type "Terminal", press Enter)
2. Type: `xcode-select --install`
3. Press Enter
4. Click "Install" when a window pops up
5. Wait 5 minutes for download
6. Come back to Cursor and try `/dex-update` again

**On Windows:**
1. Go to: https://git-scm.com/download/win
2. Click the download link
3. Run the installer (click Next on everything)
4. Restart Cursor
5. Try `/dex-update` again

This only happens once. After this, updates are automatic.

---

#### B. sw_os Checks Your Setup

**You'll see:**
```
✓ Setup verified
```

If this is your first time updating, sw_os might configure a few things behind the scenes. This is normal and automatic.

**What's actually happening:**

sw_os is telling Git where to look for updates (on GitHub). Think of it like saving a bookmark in your browser.

---

#### C. sw_os Downloads the Updates

**You'll see:**
```
⬇️ Downloading updates from GitHub...
✓ Updates downloaded
```

**What's happening:**

sw_os is getting the new files from GitHub and bringing them to your computer.

**This requires internet.** If you see an error about network, check your WiFi and try again.

---

#### D. sw_os Protects Your Work

**You'll see:**
```
💾 Saving your work...
✓ Your work is saved
```

**What's happening:**

Before changing anything, sw_os saves a snapshot of how things are right now. It's like creating a restore point on your computer.

**Why this matters:**

If something goes wrong (very rare), you can undo the update instantly with `/dex-rollback`.

This makes updating completely safe. You can always go back.

---

#### E. sw_os Applies the Updates

**You'll see:**
```
🔄 Applying updates...
```

**What's happening:**

sw_os is carefully updating its own files (the application) while leaving your files (your data) completely alone.

**Two possible outcomes:**

**Outcome 1: Clean update (most common)**
```
✓ Updates applied successfully
```

This means the update fit perfectly with your setup. No problems. This happens 95% of the time.

**Outcome 2: Overlapping changes (rare)**

Sometimes both you AND the main version changed the same file. For example:
- You customized a skill
- The update also changed that skill

**You'll see:**
```
🔍 Resolving overlapping changes...

Found changes in the same files:
- CLAUDE.md (your version vs update version)

Automatically keeping: Your customizations
Automatically updating: Core sw_os features

✓ Resolved automatically
```

**What sw_os does automatically:**

For each file with overlapping changes:

| File Type | What sw_os Keeps |
|-----------|----------------|
| Your notes, tasks, projects | **Your version (always)** |
| Your settings (user-profile, pillars) | **Your version (always)** |
| Core sw_os skills | **Update version (improvements)** |
| Core sw_os features | **Update version (improvements)** |
| Files you customized | **Your version (your changes)** |

**Translation:** Your stuff stays yours. sw_os improvements come through. Conflicts resolved automatically.

**You don't need to do anything.** sw_os handles this.

---

#### F. sw_os Updates Dependencies

**You'll see:**
```
📦 Updating dependencies...
✓ Dependencies updated
```

**What are dependencies?**

Think of sw_os like a car. The car (sw_os) needs certain parts to work: tires, battery, engine.

Dependencies are those parts. When sw_os updates, sometimes it needs newer parts.

**What's happening:**

sw_os is automatically installing or updating these parts. You don't need to do anything.

This takes 30 seconds to 2 minutes depending on your internet.

---

#### G. sw_os Verifies Everything

**You'll see:**
```
✓ Update complete! Now testing...
✅ Update successful!
```

**What's happening:**

sw_os is checking that everything works:
- Can it find your tasks?
- Can it read your profile?
- Are all the features working?

If anything fails, sw_os will offer to undo the update automatically.

---

### Step 3: Confirmation

**You'll see a summary:**

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ sw_os Updated: v1.2.0 → v1.3.0
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

What's new:
• Career coach improvements
• Task deduplication fix
• Meeting intelligence enhancement

Your data:
✓ All notes preserved
✓ All tasks preserved
✓ All customizations preserved

Time taken: 2 minutes
```

**You're done!** The new features are ready to use.

---

### Step 4: Try It Out

**Test that everything works:**

1. Run your daily plan:
   ```
   /daily-plan
   ```

2. Open a person page:
   - Go to `05-Areas/People/` and open someone

3. Check your tasks:
   - Open `03-Tasks/Tasks.md`

Everything should work normally, plus you'll have the new features from the update.

---

## If Something Goes Wrong

### The Instant Undo Button

**If anything feels wrong after updating, type:**

```
/dex-rollback
```

**What happens:**

sw_os instantly restores everything to exactly how it was before you updated.

**How long it takes:** 30 seconds

**What gets restored:**
- ✓ sw_os features go back to old version
- ✓ All your notes, tasks, projects stay as they are

**Translation:** You tried the update, didn't like it, and now you're back to the old version. Your work is completely safe.

You can try updating again later, or just stay on the old version.

---

## Understanding How This Works (Optional)

*This section is for people who want to understand what's happening behind the scenes. You can skip this if you're happy just using the commands.*

### Why GitHub?

**sw_os is "open source"** - this means:
- Anyone can see how it works
- Anyone can improve it
- It's free forever
- You're not locked into a company

GitHub is where open source projects live. It's like a public workshop where improvements happen.

**Your copy of sw_os is completely yours.** It's on your computer, not on GitHub. But GitHub is where the "official" version lives, with all the latest improvements.

### What Git Actually Does

When you run `/dex-update`, here's what Git does:

1. **Looks at GitHub** - "What's new since I last checked?"

2. **Downloads the changes** - "Let me grab those new features"

3. **Merges them into your copy** - "Let me carefully add these without touching your data"

**Why is this better than just re-downloading sw_os?**

If you re-downloaded sw_os, you'd have to:
- Delete your old version
- Download new version
- Copy all your files over manually
- Hope you didn't miss anything

Git does all this automatically in 2 minutes, without you lifting a finger.

### What "Version Control" Means

Git is a "version control" system. Think of it like Track Changes in Microsoft Word, but for an entire application.

Every time someone improves sw_os:
- Git records exactly what changed
- Git can show you the difference
- Git can combine improvements from multiple people
- Git can undo changes if something breaks

**For you, this means:**
- Updates are precise (only what changed gets updated)
- Your customizations are preserved (Git knows what you changed)
- Rollback is instant (Git remembers previous versions)

### The "Upstream" Concept

When you first downloaded sw_os, you downloaded it from GitHub. That GitHub location is called the "upstream" source.

Think of it like a river:
- **Upstream** = Where the water comes from (main sw_os on GitHub)
- **Your location** = Where you are (sw_os on your computer)

Updates flow downstream to you.

`/dex-update` is just asking: "What's new upstream? Let me get it."

### Why Your Data Is Safe

Your data is in folders that Git **ignores**:
- `00-Inbox/`
- `03-Tasks/`
- `04-Projects/`
- `05-Areas/`
- All your notes and work

Git is told: "Never track these folders. Never change them. Never touch them."

It's in a file called `.gitignore` that explicitly lists everything to ignore.

So when updates come in, Git sees your data and thinks: "Not my job, skip these."

This is why updates can't hurt your data. Git literally doesn't see it.

---

## Automatic Update Checks

**You don't have to remember to check for updates.**

Every 7 days, during `/daily-plan`, sw_os automatically checks GitHub and tells you if an update is available:

```
🎁 sw_os v1.3.0 is available. Run /dex-update to see what's new and update.

---

Here's your plan for today...
```

It's non-intrusive - just a heads up at the top of your daily plan.

**You decide when to update.** sw_os will never update itself without you asking. Running `/dex-update` shows you what's new and asks for confirmation before proceeding.

**Want to disable these notifications?**

Edit `System/user-profile.yaml` and add:

```yaml
updates:
  auto_check: false
```

---

## FAQs for Non-Technical Users

### Do I need to update?

Not required, but recommended. Updates make things better:
- New features
- Bug fixes
- Performance improvements

Your current version will keep working fine. But you miss out on improvements.

**Good rule:** Update monthly, or when you see a feature you want.

---

### How often do updates come out?

Every few weeks to every few months, depending on what's being improved.

You'll know when they're available because of the automatic check during `/daily-plan`.

---

### Will I lose my work?

**No. Never. Impossible.**

Your work is in protected folders that updates don't touch. It's technically impossible for an update to delete your notes, tasks, or projects.

The worst that can happen: A new feature doesn't work right. But your data is always safe.

And if anything breaks, `/dex-rollback` undoes the update in 30 seconds.

---

### What if my internet is slow?

The download step might take 3-5 minutes instead of 30 seconds. But it will complete.

The update process is patient - it won't time out.

---

### Can I skip an update?

Yes! If v1.3.0 comes out and you don't update, then v1.4.0 comes out, you can jump straight from v1.2.0 to v1.4.0.

Git handles this automatically.

---

### What are "breaking changes"?

Very rarely (maybe once a year), an update changes something in a big way. Examples:
- Renaming a major folder
- Changing how a feature works
- Updating the configuration format

These are marked with ⚠️ **BREAKING CHANGES** in the release notes.

**What to do:**

The update process will guide you with extra steps (usually just running one extra command). It's still safe and still has rollback.

Breaking changes come with:
- Clear explanation of what's changing
- Why it's changing
- Step-by-step migration instructions
- Extra safety checks

Don't avoid them - just read the notes carefully first.

---

### Can I see what will change before updating?

Yes! When you run:
```
/dex-update
```

sw_os shows you the release notes and asks for confirmation before proceeding. You can:
- Read what's new
- View full release notes on GitHub
- Choose [Update now] or [Cancel]

You're never forced to update - you always confirm first.

---

### What if I made changes to sw_os itself?

**Customizations in recommended places:**
- `.claude/skills-custom/` - Your custom skills
- `core/mcp-custom/` - Your custom integrations
- `CLAUDE-custom.md` - Your prompt customizations

These are protected. Updates won't touch them.

**Changes to core sw_os files:**

If you edited a core file (like `.claude/skills/daily-plan/SKILL.md`), and an update also changes that file, sw_os will:
1. Detect the overlap
2. Ask which version to keep
   - If AskUserQuestion is available, sw_os shows a guided choice
   - If not, sw_os shows a CLI prompt with the same options + tradeoffs
3. Usually keep your version (your customizations)

But **better practice:** Put your customizations in the `-custom` folders so they never conflict.

---

### Can I undo an undo?

Yes! When you run `/dex-rollback`, sw_os saves a snapshot before rolling back.

So if you rollback and then think "wait, I want the update back," you can restore it.

The rollback skill explains this.

---

### What if the update fails halfway through?

sw_os is careful about this. If something fails:

1. The update stops immediately
2. sw_os offers to restore to before the update
3. You're back to where you started (safe)
4. You can report the problem or try again later

**You're never left in a broken state.** Either the update completes successfully, or it rolls back automatically.

---

## Different Ways to Update (Summary)

### Option 1: Automatic (Recommended)
```
/dex-update
```
- Easiest
- Safest
- Handles everything
- **Use this method**

### Option 2: Manual (Advanced Users Only)

If you're comfortable with command-line tools and want to see exactly what's changing:

```bash
cd ~/Documents/dex
git fetch upstream
git merge upstream/main
```

Only use this if you understand Git. Everyone else should use `/dex-update`.

### Option 3: Re-download (Last Resort)

If Git isn't working or you just want a fresh start:

1. Download latest sw_os from GitHub as a ZIP
2. Copy your data folders (00-07, System/) from old to new
3. Delete old sw_os folder
4. Rename new folder to 'dex'
5. Open in Cursor

This works but takes 10-15 minutes vs. 2 minutes for `/dex-update`.

---

## Getting Help

### Something Broke and Rollback Didn't Fix It

1. **Don't panic** - Your data is safe (it's in separate folders)

2. **Report the issue:**
   - Go to: https://github.com/davekilleen/dex/issues
   - Click "New Issue"
   - Describe what happened and what you see

3. **Temporary workaround:**
   - Re-download sw_os
   - Copy your data folders back
   - Continue working while we fix the bug

### Questions About What Changed

- Read release notes: https://github.com/davekilleen/dex/releases
- Or run: `/dex-update` (shows release notes before asking to proceed)

### Want to Learn More About Git

- GitHub's guide: https://guides.github.com/introduction/git-handbook/
- Git for non-programmers: https://www.youtube.com/watch?v=8JJ101D3knE

But remember: **You don't need to learn Git to use sw_os.** The `/dex-update` command handles it all.

---

## Summary: The 2-Minute Update

**Update sw_os:**
```
/dex-update
```
Shows what's new, you confirm, it updates.

**Undo if needed:**
```
/dex-rollback
```
Instantly restores previous version.

**That's it.** Two commands. Your data is always safe. You can always undo.

Updates should improve your work, not create anxiety. If you're ever unsure, just ask in Cursor chat: "Should I update to v1.3.0?" and sw_os will explain what's new.

---

**Remember:** sw_os is yours. Update when you want, skip updates if you want, rollback if something feels off. You're in control.
