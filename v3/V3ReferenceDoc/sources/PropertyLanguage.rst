Properties
==========
| 
| property_reference ::= [ *relative_*name\_path ] **#** *property\_*name\_path

 A property provides information about model elements, i.e.,
component types, component implementations, subcomponents, features,
connections, flows, modes, mode transitions, subprogram calls, and
packages. A property has a name, a type, and a value. The property
definition declares a name for a given property along with the AADL
components and functionality to which the property applies. The
property type specifies the set of acceptable values for a property.
Each property has a value or list of values that is associated with
the named property in a given specification.

 A property set contains declarations of property types and property
definitions that may appear in an AADL specification. The
predeclared property sets in this standard define properties and
property types that are applicable to all AADL specifications. Users
may define property sets that are unique to their model, project or
toolset. The properties and property types that are declared in
user-defined property sets are accessed using their qualified name.
A property definition declaration within a property set indicates
the component types, component implementations, subcomponents,
features, connections, flows, modes, and subprogram calls, for which
this property applies.

Properties can have associated expressions that are statically
typed, and evaluate to a specific value. The time at which a
property expression is evaluated may depend on the property and on
how a specification is processed. For example, some expressions may
be evaluated immediately, some after binding decisions have been
made, and some reflect runtime state information, e.g., the current
mode. During analysis, all property expressions can be evaluated to
known values, if necessary, by considering all possible runtime
states. A given property definition may have a default expression.

Property Sets
-------------

 A property set defines a named group of property types, property
definitions, and property constant values.

Syntax

property\_set ::=

**property set** *defining\_­property\_set*\ \_identifier **is**

{ import\_declaration }\ :sup:`\*`

{ property\_type\_declaration

\| property\_definition\_declaration

\| property\_constant }\ :sup:`\*`

{ annex\_subclause }\ :sup:`\*`

**end** *defining\_property\_set*\ \_identifier ;

Naming Rules

1. Property set defining identifiers must be unique in the global
   namespace.

1. The defining identifier following the reserved word **end** must be
   identical to the defining identifier following the reserved word
   **property** **set**.

2. Associated with every property set is a *property set namespace* that
   contains the defining identifiers for all property types, property
   definitions, and property constants declared within that property
   set. This means that property types, properties, and property
   constants with the same identifier can be declared in different
   property sets.

3. A property, property constant, or property type declared in a
   property set is always named by its qualified name that is the
   property set identifier followed by the property identifier,
   separated by a double colon (::). This qualification is necessary
   even for references to properties, property constants, and property
   types in the same property set due to the fact that predeclared
   properties do not have to be qualified. Predeclared properties and
   property types may be referred to by their property identifiers
   without a property set qualifier.

4. The property set identifiers and package names listed in an
   import\_declaration must exist in the global namespace.

5. The property set identifier of a qualified property name must be
   listed in an import\_declaration of the property set or package
   unless it is the name of the property set that contains the qualified
   name.

Semantics

 Property definitions, property types, and property constants are
organized into property sets. They are referenced by qualifying them
with the property set name. This qualification is not required if
the property set is one of the predeclared property sets.

An import\_declaration of a property set specifies which property
sets and packages can be named in qualified references to items in
other property sets.

Property Types
--------------

 A property type declaration associates an identifier with a property
type. A property type denotes the set of legal values in a property
association that are the result of evaluating the associated
property expression.

Syntax

property\_type\_declaration ::=

*defining\_property\_type*\ \_identifier : **type** property\_type **;**

property\_type ::=

**aadlboolean** \| **aadlstring**

\| enumeration\_type \| units\_type

\| number\_type \| range\_type

\| classifier\_type

\| reference\_type

\| record\_type

enumeration\_type ::=

**enumeration (** *defining\_enumeration\_literal*\ \_identifier

{ **,** *defining\_enumeration\_literal*\ \_identifier
}\ :sup:`\*`\ **)**

units\_type ::=

**units** units\_list

units\_list ::=

**(** *defining\_unit*\ \_identifier

{ **,** *defining\_unit*\ \_identifier **=>** *unit*\ \_identifier
**\*** numeric\_literal }\ :sup:`\*` **)**

number\_type ::=

**aadlreal** [ real\_range ] [ **units** units\_designator ]

\| **aadlinteger** [ integer\_range ] [ **units** units\_designator ]

units\_designator ::=

*units\_*\ unique\_property\_type\_identifier

\| units\_list

real\_range ::= real\_lower\_bound **..** real\_upper\_bound

real\_lower\_bound ::= signed\_aadlreal\_or\_constant

real\_upper\_bound ::= signed\_aadlreal\_or\_constant

integer\_range ::= integer\_lower\_bound **..** integer\_upper\_bound

integer\_lower\_bound ::= signed\_aadlinteger\_or\_constant

integer\_upper\_bound ::= signed\_aadlinteger\_or\_constant

signed\_aadlreal\_or\_constant ::=

( signed\_aadlreal \| [ sign ] *real\_*\ property\_constant\_term )

signed\_aadlinteger\_or\_constant ::=

( signed\_aadlinteger \| [ sign ] *integer*\ \_property\_constant\_term
)

sign ::= **+** \| **-**

signed\_aadlinteger ::=

[ sign ] integer\_literal [ *unit*\ \_identifier ]

signed\_aadlreal ::=

[ sign ] real\_literal [ *unit*\ \_identifier ]

range\_type ::=

**range of** number\_type

\| **range of** *number\_*\ unique\_property\_type\_identifier

classifier\_type ::=

**classifier**

[ **(** classifier\_category\_reference { **,**
classifier\_category\_reference }\ :sup:`\*` **)** ]

