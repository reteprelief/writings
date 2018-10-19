AADL Packages
=============

The *package* concept provides a way to organize declarations of component classifiers, user defined types, property definitions, and annex libraries. 
Packages can be nested, either by syntactically nesting package declarations, or by specifying a package with a dot (**.**) separated sequence of identifiers. The latter allows for packages to be stored in separate files.

A package represents a name space for its content. A nested package, component classifier, type, or property definition is fully qualified by a *dot* separated sequence of identifiers starting with the identifier of a top-level package. A top-level package identifier refers to a package that is not nested in another package. 

Users can make content of other packages visible in the name space of the package with the *import* declaration.  
Content of imported packages can be referenced without qualification if its name is not in conflict with local content or content of other imported packages. 
In case of conflict the reference must be fully qualified.
Packages identified in fully qualified references must be listed in an *import* declaration.

Syntax
------

| AADL\_specification ::= { package\_declaration }\ :sup:`+`
| 
| package\_declaration ::= **package** *defining*\_package\_name
|   { import\_declaration }\ :sup:`\*`
|   { package\_content\_declaration }\ :sup:`\*`
| **end**  **;**
| 
| package\_name ::= *package*\_identifier { **.** *package*\_identifier }\ :sup:`\*`
| 
| package\_content\_declaration ::=
| component_classifier\_declaration
| \| type\_declaration
| \| property\_definition
| \| package\_declaration
| \| annex\_library
| 
| import\_declaration ::= **import** import\_reference { **,** import\_reference  }\ :sup:`\*` **;**
| 
| name\_path ::= identifier { **.** identifier }\ :sup:`\*`
| 
| import\_reference ::= name\_path[. \*]


Naming Rules
-------------

1. An AADL specification has a *global* name space. Top-level package identifiers reside in this name space.

2. Packages introduce name spaces for their content. Nested packages represent nested name spaces.

3. A defining package name consists of a sequence of two or more *dot* separated identifiers represents a nested package of one or more nesting levels. 

4. The name path of an import reference in an *import* declaration can be *relative* or *fully qualified* name path.

5. An import reference with a wildcard <star> indicator makes all content of the referenced package visible. An import reference without wildcard <star> makes the specific referenced package content element visible.

6. Fully qualified name path references must only reference content made accessible by an *import* declaration. 

7. Relative name path references to package content start with the name space of the package that contains the reference. 

8. A relative reference does not require the content to be made visible by an *import* declaration.

9. Package content can be referenced by its name without full qualification if it resides in the same package as the reference, or its name does not conflict with local content or content of other packages listed in with declarations. 



Semantics
---------

A *package* provides a way to organize declarations of nested packages, component classifiers, user defined types, property definitions, and annex libraries. 
Top-level packages, i.e., packages that are not nested, reside in a global name space. 

A collection of packages makes up an AADL specification.

Package content and nested package content can be referenced by a *relative* name path - starting with the enclosing name space of the reference location.

Content of other packages is made accessible through a *import* declaration. 
A name path with a wild card <star> in an *import* declaration makes all content of the identified package accessible. A fully qualified name path without wild card <star> makes the referenced content element accessible.

A relative name path in an *import* declaration makes content of a nested package accessible, while a fully qualified name path makes any package content accessible.

Imported content can be referenced by its identifier. If imported content creates a name conflict with local declarations or other imported content, the reference must be relative or fully qualified. For fully qualified references the referenced model element must have been imported.

A relative name path can reference content of package nested in the package containing the reference. It can do so without the content being imported.

Processing Requirements and Permissions
---------------------------------------

A method of implementation is permitted to enforce that the import
declaration in a package not be changed to enforce the use
restrictions between packages when classifiers are added to the
package.

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

| **package** Aircraft.Cockpit
| **import** Avionics.DataTypes.\*, Management\_Properties.\*;
| 
| **system** MFD
| Airdata: **in data port** AirData ;
| **end** ;
| 
| #Author => "Feiler";
| **end** ;``

