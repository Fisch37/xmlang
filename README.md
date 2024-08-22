# XMLang
This is the base repository for the XMLang project.

XMLang is a fun little side project I came up with
for no reason in particular other than to be funny.

The goal is as follows: Create a fully functional programming language built completely in XML.
Do this while utilising XML as much as possible. 
(E.g. using XML Entities for built-in library support)

To facilitate this goal I have set the following guidelines for language features:
1. Only use tag attributes for metadata. Never store content in them. That's what the tag content is for.
2. Only create new language features when they solve a problem that could not be solved (sensibly)* before.
3. Avoid making things implicit. XML is very verbose and that better be part of the design.

\* The definition of "sensible" may be applied loosely.

The current language specification can be found in [specifications.md](specifications.md).