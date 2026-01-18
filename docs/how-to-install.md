# How to Install Rules

This guide explains how to install and use **AI tool rules** from this repository across supported platforms.

These rules are designed to provide **consistent, high-quality AI-assisted development** by enforcing best practices, guardrails, and architectural standards per tool.

---

## General Principles

Before installing rules for any tool:

- Rules should live **inside the repository** whenever possible.
- Rules should be **version-controlled** alongside the code.
- Teams should treat rules as **code**, not documentation.
- Do not modify rules ad-hoc; changes should go through PR review.

---

## 1. Cursor

Cursor supports project-level rules via the `.cursor/rules/` directory.

### Installation Steps

1. Create the rules directory (if it does not exist):
   ```bash
   mkdir -p .cursor/rules
   ```

2. Copy the desired rule files from this repository:

   ```bash
   cp cursor/*.mdc .cursor/rules/
   ```

3. Restart Cursor or reload the project.

### Notes

* Multiple rule files can coexist.
* Rules are automatically applied when Cursor assists with code.
* Prefer **one rule per concern** (e.g., backend, frontend, security).

---

## 2. Windsurf (Codeium)

Windsurf supports Markdown-based instruction files that guide AI behavior across the project.

### Installation Steps

1. Create a Windsurf rules directory:

   ```bash
   mkdir -p windsurf
   ```

2. Copy Windsurf rule files:

   ```bash
   cp windsurf/*.mdc windsurf/
   ```

3. Open the project in Windsurf.

### Notes

* Windsurf rules are **tool-agnostic** and optimized for refactoring and large repositories.
* Avoid mixing Cursor-specific instructions in Windsurf rules.
* Keep rules concise but explicit.

---

## 3. Replit

Replit rules are optimized for **fast iteration and MVP development**.

### Installation Steps

1. Create a rules directory:

   ```bash
   mkdir -p .replit/rules
   ```

2. Copy Replit rule files:

   ```bash
   cp replit/*.mdc .replit/rules/
   ```

3. Open the project in Replit.

### Notes

* Replit environments are often ephemeral.
* Keep rules lightweight and avoid enterprise-only patterns.
* Prefer simplicity and fast feedback loops.

---

## 4. Kiro

Kiro uses **Markdown steering files** to guide AI behavior.

### Installation Steps

1. Create the Kiro steering directory:

   ```bash
   mkdir -p .kiro/steering
   ```

2. Copy Kiro rule files:

   ```bash
   cp kiro/*.md .kiro/steering/
   ```

3. Restart Kiro or reload the workspace.

### Notes

* Steering files apply across the entire repository.
* Use separate files for:

  * product context
  * technical standards
  * API or security rules
* Avoid conflicting instructions across steering files.

---

## 5. GitHub Copilot (Instructions)

GitHub Copilot supports repository-level instructions.

### Installation Steps

1. Create the instructions directory:

   ```bash
   mkdir -p .github
   ```

2. Add an instructions file:

   ```bash
   cp github/copilot-instructions.md .github/copilot-instructions.md
   ```

3. Ensure developers enable **Copilot Instructions** in their IDE.

### Notes

* Instructions apply during code generation and refactoring.
* Keep instructions opinionated but not verbose.
* Treat instruction changes as architectural decisions.

---

## Rule Management Best Practices

* Prefer **small, focused rule files** over one massive rule.
* Version rules alongside code changes.
* Document why rules exist, not just what they do.
* Review rule changes as part of normal code review.
* Avoid tool-specific hacks leaking into general rules.

---

## Troubleshooting

* If rules do not apply:

  * Restart the IDE/tool
  * Confirm file paths and extensions
  * Ensure no conflicting rules exist
* If AI output degrades:

  * Review recent rule changes
  * Simplify overlapping instructions
  * Roll back to a known-good version
