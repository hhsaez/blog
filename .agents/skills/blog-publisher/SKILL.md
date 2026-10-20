---
name: blog-publisher
description: Prepare, publish, and edit posts on Hernan Saez's personal Hugo blog at blog.hhsaez.com using hhsaez/blog on GitHub. Use for requests such as post this drawing, publish this with a one-liner, prepare a blog draft, suggest a post title, or update an existing blog post. Support images, screenshots, short notes, and longer articles while preserving the user's voice.
---

# Blog Publisher

## Destination and defaults

- Use the repository `hhsaez/blog`: https://github.com/hhsaez/blog. In local Codex, work through the checkout with `git` and use `gh` for GitHub Actions when available. In ChatGPT, use the connected GitHub plugin. Keep the workflow usable without that plugin on the user's machine.
- Publish to https://blog.hhsaez.com/.
- Follow the current repository configuration. The verified setup uses Hugo, PaperMod, `main`, and `.github/workflows/hugo.yaml`, which builds and deploys GitHub Pages on pushes to `main`.
- Use `America/Buenos_Aires` for publication dates unless instructed otherwise. Include the explicit UTC offset in timestamps.
- Treat the current repository as authoritative; never assume permissions, branch names, templates, or workflows remain unchanged.

## Interpret the request

- Treat an explicit instruction to publish or post supplied content as authorization to commit to the publishing branch and trigger deployment. Proceed without an extra confirmation when content and destination are clear.
- Treat requests to suggest, draft, prepare, preview, or review as preparation only. Show the proposed title, caption/body, and image. Keep unpublished files local unless a branch or pull request is requested; mark prepared Hugo content `draft: true`.
- Resolve follow-ups such as "publish it" from the visible conversation and approved draft. Set `draft: false` before publishing. Suggest and use a suitable title when publication is authorized and the title is left to you.
- For an instruction to edit a published post, update it in place. Preserve its URL and original publication date unless instructed otherwise.
- Do not interpret a hypothetical example or a question about publishing as permission to publish.
- Ask only for essential missing content, an inaccessible attachment, or an ambiguous target that cannot be resolved from context. Choose optional tags, slugs, or titles sensibly.

## Write with restraint

- Preserve supplied text and its language unless editing or translation is requested.
- Keep quick image posts to the image and one short caption. Suggest one concise title by default; leave the title empty if requested.
- Avoid expanding a one-liner into an essay or adding generic introductions. Do not invent claims about the user's intentions, process, materials, influences, or experiences.
- Write longer posts only when requested. Shape supplied notes into clear prose.
- Select a few relevant tags, preferring existing spellings. Previously used tags include `comics`, `projects`, `jägerlied`, and `drawing`; verify current usage before adding variants.

## Prepare the post

1. Inspect the image using the available image-viewing tool. Use an attachment's provided local copy; do not retrieve it from Library again when instructed to use that copy. If a path fails, check the supplied upload directory before asking for another upload.
2. Read applicable `AGENTS.md` instructions, current Hugo configuration, deployment workflow, and a relevant recent post. In a local checkout, inspect the branch and working tree; verify the destination remote and fetch the publishing branch before publication. Through the plugin, fetch the current branch head and tree. Check whether the proposed path already exists.
3. Use a Hugo page bundle with images beside `index.md`. Follow the existing date-based layout, typically `content/posts/YYYY/MM/DD-short-slug/`. Pick a distinct slug for each post; never overwrite another post from the same day.
4. Preserve the artwork and aspect ratio. Use safe filenames such as `cover.jpeg`. Do not crop, recolor, redraw, or otherwise alter artwork unless requested. Preserve the original source if creating an explicitly requested optimized derivative.
5. Put an image post's one-liner in `cover.caption` to match existing posts. Add a truthful `cover.alt` description and `relative: true`. Avoid embedding the same cover image or repeating its caption in the body. Put article text after the front matter when needed.
6. Parse front matter, check the date and draft status, and verify all referenced images exist and decode. Build with the configured Hugo version and theme when locally available. Do not claim a local build occurred if only metadata was checked.

Use this shape, substituting the actual date, title, tags, image, and caption; use `draft: true` during preparation:

```yaml
---
date: '2026-10-20T09:27:44-03:00'
draft: false
title: 'Expressionist Exploration'
tags:
  - drawing
  - comics
cover:
  image: cover.jpeg
  alt: 'Black-and-white ink drawing of Hellboy, with heavy shadows and bold shapes.'
  caption: 'A little Hellboy, a lot of black ink.'
  relative: true
---
```

## Publish through GitHub

Choose the available transport and use existing authentication. Never request credentials in chat. Preserve prepared files and report a specific access failure when blocked.

### Local Codex: git and gh

1. Check `git status` and the current branch. Preserve unrelated working-tree and staged changes. If the checkout has unrelated changes or unpublished commits, use an isolated worktree based on the fetched publishing branch. Do not stash, reset, or rebase the user's work automatically.
2. Add the prepared post and assets to that checkout. Initialize the theme submodule if needed for a local build. Validate the post, inspect the diff, and stage only its exact paths. Ensure unrelated staged changes cannot enter the commit.
3. Commit the post and assets together. Review the commit against its parent before pushing; confirm only intended files changed. Push normally to the verified publishing branch when publication is authorized. Never force-push. If remote changes reject the push, fetch and reconcile only the task's own isolated commit, inspect conflicts, and validate again. If branch protection requires a pull request, create one with `gh` when available and report publication as pending.
4. Record the published commit SHA. Use `gh run list --commit <sha>` and `gh run watch <run-id> --exit-status`, or available GitHub tools, to verify the exact deployment. If deployment cannot be checked, report the successful push and unverified deployment separately.

### ChatGPT: GitHub plugin

1. Discover current GitHub tool schemas and fetch the publishing branch head and tree.
2. Prepare an atomic commit containing Markdown and assets. Upload binary images through base64 blobs, upload text as UTF-8 blobs, create a tree based on the current tree, and create a commit parented to the observed head. Keep large base64 payloads in tool orchestration instead of printing them into the conversation.
3. Inspect the proposed commit's diff before publication. Confirm only intended post files changed, then advance the publishing ref with the expected head and without force. If the branch changes concurrently, reread the head, inspect conflicting paths, and rebuild on the new tree. Never discard unrelated files. Respect branch protections; if direct publication is blocked, prepare a pull request and report publication as pending.

### Verify and report

4. Prevent duplicate posts on retries by checking the current tree and recent commits before writing again.
5. Find the deployment run associated with the exact new commit. Wait for build and deployment to complete, communicating briefly if it takes time. Inspect logs on failure. Fix issues introduced by the post; report unrelated deployment problems without broad infrastructure changes.
6. Derive the post URL from actual Hugo configuration and bundle path. Check the live page and image when accessible. If live access is unavailable, distinguish successful GitHub Pages deployment from a direct page check. If deployment remains queued or fails, report that status instead of claiming the post is live.
7. Return a short confirmation with title, clickable post URL, and caption if useful. Include limitations only when relevant.

## Editing and scope

- Read the current post and preserve unrelated metadata and content during edits. Inspect the current image before replacing it.
- Keep site configuration, theme, workflows, and other posts outside the change unless requested or a narrowly necessary fix is authorized.
- Treat a future date as metadata, not a working scheduler. Check deployment triggers before promising timed publication; use an actual scheduling capability when requested and available.
- Keep repository-backed work in its repository. Do not create duplicate Library copies or commit unrelated attachments. Keep generated Hugo output out of commits.