classifier\_category\_reference ::=

-- AADL or Annex meta model classifier

*classifier*\ \_qualified\_meta\_model\_identifier

qualified\_meta\_model\_identifier ::=

[ **{** *annex*\ \_identifier **}\*\*** ] meta\_model\_class\_identifier

meta\_model\_class\_identifier ::= { identifier }\ :sup:`+`

reference\_type ::=

**reference** [ **(** reference\_category

{ **,** reference\_category }\ :sup:`\*` **)** ]

reference\_category ::=

-- AADL or Annex meta model named element

*named\_element\_*\ qualified\_meta\_model\_identifier

unique\_property\_type\_identifier ::=

[ *property\_set*\ \_identifier **::** ] *property\_type*\ \_identifier

record\_type ::=

**record (**\ {record\_field}+ **)**

record\_field ::=

*defining\_field*\ \_identifier **:** [ **list of** ]
property\_type\_designator **;**

property\_type\_designator ::=

unique\_property\_type\_identifier \|

property\_type

Naming Rules

1. All property type defining identifiers declared within the same
   property set must be distinct from each other, i.e., unique within
   the property set namespace.

1. A property type is named by its property type identifier or the
   qualified name specified by the property set/property type identifier
   pair, separated by a double colon (::). An unqualified property
   type identifier must be part of the predeclared property sets.

2. An enumeration type introduces an enumeration namespace. The
   enumeration literal identifiers in the enumeration list declare an
   ordered list of enumeration literals. They must be unique within this
   namespace.

3. A units type introduces a units namespace. The units identifiers in
   the units list declare a set of units literals. They must be unique
   within this namespace.

4. The units identifier to the right of a **=>** in a units literal
   statement must refer to a unit identifier defined earlier in the
   sequence of the same units type declaration.

5. The *classifier meta model identifier* must refer to a class in the
   AADL meta model or an Annex meta model. In the case of an Annex meta
   model, the identifier is qualified by the annex name. Acceptable
   classes are listed in tabular form in Appendix C.3 and in relevant
   Annex standards.

6. The *named element meta model identifier* must refer to a class in
   the AADL meta model or an Annex meta model that is a subclass of the
   *NamedElement* class and a structural feature, in the case of the
   AADL core language a subclass of the *ClassifierFeature* class. In
   the case of an Annex meta model, the identifier is qualified by the
   annex name. Acceptable classes are listed in tabular form in an
   appendix of this standard and in relevant Annex standards.

7. The identifiers of the property field declarations in a **record**
   property type must be unique within the record declaration, i.e., the
   record type represents a local namespace for record field
   identifiers.

Legality Rules

1. The value of the first numeric literal that appears in a range of a
   number\_type must not be greater than the value of the second
   numeric literal including the value’s units.

1. Range values must always be declared with unit literals if the
   property requires a unit literal.

2. The unique property constant identifier in an integer range must
   represent an integer constant. If the integer type requires
   units, then the constant value must include a unit literal of the
   specified units type.

3. A boundless range type may be declared such that the actual range
   declarations have no limit on the upper and lower bound.

4. The unique property constant identifier in a real range must
   represent a real constant. If the real type requires units, then
   the constant value must include a unit literal of the specified
   units type.

5. If the property requires a unit, then the unit must be specified for
   both lower and upper bound and the unit literal must be of the
   specified units type..

6. If a range is specified for **aadlinteger** or **aadlreal** then the
   actual value assigned to a property of this type must be within
   the specified range.

NOTE: In the original AADL standard reserved words were used to identify
the classifier category or reference category. Those names are
compatible with the qualified meta model identifiers of AADL V2 with the
exception of *connections*. It is now named *connection*.

Semantics

  A property type declaration associates an identifier with a
 property type.

 The **aadlboolean** property type represents the two values, true
 and false.

  The **aadlstring** property type represents all legal strings of
 the AADL.

  An **enumeration** property type represents an ordered list of
 enumeration identifiers as the set of legal values.

(5)  A **units** property type represents an explicitly listed set of
 measurement unit identifiers as the set of legal values. The second
 and succeeding unit identifiers are declared with a multiplier
 representing the conversion factor that is applied to a preceding
 unit to determine the value in terms of the specified measurement
 unit.

(6)  An **aadlreal** property type represents a real value or a real
 value and its measurement unit. If a units clause is present, then
 the type value is a pair of values, a real value and a unit. The
 unit may only be one of the enumeration literals specified in the
 units clause. If a units clause is absent, then the value is a real
 value

(7)  An **aadlinteger** property type represents an integer value or an
 integer value and its measurement unit. If a units clause is
 present, then the value is a pair of values, and unit may only be
 one of the enumeration literals specified in the units clause. If a
 units clause is absent, then the value is an integer value.

(8)  The **range** property type represents closed intervals of numbers.
 It specifies that a property of this type has a value that is a
 range term. The range type specifies the number type of values in
 the range. A property specifying a range term as its value
 indicates a least value called the lower bound of the interval, a
 greatest value called the upper bound of the interval, and
 optionally the difference between adjacent values called the
 **delta**. The delta may be unspecified, in which case the range is
 dense, but it is otherwise undefined whether the range is an
 interval of the real or the rational numbers.

(9)  A **classifier** property type represents the subset of
 syntactically legal classifier references, whose class is a
 subclass of the *Classifier* meta model class and of one of the
 meta model classes listed in the classifier identifier list. For
 core AADL this is typically the meta model class that represents a
 component category. If the classifier identifier list is absent,
 all classifier references are acceptable.

