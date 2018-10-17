Component Classifiers
=====================

Component classifiers are used to characterize components in an embedded software system architecture. AADL supports three types of component classifiers:
 
1. *Component interface*: a component specification that represents the external interface to a component. It consists of *features* that represent interaction points with other components, 
*flows* from incoming features to outgoing features, externally visible modes, and property values associated with the component, its features, and flows.
 
2. *Component implementation*:  a component blue print that represents how a component is realized in terms of interconnected subcomponents. Subcomponents represent component instances that are characterized by a component classifier reference. Connections specify flow between features of different subcomponents or a subcomponent feature and its own feature.
Flow implementations elaborate on a flow from an incoming to an outgoing feature by identifying connections and subcomponents involved in the flow. A component implementation may also specify a binding from an software application component to a platform component.
Bindings specify the binding of a software subcomponent to a platform component. A component implementation and its elements can have property values. 
 
3. *Component configuration*: a specific set of component implementations or configurations chosen for subcomponents including nested subcomponents as well as properties and bindings relevant to the configuration. 
Once configured a subcomponent architecture cannot be changed, only annotated with additional property values. Configurations can be parameterized in that users of such configurations can only provide actuals for the specified parameters.
 
A component has a *component category*. It can be defined as a generic component (using the keyword **component**), or it can represent a specific software or hardware entity through one of the following component *categories*: 
*thread*, *thread group*, *process*, *data,*
*subprogram*, *subprogram group*, 
*memory*, *bus*, *virtual bus*, *processor*, *virtual processor*,
*device*, and *system*. The specific meaning of each of the component categories are described in later sections. 

A component can have multiple implementations. They represent variants of a component that adhere to the
same interface, but may have different properties.  

Component interfaces, implementations, and configurations can be declared in terms of other component types, implementations, and configurations respectively, i.e., they can *extend* an existing classifier by adding or refining elements of the classifier. 
This permits partially complete component classifier declarations to act as templates. Such templates can
represent a common basis for the evolution of a family of related
component classifiers.

Component interfaces and configurations can also be specified as *composition* of multiple existing component types or configurations respectively.

 This standard defines basic concepts and requirements for determining compliance between a component specification via classifier and an
 actual component. This standard does not restrict the lower-level representation(s) used for software components, e.g., binary images,
 conventional programming languages, application modeling languages, nor does it restrict the lower-level representation(s) used for
 physical hardware component designs, e.g., circuit diagrams,hardware behavioral descriptions.

Component Interfaces
---------------

A component interface specifies the external interface of a component
that its implementations satisfy. It consists of features, flows, modes, and associated property values.

Features of a component represent interaction points with other components. Features are incoming and outgoing ports, required and provided access to data components, required and provided remote subprogram call, required and provided bus access, binding points between application and platform components, and named interfaces representing collections of features defined in a separate component type.

Flow specifications represent information flow from its incoming features to its outgoing features. The realization of each flow is specified in detail in each component implementation.

Component interfaces may declare modes and mode transitions. This allows users to characterize modal components and externally triggering mode transitions. 
Mode-specific property values can be associated with the component interface, its
features, and its flows.  

Component interfaces may include annex subclauses that provide annotations expressed in specific annex sublanguages, e.g., fault behavior expressed in the Error Model V2 (EMV2) annex notation.

*Syntax*

component\_interface ::=

component\_category **interface** *defining\_component\_interface\_*identifier **is**

{ feature \| flow\_spec \| mode_declaration \| annex\_subclause \| property\_association }\ :sup:`\*`

**end** **;**


component\_category ::=

generic\_component\_category

\| software\_category

\| execution\_platform\_category

\| composite\_category

abstract\_component\_category ::= 
**component**

software\_category ::= 
**data** \| **subprogram** \| **subprogram group** \|
**thread** \| **thread group** \| **process **

execution\_platform\_category ::=
**memory** \| **processor** \| **bus** \| **device** \| **virtual processor** \| **virtual bus** \| **virtual memory** \| **virtual device**

composite\_category ::= 
**system**


*Naming Rules*

1. The defining identifier for a component interface must be unique in the
name space of the package within which it is declared.

2. Each component type has a local name space. Defining identifiers of
features, modes, mode transitions, and flow
specifications must be unique in the component interface namespace.


*Semantics*

A component interface represents the interface specification of a
 component, i.e., the component category, prototypes, features, flow
 specifications, modes, mode transitions, and property values of a
 component. The component interface provides a contract for the component
 that users of the component can depend on.

The component categories are: data, subprogram, subprogram group,
 thread, thread group, and process (software categories); processor,
 virtual processor, bus, virtual bus, memory, and device (execution
 platform categories); system (compositional category), and abstract
 component (compositional category). The semantics of each category
 will be described in sections 5, 6, and 7.

Features of a component are interaction points with other
 components, i.e., ports and feature groups; subprogram parameters;
 data component access, subprogram access, and bus access. Ports
 represent directional flow of data and events between components,
 feature groups represent groups of features that are connected to
 another component, data component access represents access to
 shared data components, subprogram access represents access to a
 subprogram by a caller, and bus access represents access to a bus
 from processor, memory, device, and other bus components to
 establish hardware connectivity. Features are further described in
 Section 8.

Flow specifications indicate whether a flow of data or control
 originates within a component (*flow source*), terminates within a component (*flow sink*), or
 flows through a component from one of its incoming features to one of
 its outgoing features (*flow path*).

Mode declarations define modes of the component that are common to
 all implementations. Component interfaces can have
 mode-specific property values. Modes can be inherited from an enclosing component as expressed by a *requires\_mode* declaration. Other components can initiate mode
 transitions by supplying events to incoming event ports of a
 component that are identified as triggers in mode transitions. 
 
Property value can be associated with the component or with any of the content of the component type, i.e., with features, flows, modes.

*Examples*

**package** TypeExample

**public**

**system** **interface** File\_System

-- access to a data component

root: **requires data access** FileSystem::Directory.hashed;

**end** ;

**process** **interface** Application

-- a data out port

result: **out data port** App::result\_type;

home: **requires** **data access** FileSystem::Directory.hashed;

**end** ;

**thread** **interface** Calculate

-- a data out port without a specified type

input: **in data port** ;

result: **out data port** ;

**end** ;

**end** ;




