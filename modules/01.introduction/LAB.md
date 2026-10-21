---
duration: 2h
objectives:
  - Set up a UNIX-like development environment, native or WSL
  - Create and share the team project repository
  - Self-assess the fundamentals against the required skills
---

# Lab: set up your environment

The first labs are practice: exercises to build up the skills, in the environment and repository created today. At mid-course the labs start chaining into a team project, built and improved week after week. The final grade considers the delivered project and the quality of its commit history.

## Course repository subscription

On the course repository on GitHub, select `Watch > All Activity` to receive notifications about new content and updates.

## 1. Set up a UNIX-like development environment

Everything in this course - Node.js, Git, shell scripts, later Docker - is built UNIX-first. A proper environment now avoids the classic time sinks later: Windows paths, CRLF line endings, file watchers, permissions.

Follow the [setup guide](./SETUP.md): pick your operating system path (native UNIX or WSL 2), then install the tools - Git, two browsers, a code editor, a password manager. Node.js, also in the guide, is required from the next lab.

Verify, from your UNIX shell:

```bash
uname -s        # "Linux" or "Darwin"
git --version   # a version
echo $SHELL     # a UNIX shell, eg /bin/bash or /bin/zsh
```

## 2. The project

1. Form a team of 3 students.
2. Create a **private** GitHub repository named `ece-webtech-<team-number>`, add your team members and your instructor as collaborators. Your group and repository must appear on the instructor's spreadsheet by the end of the class. This repository is used until the final project.
3. Each member clones it from the part 1 environment.
4. Brainstorm and pick a project idea, following the [project instructions](../../assets/PROJECT_INSTRUCTIONS.md).
5. Add a `README.md`: team members' names and email addresses, a brief project description. Keep it updated throughout the semester.

> [!IMPORTANT]
> Follow the [Conventional Commits](https://www.conventionalcommits.org) convention from the very first commit - it is part of the final grade.

## 3. Patch your knowledge

The course assumes the fundamentals: terminal, Git, how the web works, HTML/CSS, JavaScript, Markdown. Go through the [required skills](./SKILLS.md): for each topic, answer the "you should be able to" line honestly, and patch the gaps with the linked resources - short, free, hands-on. This lab is the time reserved for it.

---

*The content of this document, including all text, images, and associated materials, is the exclusive property of Adaltas and is protected by applicable copyright laws. Unauthorized distribution, reproduction, or sharing of this content, in whole or in part, is strictly prohibited without the express written consent of the author(s). Any violation of this restriction may result in legal action and the imposition of penalties as prescribed by law.*