(10) A **reference** property type represents the subset of
 syntactically legal references to those model elements, whose class
 is a subclass of the meta model class *Named Element* and
 *ClassifierFeature* or a structural element of an annex clause. A
 *ClassifierFeature* is a structural element that is contained in a
 component, such as ports, flow specifications, subcomponents, or
 connections. If the identifier list is absent, all model elements
 whose class is a subclass of *NamedElement* and *ClassifierFeature*
 are acceptable.

(11) A **record** type represents a group of property associations,
 i.e., a collection of property values, where each element of the
 collection of property values is accessible by name. The record
 fields must be explicitly declared by name, each with its own
 property type.

NOTE: The classifier and reference property types support the
specification of properties representing binding constraints.

Units literals are not case sensitive. For example, *mW* and *MW*
represent the same units literal. Therefore, it is advisable to choose
literal names such as *milliWatts* and *MegaWatts*.

Examples

**property set** mine **is**

Length\_Unit : **type units** ( mm, cm => mm \* 10,

m => cm \* 100, km => m \* 1000 );

OnOff : **type aadlboolean**;

-- This type declaration references a separately declared units type

Car\_Length : **type aadlreal** 1.5 m .. 4.5 m **units**
mine::Length\_Unit ;

-- This type declaration defines the units in place

Speed\_Range : **type range of aadlreal** 0.0 kph .. 250.0 kph **units**
( kph );

Position : **type record** (

X: **aadlinteger**;

Y: **aadlinteger**; );

**end** mine;

Property Definitions
--------------------

 All property names that appear in a property association list must
be declared with property definition declarations inside a property
set. Properties are typed and are defined for any named element in
an AADL model.

Syntax

property\_definition\_declaration ::=

*defining\_property\_name*\ \_identifier\ **:**

[ **inherit** ]

( single\_valued\_property \| multi\_valued\_property )

**applies to** **(** property\_owner { **,** property\_owner
}\ :sup:`\*` **) ;**

single\_valued\_property ::=

property\_type\_designator [ **=>** *default*\ \_property\_expression ]

multi\_valued\_property ::=

{ **list of** }\ :sup:`+` property\_type\_designator

[ **=>** *default*\ \_property\_list\_value

]

property\_owner ::=

-- AADL or Annex meta model named element

*named\_element\_*\ qualified\_meta\_model\_identifier \|

unique\_classifier\_reference \| **all**

unique\_classifier\_reference ::=

package\_name **::** classifier\_identifier

NOTE: Different from AADL V1 the **access** keyword is no longer used in
property definitions. The fact that a property applies to an access
feature is already specified in the applies to clause.

Naming Rules

1. All defining identifiers of property definitions declared within the
   same property set must be distinct from each other and distinct from
   all property type defining identifiers declared within that property
   set. The property set namespace contains the defining identifiers for
   all property definitions declared within that property set.

2. A property is named by its property definition identifier or the
   qualified name specified by the property set/property definition
   identifier pair, separated by a double colon (::). An unqualified
   property identifier must be part of the predeclared property sets.

1. The named\_element\_meta\_model\_identifier must identify the name of
   a class in the AADL meta model or an Annex meta model that is a
   subclass of the AADL meta model class NamedElement. In case of an
   Annex meta model, the identifier is qualified by the annex name.
   Acceptable classes are listed in tabular form in an appendix of this
   standard and in relevant Annex standards.

2. The unique\_classifier\_reference must identify the name of a
   classifier in the public section of the named package.

Legality Rules

1. All properties are automatically defined for components of the
   category **abstract**, i.e., the category **abstract** is
   implicitly included in all **applies to** statements.

1. **Classifier** and **reference** property definition must not have a
   default value.

2. When the property owner is set to **all**, this is the only possible
   value.

Semantics

 A property definition declaration introduces a new property that is
of a specified property type, accepts a single value or a list of
values, and may specify a default property expression. This property
is defined for those named elements of an AADL model whose meta
model name is listed after the **applies to** in the list, or those
model elements that are of the specified component type. Acceptable
meta model names are listed in tabular form in Appendix C.3 of this
standard and in relevant Annex standards.

The Property\_Owner list may include component types qualified by a
package name. In this case, the property can only be associated with
components of this component type or its extensions. If the
Property\_Owner list includes NamedElement, then the property can be
associated with all model elements.

 A property defined with the reserved word **inherit** indicates that
if a property value cannot be determined for a component, then its
value will be inherited from a containing component. The detailed
rules for determining property values are described in Section 11.3.

 A property declared without a default value is considered undefined
(see also Section 11.3). A property declared to have a list of
values is considered to have an empty list if no default value is
declared.

Examples

-- added to Property Set mine from previous example

Value\_Type: **type enumeration** ( estimate, benchmark, measured );

Rotation\_Units: **type units** ( rpm );

Position : **type record** (

X: **aadlinteger**;

Y: **aadlinteger**; );

GPS\_Position : mine::Position **applies to** ( system );

Property Constants
~~~~~~~~~~~~~~~~~~

 Property constants are property values that are known by a symbolic
name. Property constants are provided in the predeclared property
sets and can be defined in property sets. They can be referenced in
property expressions by name wherever the value itself is
permissible.

Syntax

property\_constant ::=

single\_valued\_property\_constant \| multi\_valued\_property\_constant

single\_valued\_property\_constant ::=

*defining\_property\_constant*\ \_identifier **:** **constant**

property\_type\_designator

**=>** *constant*\ \_property\_expression\ **;**

multi\_valued\_property\_constant ::=

*defining\_property\_constant*\ \_identifier **:** **constant** ( **list
of** )\ :sup:`+`

property\_type\_designator

**=>** *constant*\ \_property\_list\_value **;**

