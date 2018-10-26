Components
==========

Component classifiers are used to specify components in an embedded software system architecture. AADL supports three types of component classifiers:
 
1. *Component interface*: a component specification that represents the external interface to a component. It consists of *features* that represent interaction points with other components, 
*flows* from incoming features to outgoing features, externally visible modes, and property values associated with the component, its features, and flows.
 
2. *Component implementation*:  a component blue print that represents how a component is realized in terms of interconnected subcomponents. A *subcomponent* represent component instances based on a specification identified by a component classifier. Connections specify flow between features of different subcomponents or a subcomponent feature and its own feature.
Flow implementations elaborate on a flow from an incoming to an outgoing feature by identifying connections and subcomponents involved in the flow. A component implementation may also specify a binding from an software application component to a platform component.
Bindings specify the binding of a software subcomponent to a platform component. A component implementation and its elements can have property values. 
 
3. *Configuration*: a configuration configures aspects of a component implementation. Once configured those aspects cannot be changed by further configuration. It assigns component implementations to subcomponents (including nested subcomponents) that initially have a component interface assigned. In addition it assigns property values to any model element within the component hierarchy of the component implementation.
 It also specifies bindings between application and platform components. 
 
A component has a *component category*. It can be defined as a generic component (using the keyword **component**), or it can represent a specific software or hardware entity through one of the following component *categories*: 
*thread*, *thread group*, *process*, *data,*
*subprogram*, *subprogram group*, 
*memory*, *bus*, *virtual bus*, *processor*, *virtual processor*,
*device*, and *system*. The specific meaning of each of the component categories are described in later sections. 

A component can have multiple implementations. They represent variants of a component that adhere to the
same interface.  

Component interfaces and implementations can be declared in terms of other component types and implementations, i.e., they can *extend* an existing classifier by adding or configuring elements of the classifier. 
This permits the user to represent a family of related component classifiers.

Component interfaces and configurations can also be specified as *composition* of multiple existing component interfaces or configurations respectively.

 This standard defines basic concepts and requirements for determining compliance between a component specification via classifier and an
 actual component. This standard does not restrict the lower-level representation(s) used for software components, e.g., binary images,
 conventional programming languages, application modeling languages, nor does it restrict the lower-level representation(s) used for
 physical hardware component designs, e.g., circuit diagrams,hardware behavioral descriptions.
 

Component Interfaces
--------------------

A component interface specifies the external interface of a component
that its implementations satisfy. It consists of features, flows, modes, annex subclauses, and associated property values.

Features of a component represent interaction points with other components. Features are incoming and outgoing ports, required and provided access to data components, required and provided remote subprogram call, required and provided bus access, binding points between application and platform components, and named interfaces representing collections of features defined in a separate component type.

Flow specifications represent information flow from its incoming features to its outgoing features. The realization of each flow is specified in detail in each component implementation.

A component interface may declare modes and mode transitions. This allows users to characterize modal components and externally triggering mode transitions. 
Mode-specific property values can be associated with the component interface, its features, and its flows.  

A component interface may include annex subclauses that provide annotations expressed in specific annex sublanguages, e.g., fault behavior expressed in the Error Model V2 (EMV2) annex notation.

A component interface can be defined as extension of other component interfaces. An extension can add features, flows, modes, annex subclauses, and associated property values. An extension can also configure a feature with a primitive type or component classifier.

A component interface can also be defined as a composition of multiple component interfaces. In this case all content of the interfaces being combined are available in the resulting interface. 

Syntax
^^^^^^

| component\_interface\_definition ::=
| [ component\_category ] **interface** *defining\_*component\_interface\_name 
| [ **extends** [ **reverse** ] component\_interface\_reference  { **,** [ **reverse** ] component\_interface\_reference }\ :sup:`\*` ]
| [ **is** { feature \| flow\_spec \| mode \| mode\|transition \|required_mode \| annex\_subclause \| property\_association \| configuration\_assignment }\ :sup:`\*` ]
| **end** **;**
|
| component\_interface\_name ::= identifier
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
| 
| component\_interface\_reference  ::= [ *qualifying\_*package\_name **.** ] component\_interface\_name


Naming Rules
^^^^^^^^^^^^

1. A component interface definition introduces a local name space for defining identifiers of features, modes and transitions, and flow specifications.

#. A component interface reference resolves according to naming rules for component classifier references. 

#. Defining identifiers contained in component interfaces being extended are inherited into the namespace of the component interface definition, i.e., they must remain unique. This means there cannot be two definitions with the same name in any interface being extended and any definition added in the interface definition.

Legality Rules
^^^^^^^^^^^^^^

1. The component category of the component interface definition must be the same as the category of any component interface being extended, or the component interface being extended must have been defined without a category.

