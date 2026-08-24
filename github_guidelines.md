# Bellweather Agency Github Workflow Guidelines

These guidelines apply to internal developers and external development partners working in our GitHub repositories.

## General Principles
- Avoid committing directly to `main` once a repository is past its early setup phase and has become a collaborative dev project.
- Do not commit credentials, secrets, database exports, or production data. Private API keys should be used only server-side. They may be stored separately in each environment's `wp-config.php`, which should be excluded from Git.
- PR authors and reviewers should give and receive feedback constructively and with an open mind. Focus comments on the code, explain the reasoning behind suggestions, and treat the review as a collaborative conversation.

## Creating a Pull Request
See [here](https://github.blog/developer-skills/github/beginners-guide-to-github-creating-a-pull-request/) if you are unfamiliar with pull requests. 
- Create a separate branch off of `main` for each feature, fix, or ticket. If `main` changes while you are working, rebase or merge those changes into your feature branch.
- To keep branches organized, prefix the branch name with your username, include the relevant BugHerd task number when applicable, and briefly describe the change. For example: `jamiefinucane/123-styles`.
- Keep pull requests as small and focused as possible for the type of change you are working on.
- PRs should not include any formatting/linting changes, dependency upgrades, or refactors that aren't related to the task at hand.
- Remove any `console.log()` statements or other debugging code that might have been committed while you were working

## PR Title/Description
- Use a clear title that describes the change.
- Link the relevant Bugherd and/or Wrike task.
- Link any related PRs or Issues
- Include a brief summary of what changed and why
- Include more detailed descriptions and/or code comments for any aspects of your changes that are especially unintuitive, complex, or differ from what a reviewer would expect.
- Include screenshots or instructions for testing
- Describe any known limitations or follow-up work

## Before Merging
- At least one other developer should approve the pull request.
- Authors should respond to or resolve every review comment.
- Request another review after making substantial changes.

## Once Approved
- Use `squash & merge` into `main`.
- Delete the feature branch after merging. If you continue to use the same branch once it's already been merged into `main` once, the git history becomes very confusing.