unique\_property\_constant\_identifier ::=

[ *property\_set*\ \_identifier **::** ]
*property\_constant*\ \_identifier

Naming Rules

1. The defining property constant identifier must be distinct from all
   other property constant identifiers, property definition identifiers,
   and property type identifiers in the namespace of the property set
   that contains the property constant declaration.

1. A property constant is named by its property constant identifier or
   the qualified name specified by the property set/property constant
   identifier pair, separated by double colon (::). An unqualified
   property constant identifier must be part of the predeclared property
   sets. Otherwise, the property constant identifier must appear in the
   property set namespace.

Legality Rules

1. A property constant cannot be declared for the **classifier**
   property type or the **reference** property type.

1. If a property constant declaration has more than one property
   expression, it must contain the reserved words **list of**.

2. The property type of the property constant declaration must match the
   property type of the constant property value.

Semantics

 Property constants allow integer, real, string, and other property
values to be known by symbolic name and referenced by that name in
property expressions. This reference is expressed by referencing the
unique property constant identifier.

Examples

Max\_Threads : **constant** **aadlinteger** => 256;

Predeclared Property Sets
-------------------------

 There is a standard collection of predeclared property sets named
Deployment\_Properties, Thread\_Properties, Timing\_Properties,
Memory\_Properties, Programming\_Properties, and
Modeling\_Properties, which are part of every AADL specification.
These property sets are listed in Appendix A .

In addition, there is property set AADL\_Project that declares a set
of enumeration property types and property constants for which
project-specific enumeration literals and values can be defined for
different projects. This property set is part of every AADL
specification. All of the property enumeration types and property
constants listed in Appendix A.2 must be declared in this property
set. The set of enumeration literals may vary.

Naming Rules

1. References to predeclared properties, property types, and property
   constants do not have to be qualified with the property set name. As
   a consequence the predeclared properties, property types, and
   property constants must be unique across all predeclared property
   sets.

NOTE: References to predeclared properties, property constants, and
property types may be qualified with their property set name.

Legality Rules

1. The predeclared property sets other than AADL\_Project cannot be
   modified.

1. Existing property type and property constant declarations in the
   AADL\_Project property set can be modified. New declarations must
   not be added to the AADL\_Project property set, but can be
   introduced through a separate property set declaration.

Processing Requirements and Permissions

 Additional property name declarations may not be inserted into the
standard predeclared property sets. Separate property set
declarations must be used for nonstandard property definitions.

Providers of AADL processing methods may modify the standard
property type declarations in AADL\_Project to allow additional
values for a specific property definitions. For example, additional
enumeration identifiers beyond those listed in this standard may be
added.

 Users may define additional property sets and use them in AADL
specifications. AADL tools may be created that make use of those
additional property sets.

 Additional property sets that may be suitable for a wide variety
applications may be defined in an Annex document. AADL tools that
support this Annex should include support for these additional
property sets. Similarly, AADL specifications that conform to the
Annex shall satisfy the requirements associated with the annex
property set.

Property Associations
---------------------

 A property association assigns a property value or list of property
values with a property. This may involve evaluation of a property
expressions. Property associations can be declared within component
types, component implementations, subcomponents, features,
connections, flows, modes, and subprogram calls, as well as their
respective refinement declarations. Contained property associations
permit property values to be associated with any component in the
system instance hierarchy (see Section 13.1).

Syntax

basic\_property\_association ::=

unique\_property\_identifier

( **=>** \| **+=>** )

[ **constant** ] property\_value **;**

property\_association ::=

unique\_property\_identifier

( **=>** \| **+=>** )

[ **constant** ] assignment

[ in\_binding ] **;**

contained\_property\_association ::=

unique\_property\_identifier

**=>** [ **constant** ] assignment

**applies to** contained\_model\_element\_path

{ **,** contained\_model\_element\_path }\ :sup:`\*`

[ in\_binding ] **;**

unique\_property\_identifier ::=

[ *property\_set*\ \_identifier :: ] *property\_name*\ \_identifier

contained\_model\_element\_path ::=

( contained\_model\_element { **.** contained\_model\_element
}\ :sup:`\*`

[ **@** annex\_specific\_path\ *\_fragment* ] )

\| annex\_specific\_path

contained\_model\_element ::=

*named\_element*\ \_identifier \|

*named\_element\_*\ array\_selection\_identifier

annex\_specific\_path ::=

*annex\_*\ contained\_model\_element { **.**
*annex*\ \_contained\_model\_element }\ :sup:`\*`

assignment ::= property\_value \| modal\_property\_value

modal\_property\_value ::=

{ property\_value in\_modes **,** }\ :sup:`\*` property\_value [
in\_modes ]

property\_value ::= single\_property\_value \| property\_list\_value

single\_property\_value ::= property\_expression

property\_list\_value ::=

**(** [ ( property\_list\_value \| property\_expression )

{ **,** ( property\_list\_value \| property\_expression ) }\ :sup:`\*` ]
**)**

in\_binding ::=

**in binding(** platform\ *\_*\ classifier\_reference

{ **,** platform\ *\_*\ classifier\_reference }\ :sup:`\*` **)**

*platform*\ \_classifier\_reference ::=

*processor*\ \_unique\_component\_classifier\_reference

\| *virtual\_processor*\ \_unique\_component\_classifier\_reference

\| *bus*\ \_unique\_component\_classifier\_reference

\| *virtual\_bus*\ \_unique\_component\_classifier\_reference

\| *memory*\ \_unique\_component\_classifier\_reference

Naming Rules

1. A property is named by an optional property set identifier followed
   by a property identifier, separated by a double colon (::).

