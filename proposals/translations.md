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

### Better Tooling

Translations should be pushed to transifex reguarly. We should use the [Transifex CLI cilent](https://docs.transifex.com/client/introduction).
On pre-commit, when commiting a translation file, we should push said resource onto transifex. Using a pre-commit has the least chance of a race condition
(compared to using a build). If you try to push a resource to transifex which has not first been updated the pre-commit hook should fail.
The user should then be prompted to stash changes and pull all changes into their local environment.

We should also provide a script to update **all** translations locally.

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