#. If the component interface contains requires\_mode declarations then it
must not contain any mode or mode transition declarations. 

#. In case of component interface extensions, only one interface being extended can contain mode related definitions. 
If the extensions adds mode related definitions, then they must be requires\_mode declaration if the inherited declarations are requires\_mode, 
or they must be mode and transition definitions if the inherited definitions are modes and transitions.

#. Configuration assignments to assign a primitive type or component classifier to a feature can only be declared in component interface extensions. 

Semantics
^^^^^^^^^

A component interface represents the external interface specification of a
component. The component interface provides a contract for the component
that users of the component can depend on. All interactions of this component with other components are limited to occur through the component features.

The interface specification includes: 

* features as interaction points for connections and bindings, 
* flow specifications indicating flow sources, sinks, and paths from incoming to outgoing features, 
* externally visible mode behavior, and 
* property values associated with the component and its content. Property values may be mode specific. 

Component interface extensions introduce an inheritance hierarchy for definitions and property value associations contained in interfaces. Inherited and local definitions must have unique names. Local property value associations may override previously associated values.

Component interface extension and composition allows users to represent related systems and provide libraries of common interface specifications. 

The same component interface can be included multiple times through the use of *named interface* feature declarations (see *Features* chapter). Similarly, different component interfaces with conflicting features can be combined through the use of *named interface* feature declarations.

Examples
^^^^^^^^

| **package** InterfaceExample
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

::
package InterfaceComposition
  interface Logical
	is
	temperature : out port ;
		Speed : out port ;
  end ;
  interface Physical
	is
	Network : requires bus access CANBus;
  end ;
  interface s1 extends Logical
	is
	Onemore : out port ;
  end ;
  system interface s2 extends Logical , Physical 
  end ;

  interface s3 extends Logical , Physical
	is
	Onemore : out port ;
  end ;

  bus interface CANBus end;
end; 


Component Implementations 
--------------------------

 A *component implementation* represents the realization of a
component that satisfies a component interface definition, i.e., all external interactions must occur through features in the interface. 

A component implementation consists of
1. *Subcomponents* that represent instances of component inside a given component. Subcomponents may themselves contain subcomponents leading to a component hierarchy.
#. *Connections* that represent interactions between subcomponents. 
#. *Bindings* that represent deployment of application subcomponents to platform subcomponent. 
#. *Flow sequences* that represent implementations of flow specifications in the component interface, or end-to-end flows with starting and end points within the component implementation. 
#. *Modes* that represent alternative operational modes that may manifest themselves as alternate configurations of subcomponents, connections, flow sequences, and property values.
#. *Annex subclauses* that specify additional characteristics of the component.
#. *Associations of property values* specific to the component implementation and its contained elements.

A component implementation can be defined as extension of another component implementation. The extension can add subcomponents, connections, bindings, flow sequences, modes, annex subclauses, and associated property values. 
An extension can also configure a subcomponent with a component classifier or primitive type.


Syntax
^^^^^^

| component\_implementation ::= component\_category *defining\_*component\_implementation\_name 
| ( **extends** component\_implementation\_reference )?
| ( **is** { subcomponent  \| connection \| flow\_sequence \| mode \| mode\|transition \| annex\_subclause \| property\_association \| internal\_feature \| processor\_feature}\ :sup:`\*` )?
**end** **;**


component\_implementation\_name ::= *component\_interface*\_identifier **.** *component\_implementation*\_identifier


component\_implementation\_reference ::= [ *qualifying\_*package\_name **.** ] *component\_implementation*\_name


Naming Rules
^^^^^^^^^^^^^

1. The defining name of a component implementation consists of two <dot> separated identifiers. 

#. The component interface identifier of the defining component implementation name must exist in the name space of the package containing the component implementation definition or it must be visible through an *import* declaration. 

#. The component implementation identifier of the defining component implementation name must be unique within the name space of the component interface.

#. The component implementation defines a name space for the defining identifiers of its content.  

#. The component implementation name space inherits the name space of the 
   component interface. Defining identifiers of implementation content must not conflict with defining identifiers of the respective component interface content. 

#. A component implementation extension inherits the namespace of the implementation being extended. 

Legality Rules
^^^^^^^^^^^^^^

1. The category of the component implementation must be the same as
the category of the component interface for which the component
implementation is declared. 

#. A component implementation cannot be associated with a component interface without a category. 

#. The category of a component implementation extension must be the same as the category of the implementation being extended

#. If the component interface of the component implementation contains
requires\_mode declarations then the component implementation
must not contain any mode or mode transition declarations.

#. If modes or transitions are declared in the component interface, then modes or transitions cannot be declared in any of its associated component implementations.


Consistency Rules
^^^^^^^^^^^^^^