Component Implementations 
--------------------------

 A *component implementation* represents the realization of a
component in terms of subcomponents, their connections, flow
sequences, properties, component modes and mode transitions, and other behavior specified in annexes. Flow
sequences represent implementations of flow specifications in the
component interface, or end-to-end flows with starting and end points within the component implementation. Modes represent
alternative operational modes that may manifest themselves as
alternate configurations of subcomponents, connections, call
sequences, flow sequences, and property values.

A component interface can have zero, one, or multiple component
implementations. If a component interface has zero component
implementations, then it is considered to be a leaf in the system
component hierarchy. For example, a partial AADL model may have
processes as components without realization, while a task level AADL
model expand to threads as leaves. If no implementation is
associated then the properties on the component interface provides
information about the component for analysis and system generation.

*Syntax*

component\_implementation ::=

component\_category *defining\_*component\_implementation\_name 

{ subcomponent \| internal\_feature \| processor\_feature \| subprogram\_call\_sequence \| connection \|
flow\_implemention \| end\_to\_end\_flow \| mode \| mode\|transition \| annex\_subclause \| property\_association }\ :sup:`\*`

**end** component\_implementation\_name **;**

// with section labels
// [ subcomponent\_subclause ]
// 
// [ internal\_features\_subclause ]
// 
// [ processor\_features\_subclause ]
// 
// [ subprogram\_call\_sequences\_subclause ]
// 
// [ connections\_subclause ]
// 
// [ flows\_subclause ]
// 
// [ modes\_subclause ]
// 
// [ properties\_subclause ]
// 
// { annex\_subclause }\ :sup:`\*`


component\_implementation\_name ::=

*component\_interface*\_identifier **.** *component\_implementation*\_identifier



// subcomponents\_subclause ::=
// 
// **subcomponents** { subcomponent }\ :sup:`+`
// 
// internal\_Features\_subclause ::=
// 
// **internal features** { internal\_feature }\ :sup:`+`
// 
// processor\_features\_subclause ::=
// 
// **processor features** { processor\_feature }\ :sup:`+`
// 
// subprogram\_call\_sequences\_subclause ::=
// 
// **calls** { subprogram\_call\_sequence }\ :sup:`+`
// 
// connections\_subclause ::=
// 
// **connections** { connection }\ :sup:`+` 
// 
// flows_subclause ::=
// 
// **flows** { flow\_implementation \| end\_to\_end\_flow }\ :sup:`+`



unique\_component\_implementation\_reference ::=

[ package\_name **::** ] component\_implementation\_name


*Naming Rules*

1. A component implementation name consists of a component interface
   identifier and a component implementation identifier separated by a
   dot (**.**). The first identifier of the defining component
   implementation name must name a component interface in the same package as the implementation or must be contained in a package listed in a **with** declaration.
   The defining identifier of the component implementation must be
   unique within the local namespace of the component interface.

2. Every component implementation defines a *local namespace* for all
   defining identifiers of its content. The defining identifier of its content must be unique within this
   namespace. For example, a subcomponent and a mode cannot have the
   same defining identifier within the same component implementation. 

3. This local namespace inherits the namespace of the associated
   component interface. Defining identifiers of implementation content must not conflict with defining identifiers of the respective component interface content. For example, a feature in the component interface and a subcomponent in the component implementation cannot have hte same name.


*Legality Rules*

1. The component implementation name following the
   reserved word **end** must be identical to the defining component implementation name.

2.  The category of the component implementation must be identical to
the category of the component interface for which the component
implementation is declared, or the category of the component interface must be *abstract* or not specified.

3.  If the component interface of the component implementation contains a
requires\_modes\_subclause then the component implementation
must not contain any mode or mode transition declarations.

4.  If modes are declared in the component interface, then modes cannot be
declared in component implementations.

5.  If modes or mode transitions are declared in the component interface,
then mode transitions can be added in the component
implementation. These mode transitions may be triggered by ports of the component interface or by ports of subcomponents.


*Standard Properties*

Classifier Matching Rule: **inherit** **enumeration**
(Type\_Match, Equivalence, Subset, Conversion, Complement)

This property is not a property on the component, but an annotation about type matching of features in connections.

*Semantics*

 A component implementation represents the internal structure of a
component represented by subcomponents. Interaction between
subcomponents is expressed by the connections, flows, and subprogram
call sequences. Mode declarations represent alternative runtime
configurations, i.e., subcomponent and connections can be active only in specific modes.
A component implementation and its content has property values to express its
non-functional attributes such as safety level or execution time. Property values can be different for different modes.

A component implementation is defined in the context of a component interface.
All external interactions occur through the features of the interface, i.e., the interface enforces connectivity to external component.
Some connection declarations map a subcomponent feature to a feature of the interface. That feature may be part of a connection declaration in an implementation where one of its subcomponents refers to the given component interface or implementation.
and provides a realization of its features (interface). 

A component
interface can have multiple implementations. A component implementation
can be viewed as a component variant 
with differing property values that characterize the differences
between implementations. 

 The component hierarchy of an actual system is modeled by component implementations with subcomponents, whose component classifier identifies another component implementation with subcomponents. 
 Those subcomponents may recursively identify component implementations with subcomponent.

*Processing Requirements and Permissions*

 A component implementation denotes a set of actual system
components, existing or potential, that are compliant with the
component implementation declaration as well as the associated
component interface. That is, the actual components denoted by a
component implementation declaration are always compliant with the
functional interface specified by the associated component interface
declaration. Actual components denoted by different implementations
for the same component interface differ in additional details such as
internal structure or behaviors; these differences may be specified
using properties or annex subclauses.

In general, two actual components that comply with the same
component interface and component implementation are not necessarily
substitutable for each other in an actual system. This is because an
AADL specification may be legal but not specify all of the
characteristics that are required to ensure total correctness of a
final assembled system. For example, two different versions of a
piece of source text might both comply with the same AADL
specification, yet one of them may contain a programming defect that
results in unacceptable runtime behavior. Compliance with this
standard alone is not sufficient to guarantee overall correctness of
a actual system.

*Examples*

**package** ImplementationExample

**type** Bool\_Type;

**thread** **interface** DriverModeLogic

BreakPedalPressed : **in data port** Bool\_Type;

ClutchPedalPressed : **in data port** Bool\_Type;

Activate : **in data port** Bool\_Type;

