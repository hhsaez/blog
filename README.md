# blog

# Local development
This project is based on [Hugo](https://gohugo.io)

## Cloning
Remember to recursively clone the repository in order to fetch all dependencies:

``` sh
git clone <> --recursive
```

## Adding new content

``` sh
hugo new content content/posts/foo.md
```

## Running the server 

``` sh
hugo server
```

## Running the server in draft mode

``` sh
hugo server -D
```

## Publishing with Codex

The repository includes [Blog Publisher](.agents/skills/blog-publisher/SKILL.md), a skill for preparing, publishing, and editing posts. Codex discovers it from `.agents/skills/` when launched inside this repository.

After pulling the latest changes, start Codex in the checkout:

``` sh
git pull --ff-only
codex
```

Then ask, for example:

``` text
Use $blog-publisher to prepare a draft for /path/to/drawing.jpeg with a short caption.
```

Or publish directly:

``` text
Use $blog-publisher to publish /path/to/drawing.jpeg. Suggest a title and one-line caption.
```

Draft requests remain unpublished. Explicit publication requests authorize a commit and push to `main`, which triggers the existing GitHub Pages workflow. The skill uses local `git` authentication; `gh` can verify deployment. Hugo is needed for local builds and previews. If the skill does not appear, restart Codex or check `/skills`.
