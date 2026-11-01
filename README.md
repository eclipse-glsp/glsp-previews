# glsp-previews

Hosting of GLSP example, PR, and website previews.

The `previews` branch is served by GitHub Pages at <https://eclipse-glsp.github.io/glsp-previews/>.

## Layout

Each publishing repository owns one top-level directory, named after the repository:

```
<repository>/main/                 deployment of the repository's main branch
<repository>/pr-previews/pr-<n>/   preview of pull request <n>
```

For example, [`glsp-core`](https://github.com/eclipse-glsp/glsp-core) publishes to `glsp-core/main/` and `glsp-core/pr-previews/pr-42/`.

The directory name has to match the repository name. `cleanup.yml` uses it to look up whether a pull request is still open.

## Publishing

Deployments are pushed here by the workflows of the publishing repositories, with a token that is scoped to this repository alone. A repository that builds pull request code has to split that build and the deployment into two workflows, so the token is never reachable from code contributed in a pull request. [`glsp-core`](https://github.com/eclipse-glsp/glsp-core) is the first repository to publish here and documents the setup in its README.

## Cleanup

[`cleanup.yml`](.github/workflows/cleanup.yml) runs monthly and on demand. It removes previews whose pull request is closed, then squashes the branch history into a single commit. Removing a preview is normally the publishing repository's job when a pull request closes, so this is only a safety net for runs that failed or never happened.

The squash force-pushes `previews`, so the branch must not be protected against force pushes. An organization ruleset that blocks them on default branches would break this job on a schedule nobody watches.

`.nojekyll` turns off Jekyll processing, so that files and directories starting with `_` or `.` are served as they are.
