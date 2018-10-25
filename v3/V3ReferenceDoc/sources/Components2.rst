Component Classifiers
=====================

Component classifiers are used to characterize components in an embedded software system architecture. AADL supports three types of component classifiers:
 
1. *Component interface*: a component specification that represents the external interface to a component. It consists of *features* that represent interaction points with other components, 
*flows* from incoming features to outgoing features, externally visible modes, and property values associated with the component, its features, and flows.
 
2. *Component implementation*:  a component blue print that represents how a component is realized in terms of interconnected subcomponents. A *subcomponent* represent component instances that are characterized by a component classifier reference. Connections specify flow between features of different subcomponents or a subcomponent feature and its own feature.
Flow implementations elaborate on a flow from an incoming to an outgoing feature by identifying connections and subcomponents involved in the flow. A component implementation may also specify a binding from an software application component to a platform component.
Bindings specify the binding of a software subcomponent to a platform component. A component implementation and its elements can have property values. 
 
3. *Configuration*: a configuration of existing subcomponents including nested subcomponents as well as properties and bindings relevant to the configuration. 
A configuration does not change a subcomponent architecture. Instead it expands leaf subcomponents by choosing their implementations. In addition, configurations annotate model elements with additional property values. Configurations can be parameterized in that users of such configurations can only provide actuals for the specified parameters.
 
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
 
 Syntax
 ^^^^^^
 
 component\_classifier ::= component\_interface \| component\_implementation \| configuration
 

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
| 
| component\_interface\_reference ::= *component\_interface\_*package\_content\_reference


Naming Rules
^^^^^^^^^^^^

1. A component interface introduces a local name space for defining identifiers of component interface content. Such content is feature declarations, mode declarations, and flow specification declarations.

#. The defining name of a component interface is a single identifier. 

Legality Rules
^^^^^^^^^^^^^^

1. If the component interface contains requires\_mode declarations then it
must not contain any mode or mode transition declarations.

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


