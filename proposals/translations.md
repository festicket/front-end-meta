Translations
--------------

Currently the translations setup is working for us. We keep all translations in the `festicket-app` repo.
We have to manually `push` translations into transifex. We keep all localised resources in the repo, version controlled with `git`.
Translations have to be manually `pulled` into the repo for updates. This has lead to a number of issues, documented below:


Problems
------------

### Race condition between different tasks in progress
It is often the case that one developer can push changes into transifex and then another will push and override the first set of changes.
This results, at best, in developers forming a queue around translations. At worst this results in translations being repeatedly deleted.
If a translator works on a resource and the key disappears the translations are gone forever.

### Components don't own translations
Currently if we migrate a component from `festicket-app` to `react-app-components`, you cannot migrate the translations.
This causes a difficult dependency between components. We have to keep a copy of the translations in `react-app-components` so that they are visible in storybook.
However, these translations cannot update transifex. This could very easily lead to a break in the production application, where translations are not available on the live site
because they have not been updated in `festicket-app`

### No live translations
The translations live in the `festicket-app` repository and new translations are downloaded manually by developers.
This means that there is an unknown delay between the translators' work being completed and the translations appearing on the live website.

A strict requirement in solving this is to keep the system independent from external services.

This is because Transifex doesn't allow new strings to be pushed individually.
We can only push a whole resource. This is a destructive action: if a string is stored in Transifex but is not pushed when we update the resource will be deleted.

### Manual operations
A system that involves many manual operations is subject to systematic errors.
We want to reduce manual operation as much as possible.

### Translators have no context of the strings that they are translating
String are presented as a list. This makes it difficult for translators to understand where each string is used.


Solutions
-------------

### Better CLI Tooling
We should create a `transifex-cli-tools` project for managing translations on the command line. It should contain these commands:

- `transifex-cli status`
    Prints out whether you have any changes that are not reflected in transifex.

    Options:
    - `--ci`: If there are changes on the current branch that have not been pushed to transifex we `exit(1)`;
      This is a useful saftey check for CI that will fail a build if a developer's translation files are out of sync.

- `transifex-cli diff`
    Shows a diff of all resources in the current translations directory compared to what is currently available on transifex.

- `transifex-cli merge`
    Merges together local changes with the ones that currently exist on transifex.
    Checks for conflicts and if one exists `exit(1)`.

    Options:
    - `--manual`: Pulls down a copy of any conflicting resources and saves them as `{reource}.live.json`
      This is usefull for manually resolving conficts

- `transifex-cli push`
    Push all local translations onto transifex through the REST API.
    Checks status and if there are conflicts `exit(1)`.
    If any `{translations_directory}/*.live.json`

    Options:
    - `--force`: Just override all live copies with fresh ones from the current translations directory
      This is useful after resolving onflicts. We show a diff of the changes and ask the user to confirm the operation before performaing it.

- `transifex-cli pull`
    Update local copies of all local translation files.

    Options:
    - `--force`: Just override all local copies with fresh ones from transifex
      This is useful for reseting the contents of a repository

#### Authorisation:

- Transifex User
    We take the transifex user from `process.env.TRANSIFEX_USER`.
    If `process.env.TRANSIFEX_USER` is not present in the current env prompt the user to input one.
    If `--ci` flag is used with any command `exit(1)` when the env var is not present.

- Transifex Password
    We take the transifex password from `process.env.TRANSIFEX_PASS`.
    If `process.env.TRANSIFEX_PASS` is not present in the current env prompt the user to input one.
    If `--ci` flag is used with any command `exit(1)` when the env var is not present.

- Transifex Project
    We take the transifex user from `process.env.TRANSIFEX_PROJECT`.
    If `process.env.TRANSIFEX_PROJECT` is not present in the current env use `package.json.transifex.project`.
    If `process.env.TRANSIFEX_PROJECT` or `package.json.transifex.project` is not present prompt the user to input one.
    If `--ci` flag is used with any command `exit(1)` when the project name cannot be resolved.

- Transifex Resources
    All resources should belong to a given project. We determine the project from the `package.json.name` field.
    All resources should have a category value which is set to the value of the current project (where the commands are being run).
    If there is a mismatch we `exit(1)`. This means someone has created a resource with a name that currently belongs to another project.
    When a new resource is created we will automatically use `package.json.name` as the category value.

#### Configuration

All configuration lives in `package.json.transifex`.

  - Transifex Project
    As mentioned above in Authorisation

  - Translations Directory
    The directory within the current project where all local translation resources are stored.
    Defaults to `./translations`


### Move translations to S3
We can store the translations on a S3 bucket.
Access is fast and Transifex provides webhook that can be used to update S3 every time a resource is translated.

Since our stack lives on AWS, S3 can be considered an internal service, and if Transifex is down our system will keep working.

### Translators & Zeplin
One solution for providing context to the translators is to give them access to Zeplin.
We should look at the possibility of building a Transifex/Zeplin integration that could highlight areas of the designs
that require translations.

### Highlight Translation Components
We could use CSS to provide a border around any translated piece of text.
If we use a pseudo selector we could also show the tranlsation ID visually using the `content` css attribute.
We could switch this on using a `localStorage` value or an internal browser extension.

### Use [Transifex Memory](https://docs.transifex.com/setup/translation-memory) & [Transifex Memory Autofill](https://docs.transifex.com/setup/translation-memory/enabling-autofill)
Transifex Memory keeps a database of all previous translation strings keyed by Translation ID.
This means no translations are ever truly deleted. The Autofill feature means that if any translation ID has ever been used before
it will automatically be populated by the previously entered translations strings for every language.
Using these features will mean that if a developer accidentally deletes a translation it can be auto filled when its re-uploaded.


Definitions
----------------

These terms are inspired by the ones used by Transifex, the system that we use to manage translations.

<dl>

  <dt>
    Translation string (or just string)
  </dt>
  <dd>
    A piece of copy in English that needs to be translated into other languages.
  </dd>

  <dt>
    Project
  </dt>
  <dd>
    A colletion of languages eg: `en`, `fr`, `de`, `es` ... etc
  </dd>

  <dt>
    Resource
  </dt>
  <dd>
    A JSON file containing key value pairs `{ [Translation Id]: [Translation String] }`
  </dd>

  <dt>
    Translations
  </dt>
  <dd>
    A set of translation strings grouped by Language/Resource
  </dd>

  <dt>
    Translations Memory
  </dt>
  <dd>
    A database of previously completed translations.
    Each time a translation is submitted, it's saved to this database.
    In the future, if a phrase similar to the original source string appears,
    the translation is presented to the translator as a suggestion.
  </dd>

  <dt>
    Translations Memory Autofill
  </dt>
  <dd>
    Auto completion, when a new translation string is added to a resource
    using a previously defined `id` the previously used tranlsation strings are added
    to all other languages
  </dd>

</dl>