1.  The property set identifier, if present, must appear in the global
namespace and must be the defining identifier in a property set
declaration.

2.  The property identifier must exist in the namespace of the property
set, or if the optional property set identifier is absent, in the
namespace of any predeclared property set.

3.  A property name may appear in the property association clause only
if the respective AADL model element is listed in the applies to
list of the property definition declaration.

4.  The *contained model element path* identifies named model elements
in the containment hierarchy, for which the property value holds.
The model element with the contained property association is the
root of a path.

5.  For contained property associations declared with classifiers, e.g.,
a component type or component implementation, the first identifier
in the path must appear in the local namespace of the classifier to
which the property association belongs.

6.  For contained property associations declared with model elements
with a classifier reference, e.g., subcomponents, the first
identifier must appear as an identifier within the local namespace
of the classifier that is referenced by the model element.

7.  For contained property associations declared in annex subclauses
without an annex specific path fragment, the first identifier must
refer to an element in the namespace of the annex subclause that
contains the property association.

8.  For contained property associations declared in annex subclauses
with an annex specific path fragment, the first identifier of the
annex specific path must refer to an element in the namespace of the
annex subclause declared for the last path element in the core
model.

9.  Subsequent identifiers in a contained model element path must appear
in the namespace of the model element identified by the preceding
identifier.

10.  Identifiers in an annex-specific path must appear in the namespace
of the annex that contains the property association.

11. If the identifier of a contained model element path is a
subcomponent array identifier, it can specify a subcomponent array
as a whole, an array subset, or an individual array element.

12. If a property association has an **in binding** statement, then the
unique platform classifier reference must be referenceable according
to the **with** and **renames** declarations.

1. If a property association has mode-specific values, i.e., an **in
   modes** statement for values, then the mode must refer to a mode of
   the component the property is associated with, or in the case of a
   property association of model elements that are not components, the
   modes of the containing component.

2. A property association list must have at most one property
   association for the same property. In case of binding-specific
   property associations, there must be at most one association for each
   binding.

Legality Rules

1. The property definition named by a property association must list the
   class of the model element, with which the property is
   associated, or any of its super classes in its **applies to**
   clause.

1.  A contained property association with mode-specific values can only
be applied to a single model element, i.e., can only contain a
single containment path.

2.  If a property expression list consists of a list of two or more
property expressions, all of those property expressions must be
of the same property type.

3.  If the property declaration for the associated property definition
does not contain the reserved words **list of**, the property
value must be a single property value. If the property
declaration for the associated property definition contains the
reserved words **list of**, the property list value must have
the correct number of parentheses to match the list or nested
list declaration.

4.  The property association operator **+=>** must only be used if the
property declaration for the associated property definition
contains the reserved words **list of**.

5.  A property association with an operator **+=>** must not have an
**in modes** or **in binding** statement.

6.  The property association operator **+=>** must not be used in
contained property associations.

7.  In a property association, the type of the evaluated property
expression must match the property type of the named property.

8.  A property value declared by a property association with the
reserved word **constant** cannot be changed when the rules in
the *semantics* section for determining a property value are
followed.

9.  The unique component type identifiers in the **in binding**
statement must refer to component types of the categories
**processor**, **virtual processor**, **bus**, **virtual bus**,
or **memory.**

10. If a property value with an **in modes** statement is associated
with a connection, flow implementation, or call sequence with an
**in modes** statement, then the set of modes for which the
property value applies must be contained in the set of modes for
which the connection, flow implementation, or call sequence is
active.

11. Contained property associations with annex specific paths must be
declared in the respective annex subclause.

Consistency Rules

1. If a property association has mode-specific values, i.e., values
   declared with the **in modes** statement, then the modal value
   assignment must include a value for each mode. If the modal value
   assignment includes a value without the **in modes** statement, this
   value becomes the default for all modes without an explicit
   mode-specific value.

Semantics

  Property associations determine the property value of the model
 element instances in the system instance hierarchy (see Section
 13.1). The property association of a classifier, subcomponent,
 feature, flow, connection, mode, or subprogram call and other
 declarative model elements determines the property value of all
 instances derived from the respective declaration.

 A property association may be declared for a package. In this case,
 the property value applies to the package.

  The value of a property is determined through evaluation of the
 property expression.

  Property associations are declared in the properties subclause of
 component types and component implementations. They are also
 declared as part of any other named declarative model element, such
 as features, subcomponents, connections, modes, mode transitions,
 etc. The property association of a component type acts as default
 value for all implementations, subcomponents, and instances,
 overwriting the default specified in the property definition.
 Similarly, the property association of a component implementation
 overwrites the value of a component type, and the subcomponent
 property association overwrites the value of the component
 implementation. The details of determining a property value are
 specified below.

(5)  If a property association is declared with modal property values,
 then the same rules hold for overwriting previous property
 associations. Property values are modal if they are declared with
 an **in modes**. In this case the property value applies if one of
 the specified modes is active. If a property association contains
 both mode-specific associations and value without an **in modes**
 statement, then the latter specifies the value for all modes for
 which there is not an explicit value with an **in modes**
 statements. A property value without an **in modes** statement also
 applies when the component is inactive. If a modal property
 association does not specify a property value for one of the modes
 and there is no property value without **in modes**, then the
 property value is considered to be undefined.

(6)  If a property association has an **in binding** statement, the
 property value is binding-specific. The property value applies if
 the binding is to one of the specified execution platform types of
 the categories processor, virtual processor, bus, virtual bus, or
 memory. If a property association list contains both
 binding-specific associations and an association without an in
 binding statement, then the latter applies to bindings to the
 reference processor.

