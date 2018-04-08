## Contributing Guidelines

Thank you for considering to contribute to this documentation!
Such a contribution can be useful for modders, tool-, and engine developers.

The following text lists guidelines for contributing to this documentation.
These guidelines don't have legal status, so use them as a reference and common sense - and feel free to update them as well!


### "I just want to know..."

For questions, or general meta-information about the engine or the data files, please refer to the [systemshock.org](https://www.systemshock.org) forums.
The following sub-sections will most likely be applicable for your question:

* [Engineering](https://www.systemshock.org/index.php?board=36.0) - for modding the game or creating fan-missions in general
* [Deck 13](https://www.systemshock.org/index.php?board=13.0) - for engine source-code, ports, and tool-development related topics


### Scope

This documentation is meant to describe the "meaningful" resource files of the original System Shock (1994) and related engine behaviour.
It is meant to give modders and tool-developers (possibly engine developers) a reference on what data to expect.

What does "meaningful" mean? Data that has effect on the players experience. As a rule of thumb, there are several fields in various structures that are unused. Even with the source code of the engine available, it may be academic to try to document fields and their fictionial behaviour just by their name alone. If a field is never used, it may simply be stated as such and nothing more.
Exceptions are listed on the main readme - for instance, not covering music or Mac-specific files.

What does "original" mean? The game System Shock from 1994 and its direct derivatives, such as the "Enhanced Edition" or possible (future) source ports that claim to be compatible with the data files. It may be that future source ports extend the behaviour of the original engine. If this is the case, this documentation could be extended alongside, stating the compatibilities.

This documentation is NOT a documentation on the engine itself. There may be some gray areas if related to behaviour, yet you won't find a documentation on how the renderer is working for instance.


### Conventions

This documentation is meant to be read by a Markdown-compatible viewer, though for the most part, this will remain GitHub itself.
Refer to the [conventions](conventions.md) document for formatting and content.

To reiterate from this document: Please avoid adding information "on a hunch" to the documentation. Meanwhile the source code has already been released, so if there isn't a test, or a successful tool showcasing information, the documentation should at least hold up to the original source code.

It is acceptable to change field/type names if they are "near" synonyms (or simply words written in full). The documentation should aim to stay close to the original source, not a particular fan-project (not even tools from InkyBlackness - their code should follow as well.)


### Information

As a rule of thumb, this documentation describes types (data structures) with their fields, names, and related behaviour of the engine.
This is typically a combination of a dry listing of a data structure, as described in the conventions, and then some paragraphs of prose text.

It should not contain any source code - especially copied from the original source, which uses a different license. The only algorithms explained in this documentation are that of compression, and even these could be covered in prose.


### Adding/Changing the documentation

If you find typos, new information, better (more expressive) descriptions that you would like to see handled, feel free to create an issue, or possibly even a pull-request about this.
