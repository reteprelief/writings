Notes
=====

Packages always use full path name. We could support relative package path names, i.e., reference syntactically nested packages relative to the given context. 
Issue: Syntactic distinction between absolute paths and relative path if the resolution rules are ambiguous: e.g., relative and absolute package identifier is the same.

Named interface is a feature. Handle in Feature section. Handled in implementation.

Configuration for interfaces. This implies that they can be associated with all implementations of that interface. Check for top implementation needs to handle the fact that such a configuration does not have one but needs to match interface.


Implementation ToDo
-------------------

add modes and mode transitions

add annex syntax

handle configuration of interfaces