1. If the component implementation has subcomponents, then a flow implementation must be specified for each flow specification in the component interface.

Semantics
^^^^^^^^^

A component implementation defines the internal structure of a
component represented by subcomponents. Interaction between
subcomponents is expressed by the connections, flow sequences, and bindings. Modes allow users to specify alternative runtime
configurations, i.e., subcomponent and connections can be active only in specific modes.
A component implementation and its content has property values to express its
non-functional attributes such as safety level or execution time.

A component implementation is defined in the context of a component interface.
All external interactions only occur through the features of the interface, i.e., the interface enforces connectivity to external components.

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
::
 package ImplementationExample
   thread interface Filter
     SensorReading : in data port ;
     FilteredData : out data port ;
     #Period=> 20 ms;
   end ;

   thread interface Processing
     FilteredData : in data port ;
     ActuatorCommand : out data port ;
     #Period=> 20 ms;
   end ;
    
   process interface Controller
     SensorReading : in data port ;
     ActuatorCommand : out data port ;
   end ;
    
   process Controller.basic
     filtering: thread Filter;
     computing : thread Processing ;
     FtoC: port filtering.FilteredData -> computing.FilteredData;
     ItoF: map SensorReading -> filtering.SensorReading;
     OtoC: map ActuatorCommand -> computing.ActuatorCommand;
   end ;
    
 end ;



Subcomponents
-------------

 A *subcomponent* represents a component instance contained within another component. It is declared within a component implementation.
Subcomponent declarations specify the component category and a component classifier or in the case of a data component primitive type. 
If the subcomponent classifier is an implementation or a configuration then it identifies the next layer of component instances in the component hierarchy.

A subcomponent can be declared as an array. 

A subcomponent can have property values associated as part of the subcomponent declaration. 

A subcomponent declaration may include a specification of nested subcomponents without explicit component classifier specification.

Syntax
^^^^^^

| subcomponent ::= *defining\_subcomponent\_*identifier **:** component\_category
| ( type\_reference [ array\_dimensions ] [ **{** { property\_association  }\ :sup:`+` **}** ] }
| \| nested\_subcomponent\_declaration
| **;**
| 
| type\_reference ::= component\_interface\_reference \| component\_implementation\_reference \| component\_configuration\_reference \| primitive\_type\_reference
| 
| nested\_subcomponent\_declaration ::= 
| [ array\_dimensions ] **{** { property\_association \| subcomponent \| feature \| connection }\ :sup:`+` **}**
| 
| subcomponent\_reference ::=  identifier [ array\_selection ]
| 
| array\_dimensions::= { array\_dimension }\ :sup:`+`
| 
| array\_dimension::= **[** [ array\_dimension\_size] **]**
| 
| array\_dimension\_size::= numeral \| unique\_property\_constant\_identifier
| 
| -- array selection used in contained property association and references
| 
| array\_selection::=
| { **[** selection\_range **]** }\ :sup:`+`
| 
| selection\_range::= numeral [ **..** numeral ]

Naming Rules
^^^^^^^^^^^^

1. The type references must be visible in the name scope of the package that contains the component implementation with the subcomponent declaration.


Legality Rules
^^^^^^^^^^^^^^

1. The category of the referenced component classifier must be the same as the category of the subcomponent declaration, or it may be a *generic* component classifier.

#. If the category of a subcomponent declaration is *data*, then its must reference a primitive type.

#. The classifier of a subcomponent cannot recursively contain subcomponents with the same component classifier. In other words, there cannot be a cyclic containment dependency between components.

#. A nested subcomponent declaration must either contain subcomponent and connection definitions or contain feature definitions.


Semantics
^^^^^^^^^

Subcomponent declarations represent component instances.
Subcomponents are instantiated when the containing component
implementation is instantiated. Similarly, if the subcomponent declaration references a component implementation or configuration, its subcomponents are instantiated recursively.

An array of subcomponents represents a collection of
the same component instance. This array may have one or more dimensions. 
A property value associated with a subcomponent array applies to each element in the array. Users can also specify property associations for specific array elements.

Property values can be associated with subcomponents by declaring them in curly brackets as part of the subcomponent declaration. 
Property values can also be associated by a property association declaration that references the subcomponent.

Once declared subcomponents can be configured through configuration assignments (see next section). 

Nested subcomponents can be declared as part of a subcomponent declaration inside curly brackets. This allows users to define a subcomponent hierarchy without explicit classifiers.


A component interface can have zero, one, or multiple component
implementations. If a component interface has zero component
implementations, then it is considered to be a leaf in the system
component hierarchy. For example, an AADL model may have a
thread as subcomponent with only a component interface declaration. If no implementation is
associated then the properties on the component interface provides
information about the component for analysis and system generation.


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

OrigSet: **requires data access** Sample\_Set;

