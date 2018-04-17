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

### Race condition between work in progress
Currently when two developer works on the same resource on two different branches they end up deleting each other work.

This is because Transifex doesn't allow new strings to be pushed individually, we can only push a whole resource, this is a destructive action: if a string is stored in Transifex but is not pushed when we update the resource will be deleted.

### Manual operations
A system that involve manual operation is subject to systematic errors, we want to reduce the manual operation as much as possible.

### Translator have no context of the string that are translated
String are presented as list, this makes difficult for translation to understand where each string is used.


## Proposed Solutions
A list of possible solutions to the problems listed above, the same problem may have multiple solutions, as some of this solution may solve more than one problem.