Cancel : **in data port** Bool\_Type;

OnNotOff : **in data port** Bool\_Type;

CruiseActive : **out data port** Bool\_Type;

**end** DriverModeLogic\ **;**

-- Two implementations whose source texts use different variable names

-- for their cruise active port

**thread** DriverModeLogic.Simulink

#Dispatch\_Protocol=>Periodic;

#Period=> 10 ms;

CruiseActive#Source\_Name => CruiseControlActive;

**end** DriverModeLogic.Simulink\ **;**

**thread implementation** DriverModeLogic.C

**properties**

#Dispatch\_Protocol=>Periodic;

#Period=> 10 ms;

#CruiseActive#Source\_Name => CCActive;

**end** DriverModeLogic.C\ **;**

**end** ImplementationExample;



Component Classifier Extension and Composition
----------------------------------------

 Component interface, implementations and configurations can be declared in terms of other component classifiers. 
A component interface can *extend* another component interface 
inheriting its declared content and property associations. 
A component implementation can *extend* another component implementation, inheriting its content and property associations.
A component configuration can *extend* an other component configuration or component implementation inheriting its content and property associations.
The component classifier extending another component classifier can add additional content, as well as add and override property values.

 A component interface can be composition of multiple component interfaces. This is expressed by listing multiple component interface references after the **extends** keyword.
In this case the content of all component interfaces listed after the **extends** is inherited. The inherited content must be unique. For example, there cannot be two features inherited from different component interfaces that have the same name.
If a property is inherited from two component interfaces then its value must be the same.

 A component configuration can be a composition of multiple component configurations. This is expressed by listing multiple component interface or configuration references after the **extends** keyword.
In this case the content of all component interface or configurations listed after the **extends** is inherited. The inherited content must be unique. For example, there cannot be two features inherited from different component types that have the same name.
If a property is inherited from two component types then its value must be the same.

 Component type extensions form an *extension hierarchy*, i.e., a
component type that extends another component type can also be
extended. Component types being extended are referred to as *ancestors*, while component
types extending a component type are referred to as *descendants*.

*Syntax*

component\_type\_extension ::=

component\_category *defining\_component\_type\_*identifier

**extends** unique\_component\_type\_reference { **,** unique\_component\_type\_reference }\ :sup:`*`

`[` **features** ( { feature \| feature\_refinement }\ :sup:`+` \|
none\_statement ) `]`

[ flows\_subclause ]

[ modes\_subclause ]

[ properties\_subclause ]


unique\_component\_type\_reference ::=

[ package\_name **::** ] *component\_type\_*\identifier

*Naming Rules*

1. The component type identifier of the ancestor in a component type
   extension, i.e., that appears after the reserved word **extends**,
   must exist in the same package as the descendant, or in a package listed in a *with* declaration.

2. When a component type extends another component type, its namespace includes all the identifiers in the namespaces of its
   ancestors. This means that all inherited content as well as any locally declared content must have a unique name. 


*Legality Rules*


1. The category of the component type being extended must match the
   category of the extending component type, i.e., they must be
   identical or the category being extended must be **abstract**.

2. If the extended component type and an ancestor component type in the
   extends hierarchy contain modes subclauses, they must both have only mode declarations or requires\_mode declarations.
   

*Standard Properties*

Classifier Substitution Rule: **inherit** **enumeration** (Classifier
Match, Type Extension, Signature\_Match)

This property is not a property on the component, but an annotation about replacement of classifiers in feature refinement.

*Semantics*

 A component type can be declared as an extension of other
 component types resulting in a component type extension hierarchy. 
 A component type extension inherits the content
 of the component type(s) being extended.  A component
 type extension can refine inherited features and it can add additional content.  Similarly, it can add new property values or override property values.
 A component type extension can change an *abstract* category into any of the concrete component categories.

 Component type extension allows users to represent families of components with partially defined interfaces getting refined and extended.
 Component type composition allows users to define libraries of component interfaces that can be combined to represent the interface of a component. 
 For example, we may have a component type representing the logical application interface and a second component type represents the hardware platform interface. 
 This supports
 evolutionary development and modeling of system families by
 declaring partially complete component types that get refined in
 extensions.

 Each annex defines whether annex declarations in annex subclauses are inherited by the descendant.


Component Implementation Extensions
-----------------------------------

 A component implementation can be declared as an extension of
another component implementation. In that case, the component
implementation inherits the declarations of its ancestors as well as
its component type. A component implementation extension can refine
inherited declarations, and add subcomponents, connections,
subprogram call sequences, flow sequences, mode declarations, and
property associations.

 Component implementations build on the component type *extension
hierarchy* in two ways. First, a component implementation is a
realization of a component type (shown as dashed arrows in Figure
3). As such it inherits features and property associations of its
component type and any component type ancestor. Second, a component
implementation declared as extension inherits subcomponents,
connections, subprogram call sequences, flow sequences, modes,
property associations, and annex subclauses from the component
implementation being extended (shown as solid arrows in Figure 3). A
component implementation can extend a component implementation that
in turn extends another component implementation, e.g., in Figure 3
*GPS*. Handheld extends *GPS.Basic* and is extended by
*GPS\_Secure.Handheld*. Component implementations higher in the
extension hierarchy are called ancestors and those lower in the
hierarchy are called descendants. A component implementation can
extend another component implementation of its own component type,
e.g., *GPS.Handheld* extends *GPS.Basic*, or it can extend the
component implementation of one of its ancestor component types,
e.g., *GPS\_Secure.Handheld* extends *GPS.Handheld*, which is an
implementation of the ancestor component type *GPS*. The component
type and implementation extension hierarchy is illustrated in Figure
3.
 
*Syntax*

component\_category *defining\_*component\_implementation\_name

**extends** unique\_component\_implementation\_reference 


[ **subcomponents **

( { subcomponent \| subcomponent\_refinement }\ :sup:`+` \|
none\_statement ) ]

[ **internal features** { internal\_feature }\ :sup:`+` ]

[ **processor features** { processor\_feature }\ :sup:`+` ]

[ **calls** ( { subprogram\_call\_sequence }\ :sup:`+` \|
none\_statement ) ]

[ **connections**

( { connection \| connection\_refinement }\ :sup:`+` \| none\_statement
) ]

