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
--------------------

A component interface specifies the external interface of a component
that its implementations satisfy. It consists of features, flows, modes, and associated property values.

Features of a component represent interaction points with other components. Features are incoming and outgoing ports, required and provided access to data components, required and provided remote subprogram call, required and provided bus access, binding points between application and platform components, and named interfaces representing collections of features defined in a separate component type.

Flow specifications represent information flow from its incoming features to its outgoing features. The realization of each flow is specified in detail in each component implementation.

Component interfaces may declare modes and mode transitions. This allows users to characterize modal components and externally triggering mode transitions. 
Mode-specific property values can be associated with the component interface, its
features, and its flows.  

Component interfaces may include annex subclauses that provide annotations expressed in specific annex sublanguages, e.g., fault behavior expressed in the Error Model V2 (EMV2) annex notation.

Syntax
^^^^^^

| component\_interface ::=
| component\_category? **interface** *defining\_component\_interface\_*identifier **is**
| { feature \| flow\_spec \| mode \| mode\|transition \|required_mode \| annex\_subclause \| property\_association }\ :sup:`\*`
| **end** **;**
| 
| component\_category ::=
| generic\_component\_category
| \| software\_category
| \| execution\_platform\_category
| \| composite\_category
| 
| generic\_component\_category ::= **component**
| 
| software\_category ::= **data** \| **subprogram** \| **subprogram group** \| **thread** \| **thread group** \| **process **
| 
| execution\_platform\_category ::= **memory** \| **processor** \| **bus** \| **device** \| **virtual processor** \| **virtual bus** \| **virtual memory** \| **virtual device**
| 
| composite\_category ::= **system**


Naming Rules
^^^^^^^^^^^^

1. A component interface introduces a local name space for defining identifiers of component interface content. Such content is feature declarations, mode declarations, and flow specification declarations.

Legality Rules
^^^^^^^^^^^^^^

1. If the component interface contains requires\_mode declarations then it
must not contain any mode or mode transition declarations.

Semantics
^^^^^^^^^

A component interface represents the external interface specification of a
component. The component interface provides a contract for the component
that users of the component can depend on. 
The interface specification includes: 

* features as interaction points for connections and bindings, 
* flow specifications indicating flow sources, sinks, and paths from incoming to outgoing features, 
* externally visible mode behavior, and 
* property values associated with the component and its content. Property values may be mode specific. 

Component interfaces can be specified with a component category. If the category is omitted it is considered to be generic, i.e., category *component*.  The semantics of each of the component categories are described in later sections.

Examples
^^^^^^^^

| **package** TypeExample
| **import** FileSystem.\*, App.\* ;
| 
| **system** **interface** File\_System
| -- access to a data component
| root: **requires data access** Directory;
| **end** ;
| 
| **process** **interface** Application
| -- a data out port
| result: **out data port** App.result\_type;
| home: **requires** **data access** Directory;
| **end** ;
| 
| **thread** **interface** Calculate
| -- a data out port without a specified type
| input: **in data port** ;
| result: **out data port** ;
| **end** ;
| 
| **end** ;


Component Implementations 
--------------------------

 A *component implementation* represents the realization of a
component in terms of subcomponents, their connections, flow
sequences, bindings, properties, component modes and mode transitions, and other behavior specified in annexes. 

* Subcomponents represent instances of component inside a given component. Subcomponents may themselves contain subcomponents leading to a component hierarchy.
* Connections represent interactions between subcomponents. 
* Bindings represent deployment of application subcomponents to pltform subcomponent. 
* Flow sequences represent implementations of flow specifications in the component interface, or end-to-end flows with starting and end points within the component implementation. 
* Modes represent alternative operational modes that may manifest themselves as alternate configurations of subcomponents, connections, flow sequences, and property values.

A component interface can have zero, one, or multiple component
implementations. If a component interface has zero component
implementations, then it is considered to be a leaf in the system
component hierarchy. For example, an AADL model may have a
thread as subcomponent with only a component interface declaration. If no implementation is
associated then the properties on the component interface provides
information about the component for analysis and system generation.