(7)  Contained property associations can associate property values to
 any named model element down the system hierarchy relative to the
 location of the declaration.

(8)  The **applies to** clause of a contained property association may
 specify multiple contained model element paths. In that case the
 property value is associated with each of the model elements. If
 the property association specifying contained model elements
 includes an **in modes** statement, then the named modes must exist
 in the last component of the contained element path. If the path
 refers to a model element that is not a component, e.g., a feature,
 then the modes must exist in the containing component of that model
 element.

(9)  A contained model element path may include the name of an array of
 components, or an index range of the array subset. In that case the
 property value is associated with each of the model elements in the
 array or its subset, or model elements identified by the remaining
 path for each of the array elements.

(10) Contained property associations can be used to record system
 instance specific property values for all model elements in a
 system instance. This permits AADL analysis tools to record system
 instance specific information about an actual system in a single
 location in an extension of the top-level system implementation.
 For example, a resource allocation tool can record the actual
 bindings of threads to processors and source text to memory through
 a set of contained property associations, and can keep multiple
 such binding configurations in different extensions of the same
 system implementation.

(11) The property value is determined according to the following rules:

-  If a property value is not present after applying all of the rules
   below, it is determined by the default value of its property
   definition declaration. If not present in the property definition
   declaration, the property value is undefined.

-  For classifier types, i.e., component types and feature group types
   and subclasses of ComponentType in Annex meta models, the property
   value of a property is determined by its property association in the
   properties subclause. If not present, the property value is
   determined by the first ancestor classifier type in the extends
   hierarchy with its property association. Otherwise, it is considered
   not present.

-  For classifier implementations, i.e., a component implementations and
   subclasses of ClassifierImplementation in Annex meta models, the
   property value of a property is determined by its property
   association in the properties subclause. If not present, the property
   value is determined by the first ancestor classifier implementation
   in the extends hierarchy with its property association. If not
   present, it is determined by the property value of the classifier
   implementation’s classifier type according to the classifier type
   rules.

-  For modes, connections, flow sequences, or other model elements
   without a classifier reference, the property value of a property is
   determined by its property association in the element declaration. If
   not present and the model element is refined, then the property value
   is determined by a property association in the model element
   declaration being refined; this is done recursively along the
   refinement sequence. If not present and the property definition has
   been declared as **inherit**, it is determined by the property value
   of the closest containing component in the containment hierarchy of
   the system instance. This inherited property value must not be
   mode-specific. Otherwise, it is considered not present.

-  For subcomponents, features and other model elements with classifier
   references, the property value of a property is determined by its
   property association in the model element declaration. If not present
   and the model element is refined, then the property value is
   determined by a property association in the model element declaration
   being refined; this is done recursively along the refinement
   sequence. If not present in the model element, it is determined by
   the model element’s classifier reference according to the respective
   classifier rules described above. If not present for a feature that
   is a member of a feature group and the property definition has been
   declared as **inherit**, it is determined by the property value of
   the containing feature group type; or if not present and the feature
   group type is an extension of another freature group type, it is
   determined by the feature group type being extended. If not present
   and the property definition has been declared as **inherit**, it is
   determined by the property value of the closest containing component
   in the containment hierarchy of the system instance. This inherited
   property value must not be mode-specific. Otherwise, it is considered
   not present.

-  For subprogram calls in call sequences and other model elements with
   classifier or feature references, the property value of a property is
   determined by its property association in the model element. If not
   present and the reference is a classifier reference, the property
   value is determined by the classifier according to its rules
   described above. If not present and the reference is a feature
   reference in a type, the property value is determined by the feature
   according to the feature rules described above. If not present and
   the property definition has been declared as **inherit**, it is
   determined by the property value of the closest containing component
   in the containment hierarchy of the system instance. This inherited
   property value must not be mode-specific. Otherwise, it is considered
   not present.

-  For component, feature, connection, flow, mode, or other model
   element instances in the system instance hierarchy, the property
   value of a property is determined by the contained property
   association highest in the system instance hierarchy that references
   the component, feature, connection, flow, mode, or other model
   element. If not present, then the property value is determined by the
   respective subcomponent, mode, connection, feature, or other model
   element declaration that results in the instance according to the
   rules above. If not present and the property definition has been
   declared as **inherit**, then it is determined by the property value
   of the closest containing component in the containment hierarchy of
   the system instance. This inherited property value must not be
   mode-specific. Otherwise, it is undefined.

|image18|

Figure − Property Value Determination
 

 Figure 21 illustrates the order in which the value of a property is
determined. Instance4 is an element in the system instance
hierarchy. The value of one of its properties is determined by first
looking for a property associated with the instance itself – shown
as step 1. This is specified by a contained property association.
The contained property association for this instance declared in a
component implementation highest in the instance hierarchy
determines that value. If no instance value exists, the
implementation (ImplA) of the instance is examined (step 2). If it
does not exist, ancestor implementations are examined (step 3). If
the property value still has not been determined, the component type
is examined (step 4). If not found there, its ancestor component
types are examined (step 5). If not found and the property is
inherited, for subcomponents and features, the enclosing
implementation is examined. Otherwise, the containing component in
the component instance hierarchy is examined (step 6). Finally, the
default value is considered.

Two property association operators are supported. The property
association operator **=>** results in a new value for the property.
The property association operator **+=>** results in the addition of
a value to a property value list. In the case of nested lists, the
value is added to the outermost list.

 A property value list is evaluated by evaluating each of the
property expressions, and appending the values in order. If the
property expression evaluates to a list, all the list elements are
appended. If the property expression evaluates to undefined, it is
treated as an empty list.

 A property value declared by a property association with the