[ **flows** ( { flow\_implementation \|

end\_to\_end\_flow \| end\_to\_end\_flow \_refinement }\ :sup:`+`

\| none\_statement ) ]

[modes\_subclause ]

[ **properties** ( { property\_association \|
contained\_property\_association }\ :sup:`+`

\| none\_statement ) ]

{ annex\_subclause }\ :sup:`\*`

**end**  ;



4. Refinement identifiers of prototype, subcomponent, and connection
   refinements must exist in the local namespace of an ancestor
   component implementation.

5. In a component implementation extension, the component type
   identifier of the component implementation being extended, which
   appears after the reserved word **extends**, must be the same as or
   an ancestor of the component type of the extension. The component
   implementation being extended may exist in another package. In this
   case the component implementation name is qualified with the package
   name.

6. When a component implementation **extends** another component
   implementation, the local namespace of the extension is a superset of
   the local namespace of the ancestor. That is, the local namespace of
   a component implementation inherits all the identifiers in the local
   namespaces of its ancestors (including the identifiers of their
   respective component type namespaces).


Legal


3.  If the component implementation extends another component
implementation, the category of both must match, i.e., they must
be identical or the category being extended must be
**abstract**.


*Standard Properties*

Classifier Substitution Rule: **inherit** **enumeration** (Classifier
Match, Type Extension, Signature Match)

This property is not a property on the component, but an annotation about replacement of classifiers in feature refinement.

semantics




Subcomponents
-------------

 A *subcomponent* represents a component contained within another
component, i.e., declared within a component implementation.
Subcomponents contained in a component implementation may be
instantiations of component implementations that contain
subcomponents themselves. This results in a component containment
hierarchy that ultimately describes the whole actual system as a
system instance. Figure 4 provides an illustration of a containment
hierarchy using the graphical AADL notation (see Appendix D). In
this example, Sys1 represents a system. The implementation of the
system contains subcomponents named C3 and C4. Component C3, a
subcomponent in Sys1’s implementation, contains subcomponents named
C1 and C2. Component C4, another subcomponent in Sys1’s
implementation, contains a second set of subcomponents named C1 and
C2. The two subcomponents named C1 and those named C2 do not violate
the unique name requirement. They are unique with respect to the
local namespace of their containing component’s local namespace.

Figure − Component Containment Hierarchy


 A subcomponent declaration may resolve required subcomponent access
declared in the component type of the subcomponent. For details on
required subcomponent access see Section 8.4.

A subcomponent can be declared to apply to specific modes (rather
than all modes) defined within the component implementation.

 Subcomponents can be refined as part of component implementation
extensions. Refinement allows classifier references to be completed,
abstract subcomponents to be refined into one of the concrete
categories, and subcomponent property values to be associated. The
resulting refined subcomponents can be refined themselves.

 An array of subcomponents can be declared to represent a set of
subcomponents with the same component type. This array may have one
or more dimensions.

Syntax

subcomponent ::=

*defining\_subcomponent*\ \_identifier :

component\_category

[ unique\_component\_classifier\_reference [prototype\_bindings]

\| *prototype*\ \_identifier ]

[ array\_dimensions [ array\_element\_implementation\_list ] ]

[ **{** { *subcomponent*\ \_property\_association

\| contained\_property\_association }\ :sup:`+` **}** ]

[ component\_in\_modes ] **;**

subcomponent\_refinement ::=

*defining\_subcomponent*\ \_identifier : **refined to**

component\_category

[ unique\_component\_classifier\_reference [ prototype\_bindings ]

\| *prototype*\ \_identifier ]

[ array\_dimensions [ array\_element\_implementation\_list ] ]

[ **{** { *subcomponent*\ \_property\_association

\| contained\_property\_association }\ :sup:`+` **}** ]

[ component\_in\_modes ] **;**

unique\_component\_classifier\_reference ::=

( unique\_component\_type\_reference

\| unique\_component\_implementation\_reference )

array\_dimensions ::=

{ array\_dimension  }\ :sup:`+`

array\_dimension ::=

**[** [ array\_dimension\_size ] **]**

array\_dimension\_size ::=

numeral \| unique\_property\_constant\_identifier

array\_element\_implementation\_list ::=

**(** unique\_component\_implementation\_reference [ prototype\_bindings
]

{ **,** unique\_component\_implementation\_reference [
prototype\_bindings ] }\ :sup:`\*` **)**

-- array selection used in contained property association and references

array\_selection\_identifier ::=

identifier array\_selection

array\_selection ::=

{ **[** selection\_range **]** }\ :sup:`+`

selection\_range ::=

numeral [ **..** numeral ]

NOTE: The above grammar rules characterize the common syntax for
subcomponent of all component categories. The sections defining each of
the component categories will specify further restrictions on the
syntax.

Naming Rules

1. The defining identifier of a subcomponent declaration placed in a
   component implementation must be unique within the local namespace of
   the component implementation that contains the subcomponent.

1. The defining identifier of a subcomponent refinement must exist as a
   defining subcomponent identifier in the local namespace of an
   ancestor component implementation.

2. The component type identifier or the component implementation name of
   a component classifier reference must exist in the package namespace.

3. The prototype identifier of a prototype reference must exist in the
   local name space of the component implementation.

4. The prototype referenced by the prototype binding declarations must
   exist in the local namespace of the component classifier being
   referenced.

5. The modes named in the **in modes** statement of a subcomponent must
   refer to modes in the component implementation that contains the
   subcomponent or its component type. The modes named in the **in
   modes** statement of a property association of a subcomponent must
   refer to modes of the subcomponent, or in the case of a contained
   property association to modes of the last component in the component
   path (see Section 11.3).

Legality Rules

1. The category of the subcomponent declaration must match the category
   of its corresponding component classifier reference or its
   prototype reference, i.e., they must be identical, or in the case
   of a classifier reference the referenced classifier category may
   be *abstract*.

2. The component classifier reference of a subcomponent declaration may
   include prototype bindings for a subset or all of the component
   classifier prototypes. This represents an unnamed component
   classifier extension of the referenced classifier.

1. In a subcomponent refinement declaration the component category may
   be refined from **abstract** to one of the concrete component
   categories. Otherwise the category must be the same as that of
   the subcomponent being refined.

