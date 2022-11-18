# Contributing

In this section we will focus on the steps and nuances of developing Kongponents. Lets start with installation.

## Installation

Clone the Kongponents repository

```sh
git clone https://github.com/Kong/kongponents.git
```

Install dependencies

```sh
cd kongponents && yarn install --frozen-lockfile
```

Next, let's generate [the CLI](#cli) that can be used to easy scaffold new Kongponent components. _This likely was ran automatically after installing dependencies._

```sh
yarn build:cli
```

Run the docs local dev server with hot-module reload

```sh
yarn docs:dev
```

Build the docs and preview the built files locally

```sh
yarn docs:preview
```

Perform a full build of all Kongponents and the Docs site

```sh
yarn build
```

## CLI

It is **highly recommended** to utilize the included CLI when creating new Kongponents as it will scaffold all the necessary files.

```sh
yarn create-kongponent
```

## Create a new Kongponent

When creating a new component with the CLI it will perform the following actions:

- Creates `/src/components/{KongponentName}/` directory with the following files:
  - `{KongponentName}.cy.ts` - Cypress Component Test file
  - `{KongponentName}.vue` - Component file
- Adds `/src/components/{KongponentName}/{KongponentName}.vue` to the exports in `/src/components/index.ts`
- Creates a VitePress markdown file at `/docs/components/{kongponent}.md` (you have to manually add this file to the VitePress sidebar in `docs/.vitepress/config.ts`).

::: warning NOTE
If your component is exported via an `index.ts` file, or anything other than the default `{KongponentName}.vue` file, you will need to modify `/src/components/index.ts` accordingly.
:::

Once ran, this will be the resulting file structure:

```bash
├── docs/
│   └── components/
│       └── {kongponent}.md
└── src/
    └── components/
        └── {KongponentName}/
          ├── {KongponentName}.cy.ts
          └── {KongponentName}.vue
```

## Edit the Doc file

Each component has an associated file in the `/docs/components` directory. After scaffolding the new component file, a doc file should be present named the same as your new component. Below are the steps to add the file to the docs site and how to get started editing.

### Renaming the file (if needed)

The docs markdown file should be named correctly if generated from the [`create-kongponent` CLI](#cli). If necessary, rename the file to correspond to what type of component it is. For documentation purposes page names should be based on what the component is vs its Kongponent `K` name.

#### Examples

- `kbutton.md` &rarr; `button.md`
- `kcard.md` &rarr; `card.md`
- `kdatetimepicker` &rarr; `datetime-picker.md`

### Update the page title

Update the first line of the doc to match the file name. This is what is displayed as the page title & in the sidebar.

```md
# {Name}

**{KongponentName}** - description
```

### Add the doc file to the sidebar

Although the CLI will create a file in the docs directory, the new doc file **is not automatically added to the docs sidebar config**.

Add the component to the desired location in the sidebar

```ts{10-15}
// docs/.vitepress/config.ts

sidebar: {
  // Components sidebar
  '/components/': [
    {
      text: 'Components',
      collapsible: true,
      items: [
        ...
        {
          // The name of the rendered element, e.g. "Alert"
          text: '{Component Name}',
          // The name of the `.md` markdown file, without the extension
          link: '/components/{component-name}',
        },
        ...
      ]
    }
  ]
}
```

### Importing type declarations and interfaces

When importing type declarations or interfaces, you **must** use a relative path instead of the `@/...` alias so that the types are properly resolved within consuming packages. See the example below:

```ts
import { StepperState } from './KStepState.vue'
```

## Committing Changes

This repo uses [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

[Commitizen](https://github.com/commitizen/cz-cli) and [Commitlint](https://github.com/conventional-changelog/commitlint) are used to help build and enforce commit messages.

It is __highly recommended__ to use the following command in order to create your commits:

```sh
yarn commit
```

This will trigger the Commitizen interactive prompt for building your commit message and will allow you to preview your commit.

### Enforcing Commit Format

[Lefthook](https://github.com/evilmartians/lefthook) is used to manage Git Hooks within the repo. See see the current `/lefthook.yml` here:

<<< @/../lefthook.yml{yaml}

A `commit-msg` hook is automatically setup that enforces commit message stands with `commitlint`.

A `pre-push` hook is configured to run ESLint before pushing your changes to the remote repository.

## Recommended IDE Setup

We recommend using [VSCode](https://code.visualstudio.com/) along with the [Volar extension](https://marketplace.visualstudio.com/items?itemName=johnsoncodehk.volar)

### Type Support For `.vue` Imports in TS

Since TypeScript cannot handle type information for `.vue` imports, they are shimmed to be a generic Vue component type by default. In most cases this is fine if you don't really care about component prop types outside of templates. However, if you wish to get actual prop types in `.vue` imports (for example to get props validation when using manual `h(...)` calls), you can enable Volar's `.vue` type support plugin by running `Volar: Switch TS Plugin on/off` from VSCode command palette.