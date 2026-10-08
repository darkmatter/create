# @darkmatter/create

The Darkmatter template index for [Vite+](https://viteplus.dev/guide/create#organization-templates).
`vp create @darkmatter` reads the `createConfig.templates` list in
`package.json` and offers those templates in a picker. The Vite+ built-in
templates always appear at the end of that picker.

## Use

```sh
vp create @darkmatter            # pick a template
vp create @darkmatter:monorepo   # use one template directly
vp create @darkmatter --no-interactive   # print the template list
```

To make `vp create` (no argument) open this picker inside a repo, add this to
the repo's root `vite.config.ts`:

```ts
import { defineConfig } from "vite-plus";

export default defineConfig({
  create: { defaultTemplate: "@darkmatter" },
});
```

## Templates

| Name       | Source                                                          |
| ---------- | --------------------------------------------------------------- |
| `monorepo` | [`darkmatter/template`](https://github.com/darkmatter/template) |

## Add a template

Add an entry to `createConfig.templates` in `package.json`. Each entry needs a
kebab-case `name`, a one-line `description`, and a `template` source:

- `github:darkmatter/<repo>` for a template kept in its own public repo.
- `./templates/<name>` for a template kept in this package. Also add
  `templates` to `files` so it is published. Vite+ copies the directory as-is
  and renames `_gitignore` and `_npmrc` to their dotfile names.

Set `"monorepo": true` on templates that create a whole repo. Vite+ hides
those when `vp create` runs inside an existing monorepo.

Vite+ fails with an error when the list is invalid. To check a change before
publishing, publish it to a local registry such as
[Verdaccio](https://verdaccio.org), point the scope at it with an `.npmrc`
line like `@darkmatter:registry=http://127.0.0.1:4873/`, and run
`vp create @darkmatter --no-interactive` next to that `.npmrc`.

Vite+ converts every scaffolded project to Vite+: it adds `vite.config.ts`,
rewrites lint, format, and test scripts to `vp`, and installs `vite-plus`.
Templates must still install and build after that conversion.

## Release

Bump `version` in `package.json`, then push a matching `v<version>` tag. The
`Publish to npm` workflow publishes the package with provenance through npm
trusted publishing, so the package must have `darkmatter/create` and the
`publish.yml` workflow registered as its trusted publisher on npmjs.com.
