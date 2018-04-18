# Translations

## Definitions
These terms are inspired by the ones used by Transifex, the system that we use to manage translations.

**Translation string (or just string)**: A piece of copy in English that needs to be translated into other languages.

**Resource**: A file containing a set of strings.

**Project**: A set of resources. Currently we have two projects: one for legacy/database strings (Festicket Production) and one for the frontend application (Festicket App).

**Translations**: the group of all the already translated strings grouped by destination language and resource.

**Translations Memory**: a database of previously completed translations. Each time a translation is submitted, it's saved to this database. In the future, if a phrase similar to the original source string appears, the translation is presented to the translator as a suggestion.

**Translations Memory Autofill**: (currently disabled) when a new string is submitted and Translations Memory contains a 100% match the suggestion is automatically applied.

## Problems
A list of the problems with our current translations system.

### No live translations
The translations live in the repository and new translations are downloaded manually by developers. This means that there is an unknow delay between the translators' work and the translations appearing on the live website.

A strict requirement in solving this is to keep the system independent from external services.

### Race condition between different tasks in progress
Currently when two developers work on the same resource on two different branches they will override each other's work.

This is because Transifex doesn't allow new strings to be pushed individually. We can only push a whole resource. This is a destructive action: if a string is stored in Transifex but is not pushed when we update the resource will be deleted.

### Manual operations
A system that involves many manual operation is subject to systematic errors. We want to reduce manual operation as much as possible.

### Translators have no context of the strings that they are translating
String are presented as a list. This makes it difficult for translators to understand where each string is used.

## Proposed Solutions
Here is a list of proposed solutions to the problems listed above. The same problem may have multiple solutions, and some of these solutions may solve more than one problem.

### Move translations to S3
This will solve **No live translations** and reduce **Manual operations**.

We can store the translations on a S3 bucket. Access is fast and Transifex provides webhook that can be used to update S3 every time a resource is translated.

Since our stack lives on AWS S3 can be considered an internal service, and in case of Transifex being down our system will keep working.

### Pushing translation as part of the master build
This will reduce **Manual operations** and eliminate every possible **race condition**.

But will also mean that translation can't be tested/prepared before we deploy to the production environment.

A solution to this last point may be a smart use of feature switchers (we merge and deploy a new feature without actually enabling it).

### Use branch to namespace resources
This will reduce **Manual operations** and eliminate every possible **race condition**.

We can push on every build. Adding the name of the git branch to the name of each resource we can eliminate every possible conflict.

This will create a series of resources not actually used by our system, but available to translators to work with.

On the master release we will update the actual resource and delete this temporary one, at this point the Translations Memory Autofill will automatically apply the translations of the working in progress resource to the actual resource.

This system can be improved using a different project for the branches, or selectively pushing only the resource that need to be translated before a release.

### Create a non destructive push
This will reduce **Manual operations** and eliminate every possible **race condition**.

We can build a non destructive push and use it on every build.

Downloading the list of the strings and merge them with the local version before pushing, this will avoid to delete anything ever from transifex.

This will avoid the case in which two people are working on the same resource but on different strings, but will not solve the problem of two developer working on the same string.

### Talk with translator and use Zeplin
This will provide **context for translators**.

Everyday comunication with translators is a low effort task, Zeplin can be used to share some visual reference regarding page statuses or not yet published work (even if it doesn't contain the final copy).
