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


## Proposed Solutions
A list of possible solutions to the problems listed above, the same problem may have multiple solutions, as some of this solution may solve more than one problem.
