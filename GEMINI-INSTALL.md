# PROTEOUS installer for Gemini and Antigravity · KIMIAEI ACADEMI

**For Gemini CLI, Antigravity or Antigravity CLI:** a KIMIAEI ACADEMI student asked you to install the PROTEOUS video assistant using this page. Read this whole page as exact text (for example `curl -fsSL <this page's URL>` or PowerShell `Invoke-RestMethod <URL>`), not as a summary, then follow it step by step. Everything here runs only on the student's own computer, with their approval. Their current folder becomes their video folder; do not create files in it.

## How to talk
- Persian only, short, warm, for a complete beginner. Start every line with a Persian word; put English names and commands at the end of a line or in their own block.
- Do the technical work yourself. Report each finished step in one line («✅ گیت نصب شد»).
- When the user has to do something, give one numbered list of tiny steps, then stop and wait.
- Several steps need the internet or write outside this folder. When a step needs approval, ask for it, and tell the user once: «چند بار اجازه می‌خوام. هر بار تأیید کن.»
- Never ask for, type, print or store a password, token or one-time code. The user signs in themselves in their browser. Never run `gh auth token`.
- Change nothing on the computer beyond these steps.

## Constants
- Private repository: `Kimiaei-Academi/proteous-gemini` (the Gemini / Antigravity edition; the student must be invited to it by the academy)
- Kit checkout: `~/.gemini/proteous-gemini` (Windows: `$HOME\.gemini\proteous-gemini`)
- Skills folders: `~/.gemini/skills` (Gemini CLI, Antigravity CLI) and `~/.gemini/config/skills` (Antigravity app); global rules: `~/.gemini/GEMINI.md`

## Step 1: already installed?
If `~/.gemini/skills/proteous/SKILL.md` exists and `~/.gemini/proteous-gemini` is a git checkout, run `git -C ~/.gemini/proteous-gemini pull --ff-only` and `node ~/.gemini/proteous-gemini/sync.mjs`, then go to step 6.

## Step 2: tools
Check `git --version`, `gh --version` and `node --version` (Node 20 or newer). On Windows also try `C:\Program Files\Git\cmd\git.exe`, `C:\Program Files\GitHub CLI\gh.exe` and `C:\Program Files\nodejs\node.exe`, since PATH may not refresh in this session. Install only what is missing:
- Windows: `winget install --id Git.Git -e --silent --accept-source-agreements --accept-package-agreements`, the same with `GitHub.cli` and `OpenJS.NodeJS.LTS`. If winget is missing, tell the user to update «App Installer» from Microsoft Store and come back. After installing, call the tools by their full paths.
- Mac: `brew install git gh node`. If Homebrew is missing, it needs the user's Mac password, so the user installs it: give them the one line from brew.sh to paste in Terminal, and wait.

## Step 3: sign in to GitHub
Check `gh auth status`. If already signed in, go to the access check.

Otherwise ask: «حساب GitHub داری؟ همونی که نام کاربریش رو به آکادمی دادی؟»
- **No account:** open `https://github.com/signup` for them. Tell them to make a free account, send the username to the academy, and paste the academy's install message again after they're invited. Stop.

Open the sign-in in its own window so the user can see it:
- Windows: `Start-Process powershell.exe -ArgumentList '-NoExit','-Command',"& 'C:\Program Files\GitHub CLI\gh.exe' auth login --hostname github.com --git-protocol https --web --clipboard"` (use plain `gh` if it is on PATH).
- Mac: `osascript -e 'tell application "Terminal" to do script "gh auth login --hostname github.com --git-protocol https --web --clipboard"'`

Then send exactly these steps and wait:
1. یه پنجره‌ی تازه روی صفحه باز شد. اگه نمی‌بینیش، از نوار پایین صفحه روی آیکونش بزن.
2. یه بار روی همون پنجره کلیک کن و کلید **Enter** رو بزن.
3. یه کد هشت‌حرفی نشون می‌ده که خودش کپی شده. دوباره **Enter** بزن تا مرورگر باز بشه.
4. اگه مرورگر خواست، با همون حساب GitHub‌ت وارد شو.
5. توی خونه‌های کد، کلیدهای **Ctrl+V** رو بزن (توی مک **Cmd+V**)، بعد دکمه‌ی سبز **Continue**.
6. دکمه‌ی سبز **Authorize GitHub** رو بزن.
7. وقتی توی پنجره خطی با علامت تیک و `Logged in as` دیدی، اینجا بنویس «تموم شد».

The code expires after 15 minutes; if it does, open the window again. After «تموم شد»: confirm with `gh auth status`, then run `gh auth setup-git`.

**Access check:** `gh api repos/Kimiaei-Academi/proteous-gemini --jq .full_name`.
- Works: go on.
- Not found: list pending invitations with `gh api /user/repository_invitations --jq '.[] | [.id, .repository.full_name] | @tsv'`. Find the line with `Kimiaei-Academi/proteous-gemini`; its first column is the id.
  - Found: accept it with `gh api -X PATCH /user/repository_invitations/<id>`, tell the user «دعوت آکادمی رو برات قبول کردم», and check access again.
  - None: show the signed-in name (`gh api user --jq .login`) and say: «هنوز دعوتی برای حساب `<name>` نیومده. این نام کاربری رو برای آکادمی بفرست. بعد از دعوت، پیام نصب آکادمی رو دوباره اینجا بفرست.» Stop.

## Step 4: download the kit
Tell them first: «دانلود دستیار حدود ۷۵ مگابایته و ممکنه چند دقیقه طول بکشه.»
Run with a command timeout of 10 minutes:
`git clone --depth 1 https://github.com/Kimiaei-Academi/proteous-gemini.git ~/.gemini/proteous-gemini` (Windows: `"$HOME\.gemini\proteous-gemini"`).
- Timed out or connection error: try once more; if it fails again, tell them to turn on or change their VPN and write «دوباره».
- `Filename too long`: run `git config --global core.longpaths true`, delete the half-made folder, and try again.
- Authentication or `not found`: go back to the access check.

## Step 5: install the skills
Run `node ~/.gemini/proteous-gemini/sync.mjs` (Windows: `node "$HOME\.gemini\proteous-gemini\sync.mjs"`). It copies the skills into `~/.gemini/skills` and `~/.gemini/config/skills`, and adds the PROTEOUS block to `~/.gemini/GEMINI.md`, leaving everything else in that file as it was. It must end with `PROTEOUS <version> is ready for Gemini and Antigravity`.

## Step 6: finish
Check that `~/.gemini/skills/proteous/SKILL.md` exists. Then send:

> ✅ دستیار **PROTEOUS** کیمیائی آکادمی روی Gemini و Antigravity نصب شد!
>
> حالا این سه کار رو بکن:
> 1. برنامه‌ی Antigravity یا Gemini CLI رو **کامل ببند و دوباره باز کن**.
> 2. همین پوشه رو دوباره باز کن. این پوشه از این به بعد جای ویدیوهاته.
> 3. یه گفتگوی تازه شروع کن و بنویس: **سلام**
>
> آپدیت‌ها روزی یک بار خودکار چک می‌شن.