2. The Classifier\_Substitution\_Rule property specifies the rule to be
   applied when a refinement supplies a classifier and the original
   subcomponent declaration already has a component classifier. This
   property can be applied to individual subcomponents or features,
   or it can be inherited from classifiers. The following rules are
   supported:

-  Classifier\_Match: The component type of the refinement must be
   identical to the component type of the classifier being refined. If
   the original declaration specifies a component implementation, then
   any implementation of that type can replace this original
   implementation. This is the default rule.

-  Type\_Extension: Any component classifier whose component type is an
   extension of the component type of the classifier in the subcomponent
   being refined is an acceptable substitute.

-  Signature\_Match: The component type of the refinement must match the
   signature of the component type of the classifier being refined.

1. In the case of a signature match, the component type of the
   subcomponent being refined must have a subset of the features of
   the component type in the refinement. The features are compared
   by name matching; the feature categories and direction (in data
   port, provides data access, etc.) must be the same and any
   feature classifier must match according to rules defined for
   Classifier\_Match. In addition, if flow specifications are
   present in the component type being refined, then the component
   type of the refinement must have at least the same set of flow
   specifications. Flow specifications with the same name must have
   the same source and destination ports.

2. The component category and optional component classifier or prototype
   reference can be followed by a set of array dimensions to define
   the subcomponent as an array of actual subcomponents.

3. The array size specification for the dimensions is optional. In this
   case the array declaration is considered incomplete. If the size
   of the array dimension is specified it must be specified for all
   dimensions in the same declaration.

4. When refining a subcomponent array the number of dimensions of the
   array cannot be changed, but the array size can be specified for
   each dimension if it was not specified in the subcomponent
   declaration being refined.

5. When the subcomponent is declared as an array with array dimension
   sizes then a list of component implementations can be supplied,
   one for each element of the array. Different implementations of
   the same component type can be chosen. The number of elements in
   the list must correspond to the number of elements in the
   component array. In the case of multi-dimensional arrays, the
   list elements are assigned by incrementing the index of the last
   dimension first.

6. Selecting index ranges in one or more dimensions of an array is only
   possible if the size of the array for these dimensions is already
   defined. The index range of a dimension is from 1 to the size of
   the dimension. Specification of array index ranges is limited to
   the **applies to** subclause of contained property associations.
   Specification of a single array element is limited to the
   **applies to** subclause of contained property associations and
   to the values of **reference** properties.

7. An array element implementation list is valid only if (a) the
   subcomponent classifier is a component type and (b) all component
   implementations in the list are implementations of the specified
   type.

Consistency Rules

1. The classifier of a subcomponent cannot recursively contain
   subcomponents with the same component classifier. In other words,
   there cannot be a cyclic containment dependency between components.

Standard Properties

Classifier Substitution Rule: **inherit** **enumeration** (Classifier
Match, Type Extension, Signature Match)

Acceptable Array Size: **list of** Size Range

Semantics

  Subcomponents declared in a component implementation are considered
 to be contained in the component implementation. Contained
 subcomponents are instantiated when the containing component
 implementation is instantiated. Thus, the component containment
 hierarchy describes the hierarchical structure of the actual
 system.

 A component implementation can contain *incomplete* subcomponent
 declarations, i.e., subcomponent declarations with no component
 classifier references, or if the component classifier reference
 only consists of a component type name for a component type with
 more than one component implementation. A subcomponent declaration
 is also incomplete when it consists of the declaration of an array
 of subcomponents for which the array sizes are not specified. This
 is particularly useful during early design stages where details may
 not be known or decided. Such incomplete subcomponent declarations
 can be refined in component implementation extensions.

  A subcomponent declaration can be parameterized by referring to a
 prototype. In this case the component category and component
 classifier bound to the prototype is used when the system is
 instantiated.

  A subcomponent declaration can reference a component classifier
 with prototype bindings. The prototype binding can refer to other
 classifiers or to a prototype of the component type or
 implementation that contains the subcomponent. In the latter case,
 the prototype actual is passed down levels of the component
 hierarchy and effectively allows the system subcomponents to be
 configured from a higher level component.

(5)  A component classifier reference with prototype bindings that refer
 to component classifiers effectively is an unnamed extension of the
 classifier being referenced. In other words, it could have been
 declared as a component type or component implementation extension
 with a new defining identifier and this identifier could have been
 referenced in the subcomponent declaration. Two unnamed component
 classifier extensions are not considered to be extensions of each
 other.

(6)  The optional component\_in\_modes subclause specifies the modes in
 which the subcomponent is active. An component\_in\_modes in a
 subcomponent refinement replaces previously specified subsets of
 modes. A subcomponent or subcomponent refinement without
 component\_in\_modes specifies that the subcomponent is active in
 all modes. The component\_in\_modes refer to modes of the component
 implementation that contains the subcomponent or to the modes of
 its component type. The component\_in\_modes may map mode
 identifiers of the containing component to the mode identifiers
 specified in the **requires modes** clause of the subcomponent’s
 component type (see Section 12).

(7)  A subcomponent can have property values associated to itself, or a
 contained property association can be declared for one of the
 subcomponents in its containment hierarchy, as well as those
 subcomponents’ features, modes, subprogram call sequences,
 connections, and flows, or model elements in any annex subclause of
 a subcomponent (see Section 11.3). Subcomponent refinements may
 declare property associations – that override the property values
 declared in the subcomponent being refined. Property associations
 can have in\_modes statements that refer to modes of the component
 implementation that contains the subcomponent, or in the case of
 contained property associations also to modes of the last
 subcomponent named in the path of the **applies to** (see Section
 11.3).

(8)  The arrays of subcomponents are used to simplify the declaration of
 a multiplicity of subcomponents with the same classifier without
 declaring each of them separately. If a size of a subcomponent
 array is not known the array is incomplete and is assumed to have
 one element for the purpose of system instances of incomplete
 models. A subcomponent array can only be refined by adding array
 sizes to the dimensions if they are without a size.

(9)  All elements of a subcomponent array have the same component
 classifier, i.e., they are of the same kind. A subcomponent array
 can also be declared to have the same component type, but its
 elements vary in their implementation, e.g., to represent variants
 in an N-Version redundancy pattern.

(10) A property association declared with a subcomponent array applies
 to each element in the array. Contained property associations
 declared in the enclosing component implementation can be used to
 associate different property values to different elements or
 subsets of the subcomponent array.