SampleSet: **requires data access** Sample\_Set;

**end** ;

**thread interface ** Collect\_Samples

Input\_Sample: **in event data port**�Sample;

SampleSet�: **requires data access** Sample\_Set;

**end** ;

**thread** Collect\_Samples.Batch\_Update
Input\_Sample#Source\_Name => InSample;

**end** ;

**thread interface** Distribute\_Samples

SampleSet: **requires data access** Sample\_Set;

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



Configurations
--------------

A configuration declaration allow users to configure an existing architecture design by expanding its component hierarchy, but not change it. A configuration declaration can

* assign a component implementation or configuration with previously declared subcomponents down the component hierarchy, which will be used during model instantiation
* assign a primitive type or component interface to features of components in the component hierarchy
* associate property values with existing model elements down the component hierarchy
* associate annex subclauses to classifiers

A configuration is declared with respect to a component implementation or another configuration.

A configuration can be parameterized. If a parameterized configuration is assigned to a subcomponent, then the component hierarchy represented by the subcomponent can only be configured through the parameters.

!!! Assignment or property values into a parameterized configuration.

Syntax
^^^^^^

| configuration ::=
| **configuration** *defining\_*configuration\_name [ configuration\_parameters ] 
| [ **extends** implementation\_reference  { **,** component\_interface\_reference }\ :sup:`\*` ]
| [ **is** configuration\_content ]
| **end** **;**
| 
| configuration\_name ::= *component\_interface*\_identifier **.** *configuration*\_identifier
| 
| configuration\_reference ::= [ *qualifying\_*package\_name **.** ] configuration\_name
| 
| parameterized\_configuration\_reference ::= configuration\_reference [ **(** configuration\_actual { **,** configuration\_actual }\ :sup:`\*` **)** ]
| 
| configuration\_parameter ::= *defining\_parameter\_*identifier **:** expected\_type\_reference
| 
| configuration\_content ::= { configuration\_assignment \| property\_association \| mode\_assignment }\ :sup:`\*`
| 
| configuration\_assignment ::= model\_element\_reference **=>** 
| ( assigned\_configuration\_value [ sub\_configuration ] ) \| sub\_configuration
| 
| assigned\_configuration\_value ::= primitive\_type\_reference \| component\_implementation\_reference \| configuration\_reference \| configuration\_parameter\_reference
| 
| sub\_configuration ::= **{** configuration\_content **}**

Naming Rules
^^^^^^^^^^^^
1. The defining name of a configuration consists of two <dot> separated identifiers. 

#. The component interface identifier of the defining configuration name must exist in the name space of the package containing the configuration definition or it must be visible through an *import* declaration. 

#. The configuration identifier of the defining configuration name must be unique within the name space of the component interface.

#. The configuration defines a name space for the defining identifiers of its parameters.  

#. The configuration name space inherits the name space of the component interface. Defining identifiers of configuration must not conflict with defining identifiers of the respective component interface content.

#. The model element reference of configuration assignments is resolved in the context of the configuration name space. In the case of configuration assignments in subconfigurations the name space of the model element referenced by the enclosing configuration assignment is used.

Legality Rules
^^^^^^^^^^^^^^

1. For subcomponent model element references that are not data components the assigned configuration value must be a component implementation or configuration. 

#. For data subcomponent model element references the assigned value must be a primitive type. !!!No previous value or an extension of the previous value.

#. In the case a component implementation is assigned to a subcomponent the previous classifier of the subcomponent must be the component interface for the implementation.

#. In the case a configuration is assigned to a subcomponent it must be an extension of a previously assigned component interface or implementation. 

#. If a subconfiguration contains configuration assignments or mode assignments then the referenced model element of the configuration assignment with the subconfiguration must be a subcomponent. 

#. For feature model element references to ports, data access features, or abstract features, the assigned configuration value must be a primitive type. For other access features it must be a compatible component classifiers. 

#. !!! assignment of primitive types to subcomponents or any value to features: no type before vs. extension of previously assigned type.

Semantics
^^^^^^^^^

A configuration can elaborate an existing architecture design by expanding its component hierarchy, but not change it. 
It does so by assigning to an existing subcomponent a component implementation, if the original classifier is a component interface, or a configuration that is an extension of a previously assigned component implementation.. 

A configuration can assign component classifiers or primitive types with features.

A configuration can associate property values to any model element within the component hierarchy represented by the configuration. 

A configuration can add annex subclauses to components in the component hierarchy within the subcomponent.


If the component configuration is parameterized parameter actuals may be included with the configuration reference.

If the category is *data* then a primitive type reference identifies the data type. A primitive type is one of the predeclared base types or a user defined type.

If the referenced component configuration is parameterized then parameter actuals may be included with the configuration reference.

