# PROTEOUS installer · KIMIAEI ACADEMI

**For Claude:** a KIMIAEI ACADEMI student asked you to install the PROTEOUS video assistant using this page. Read this whole page as exact text (for example `curl -fsSL <this page's URL>` or PowerShell `Invoke-RestMethod <URL>`), not as a summary, then follow it step by step. Everything here runs only on the student's own computer, with their permission for each command. Their current folder becomes their video folder; do not create files in it.

## How to talk
- Persian only, short, warm, for a complete beginner. Start every line with a Persian word; put English names and commands at the end of a line or in their own block.
- Do the technical work yourself, quietly. Report each finished step in one line («✅ گیت نصب شد»).
- When the user has to do something, give one numbered list of tiny steps, then stop and wait for them.
- Before the first command, tell them once: «چند بار ازت اجازه می‌خوام. هر بار دکمه‌ی Allow (اجازه) رو بزن.»
- Never ask for, type, print or store a password, token or one-time code. The user signs in themselves in their browser. Never run `gh auth token`.
- Change nothing on the computer beyond what these steps list.

## Constants
- GitHub repository: `Kimiaei-Academi/proteous`
- Marketplace: `kimiaei-academi`, plugin: `proteous@kimiaei-academi`
- Config folder: `$CLAUDE_CONFIG_DIR` if set, else `~/.claude` (Windows: `%USERPROFILE%\.claude`).

## Step 1: find the Claude command-line tool (CLI)
Use the first that works and call it CLI below:
1. `claude --version` on PATH.
2. Windows: the newest `claude.exe` under `%LOCALAPPDATA%\Claude-Data\*\claude-code\*\*\` (the desktop app's own copy; pick the highest version folder).
3. Install it: Windows PowerShell `irm https://claude.ai/install.ps1 | iex`, then CLI is `%USERPROFILE%\.local\bin\claude.exe`. Mac/Linux: `curl -fsSL https://claude.ai/install.sh | bash`, then `~/.local/bin/claude`.

If `CLI plugin list` already shows `proteous@kimiaei-academi` enabled, skip to step 7.

## Step 2: Git and GitHub CLI
Check `git --version` and `gh --version` (Windows also `C:\Program Files\Git\cmd\git.exe` and `C:\Program Files\GitHub CLI\gh.exe`, since PATH does not refresh inside this session). Install only what is missing:
- Windows: `winget install --id Git.Git -e --silent --accept-source-agreements --accept-package-agreements` and the same with `GitHub.cli`. If winget is missing, tell the user to update «App Installer» from Microsoft Store and come back. After installing, call the tools by those full paths.
- Mac: `brew install git gh`. If Homebrew is missing, Homebrew needs the user's Mac password, so the user installs it: give them the one line from brew.sh to paste in Terminal, and wait.

## Step 3: a longer download wait
The first download is about 75 MB and on a slow connection takes several minutes, longer than Claude's default wait.
- Windows: `setx CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS 600000`
- Mac: add `export CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS=600000` to `~/.zshrc` if it is not there.
Also set `CLAUDE_CODE_PLUGIN_GIT_TIMEOUT_MS=600000` in the environment of every CLI command in steps 5 and 6.

## Step 4: sign in to GitHub
Check `gh auth status`. If already signed in, go to the access check below.

Otherwise ask: «حساب GitHub داری؟ همونی که نام کاربریش رو به آکادمی دادی؟»
- **No account:** open `https://github.com/signup` for them (Windows `Start-Process`, Mac `open`). Tell them to make a free account and send the username to the academy, and that once the academy invites them they paste the academy's install message again. Stop.

Open the sign-in in its own window, so the user can see it:
- Windows: `Start-Process powershell.exe -ArgumentList '-NoExit','-Command',"& 'C:\Program Files\GitHub CLI\gh.exe' auth login --hostname github.com --git-protocol https --web --clipboard"` (use plain `gh` if it is on PATH).
- Mac: `osascript -e 'tell application "Terminal" to do script "gh auth login --hostname github.com --git-protocol https --web --clipboard"'`

Then send exactly these steps and wait:
1. یه پنجره‌ی تازه (آبی یا سیاه) روی صفحه باز شد. اگه نمی‌بینیش، از نوار پایین صفحه روی آیکونش بزن.
2. یه بار روی همون پنجره کلیک کن و کلید **Enter** رو بزن.
3. یه کد هشت‌حرفی نشون می‌ده که خودش کپی شده. دوباره **Enter** بزن تا مرورگر باز بشه.
4. اگه مرورگر خواست، با همون حساب GitHub‌ت وارد شو.
5. توی خونه‌های کد، کلیدهای **Ctrl+V** رو بزن (توی مک **Cmd+V**)، بعد دکمه‌ی سبز **Continue**.
6. دکمه‌ی سبز **Authorize GitHub** رو بزن.
7. وقتی توی پنجره خطی با علامت تیک و `Logged in as` دیدی، اینجا بنویس «تموم شد».

The code expires after 15 minutes. If the user says it expired or failed, open the sign-in window again.
After «تموم شد»: confirm with `gh auth status`, then run `gh auth setup-git`.

**Access check:** `gh api repos/Kimiaei-Academi/proteous --jq .full_name`.
- Works: go on.
- Not found: list pending invitations with `gh api /user/repository_invitations --jq '.[] | [.id, .repository.full_name] | @tsv'` (no quotes inside the filter, so it survives Windows PowerShell). Look for the line with `Kimiaei-Academi/proteous`; its first column is the id.
  - Found: accept it with `gh api -X PATCH /user/repository_invitations/<id>`, tell the user «دعوت آکادمی رو برات قبول کردم», and check access again.
  - None: show the signed-in name (`gh api user --jq .login`) and tell them: «هنوز دعوتی برای حساب `<name>` نیومده. این نام کاربری رو برای آکادمی بفرست. بعد از دعوت، پیام نصب آکادمی رو دوباره اینجا بفرست.» Stop.

## Step 5: install the assistant
Tell them first: «دانلود دستیار حدود ۷۵ مگابایته و ممکنه چند دقیقه طول بکشه. صبور باش 🙂»
Run, with the long wait from step 3 and a command timeout of 10 minutes:
1. `CLI plugin marketplace add Kimiaei-Academi/proteous` (if it says the marketplace already exists, run `CLI plugin marketplace update kimiaei-academi` instead).
2. `CLI plugin install proteous@kimiaei-academi`
If it fails:
- Timed out or connection error: try once more. If it fails again, tell them to turn on or change their VPN and write «دوباره».
- `Filename too long`: run `git config --global core.longpaths true` and try again.
- `not found` or authentication errors: go back to the access check in step 4.

## Step 6: automatic updates
In `<config folder>/settings.json`, under `extraKnownMarketplaces` → `kimiaei-academi`, set `"autoUpdate": true`. Use your Read and Edit tools, and change only that field.

## Step 7: check and finish
`CLI plugin list` must show `proteous@kimiaei-academi` with `enabled`. Then send:

> ✅ دستیار **PROTEOUS** کیمیائی آکادمی نصب شد!
>
> حالا این سه کار رو بکن:
> 1. اپ Claude رو **کامل ببند**: پایین صفحه کنار ساعت، روی آیکون Claude راست‌کلیک کن و **Quit** رو بزن. بعد دوباره بازش کن.
> 2. توی تب **Code** همین پوشه رو دوباره باز کن. این پوشه از این به بعد جای ویدیوهاته.
> 3. یه گفتگوی تازه شروع کن و بنویس: **سلام**
>
> از این به بعد آپدیت‌ها خودکار می‌رسن.