Processing Requirements and Permissions

 If the subcomponent declaration references a component type and the
type has a single implementation then a method of processing (tool)
is permitted to generate a complete system instance by choosing the
single implementation even if it is not named. If the referenced
component type has multiple implementations then the implementation
must be explicitly referenced. However, some project may impose
design constraints that require modelers to completely specify such
classifier references.

Examples

 The example illustrates modeling of source text data types as data
component types without any implementation details. It illustrates
the use of **package** to group data component type declarations. It
illustrates both component classifier references to component types
and to component implementations. It illustrates the use of ports as
well as required and provided data access, and required subprogram
access. In that context it illustrates the ways of resolving
required access. The Data Modeling Annex (Annex Document B) provides
guidance on how to effectively represent data models of applications
in AADL.

**package** Sampling

**public**

**data** Sample

**properties**

Data\_Size => 16 Bytes;

**end** Sample;

**data** Sample\_Set

**properties**

Data\_Size => 1 MByte;

**end** Sample\_Set;

**data implementation** Sample\_Set.impl

**subcomponents**

Data\_Set: **data** Sample ;

**end** Sample\_Set.impl;

**data** Dynamic\_Sample\_Set **extends** Sample\_Set

**end** Dynamic\_Sample\_Set;

**data implementation** Dynamic\_Sample\_Set.impl **extends**
Sample\_Set.impl

**properties**

Data\_Size => 8 Bytes **applies to** Data\_Set;

end Dynamic\_Sample\_Set.impl;

**end** Sampling;

**package** SamplingTasks

**public**

**with** Sampling;

**thread** Init\_Samples

**features**

OrigSet : **requires data access** Sampling::Sample\_Set;

SampleSet : **requires data access** Sampling::Sample\_Set;

**end** Init\_Samples;

**thread** Collect\_Samples

**features**

Input\_Sample : **in event data port** Sampling::Sample;

SampleSet : **requires data access** Sampling::Sample\_Set;

Filtering\_Routine: **requires subprogram access** Sample\_Subprogram;

**end** Collect\_Samples;

**thread implementation** Collect\_Samples.Batch\_Update

**properties**

Source\_Name => ″InSample″ **applies to** Input\_Sample;

**end** Collect\_Samples.Batch\_Update;

**thread** Distribute\_Samples

**features**

SampleSet : **requires data access** Sampling::Sample\_Set;

UpdatedSamples : **out event data port** Sampling::Sample;

**end** Distribute\_Samples;

**process** Sample\_Manager

**features**

Input\_Sample: **in event data port** Sampling::Sample;

External\_Samples: **requires data access** Sampling::Sample\_Set;

Result\_Sample: **out event data port** Sampling::Sample;

**end** Sample\_Manager;

**process implementation** Sample\_Manager.Slow\_Update

**subcomponents**

Samples: **data** Sampling::Sample\_Set;

Init\_Samples : **thread** Init\_Samples;

-- the required access is resolved to a subcomponent declaration

Collect\_Samples: **thread** Collect\_Samples.Batch\_Update;

Distribute: **thread** Distribute\_Samples;

Sample\_Filter: **subprogram** Sample\_Subprogram.Simple;

**connections**

ISSSConn: **data** **access** Samples <-> Init\_Samples.SampleSet;

ISOSConn: **data access** External\_Samples <-> Init\_Samples.OrigSet;

CSSSConn: **data access** Samples <-> Collect\_Samples.SampleSet;

CSISConn: **port** Input\_Sample -> Collect\_Samples.Input\_Sample;

DSSConn: **data access** Samples <-> Distribute.SampleSet;

DUSConn: **port** Distribute.UpdatedSamples -> Result\_Sample;

CSFRConn: **subprogram access** Sample\_Filter <->
Collect\_Samples.Filtering\_Routine;

**end** Sample\_Manager.Slow\_Update;

**subprogram** Sample\_Subprogram

**end** Sample\_Subprogram;

**subprogram implementation** Sample\_Subprogram.Simple

**end** Sample\_Subprogram.Simple;

**end** SamplingTasks;

 This example illustrates the use of arrays in defining a triple
redundancy pattern with a voter. The pattern is defined as an
**abstract** component (see Section 4.6) that uses data ports. The
connections are defined with a connection pattern property to
indicate how the elements of the source array are connected to the
destination. Each instance of MyProcess is connected to a separate
port of the Voter. Note that the number of replicates could be kept
flexible by specifying the array dimension size through a property.

**package** Redundancy

**public**

**abstract** Triple

**features **

input: **in** **data port**;

output: **out data port**;

**end** Triple;

**abstract implementation** Triple.impl

**subcomponents **

MyProcess: **abstract** Calculate [3];

MyVoter: **abstract** Voter;

**connections**

extinput: **port** input -> MyProcess.input

{ Connection\_Pattern => (( One\_To\_All )); };

tovoter: **port** MyProcess.output -> MyVoter.input

{ Connection\_Pattern => (( One\_To\_One )); };

extoutput: **port** MyVoter.output -> output;

**end** Triple.impl;

**abstract** Calculate

**features **

input: **in** **data port**;

output: **out data port**;

**end** Calculate;

**abstract** Voter

**features **

input: **in** **data port** [3];

output: **out data port**;

**end** Voter;

**end** Redundancy;

Abstract Components
-------------------

 The component category **abstract** represents an abstract
component. Abstract components can be used to represent component
models. Abstract component can contain any component and can be
contained in any component. The abstract component category can
later be refined into one of the concrete component categories: any
of the software components, hardware components, and composite
components. When an abstract component is refined into a concrete
component category it must adhere to the containment rules imposed
by the concrete category. For example, an abstract subcomponent of a
process can only be refined into a thread or thread group.

Legality Rules

+----------------+---------------------------------------+------------------------+
| **Category**   | **Type**  | **Implementation** |
+----------------+---------------------------------------+------------------------+
| **abstract**   | Features: | Subcomponents: |
||   ||
|| -  port   | -  data|
||   ||
|| -  feature group  | -  subprogram  |
||   ||
|| -  provides data access   | -  subprogram group|
||   ||
|| -  provides subprogram access | -  thread  |
||   ||
|| -  provides subprogram group access   | -  thread group|
||   ||
|| -  provides bus access| -  process |
||   ||
|| -  provides virtual bus access| -  processor   |
||   ||
|| -  requires data access   | -  virtual processor   |
||   ||
|| -  requires subprogram access | -  memory  |
||   ||
|| -  requires subprogram group access   | -  bus |
||   ||
|| -  requires bus access| -  virtual bus |
||   ||
|| -  requires virtal bus access | -  device  |
||   ||
|| -  feature| -  system  |
||   ||
||   | -  abstract|
+----------------+---------------------------------------+------------------------+

