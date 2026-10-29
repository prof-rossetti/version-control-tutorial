# Using Claude Code for Version Control

Claude Code is Anthropic's agentic coding tool. Instead of typing git commands yourself, you describe what you want in plain English ("commit my changes with a descriptive message") and Claude Code runs the commands for you, asking permission before it changes anything. This appendix shows how to install it, set it up, and use it for both the **Version Control Workflow** and the **Pull Request Workflow** exercises.

> [!IMPORTANT]
> **How GitHub steps are marked.** Anything that touches GitHub (creating or cloning a repo, pushing, pull requests, merging) is flagged with a callout like this one, so you can tell the Claude Code and git parts apart from the GitHub parts.

---

## Installation

**What you need first:** a terminal, a project folder to work in, and a Claude subscription (Pro, Max, Team, or Enterprise), a Claude Console account, or access through a supported cloud provider. On native Windows, [Git for Windows](https://git-scm.com/downloads/win) is recommended so Claude Code can use its Bash tool (without it, Claude Code uses PowerShell instead).

Claude Code runs in several places: the terminal (CLI), a Desktop app, VS Code, JetBrains IDEs, the web, and mobile. This tutorial uses the **CLI** because it is the most complete version and works the same on every operating system.

Open a terminal and run the command for your system (official page: [https://code.claude.com/docs/en/quickstart](https://code.claude.com/docs/en/quickstart)):

```bash
# macOS, Linux, WSL
curl -fsSL https://claude.ai/install.sh | bash
```

```powershell
# Windows PowerShell
irm https://claude.ai/install.ps1 | iex
```

The installer prints no progress while it downloads, so give it a minute. When it finishes, **open a new terminal window** and confirm it worked:

```bash
claude --version
```

A working install prints a version number followed by `(Claude Code)`. If your shell says `claude` is not found, the install folder is not on your PATH yet; see the [troubleshooting page](https://code.claude.com/docs/en/troubleshoot-install.md#command-not-found-claude-after-installation).

<img src="docs/images/install.png" alt="Claude Code install check" width="600">

---

## Configuration

### Sign in to Claude Code

Open your terminal in any project folder and start Claude Code:

```bash
cd /path/to/your/project
claude
```

On first use it prompts you to log in. Follow the prompts and finish authentication in your browser. Your credentials are stored, so you only do this once. To switch accounts later, type `/login` inside a running session.

<img src="docs/images/login-1.png" alt="Claude Code login prompt" width="600">

<img src="docs/images/login-2.png" alt="Claude Code login confirmation" width="600">

### Understand permissions

By default, Claude Code asks before it does anything that could change your system: writing files or running Bash commands (which includes git). This is what keeps you in control, so read each prompt and approve with **Yes**. Press `Shift+Tab` at any time to cycle the session's permission mode.

You can allow permissions from inside Claude Code with /permissions and a plain-language request, such as /permissions allow all git functions in this session. Rules added this way last only for the current session. For permanent rules, edit a settings file: ~/.claude/settings.json applies to every session on the machine, and a project's .claude/settings.json applies to that project. Shift+Tab cycles the session's permission mode. Otherwise, you can approve each request with Yes as it comes up.

### Optional: write a CLAUDE.md for your repo

`CLAUDE.md` is a file Claude reads at the start of every conversation. Run `/init` to generate a starter one, then add short notes about repo etiquette, such as your branch naming and pull request conventions. Keep it brief: if removing a line would not cause Claude to make mistakes, cut it.

```markdown
# Workflow
- Name feature branches like new-feature-123
- Write commit messages in the imperative ("Update README contents")
```

### Link your GitHub account

GitHub side Claude Code runs git for you, but it needs credentials to talk to GitHub. The standard way is the **GitHub CLI (`gh`)**. Claude knows how to use `gh` for opening pull requests, creating issues, and reading comments. Without it, Claude can still hit the GitHub API, but unauthenticated requests often run into rate limits.

> 1. Install `gh` from [https://cli.github.com](https://cli.github.com/) (or `brew install gh` on macOS).
> 2. Log in: `gh auth login` (choose GitHub.com, then follow the browser prompts).
> 3. Check it worked: `gh auth status`
> 4. Make plain `git push` use those credentials: `gh auth setup-git`
>
> _Source for the gh recommendation: the Best practices page. The exact `gh` steps above come from the GitHub CLI docs; double-check them there._

<img src="docs/images/gh-auth-1.png" alt="GitHub CLI setup" width="456">
<img src="docs/images/gh-auth-2.png" alt="GitHub CLI status" width="460">

---

## Usage

Tip for every prompt below: be specific. "Commit my changes with the message 'Update README contents'" works better than "save my work." Afterward, ask Claude to show evidence, such as the output of `git status` or `git log`, rather than just trusting it said "done."

### Version Control Workflow

This follows the "Version Control Workflow" exercise: create a repo, clone it, edit the README, commit, and push.

**1. Create the remote repository.**

> [!IMPORTANT]
> **GitHub side.** On GitHub, create a new repository named something like `my-first-repo` and include a `README.md`. Or from a terminal with `gh`:
>
> ```bash
> gh repo create my-first-repo --public --add-readme --clone
> ```
>
> The `--clone` flag also does step 2 below.

**2. Clone it to your computer.**

> [!IMPORTANT]
> **GitHub side.** If you created the repo on the GitHub website, clone it (replace `YOUR_USERNAME`):
>
> ```bash
> git clone git@github.com:YOUR_USERNAME/my-first-repo.git
> ```

**3. Start Claude Code inside the repo.**

```bash
cd my-first-repo
claude
```

**4. Edit the README.** Ask Claude Code to make the change:

```text
Replace the contents of README.md with a heading "My First Repo!", a sentence
saying this is the README.md file and it uses the markdown language, and a list
with Item 1, Item 2, and Item 3.
```

Claude shows you the change and asks permission. Approve it.

<img src="docs/images/edit-readme.png" alt="Claude Code editing the README" width="588">

**5. Commit.**

```text
What files have I changed?
```

```text
Commit my changes with the message "Update README contents"
```

<img src="docs/images/commit.png" alt="Claude Code commit" width="600">

**6. Push.**

> [!IMPORTANT]
> **GitHub side.**
>
> ```text
> Push the commit to origin main
> ```
>
> Then refresh the repo page on GitHub and confirm the new README shows up. That is the success criterion for this exercise. Repeat the edit, commit, and push at least once more.

<img src="docs/images/push.png" alt="Claude Code push" width="600">

### Pull Request Workflow

This follows the "Pull Request Workflow" exercise. The idea: never commit directly to `main`; make changes on a feature branch, then merge them through a pull request. Start Claude Code in your repo folder, then work through the steps below.

**3. Sync your local `main`.**

> [!IMPORTANT]
> **GitHub side.**
>
> ```text
> Check out the main branch, then fetch and pull the latest changes from origin
> ```

**4. Create a feature branch.**

```text
Create a new branch called new-feature-123 and switch to it
```

**5. Make a change and commit it.** For anything bigger than a trivial edit, you can ask Claude to plan first (press `Shift+Tab` until the status bar shows plan mode, or start with `claude --permission-mode plan`). Plan mode lets Claude read files and propose a plan without changing anything. For a small change like a README tweak, skip the plan and just ask:

```text
Add a "Contributing" section to the README, then commit it with a descriptive message
```

**Optional but useful: get a review before you open the PR.** Run the bundled `/code-review` command in your session. It reviews your current changes for bugs using a fresh subagent and reports findings back, and it works on any plan.

> [!NOTE]
> **About the managed "Code Review" service.** Anthropic also offers a separate **Code Review** service that automatically reviews every pull request on GitHub. It requires a Team or Enterprise plan and an organization Owner to install the Claude GitHub App, so most students will not have access. See [https://code.claude.com/docs/en/code-review](https://code.claude.com/docs/en/code-review).

**6. Push the feature branch.**

> [!IMPORTANT]
> **GitHub side.**
>
> ```text
> Push the new-feature-123 branch to origin
> ```

**7. Create the pull request.**

> [!IMPORTANT]
> **GitHub side.**
>
> ```text
> Open a pull request from new-feature-123 into main with a short description of the change
> ```
>
> Claude Code uses `gh pr create` for this, which is why linking `gh` in the Configuration section matters. Equivalent prompt from the Best practices page: _"commit with a descriptive message and open a PR."_

<img src="docs/images/pr.png" alt="Pull request creation" width="600">

**8. Review the changes and comment.**

> [!IMPORTANT]
> **GitHub side.** Open the pull request on GitHub, look at the **Files changed** tab, and optionally leave a comment. This step is done in the GitHub web interface.

**9. Merge.**

> [!IMPORTANT]
> **GitHub side.** When you are satisfied, click **Merge pull request** in the GitHub interface. To repeat the workflow, start again at step 3.

<img src="docs/images/merge.png" alt="Merging the pull request" width="600">

### Contributing this appendix to the tutorial repo

> [!IMPORTANT]
> **GitHub side.** This is the same Pull Request Workflow, but against someone else's repo, so you start by forking it:
>
> 1. Fork the course repo and clone your fork. With `gh`: `gh repo fork OWNER/REPO --clone` (use the real owner and repo name from the repo's GitHub URL).
> 2. Create a feature branch, such as `add-claude-code-appendix`.
> 3. Add this markdown file in the directory the instructor specifies and the screenshots in the `images/` directory.
> 4. Commit, push the branch to your fork, and open a pull request against the instructor's repo.
>
> _Updating `quarto.yml` and previewing the site locally come later, per the instructor._

---

## Tips and common pitfalls

- **Give Claude a way to verify its work.** After a commit or push, ask it to show `git status` or `git log` so you see evidence, not just a claim of success.
- **Undo mistakes early.** Press `Esc` to stop Claude mid-action, or say "undo that." `/rewind` can restore earlier conversation and code state.
- **Checkpoints are not git.** Rewind only tracks edits made through Claude's file tools, not changes made by Bash commands, so git is still your real safety net.
- **Use `/clear` between unrelated tasks.** Claude's context window fills up, and long, cluttered sessions make it perform worse. If you have corrected it twice on the same issue, clear and write a better first prompt.
- **Skip planning for tiny changes.** If you could describe the whole diff in one sentence, just ask Claude to do it.

## Going further

Claude Code also runs in a Desktop app (with a visual diff viewer), VS Code, JetBrains IDEs, on the web, and on mobile, and integrates with GitHub Actions for automated pull request reviews in CI. Overview: [https://code.claude.com/docs/en/platforms](https://code.claude.com/docs/en/platforms)

## Sources

- Quickstart: [https://code.claude.com/docs/en/quickstart](https://code.claude.com/docs/en/quickstart)
- Platforms and integrations: [https://code.claude.com/docs/en/platforms](https://code.claude.com/docs/en/platforms)
- Code Review: [https://code.claude.com/docs/en/code-review](https://code.claude.com/docs/en/code-review)
- Best practices: [https://code.claude.com/docs/en/best-practices](https://code.claude.com/docs/en/best-practices)