reserved word **constant** cannot be changed. For example, if a
property association is defined as **constant** for a component
type, then there cannot be a property association for the same
property in any component implementation of the type, nor any
subcomponent, nor contained property association that applies to a
component of the component type.

(5) If the property type is a **record**, a property association
represents the assignment of a new record value. If a subset of
record fields is assigned a value, the remaining fields have an
undefined value and not inherited.

(6) Component instance property associations with specified contained
subcomponent identifier sequences allow separate property values to
be associated with each component instance in the containment
hierarchy. In particular, it permits separate property values such
as actual processor binding property values or result values from an
analysis method to be associated with each component in the system
instance containment hierarchy.

Property Expressions
--------------------

 A property expression represents the value that is associated with a
property through a property association. The type of the property
expression must match the property type declared for the property.

Syntax

property\_expression ::=

boolean\_term

\| real\_term

\| integer\_term

\| string\_term

\| enumeration\_term

\| unit\_term

\| real\_range\_term

\| integer\_range\_term

\| property\_term

\| component\_classifier\_term

\| reference\_term

\| record\_term

\| computed\_term

boolean\_term ::=

boolean\_value

\| *boolean\_*\ property\_constant\_term

boolean\_value ::= **true** \| **false**

real\_term ::=

signed\_aadlreal\_or\_constant

integer\_term ::=

signed\_aadlinteger\_or\_constant

string\_term ::= string\_literal \| *string*\ \_property\_constant\_term

enumeration\_term ::=

*enumeration*\ \_identifier \| *enumeration*\ \_property\_constant\_term

unit\_term ::=

*unit*\ \_identifier \| *unit*\ \_property\_constant\_term

integer\_range\_term ::=

integer\_term **..** integer\_term [ **delta** integer\_term ]

\| *integer\_range*\ \_property\_constant\_term

real\_range\_term ::=

real\_term **..** real\_term [ **delta** real\_term ]

\| *real\_range*\ \_property\_constant\_term

property\_term ::=

[ *property\_set*\ \_identifier **::** ] *property\_name*\ \_identifier

property\_constant\_term ::=

[ *property\_set*\ \_identifier **::** ]
*property\_constant*\ \_identifier

component\_classifier\_term ::=

**classifier (**

( unique\_component\_type\_reference \|

unique\_component\_implementation\_reference ) **)**

reference\_term ::=

**reference (** contained\_model\_element\_path **)**

record\_term ::=

**[** *record\_field*\ \_identifier => property\_value **;**

{ *record\_field*\ \_identifier => property\_value\ **;** }\ :sup:`\*`
**]**

computed\_term ::=

**compute (** *function\_*\ identifier **)**

Naming Rules

1. The component type identifier or component implementation name of a
   subcomponent classifier reference must appear namespace of the
   specified package.

1. The enumeration identifier of a property expression must have been
   declared in the enumeration list of the property type that is
   associated with the property.

2. For reference terms the naming rules (N5) .. (N11) in Section 11.3
   are applicable in order to resolve contained model element paths.

3. If a **classifier** property is associated with a core AADL model
   element, then the classifier meta model identifier of a classifier
   term can only refer to a core AADL meta model class.

4. If a **classifier** property is associated with an AADL annex model
   element, then the classifier meta model identifier of a classifier
   term can refer to a core AADL meta model class or an annex meta model
   class of the same annex.

5. If a **reference** property is associated with a core AADL model
   element, then the contained model element path of a reference term
   can only refer to a core AADL model element. The first identifier in
   the path must be defined in the namespace of the directly enclosing
   component that contains the property association or the classifier of
   the subcomponent when associated with a subcomponent.

6. If a **reference** property is associated with an AADL annex model
   element, then the contained model element path of a reference term
   can refer to a core AADL model element or an annex model element of
   the same annex.

7. The field identifier of a record expression must exist in the local
   namespace of the record type.

8. The function identifier of a **compute** expression must exist as
   function in the source text.

Legality Rules

1. If the base type of a property number type or range type is integer,
   then the numeric literals must be integers.

1. The type of a property named in a property term must be identical to
   the type of the property name in the property association.

2. The type of a property constant named in a property constant term
   must match the type of the property name in the property
   association.

3. Property references in *property\_term* or *property\_constant\_term*
   of property expressions must be applicable to the model element
   to which the property association applies.

4. Property references in *property\_term* or *property\_constant\_term*
   of property expressions cannot be circular. If a property has a
   property expression that refers to a property or property
   constant, then that expression evaluation cannot directly or
   indirectly depend on the value of the original property or
   property constant.

5. If the contained model element path of a reference term includes a
   subcomponent array identifier that does not identify a single
   element in the array, then the expression results in a list of
   references.

Semantics

 Every property expression can be evaluated to produce a value, a
range of values, or a reference. It can be statically determined
whether this value satisfies the property type designator of the
property name in the property association. The value of the property
association may evaluate undefined, if no property association or
default value has been declared.

*Boolean terms* are of property type **aadlboolean**. The reserved
words **true** and **false** evaluate to the Boolean values true and
false.

  *Number terms* evaluate to a numeric value denoted by the numeric
 literal, or evaluate to a pair consisting of a numeric value and
 the specified units identifier. A number term satisfies an
 **aadlinteger** property type if the numeric value is a numeric
 literal without decimal point or exponent. Otherwise, it satisfies
 the **aadlreal** property type. If specified, the units identifier
 must be one of the unit identifiers in the unit designator of the
 property type. Furthermore, the value must fall within the
 optionally specified range of the property type – taking into
 account unit conversion as necessary.

 *Enumeration terms* evaluate to enumeration identifiers. The
 **enumeration** property type of the property name is satisfied if
 the enumeration identifier is declared in the enumeration list of
 the property type.

  *Range terms* are of **range** property type and are represented by
 number terms for lower and upper range bounds plus and an optional
 **delta** value. Range terms evaluate to two or three numeric
 values that and each must satisfy the number type declared as part
 of the range property type. The **delta** value represents the
 maximum difference between two values, e.g., between two values of
 a data stream coming through a data port.

  *String terms* are of **aadlstring** property type. A string
 literal evaluates to the string of characters denoted by that
 literal.