1. An **abstract** component type declaration can contain feature
   declarations (including abstract feature declarations), flow
   declarations, as well as property associations.

1. An **abstract** component implementation can contain subcomponent
   declarations of any category. Certain combinations of
   subcomponent categories are only acceptable if they are
   acceptable in one of the concrete component categories.

2. An **abstract** component implementation can contain a modes
   subclause, a connections subclause, a flows subclause, and
   property associations.

3. An **abstract** subcomponent can be contained in the implementation
   of any component category.

4. If an **abstract** subcomponent is refined to a concrete category,
   the concrete category must be acceptable to the component
   implementation category whose subcomponent is being refined.

5. An **abstract** subcomponent can be declared as an array of
   subcomponents.

6. If an **abstract** component type is refined to a concrete category,
   the features, modes, and flow specifications of the abstract
   component type must be acceptable for the concrete component
   type.

7. If an **abstract** component implementation is refined to a concrete
   category, the subcomponents, call sequences, modes, flow
   implementations, and end-to-end flows of the abstract component
   implementation must be acceptable for the concrete component
   implementation.

Standard Properties

 An **abstract** component can have property associations of
properties that apply to any concrete category. However, when
refined to a concrete category, properties that do not apply to the
concrete category will be ignored. A method of processing may
provide a warning about ignored properties.

Semantics

 The component of category **abstract** represents an abstract
component. It can be used to represent conceptual architectures.
This abstract component can be refined into a runtime architecture
by refining the component category into a software, composite, or
hardware component. Such a refinement from a conceptual architecture
to a runtime architecture is illustrated in the example below.

Alternatively, the conceptual architecture can be defined in terms
of abstract components and the runtime architecture can be defined
separately in terms of threads and processes. A user-defined
property of the **reference** property type can be used to specify
the mapping of conceptual components to runtime architecture
components.

Examples

 A conceptual architecture and its refinement into a runtime
architecture.

**package** CarSystem

**public**

**bus** Manifold

**end** Manifold;

**abstract** car

**end** car;

**abstract implementation** car.generic

**subcomponents**

PowerTrain: **abstract** power\_train;

ExhaustSystem: **abstract** exhaust\_system;

**end** car.generic;

**abstract** power\_train

**features**

exhaustoutput: **requires bus access** Manifold;

**end** power\_train;

**abstract** exhaust\_system

**features**

exhaustManifold: **provides bus access** Manifold;

**end** exhaust\_system;

-- runtime architecture

**system** carRT **extends** car

**end** carRT;

**system implementation** carRT.impl

**extends** car.generic

**subcomponents**

PowerTrain : **refined to system** power\_train;

ExhaustSystem : **refined to system** exhaust\_system;

**end** carRT.impl;

**end** CarSystem;

Prototypes
----------

 Prototypes represent parameterization of component type, component
implementation, and feature group type declarations. They allow
classifiers and features to be supplied when a component type,
component implementation, or feature group is being extended or
being used in a subcomponent declaration. The component classifier
prototypes can be referenced in place of classifiers in feature
declarations, in subcomponent declarations. The feature prototypes
can be referenced in abstract feature declarations. Prototypes can
also be referenced as actuals in prototype bindings; this allows
parameterization via prototype to be propagated down the system
hierarchy.

Syntax

prototype ::=

*defining\_prototype*\ \_identifier **:**

( component\_prototype

\| feature\_group\_type\_prototype

\| feature\_prototype )

[ **{** { *prototype*\ \_property\_association }\ :sup:`+` **}** ] **;
**

component\_prototype ::=

component\_category [ unique\_component\_classifier\_reference ] [
**[]** ]

feature\_group\_type\_prototype ::=

**feature group** [ unique\_feature\_group\_type\_reference ]

feature\_prototype ::=

[ **in** \| **out** ] **feature**
[unique\_component\_classifier\_reference ]

prototype\_refinement ::=

*defining\_prototype*\ \_identifier **:** **refined to **

( component\_prototype

\| feature\_group\_type\_prototype

\| feature\_prototype )

[ **{** { *prototype*\ \_property\_association }\ :sup:`+` **}**
]\ **;**

prototype\_bindings ::=

**(** prototype\_binding { **,** prototype\_ binding }\ :sup:`\*` **)**

prototype\_binding ::=

*prototype*\ \_identifier **=> **

( component\_prototype\_actual \| component\_prototype\_actual\_list

\| feature\_group\_type\_prototype\_actual \| feature\_prototype\_actual
)

component\_prototype\_actual ::=

component\_category

( unique\_component\_classifier\_reference [ prototype\_bindings ]

\| *prototype*\ \_identifier )

component\_prototype\_actual\_list ::=

**(** component\_prototype\_actual { **,** component\_prototype\_actual
}\ :sup:`\*` **)**

feature\_group\_type\_prototype\_actual ::=

( **feature group** unique\_feature\_group\_type\_reference [
prototype\_bindings ] )

\| (**feature group** *feature\_group\_type\_prototype*\ \_identifier )

feature\_prototype\_actual ::=

( (( **in** \| **out** \| **in out**) ( **event** \| **data** \| **event
data** ) **port** ) \|

( ( **requires** \| **provides** )

( **bus** \| **virtual bus** \| **data** \| **subprogram group** \|
**subprogram** ) **access** )

**[** unique\_component\_classifier\_reference ] )

\| ( [ **in** \| **out** ] **feature**
*feature\_prototype*\ \_identifier )

Naming Rules

1. The prototype identifier on the left-hand side of a prototype binding
   must exist in the local namespace of the classifier for which the
   prototype binding is defined.

1. The prototype identifier on the right-hand side of a prototype
   binding, if present, must exist in the local namespace of the
   classifier that contains the prototype binding.

2. Unique component classifier references must exist in the public
   section of the package being identified in the reference.

3. Unique feature group type references must exist in the public section
   of the package being identified in the reference.

