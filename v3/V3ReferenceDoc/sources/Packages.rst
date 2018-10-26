AADL Packages
=============

The *package* concept provides a way to organize definitions of component classifiers, user defined types, properties, and annex libraries.  

Packages can be *nested*, either by syntactically nesting package declarations, or by specifying a package with a dot (**.**) separated sequence of identifiers as name. 

*Top-level* packages, i.e., packages that are not syntactically nested, can be stored in separate files.

A package introduces a name space for its content. Top-level packages, i.e., the first identifier of a top-level package definition, resides in the *global* name space. 

Definitions within the same package can be referenced by their defining name. Definitions contained in other packages can be referenced by their defining name if the package content is made visible by an *import* declaration.

References to definitions in packages can always be referenced by including a qualifying package path name. 
This package name is first interpreted *relative* to the package containing the reference, i.e., refer to a definition inside a package nested in the package containing the reference.
If not found the package name is interpreted as a *full* name path starting with a top-level package identifier.

Syntax
------

| AADL\_specification ::= { package\_declaration }\ :sup:`+`
| 
| package\_definition ::= **package** *defining*\_package\_name
| { import\_declaration 
| \| component\_classifier\_definition
| \| type\_definition
| \| property\_definition
| \| package\_definition
| \| annex\_library\_definition
| }\ :sup:`\*`
| **end**  **;**
| 
| package\_name ::= *package*\_identifier { **.** *package*\_identifier }\ :sup:`\*`
| 
| import\_declaration ::= **import** import\_reference **;**
| 
| import\_reference ::= wildcard\_package\_name \| package\_name \| component\_classifier\_reference \| primitive\_type\_reference 

| wildcard\_package\_name\_path ::= package\_name  .\*
| 
| component\_classifier\_reference ::= [ *qualifying\_*package\_name **.** ] component\_classifier\_name
| 
| primitive\_type\_reference ::= [ *qualifying\_*package\_name **.** ] primitive\_type\_name
| 
| property\_reference ::= [ *qualifying\_*package\_name **.** ] property\_name



Naming Rules
-------------

1. Each package definition introduces a namespace. Definitions contained in this package reside in its namespace. 

#. Top-level package identifiers reside in a *global* name space. Subsequent identifiers of top-level package definitions represent nested package definitions and reside in the namespace of the respective enclosing package.

#. For the defining package name of a syntactically nested package, the first identifier resides in the namespace of the package containing the package definition. Subsequent identifiers reside in the namespace of the respective enclosing package. 

#. The package name path of a reference must resolve as a *relative* name path starting with the package containing the reference, or if not resolved as a *full* name path starting with a top-level package.

#. An import reference with a wildcard <star> indicator makes all contained definitions of the referenced package visible to the package containing the *import* declaration. 

#. An import reference to a component classifier, primitive type, or nested package makes the referenced definition visible to the package containing the *import* declaration.

#. If multiple definitions with the same name are made visible through an *import* declaration, then references to them must be qualified by their package name.

#. The component classifier reference, or primitive type reference without a qualifying package name path must resolve to a definition in the name space of the package containing the reference. If not present it must resolve to a definition made visible by import declarations.

#. If a definition with the same name exists in the namespace of the package containing the reference and as a definition made visible by an *import* declaration, then references to the *imported* definition must be qualified.
 
Semantics
---------

A *package* provides a way to organize declarations of nested packages, component classifiers, user defined types, property definitions, and annex libraries. 
Top-level packages, i.e., packages that are not nested, reside in a global name space. 

A collection of packages makes up an AADL specification.

Definitions contained in other packages are made visible to a given package through an *import* declaration. Such visible definitions can be referenced by their name without qualifying package name. 
A package name with a wild card <star> makes all contained definitions of the referenced package visible. 
An import reference without wildcard makes the referenced definition visible.

A fully qualified name path of an import reference can make the contained definitions of any package visible. 
A relative name path of an import reference can make the contained definitions of a nested package accessible.

The reference to a definition in a package, i.e., a reference to component classifier, type, property definitions, or nested package is resolved in the following order:

1. as name to a definition in the name space of the package containing the reference, i.e., a definition within the same package

#. as name to a definition made visible by an import declaration. 

#. as name with a *relative* qualifying package name, i.e., a reference to a definition in a nested package contained in the package with the reference

#. as name with a *full* qualifying package name, i.e., a reference starting with a top-level package 

References can always be qualified by a package name. This allows users to refer to imported definitions with the same name as other imported definitions or definitions in the same package. 

For qualified references the referenced definition does not have to be made visible through an *import* declaration.


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