Syntax
^^^^^^

component\_implementation ::=

component\_category *defining\_*component\_implementation\_name 

{ subcomponent \| internal\_feature \| processor\_feature \| subprogram\_call\_sequence \| connection \|
flow\_implemention \| end\_to\_end\_flow \| mode \| mode\|transition \| annex\_subclause \| property\_association }\ :sup:`\*`

**end** **;**


component\_implementation\_name ::=

*component\_interface*\_identifier **.** *component\_implementation*\_identifier



Naming Rules
^^^^^^^^^^^^^

1. A component implementation name consists of a component interface
   identifier and a component implementation identifier separated by a
   dot (**.**). The component interface identifier must be accessible in the package name space that contains the component implementation declaration, i.e., either declared in the same package or made accessible by a *import* declaration. 
   The defining identifier of the component implementation must be unique within the name space of the component interface.

2. Every component implementation defines a name space for the defining identifiers of its content.  

3. The component implementation name space inherits the name space of the 
   component interface. Defining identifiers of implementation content must not conflict with defining identifiers of the respective component interface content. For example, a feature in the component interface and a subcomponent in the component implementation cannot have hte same name.


Legality Rules
^^^^^^^^^^^^^^

1. The category of the component implementation must be identical to
the category of the component interface for which the component
implementation is declared. The category of the component implementation must be *component* if the category of the component interface is not specified.

2. If the component interface of the component implementation contains
requires\_mode declarations then the component implementation
must not contain any mode or mode transition declarations.

3. If modes are declared in the component interface, then modes cannot be
declared in component implementations.

4. If modes or mode transitions are declared in the component interface,
then mode transitions can be added in the component
implementation. These mode transitions may be triggered by ports of subcomponents in addition to ports of the component interface.


Consistency Rules
^^^^^^^^^^^^^^

1. If the component implementation has subcomponents, then a flow implemenation must be specified for each flow specificaiton in the component interface.

Semantics
^^^^^^^^^

 A component implementation defines the internal structure of a
component represented by subcomponents. Interaction between
subcomponents is expressed by the connections, flows, and bindings. Modes allow users to specify alternative runtime
configurations, i.e., subcomponent and connections can be active only in specific modes.
A component implementation and its content has property values to express its
non-functional attributes such as safety level or execution time.

A component implementation is defined in the context of a component interface.
All external interactions occur through the features of the interface, i.e., the interface enforces connectivity to external components.

A component interface can have multiple implementations. A component implementation
can be viewed as a component variant 
with differing property values that characterize the differences
between implementations. 

The component hierarchy of an actual system is modeled by component implementations with subcomponents, whose component classifier identifies another component implementation with subcomponents. 
Those subcomponents may recursively identify component implementations with subcomponent.

Processing Requirements and Permissions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

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

Examples
^^^^^^^^

**package** ImplementationExample

**type** Bool\_Type;

**thread** **interface** DriverModeLogic

BreakPedalPressed : **in data port** Bool\_Type;

ClutchPedalPressed : **in data port** Bool\_Type;

Activate : **in data port** Bool\_Type;

Cancel : **in data port** Bool\_Type;

OnNotOff : **in data port** Bool\_Type;

CruiseActive : **out data port** Bool\_Type;

**end** **;**

-- Two implementations whose source texts use different variable names

-- for their cruise active port

**thread** DriverModeLogic.Simulink

#Dispatch\_Protocol=>Periodic;

#Period=> 10 ms;

CruiseActive#Source\_Name => CruiseControlActive;

**end** DriverModeLogic.Simulink\ **;**

**thread** DriverModeLogic.C

#Dispatch\_Protocol=>Periodic;

#Period=> 10 ms;

#CruiseActive#Source\_Name => CCActive;

**end** **;**

**end** **;**

Component Configurations
------------------------


Naming Rules
^^^^^^^^^^^^

1. The subcomponent reference in a subcomponent configuration declaration must exist in the name space of an
   ancestor component implementation.

3. The configuration parameter identifier of a prototype reference must exist in the
   local name space of the component implementation.

