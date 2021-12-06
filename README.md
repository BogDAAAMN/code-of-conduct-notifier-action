# Code of Conduct Notifier Action

## Description
The challenge of managing online communities is continuously growing. Often times the members are spread across multiple platforms, making it harder to get a complete snapshot of its members' interactions and behavior. While platforms employ different tools and methods to moderate (potentially harmful) conversations, the moderators' priority will always be to create a safe and positive community experience for everybody. This Action aims to help just a little bit the GitHub communities moderators into enforcing their Code of Conduct. 

## Usage
This is a simple GitHub Action that notifies the members of the community of their Code of Conduct document when this is mentioned. The Action will reply to issues and pull requests comments when the keywords _code of conduct_ are mentioned. 

## Inputs

### `comment-body`
**Default**: `"For questions about our community guidelines, please refer to our Code of Conduct: <URL>."`

The comment that the bot will post about the Code of Conduct. It can use the tag `<URL>` to be replaced with the Code of Conduct URL mentioned in the `coc-url` input. It supports multiline and markdown formatting, see [Workflows](#workflows) for examples.

### `coc-url`
**Default**: `"CODE_OF_CONDUCT.md"`

The URL to the Code of Conduct. Used to be mentioned in the body of the comment, where the tag `<URL>` is placed. It supports both relative paths (_e.g._ `/docs/CODE_OF_CONDUCT.md`) and absolute paths (_e.g._ `https://example.com/codeofconduct.html`), see [Workflows](#workflows) for examples.

## Workflows
For all the use cases, I strongly recommend using the `if: contains(github.event.comment.body, 'code of conduct')` conditional for the job. It will skip the processing (_15s_ per comment) of the comment altogether if it does not contain the keywords _code of conduct_. The conditional is **not** case sensitive, it can mention _Code of Conduct_ or other case-y variations. 

### Default behavior 
```yaml
name: CoC Notifier

on:
  issue_comment: # Action gets triggered on both issues and PRs comments
    types:
      - created

jobs:
  notify:
    name: "Code of Conduct Notifier"
    runs-on: ubuntu-latest
    if: contains(github.event.comment.body, 'code of conduct')
    steps:
      - uses: BogDAAAMN/code-of-conduct-notifier-action@v1.0.0
```

### Markdown body
```yaml
name: CoC Notifier

on:
  issue_comment: # Action gets triggered on both issues and PRs comments
    types:
      - created

jobs:
  notify:
    name: "Code of Conduct Notifier"
    runs-on: ubuntu-latest
    if: contains(github.event.comment.body, 'code of conduct')
    steps:
      - uses: BogDAAAMN/code-of-conduct-notifier-action@v1.0.0
        with:
          comment-body: "For questions about our **community guidelines**, please refer to our [Code of Conduct](<URL>)."
```

### Multiline body
```yaml
name: CoC Notifier

on:
  issue_comment: # Action gets triggered on both issues and PRs comments
    types:
      - created

jobs:
  notify:
    name: "Code of Conduct Notifier"
    runs-on: ubuntu-latest
    if: contains(github.event.comment.body, 'code of conduct')
    steps:
      - uses: BogDAAAMN/code-of-conduct-notifier-action@v1.0.0
        with:
          comment-body: |
            For questions about our **community guidelines**, please refer to our [Code of Conduct](<URL>).
            As always, our moderator team is happy to answer questions or provide more detail.
```

### Absolute URL
```yaml
name: CoC Notifier

on:
  issue_comment: # Action gets triggered on both issues and PRs comments
    types:
      - created

jobs:
  notify:
    name: "Code of Conduct Notifier"
    runs-on: ubuntu-latest
    if: contains(github.event.comment.body, 'code of conduct')
    steps:
      - uses: BogDAAAMN/code-of-conduct-notifier-action@v1.0.0
        with:
          coc-url: https://example.com/code-of-conduct.html
          comment-body: |
            For questions about our **community guidelines**, please refer to our [Code of Conduct](<URL>).
            As always, our moderator team is happy to answer questions or provide more detail.
```

### Relative URL
```yaml
name: CoC Notifier

on:
  issue_comment: # Action gets triggered on both issues and PRs comments
    types:
      - created

jobs:
  notify:
    name: "Code of Conduct Notifier"
    runs-on: ubuntu-latest
    if: contains(github.event.comment.body, 'code of conduct')
    steps:
      - uses: BogDAAAMN/code-of-conduct-notifier-action@v1.0.0
        with:
          coc-url: /docs/CODE_OF_CONDUCT.md
          comment-body: "For questions about our **community guidelines**, please refer to our [Code of Conduct](<URL>)."
```

## Sources 
The information presented in the description of the action, the inspiration, and the default messages used by the action are shaped from the following sources:

- [Constellation Report](https://orbit.love/constellation-report-state-of-community-tools-2021): State of Community Tools 2021 by [Orbit](https://orbit.love/)
- [Mozilla Community Participation Guidelines](https://www.mozilla.org/en-US/about/governance/policies/participation/) by [Mozilla Corporation](https://www.mozilla.org/en-US/about/)
- Feedback and conversations with [Virtual Coffee](https://virtualcoffee.io/) friends
