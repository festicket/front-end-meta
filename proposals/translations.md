# Translations

## Definitions
The sets of terms is inspired by the one used by Transifex, the system that we use to manage translations.

**Translation string (or just string)**: A piece of context in the english that needs to be translated in other languages.

**Resource**: A file containing more a set of strings.

**Project**: A set of resources, currently we have two projects, one for legacy/dabatabse strings (Festicket Production) and one for the frontend application (Festicket App).

**Translations**: the group of all the already translated strings grouped by destination language and resource.

**Translations Memory**: a database of previously completed translations. Each time a translation is submitted, it's saved to this database. In the future, if a phrase similar to the original source string appears, the translation is presented to the translator as a suggestion.

**Translations Memory Autofill**: (currently disabled) when a new string is submitted and Translations Memory contain a 100% match the sudgestion is automatically applied it's automatically apply.

## Problems
A list of the problems of our current translations system.

### No live translations
The translations lives in the repository and new translations are downloaded manually by developers. This means that there is an unknow delay between the translators work and the translations appearing on the live website.

A strict requirement in solving this is to mantain the system indipendent from external services.

### Race condition between work in progress
Currently when two developer works on the same resource on two different branches they end up deleting each other work.

This is because Transifex doesn't allow new strings to be pushed individually, we can only push a whole resource, this is a destructive action: if a string is stored in Transifex but is not pushed when we update the resource will be deleted.

### Manual operations
A system that involve manual operation is subject to systematic errors, we want to reduce the manual operation as much as possible.

### Translator have no context of the string that are translated
String are presented as list, this makes difficult for translation to understand where each string is used.


## Proposed Solutions
A list of possible solutions to the problems listed above, the same problem may have multiple solutions, as some of this solution may solve more than one problem.

### Move translation to S3
We can store the translations on a S3 bucket, access is fast and Transifex provides webhook that can be used to update S3 every time a resource is translated.

Since our stack lives on AWS S3 can be considered an internal service, and in case of Transifex being down our system will keep working.

### Pushing translation as part of the master build
This will reduce manual operations and eliminate every possible race condition.
But will also mean that translation can't be tested/prepared before we deploy to the production enviroment.
A solution to this last point may be a smart use of feature switchers (we merge and deploy a new feature without actually enabling it).

### Use branch to namespace resources
We can add to the name of each resource the name of the git branch that we used to push the translations (or an hash derived from the branch name).
This will create a series of resources not actually used by our system, but available to translators to work with.

On the master release we will update the actual resource and delete this temporary one, at this point the Translations Memory Autofill will automatically apply the translations of the working in progress resource to the actual resource.

This system can be improved using a different project for the branches, or selectively pushing only the resource that need to be translated before a release.

### Create a non destructive push
We can easily build a non destructive verison of push. We can download the list of the strings and merge them with the local version before pushing, this will avoid to delete anything ever from transifex.

This will avoid the case in wich two people are working on the same resource but on differents strings, but will not solve the problem of two developer working on the same string.


### Talk with translator and use Zeplin
Everyday comunication with translators is a low effort task, Zeplin can be used to share some visual reference regarding page statuses or not yet published work (even if it doesn't contain the final copy).