4. The parameter referenced by the prototype binding declarations must
   exist in the name space of the component configuration being
   referenced.


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

 A *subcomponent* represents a component instance contained within another
component, i.e., declared within a component implementation.
Subcomponent declarations specify the component category and a component classifier or primitive type. If the component classifier is an interface then it can be configured with a realization, i.e., a component implementation or configuration.
If the subcomponent classifier is an implementation or a configuration then it identifies the next layer of component instances.
If the component configuration is parameterized parameter actuals may be included with the configuration reference.
If the category is *data* then a primitive type reference identifies the data type. A primitive type is one of the predeclared base types or a user defined type.

A subcomponent can be declared to apply to specific modes (rather
than all modes) defined within the component implementation, i.e., it may belong to a mode specific runtime configuration.

A subcomponent can be configured by assigning a component implementation or configuration that matches the originally specified classifier.

An array of subcomponents can be declared to represent a set of
subcomponents with the same component type. This array may have one
or more dimensions.

Syntax
^^^^^^

subcomponent ::=

*defining\_subcomponent*\ \_identifier :

component\_category

[ unique\_component\_classifier\_reference [ configuration\_actuals ]

[ array\_dimensions [ array\_element\_implementation\_list ] ]

[ **{** { property\_association }\ :sup:`+` **}** ]

[ component\_in\_modes ] **;**


subcomponent\_configuration ::=

*subcomponent\_*identifier **=>**

[ component\_realization\_reference [ configuration\_actuals ]

[ array\_dimensions [ array\_element\_implementation\_list ] ]

[ **{** { *subcomponent*\ \_property\_association

\| contained\_property\_association }\ :sup:`+` **}** ]

[ component\_in\_modes ] **;**


component\_realization\_reference ::=

( component\_implementation\_reference

\| component\_configuration\_reference )


array\_dimensions ::=

{ array\_dimension  }\ :sup:`+`


array\_dimension ::=

**[** [ array\_dimension\_size ] **]**


array\_dimension\_size ::=

numeral \| unique\_property\_constant\_identifier


-- array selection used in contained property association and references

array\_selection\_identifier ::=

identifier array\_selection

array\_selection ::=

{ **[** selection\_range **]** }\ :sup:`+`

selection\_range ::=

numeral [ **..** numeral ]


Legality Rules
^^^^^^^^^^^^^^

1. The category of the referenced component classifier must be the same as the category of the subcomponent declaration, or it may be a *generic* component classifier.

2. If the category of a subcomponent declaration is *data*, then its must reference a primitive type.

#. The classifier of a subcomponent cannot recursively contain
   subcomponents with the same component classifier. In other words,
   there cannot be a cyclic containment dependency between components.

6. Selecting index ranges in one or more dimensions of an array is only
   possible if the size of the array for these dimensions is already
   defined. The index range of a dimension is from 1 to the size of
   the dimension. Specification of array index ranges is limited to
   the **applies to** subclause of contained property associations.
   Specification of a single array element is limited to the
   **applies to** subclause of contained property associations and
   to the values of **reference** properties.

Standard Properties
^^^^^^^^^^^^^^^^^^^

Acceptable Array Size: **list of** Size Range

Semantics
^^^^^^^^^

Subcomponent declarations represent component instances.
Subcomponents are instantiated when the containing component
implementation is instantiated. Similarly, if the subcomponent declaration references a component realization, its subcomponents are instantiated recursively.

Once declared subcomponents can be configured. Configuration assignments associate a component implementation or configuration with an existing subcomponent declaration. 
Configuration assignments elaborate the component hierarchy by replacing a component interface with an implementation or configuration. 
Configuration assignments also associate collections of property values with the subcomponent and any model element in the component hierarchy within the subcomponent.
Finally, configuration assignments can associate annex subclauses to components in the hierarchy within the subcomponent.

If the referenced component configuration is parameterized then parameter actuals may be included with the configuration reference.

If the category is *data* then a primitive type reference identifies the data type. A primitive type is one of the predeclared base types or a user defined type.

A subcomponent can be declared to apply to specific modes (rather
than all modes) defined within the component implementation, i.e., it may belong to a mode specific runtime configuration.

An array of subcomponents can be declared to represent a set of
subcomponents with the same component type. This array may have one
or more dimensions.

A property value associated with a subcomponent array applies to each element in the array. Users can also specify property associations for specific array elements.

Processing Requirements and Permissions
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

If the subcomponent declaration references a component interface and the
interface has a single implementation then a method of processing (tool)
is permitted to generate a complete system instance by choosing the
single implementation even if it is not named. If the referenced
component interface has multiple implementations then the implementation
must be explicitly identified. However, some project may impose
design constraints that require modelers to completely specify such
classifier references.

Examples
^^^^^^^^

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

**type** Sample {#Data\_Size => 16 Bytes;};

**type** Sample\_Set {#Data\_Size => 1 MByte;};

**end** ;

**package** SamplingTasks

**with** Sampling;

**thread interface ** Init\_Samples

OrigSet : **requires data access** Sample\_Set;

SampleSet : **requires data access** Sample\_Set;

**end** ;

**thread interface ** Collect\_Samples

Input\_Sample : **in event data port** Sample;

SampleSet : **requires data access** Sample\_Set;

**end** ;

**thread** Collect\_Samples.Batch\_Update
Input\_Sample#Source\_Name => InSample;

**end** ;

**thread interface** Distribute\_Samples

SampleSet : **requires data access** Sample\_Set;

UpdatedSamples : **out event data port** :Sample;

**end** ;

**process** Sample\_Manager

Input\_Sample: **in event data port** Sample;

External\_Samples: **requires data access** Sample\_Set;

Result\_Sample: **out event data port** Sample;

**end** ;

**process** Sample\_Manager.Slow\_Update

Samples: **data** Sample\_Set;

Init\_Samples : **thread** Init\_Samples;

-- the required access is resolved to a subcomponent declaration

Collect\_Samples: **thread** Collect\_Samples.Batch\_Update;

Distribute: **thread** Distribute\_Samples;

Sample\_Filter: **subprogram** Sample\_Subprogram.Simple;

ISSSConn: **data** **access** Samples <-> Init\_Samples.SampleSet;

ISOSConn: **data access** External\_Samples <-> Init\_Samples.OrigSet;

CSSSConn: **data access** Samples <-> Collect\_Samples.SampleSet;

CSISConn: **port** Input\_Sample -> Collect\_Samples.Input\_Sample;

DSSConn: **data access** Samples <-> Distribute.SampleSet;

DUSConn: **port** Distribute.UpdatedSamples -> Result\_Sample;

CSFRConn: **subprogram access** Sample\_Filter <->
Collect\_Samples.Filtering\_Routine;

**end** ;

**end** ;

 This example illustrates the use of arrays in defining a triple
redundancy pattern with a voter. The pattern is defined as generic component that uses data ports. The
connections are defined with a connection pattern property to
indicate how the elements of the source array are connected to the
destination. Each instance of MyProcess is connected to a separate
port of the Voter. Note that the number of replicates could be kept
flexible by specifying the array dimension size through a property.

**package** Redundancy

**interface** Triple

input: **in** **data port**;

output: **out data port**;

**end** ;

**component** Triple.impl

MyProcess: **component** Calculate [3];

MyVoter: **component** Voter;

extinput1: **port** input -> MyProcess[1].input;

extinput2: **port** input -> MyProcess[2].input;

extinput3: **port** input -> MyProcess[3].input;

tovoter1: **port** MyProcess[1].output -> MyVoter.input[1];

tovoter2: **port** MyProcess[21].output -> MyVoter.input[2];

tovoter3: **port** MyProcess[3].output -> MyVoter.input[3];

extoutput: **port** MyVoter.output -> output;

**end** ;

**interface** Calculate

input: **in** **data port**;

output: **out data port**;

**end** ;

**interface** Voter

input: **in** **data port** [3];

output: **out data port**;

**end**;

**end** ;