(5)  *Property terms* evaluate to the value of the referenced property.
 This allows one property value to be expressed in terms of another.
 The value of the referenced property is determined in the context
 of the element for which the property value is being determined.
 For example, the Deadline property has the property term Period as
 its default property expression. If this default value is not
 overwritten by another property association, the value of Deadline
 of a thread subcomponent is determined by evaluating the property
 term in the context of the thread subcomponent, i.e., the Deadline
 value is determined by the Period value for the thread subcomponent
 rather than the context of the default value declaration. The value
 of the referenced property may be undefined, in which case the
 property term evaluates to undefined.

(6)  *Property constant terms* evaluate to the value of the referenced
 property constant. This allows one property value to be expressed
 symbolically in terms of a constant identifier rather than the
 actual value.

(7)  *Component classifier terms* are of the property type
 **classifier**. They evaluate to a classifier reference.

(8)  *Reference terms* are of **reference** property type and evaluate
 to a reference. This reference may be a reference to a model
 element in the model containment hierarchy, e.g., to a contained
 component, to a connection, mode, feature, or an annex model
 element.

(9)  *Record terms* evaluate to a record value, which consists of a
 separate value for each named field in the record expression. Any
 record field defined in a record type, for which there is no value
 in the record expression, the field value is determined by the
 default value; otherwise it is considered not present.

(10) *Computed terms* allow a user-defined function to be called to
 calculate a property value. This function is called every time the
 value of the property is accessed. This function gets the model
 element as input parameter. This function may access other
 properties of the same and other model elements to calculate its
 value and it is assumed to complete its computation in finite time.
 A typical use of this function is to calculate the value of a
 property based on the value of a property of its subcomponents. It
 takes the property association and the model element it is
 associated with as parameters. The function is expected to be
 without side effects and to return a value that is consistent with
 the type of the property. The function being called is assumed to
 exist in a library that is supplied to methods of processing.

NOTE: Expressions of the property type **reference** or **classifier**
are provided to support the ability to refer to classifiers and model
elements in the model. For example, they are used to specify bindings of
application components to execution platform components.

Processing Requirements and Permissions

 A method of processing specifications may define additional rules to
determine if an expression value is legal for a property, beyond the
restrictions imposed by the declared property type. The declared
property type represents a minimum set of restrictions that must be
enforced for every use of a property.

If an associated expression or default value is not specified for a
property, a method of processing specifications is permitted to
reject that specification as erroneous. A method of processing
specifications is permitted to construct a default expression,
providing that default is made known to the developers. This
decision may be made on a per property basis. If a property value is
not required for a specific development activity, then the method of
processing associated with this activity must accept a specification
in which that property has no associated value.

 A method of processing specifications may impose additional
restrictions on the use of property expressions whose value depends
on the current mode of operation, or on bindings. For example,
mode-dependent values may be allowed for some properties but
disallowed for others. Mode-dependent property expressions may be
disallowed entirely.

 A method of processing specifications may access and change field
values of a record property value programmatically.

(5) A method of processing specifications may impose restrictions on the
use of computed values in order to allow the computed value function
to compute the value once and store the result as a cached value.
For example, it may assume that the values of properties used in the
computation have been declared through property associations. In
that case the computed property value is not changed
programmatically by a method of processing and can be determined at
model instantiation time.

Examples

-- This is an AADL fragment inside a package

**thread** Producer

**end** Producer;

**thread** **implementation** Producer.Basic

**properties**

Compute\_Execution\_Time => 0ms..10ms **in binding** (
powerpc.speed\_350Mhz );

Compute\_Execution\_Time => 0ms..8ms **in binding** (
powerpc.speed\_450MHz );

**end** Producer.Basic ;

**process** Collect\_Samples

**end** Collect\_Samples;

**system** Software

**end** Software;

**
system implementation** Software.Basic

**subcomponents**

Sampler\_A : **process** Collect\_Samples;

Sampler\_B : **process** Collect\_Samples

{

-- A property with a list of values

Source\_Text => ( collect\_samples.ads, collect\_samples.adb ) ;

Period => 50 ms;

} ;

**end** Software.Basic;

**device** car

**properties**

mine::Car\_Length => 3.25 meter;

mine::Position => ( x => 3; y => 4;);

mine::Car\_Name => ( US => Rabbit; Germany => Golf; );

**end** car;

**system** Hardware

**end** Hardware;

**system implementation** Hardware.Basic

**subcomponents**

Host\_A: **processor**;

Host\_B: **processor**;

**end** Hardware.Basic ;

**system** Total\_System

**end** Total\_System;

**system implementation** Total\_System.SW\_HW

**subcomponents**

SW : **system** Software.Basic;

HW : **system** Hardware.Basic;

**properties**

-- examples of contained property associations

-- in a subcomponent of SW we are setting the binding to a

-- component contained in HW

Allowed\_Processor\_Binding => ( **reference** ( HW.Host\_A ) )

**applies to** SW.Sampler\_A;

Allowed\_Processor\_Binding => ( **reference** ( HW.Host\_B ) )

**applies to** SW.Sampler\_B;

**end** Total\_System.SW\_HW;
