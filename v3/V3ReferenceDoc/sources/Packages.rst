AADL Packages
=============

The *package* concept provides a way to organize definitions of component classifiers, data types, properties, and annex libraries.  

Packages can be *nested*, either by syntactically nesting package declarations, or by specifying a package with a dot (**.**) separated sequence of identifiers as name. 
Defining package names reside in a *global* name space. For syntactically nested packages the defining package name is prefixed by the names of syntactically enclosing packages.

*Top-level* packages, i.e., packages that are not syntactically nested, can be stored in separate files.

A package introduces a name space for directly contained component classifier, data type, and property definitions. 

Definitions within the same package can be referenced by their defining name. Definitions contained in other packages can be referenced by their defining name if the package content is made visible by an *import* declaration.

References to component classifiers, data types, properties can always be qualified by the name of the enclosing package.

Syntax
------

| AADL\_specification ::= { package\_declaration }\ :sup:`+`
| 
| package\_definition ::= **package** *defining\_*package\_name
|    { import\_declaration \| [ **private** ] package\_element }\ :sup:`\*`
| **end**  **;**
| 
| package\_name ::= *package*\_identifier { **::** *package*\_identifier }\ :sup:`\*`
| 
| import\_declaration ::= **import** import\_reference **;**
| 
| import\_reference ::= wildcard\_package\_reference 
|   \| component\_classifier\_reference [ alias\_definition ] 
|   \| data\_type\_reference [ alias\_definition ]
|   \| property\_reference [ alias\_definition ]
| 
| alias\_definition ::= **as** *alias\_*identifier
| 
| package\_element ::= 
|   component\_classifier\_definition \| type\_definition \| property\_definition
|   \| *nested\_*package\_definition \| annex\_library\_definition 
| 
| component\_classifier\_definition ::= 
|   component\_interface\_definition \| component\_implementation\_definition \| configuration\_definition
| 
| wildcard\_package\_reference ::= package\_name  **::\* **
| 
| component\_classifier\_reference ::= [ package\_qualifier ] component\_classifier\_name
| 
| data\_type\_reference ::= [ package\_qualifier ] data\_type\_name
| 
| property\_reference ::= [ package\_qualifier ] property\_name
| 
| package\_qualifier ::= *qualifying\_*package\_name **::**



Naming Rules
-------------

1. Each package definition introduces a namespace for component classifier, data type, and property definitions contained in the package. 

#. Defining package names reside in a *global* name space. For a syntactically nested package, the defining package name is prefixed with the defining package name of enclosing the packages. 

#. An import reference with a wildcard <star> makes all contained definitions of the referenced package that are not declared as **private** visible to the package containing the *import* declaration. 

#. An import reference to a component classifier, data type, or property definition makes the referenced definition visible to the package containing the *import* declaration. 

#. The alias identifier of an alias definition for an imported component classifier, data type, or property definition resides in the namespace of the package with the import declaration. 

#. If multiple definitions with the same name are made visible through  *import* declarations, then references to them must be qualified by their package name.

#. A component classifier, data type, or property reference without a qualifying package name path must resolve to a component classifier, data type, or property definition or alias definition in the name space of the package containing the reference. If not present it must resolve to a definition made visible by import declarations.

#. If present, the qualifying package name of a component classifier, data type, or property reference must resolve to a package definition in the global name space.

#. If a definition with the same name exists in the namespace of the package containing the reference and as a definition made visible by an *import* declaration, then references to the *imported* definition must be qualified.

#. A package element marked as *private* must only be referenced within the package it is defined in.
 

Semantics
---------

A *package* provides a way to organize declarations of nested packages, component classifiers, user defined types, property definitions, and annex libraries. 
The double-colon separated defining names of packages reside in a global name space. 

A collection of packages makes up an AADL specification.

Definitions contained in other packages are made visible to a given package through an *import* declaration. Such visible definitions can be referenced by their name without qualifying package name. 
A package name with a wild card <star> makes all contained component classifier, data type, and property definitions of the referenced package visible unless they are declared as **private**. 
An import reference without wildcard makes the referenced component classifier, data type, and property definition visible. In this case users can define a local alias to identify the definition made visible. This allows for deconflicting multiple definitions with the same made visible through import.

A reference to a component classifier, data type, property definition without qualifying package name is resolved in the following order:

1. as name to a definition in the name space of the package containing the reference, i.e., a definition within the same package

#. as identifier of an alias definition, which resolves to the qualified references of the definition in the import declaration.

#. as name to a definition made visible by an import declaration. 

References can always be qualified by a package name. This allows users to refer to imported definitions that have the same name as local definitions or other imported definitions. 

For references qualified with a package name, the referenced component classifier, data type, or property does not have to be made visible through an *import* declaration.


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
A third package contains a data type 'tt' and a nested package with a data type 'tt'. The component interface definition references each of those data types as well as a qualified data type reference without an 'import' declaration. 

| package PackP::Q 
|	 type date ;
|	
|	 package nested  
|	 	type signal ;
|	 end ;
| end ;

| package PackP::R 
|   import PackP::Q::*;              -- makes the type 'date' visible
|   import PackP::Q::nested::signal; -- makes the type 'signal' visible|
|	interface mine is
|	  p1: in feature date;
|	  p2: in feature signal; 
|	end;
| end;
 
| package PackC
|	type tt;
|	package packcc 
|		type tt;
|	end;
|	interface mine
|	is
|		name : in feature PackP::Q::date; -- fully qualified reference
|		surname : in feature tt ;       -- reference to type 'tt' in 'packC'
|		sig : in feature PackC::packcc::tt;     -- reference to type 'tt' in nested package
|	end ;
| end ;