Legality Rules

1. The component category declared in the component prototype binding
   must match the component category of the prototype being referenced,
   i.e., they must be identical, or the declared category component
   category of the prototype must be **abstract**.

1.  The component category of the optional component classifier
reference in the prototype declaration must match the category
in the prototype declaration.

2.  If the component prototype only specifies a component category, then
any component type and component implementation of that category
is acceptable; in the case of the category **abstract** any
component type and component implementation of any category is
acceptable.

3.  If the component prototype declaration includes a component
classifier reference, then the classifier supplied in the
prototype binding must match according to the
Prototype\_Substitution\_Rule property. This property specifies
the rules to be applied to determine an acceptable classifier
supplied to the prototype. This property can be associated with
a prototype declaration or the enclosing component type or
component implementation. The rules are the same as those of the
Classifier\_Substitution\_Rule property.

4.  The category of the component implementation that contains the
prototype declaration places restrictions on the set of
acceptable categories for the prototype declaration and the
supplied classifiers. The nesting rules for each category are
defined in the respective component category section of this
document. For example, if the component implementation is a
**thread group** implementation, then the prototype referenced
in a subcomponent declaration must be of the category **thread
group**, **thread**, **subprogram, subprogram group**, **data**,
or **abstract**.

5.  If the direction is declared for feature prototypes, then the
prototype actual satisfies the direction according to the same
rules as for feature refinements (see Section 8); in the case of
ports the direction must be **in** or **out**; in the case of
data access, the access right must be read-only for **in** and
write-only for **out**; in the case of bus access, subprogram
access and subprogram group access the direction is ignored.

6.  In the case of feature group prototypes, the supplied feature group
types must match the declared feature group type, if any. The
Prototype\_Substitution\_Rule property rules apply to feature
group types instead of component types.

7.  A classifier supplied in a feature prototype binding must match the
classifier of the prototype declaration, if present, according
to the Prototype\_Substitution\_Rule property rules.

8.  Component prototypes declared with square brackets specify that they
expect a list of component classifiers. These prototypes can
only be referenced in subcomponent array declarations. The
component classifier list supplies the classifiers for each of
the elements in the component array.

9.  The component category of the classifier reference or prototype
reference in a prototype binding declaration must match the
category of the prototype.

10. If a direction is specified for an abstract feature in a prototype
declaration, then the direction of the prototype actual must
match that declared in the prototype.

11. Component prototype bindings must only bind component prototypes,
feature group prototype bindings must only bind feature group
prototypes, and feature bindings must only bind feature
prototypes.

12. Component prototype refinements must only refine component
prototypes, feature group prototype refinements must only refine
feature group prototypes, and feature refinements must only
refine feature prototypes.

13. Prototype refinements are only allowed if it has not been bound to
an actual.

Standard Properties

Prototype Substitution Rule: **inherit** **enumeration** (Classifier
Match, Type Extension, Signature Match)

Semantics

 Prototypes can specify a parameterization of component classifiers
that can be referenced in feature declarations or in subcomponent
declarations. The same prototype can be referenced several times in
a component type and its component implementations to indicate that
the same actually supplied classifier is to be used. The supplied
component classifier may include prototype bindings if the
classifier has unbound prototypes. Such a component classifier is
effectively an unnamed extension of the classifier being referenced
(see Section 4.5).

Prototypes can specify a parameterization of abstract features
(**feature**) as well as feature group types for feature groups. The
prototype binding of an abstract feature can supply concrete
features

 Prototypes can only be bound once. Prototypes can be referenced in
prototype bindings, i.e., bound classifiers and features can be
passed down the component hierarchy.

 The prototype declaration specifies constraints on the component
category, on the feature kind, and on the classifier that can be
supplied. The Prototype\_Substitution\_Rule property specifies
whether the match requires matching classifiers, allows classifier
substitution, or allows any classifier with matching signature.

(5) A prototype refinement can increase the constraints on classifiers
to be supplied. The newly specified category, classifier, and array
dimensions must satisfy the same matching rules as the prototype
bindings.

Examples

 This example defines a generic component with a flow through one in
port and one out port. The abstract component type specifies the
data type used on the port as one prototype and an incoming abstract
feature as second prototype. This allows us to supply an event data
or data port as the incoming port for this pattern. The outgoing
port has been fixed to be a data port. The example also defines a
primary/backup redundant implementation of the flow component as a
pattern. It has a single prototype, namely the component that is to
be implemented as a dual redundant component. This prototype is used
to specify that both copies of the subcomponent are of the same
classifier. It takes the data type prototype as its prototype actual
to ensure that the data type of the pattern and the data type of the
supplied control prototype will match. These abstract components are
then refined into a controller process and its dual redundant
instantiation as a system.

**package** PrototypeExample

**public**

-- a generic component interface with one in and one out port

**abstract** flowComponent

**prototypes**

dt: **data**;

incoming: **in feature**;

**features**

insignal: **in feature** incoming;

outsignal: **out data port** dt;

**end** flowComponent;

-- a dual redundant component pattern

**abstract implementation** flowComponent.primaryBackup

**prototypes**

control: **abstract** flowComponent;

**subcomponents**

primary: **abstract** control;

backup:\ **abstract** control;

**connections**

inprimary: **feature** insignal -> primary.insignal;

inbackup: **feature** insignal -> backup.insignal;

outprimary: **port** primary.outsignal -> outsignal;

outbackup: **port** backup.outsignal -> outsignal;

**modes**

Primarymode: **initial mode**;

Backupmode: **mode**;

**end** flowComponent.primaryBackup;

**data** signal

**end** signal;

**data implementation** signal.unit16

**end** signal.unit16;

-- a controller to be realized as dual redundant system

**process** controller **extends** flowComponent ( dt => **data**
signal.unit16,

incoming => **event data port** signal.unit16 )

**end** controller;

-- the dual redundant controller system interface

**system** DualRedundantController **extends**

flowComponent (dt => **data** signal.unit16,

incoming => **in** **event data port** signal.unit16)

**end** DualRedundantController;

-- the dual redundant instance of the controller

**system implementation** DualRedundantController.PrimaryBackup

**extends** flowComponent.primaryBackup (control => **process**
controller)

**end** DualRedundantController.PrimaryBackup;

**end** PrototypeExample;

