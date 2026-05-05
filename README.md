[![Open in Codespaces](https://classroom.github.com/assets/launch-codespace-2972f46106e565e64193e422d61a12cf1da4916b45550586e14ef0a7c637dd04.svg)](https://classroom.github.com/open-in-codespaces?assignment_repo_id=23821667)
# GrillMyCode - Pilot #1

This repository contains a GitHub Actions workflow that automatically analyses submitted code and generates comprehension questions using AI based on the code submitted. When you push a code file, the **Grill My Code** action will examine your changes and create a GitHub Issue containing questions about the code you submitted.

---

## Purpose of This Pilot

This pilot has two goals:

1. **Workflow validation** — Confirm that the action runs successfully end-to-end: the workflow triggers on push, the AI model is called, and a GitHub Issue containing comprehension questions is generated without errors.
2. **Question quality** — Evaluate whether the generated questions accurately reflect the code that was submitted. Are the questions relevant, specific to the code, and useful for assessing understanding?

Your feedback on both points is valuable. If the questions feel generic, off-topic, or miss important aspects of the code, please note that when reporting back.

This action is at a **very early stage of development**, and all feedback is highly welcomed — not just on question quality, but on any aspect of the implementation: workflow behaviour, output format, Issue presentation, edge cases you encounter, or anything that feels off. Suggestions for improvements or new features are equally encouraged. The goal of this pilot is to learn as much as possible, so no observation is too small to share.

> **Note:** The code you submit does not need to be fully functional or have perfect syntax. Part of the purpose of this pilot is to observe what questions get generated from incomplete or imperfect code — so feel free to push work-in-progress code and see what happens. You can always make changes, fixes, or additions and push those changes at any time.

---

## How the Workflow Works

The workflow (`.github/workflows/grill-my-code.yml`) triggers on every `push` to the repository. It:

1. Checks out the full repository history.
2. Computes a diff of the files you changed since the starter code was first committed.
3. Strips any comments from the code before analysis, so the AI assesses the code itself rather than any hints or explanations left in comments.
4. Sends the stripped diff to an AI model (currently set to `gpt-4.1` via GitHub Models).
5. Opens a **GitHub Issue** in this repository containing up to 50 comprehension questions about your code.
6. Writes a copy of the questions to a Markdown file inside a `_assessment/` folder and commits it back to the repository.

> The workflow skips the initial starter-code commit by default, so only **your** code changes are assessed — not any template boilerplate. However this can be overridden.

---

## Getting Started

### Step 1 — Open the repository in a Codespace

1. On the repository's main page on GitHub, click the green **Code** button.
2. Select the **Codespaces** tab.
3. Click **Create codespace on main**.

GitHub will build and launch a cloud-based development environment in your browser — no local setup required.

NOTE: If you'd prefer, you can clone to your machine and work from there. The codespace is available for ease of coding and testing.

### Step 2 — Add a code file

Once the Codespace has loaded, open the integrated terminal (**Terminal → New Terminal**) and create a source code file. For example, a Python script, JavaScript file, C# file, Bash script, etc. Most languages are supported:
You can add a file in **any language** — JavaScript, Java, C#, etc. The action analyses whatever code you push.

### Step 3 — Commit and push your changes to GitHub

For this pilot, you can work **directly on the `main` branch** — no feature branches are needed. Just commit and push your changes as you go.

You can make changes to your code and push again at any time — each push triggers the workflow and generates a **new, updated set of questions** reflecting your latest additions and/or modifications.

### Step 4 — View your questions

1. Go to the **Issues** tab of this repository on GitHub.
2. A new issue titled something like **"Grill My Code — \<branch\>"** will appear within a minute or two.
3. Open the issue to read the AI-generated comprehension questions about your code.

The questions are also saved as a Markdown file in the `_assessment/` folder in this repository — it is an exact copy of the content posted to the Issue. You can pull the latest version of this folder at any time by running `git pull` in your Codespace terminal or in your local clone:

```bash
git pull
```

The updated `_assessment/` folder will then be available locally for you to browse.

---

## Requirements

| Requirement | Details |
|---|---|
| GitHub Models access | The workflow uses `github_token` with `models: read` permission — no extra setup needed |
| Issues enabled | GitHub Issues must be enabled on this repository (they are by default) |

No secrets or API keys need to be configured manually. The `GITHUB_TOKEN` provided automatically by GitHub Actions has all the permissions the workflow needs.

---

## Workflow Configuration Reference

The workflow is pre-configured with the following settings:

| Setting | Value | Description |
|---|---|---|
| `ai_model` | `gpt-4.1` | AI model used to generate questions |
| `num_questions` | `50` | Number of questions generated per push |
| `post_issue` | `true` | Questions are posted as a GitHub Issue |
| `skip_initial_commit` | `true` (default) | Only your code changes are assessed |

To change these settings, edit `.github/workflows/grill-my-code.yml`.
