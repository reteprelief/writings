AADL Packages
=============

The *package* concept provides a way to organize declarations of component classifiers, user defined types, property definitions, and annex libraries.  
Packages can be nested, either by syntactically nesting package declarations, or by specifying a package with a dot (**.**) separated sequence of identifiers as name. The latter allows for packages to be stored in separate files.

A package introduces a name space for its content. Top-level packages, i.e., the first identifier of package definition that is not nested, resides in the *global* name space.

A reference to definitions inside a package can be a *relative* or *fully qualified* name path. 
A relative name path starts in the name space of the package, component classifier, type, or property definition that contains the reference.
A fully qualified name path starts with a top-level package identifier.

Users can make content of other packages visible to a given package through an *import* declaration.  
Content of imported packages can be referenced by just its name if the if it does not exist in the name space of the package containing the import declaration or conflict with other content made visible by import declarations. 
In case of conflict a relative or fully qualified name path is used to uniquely identify the definition being referenced.

Syntax
------

| AADL\_specification ::= { package\_declaration }\ :sup:`+`
| 
| package\_definition ::= **package** *defining*\_package\_name
|   { import\_declaration }\ :sup:`\*`
|   { package\_content\_declaration }\ :sup:`\*`
| **end**  **;**
| 
| package\_name ::= *package*\_identifier { **.** *package*\_identifier }\ :sup:`\*`
| 
| package\_content\_declaration ::=
| component\_classifier\_definition
| \| type\_definition
| \| property\_definition
| \| package\_dedefinition
| \| annex\_library\_definition

| name\_path ::= identifier { **.** identifier }\ :sup:`\*`
| 
| import\_declaration ::= **import** import\_reference **;**
| 
| import\_reference ::= name\_path [ .\* ]
| 
| package\_content\_reference ::= name\_path


Naming Rules
-------------

1. An AADL specification has a *global* name space. Top-level package identifiers reside in this name space.

#. Packages introduce name spaces for their content. Nested packages represent nested name spaces.

#. A defining package name consists of a sequence of two or more *dot* separated identifiers represents a nested package of one or more nesting levels. 

#. A relative name path reference to package content starts with the name space of the package that contains the reference. A fully qualified name path reference starts with the global name space. Successive identifiers in the name path are resolved in the predecessor name space.

#. The name path of an import reference in an *import* declaration can be a *relative* or *fully qualified* name path.  

#. An import reference with a wildcard <star> indicator makes all content of the referenced package visible to the package containing the *import* declaration. 

#. An import reference without wildcard <star> makes the specific referenced package content element visible to the package containing the *import* declaration.

#. If the first identifier of a package content reference does not identify a package, it must resolve to a definition in the name space of the package containing the reference. Otherwise it must resolve to a definition made visible by import declarations.

#. If the first identifier of a package content reference does not identify a package, it is resolved as a relative name path. Otherwise it is resolved as a fully qualified name path.

Semantics
---------

A *package* provides a way to organize declarations of nested packages, component classifiers, user defined types, property definitions, and annex libraries. 
Top-level packages, i.e., packages that are not nested, reside in a global name space. 

A collection of packages makes up an AADL specification.

Definitions contained in other packages are made visible to a given package through an *import* declaration. 
A name path with a wild card <star> in an *import* declaration makes all contained definitions of the referenced package visible. 
A fully qualified name path without wild card <star> makes the referenced definition visible.

A fully qualified name path of an import reference can make the contained definitions of any package visible. 
A relative name path of an import reference can make the contained definitions of a nested package accessible.

A package content reference, i.e., a reference to component classifier, type, property definitions, or nested package is resolved as follows:

1. in the name space of the package containing the reference

#. to a definition made visible by an import declaration

#. interpreted as relative name path, i.e., a reference to a definition in a nested package contained in the package with the reference

#. interpreted as fully qualified name path, i.e., a reference starting with a top-level package 

Reference can be a relative or fully qualified name path without the referenced definition visible through an *import* declaration.


Processing Requirements and Permissions
---------------------------------------

A method of processing must accept an AADL specification presented
as a single string of text in which declarations may appear in any
order. An AADL specification may be stored as multiple pieces of
specification text that are named or indexed in a variety of ways,
e.g., a set of source files, a database, a project library.
Preprocessors or other forms of automatic generation may be used to
process AADL specifications to produce the required specification
text. This approach makes AADL scalable in handling large models.

Examples
--------
Two packages with nested names. The first contains a syntactically nested package. The second makes the content of the first package visible via 'import'.
A third package contains a type 'tt' and a nested package with a type 'tt'. The component interface definition references each of those types as well as a fully qualified type reference without an 'import' declaration. ::

 package PackP.Q 
	type date ;
	package nested  
		type signal ;
	end ;
 end ;

 package PackP.R 
 import PackP.Q.*; -- makes the type 'date' and the package 'nested' visible
	interface mine is
	 p1: in feature date;
	 p2: in feature nested.signal; -- reference to a type in the package 'nested'
	end;
 end ;
 
 package PackC 
	type tt;
	package packcc 
		type tt;
	end;
	interface mine
	is
		name : in feature PackP.Q.date; -- fully qualified reference
		surname : in feature tt ;       -- reference to type 'tt' in 'packC'
		sig : in feature packcc.tt;     -- reference to type 'tt' in nested package
	end ;
 end ;