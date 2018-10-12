AADL Packages
=============

 The *package* concept provides a way to organize declarations of component classifiers, user defined types, property definitions, and annex libraries. 
Package names consist of a **::** separated sequence of identifiers and reside in a global name space.
A package represents a name space for its content. A component classifier, type, or property is uniquely identified by qualifying its name with the package name. References to content in the same package do not require qualification by package name. 

 Users indicate their intent to reference content of other packages by a **with** declaration, identifying the packages whose content becomes visible in a given package. 
Content of other packages can be referenced without qualification if its name is not in conflict with local content or content of other packages in **with** declarations. In case of conflict the reference must be qualified.
Packages identified in qualified references must still be listed in a **with** declaration.
 
 Packages can be nested, i.e., a package can be declared inside another package. This allows for grouping of packages. The content of a nested package is uniquely identified by any enclosing package names followed by the given package name and the element contained in the package.

*Syntax*

AADL\_specification ::=

{ package\_declaration }\ :sup:`+`


package\_declaration ::=

**package** *defining*\_package\_name

{ with\_declaration }\ :sup:`\*`

{ package\_content\_declaration }\ :sup:`\*`

[ properties\_subclause ]

**end**  **;**


package\_name ::=

{ *package*\_identifier **::** }\ :sup:`\*` *package*\_identifier


package\_content\_declaration ::=

component_classifier\_declaration

\| type\_declaration

\| property\_definition

\| package\_declaration

\| annex\_library


with\_declaration ::=

**with** package\_name { **,** package\_name }\ :sup:`\*`**;**


properties\_subclause ::=
**properties** { property\_association }\ :sup:`+`


qualified\_package\_content\_reference ::=
package\_name **::** package\_content_name


*Naming Rules*

1. An AADL specification has one *global* namespace. Package names reside in this name space and must be unique.

2. A defining package name consists of a sequence of one or more 
   identifiers separated by a double colon ( **::** ). A package
   name must be unique in the global namespace.

3. Packages represent namespaces for their content. The names of content must be unique within the package namespace. This includes packages declared inside packages.

4. Packages declared inside other packages represent a namespace local to the package, i.e., its content names must be unique within the package namespace.

5.  The package name in a *with\_declaration* must exist in the global
name space or identify a package declared inside a package that exists in the global namespace.

6. Package content in other packages can only be referenced if that package is listed in a *with\_declaration*.

7. Package content can be referenced by its name without qualification if it resides in the same package as the reference, or its name does not conflict with local content or content of other packages listed in with declarations. 
If the referenced content from another package cannot be uniquely identified it must be qualified by the package name separated by **::** and the package name must exist in one of the with declarations.



*Semantics*

 A *package* provides a way to organize declarations of component classifiers, user defined types, property definitions, and annex libraries. 
Packages reside in a global namespace. A collection of packages makes up an AADL specification.

 A package name consists of a double colon (**::**) separated sequence of identifiers to create globally unique package names. 
 
 Packages can be declared inside other packages. This allows for grouping of packages.
 
 The content of packages can be referenced within the same package without qualification. 

 The content of other packages can be referenced if they are listed in **with** declarations. The reference to such content must be qualified by the package name if the content name conflicts with local content or content of other packages listed in **with** declarations.

 Property associations declared in the properties section of a
package are associated with the package itself. 

*Processing Requirements and Permissions*

 A method of implementation is permitted to enforce that the with
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

*Examples*

**package** Aircraft::Cockpit

**with** Avionics::DataTypes, Safety\_Properties;

**system** MFD

Airdata: **in data port** AirData ;

**end** ;

#Annotations::Author => "Feiler";

**end** ;

