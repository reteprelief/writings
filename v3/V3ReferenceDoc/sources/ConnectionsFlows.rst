Connections 
============

 A *connection* is a linkage between features of two components that
represents communication of data and control between components.
This can be the transmission of control and data between ports of
different threads or between threads and processor or device
components. A connection may denote an event that triggers a mode
transition. The timing of data and control transmission depends on
the connection category and on the dispatch protocol of the
connected threads. The flow of data between parameters of subprogram
calls within a thread may be specified using connections. Finally,
connections designate access to shared components.

 The AADL supports connections between abstract features, feature
groups connections, port connections, parameter connections, and
access connections. Port connections represent directional flow of
data and control between two concurrently executing components,
i.e., between two threads or between a thread and a processor or
device. Parameter connections denote the flow of data through the
parameters of a sequence of subprogram calls within a thread. Data
access connections designate access to shared data components by
concurrently executing threads or by subprograms executing within a
thread. Bus access connections represent communication between
processors, memory, and devices by accessing a shared bus.
Subprogram access connections represent the binding of a subprogram
call to a subprogram instance being called.

When an AADL model with a thread architecture is instantiated, a
connection instance is created from the ultimate source to the
ultimate destination component by following a sequence of connection
declarations. The ultimate source and ultimate destination typically
are the active components in the instance model or components whose
access is shared. This connection instance is referred to as
semantic connection and its semantic details are defined for each of
the different types of connections. If the AADL model is not fully
detailed to the thread level, connection instances are created
between the leaf components in the system hierarchy.

Syntax

connection ::=

*defining\_connection\_*\ identifier **:**

( feature\_connection

\| port\_connection

\| parameter\_connection

\| access\_connection

\| feature\_group\_connection )

[ **{** { property\_association }\ :sup:`+` **}** ]

[ in\_modes\_and\_transitions ] ;

connection\_refinement ::=

*defining\_connection\_*\ identifier **: refined to**

( feature\_connection\_refinement

\| port\_connection\_refinement

\| parameter\_connection\_refinement

\| access\_connection\_refinement

\| feature\_group\_connection\_refinement )

[ **{** { property\_association }\ :sup:`+` **}** ]

[ in\_modes\_and\_transitions ] ;

Naming Rules

1. The defining identifier of a defined connection declaration must be
   unique in the local namespace of the component implementation with
   the connection subclause. This is also the case for mode-specific
   connection declarations, i.e., declarations with an
   in\_modes\_and\_transition subclause.

1. The connection identifier in a connection refinement declaration must
   refer to a named connection declared in an ancestor component
   implementation.

Legality Rules

1. A connection refinement must contain at least one of the following: a
   connection source and destination subclause, a property
   association, an **in modes** subclause.

1. If a semantic connection may be active in a particular mode, then the
   ultimate source and ultimate destination components must be part
   of that mode.

2. If a semantic connection may be active in a particular mode
   transition, then the ultimate source component must be part of a
   system mode that includes the old mode identifier and the
   ultimate destination component must be part of a system mode that
   includes the new mode identifier.

Semantics

 Connections define directional and bidirectional connectivity
between abstract features, ports, access features, parameters, and
between feature groups.

Connections can have properties. Connections can be defined to be
active in certain modes or in mode transitions, as indicated by the
in\_modes\_and\_transition subclause.

1. .. rubric:: Feature Connections
  :name: feature-connections

 A feature connection can be declared between two abstract features
of components. If the features specify a direction, then the source
of the connection is the feature with the **out** direction and the
destination is the feature with the **in** direction. Otherwise, the
connection is considered to be bidirectional.

 A feature connection can connect nested features, i.e., features
inside feature groups, when the connection connects two
subcomponents. This provides a way of connecting subcomponents with
feature groups, whose nesting hierarchies differ.

Syntax

feature\_connection ::=

**feature** *source*\ \_feature\_reference connection\_symbol

*destination*\ \_feature\_reference

connection\_symbol ::=

directional\_connection\_symbol \| bidirectional\_connection\_symbol

directional\_connection\_symbol ::= **->**

bidirectional\_connection\_symbol ::= **<->**

feature\_reference ::=

-- feature in the component type

*component\_type\_feature*\ \_identifier \|

-- feature in a feature group of the component type

*component\_type\_feature\_group*\ \_identifier **.**
*feature*\ \_identifier \|

-- feature in a subcomponent

*subcomponent*\ \_identifier **.** *feature*\ \_identifier

{ **.** *nested\_feature\_*\ identifier }\*

-- feature in a call

*subprogram\_call*\ \_identifier **.** *feature*\ \_identifier

feature\_connection\_refinement ::=

**feature **

Naming Rules

1. A source or destination reference in a feature connection declaration
   must reference a feature declared in the component type, a feature in
   a feature group of the component type, or a feature of one of the
   subcomponents. The source and destination features must be abstract
   features, ports, parameters, access features, or feature groups.

1. The subcomponent reference may refer to a subcomponent or a
   subcomponent array.

Legality Rules

1. The feature identifier of a subcomponent reference may refer to a
   feature array, if the subcomponent is a thread, device, or processor.

The direction declared for the destination feature of a feature
connection declaration must be compatible with the direction declared
for the source feature as defined by the following rules:

1. If the feature connection declaration represents a connection between
   features of sibling components, then the source must be an
   outgoing feature and the destination must be an incoming feature.
   An *outgoing feature* is an abstract feature with no direction or
   **out** direction, an outgoing port or parameter with an **out**
   or **in out** direction, or an access feature, in the case of
   data access with at least write access. An *incoming feature* is
   an abstract feature with no direction or **in** direction, an
   incoming port or parameter with an **in** or **in out**
   direction, or an access feature, in the case of data access with
   at least read access.

2. If the feature connection declaration represents a connection between
   features up the containment hierarchy, then the source and
   destination must both be outgoing features.

3. If the feature connection declaration represents a connection between
   features down the containment hierarchy, then the source and
   destination must both be incoming features.

4. If the feature connection declaration specifies a directional
   connection, then the direction of the connection must be
   supported by the direction of the source and destination
   features.

5. The individual connections of a semantic connection must be
   bidirectional or have the same direction. The direction of the
   connection is determined by the direction of the source and
   destination feature and by the direction of the connection
   declarations.

6. If the feature connection is between two subcomponents, then nested
   features can be referenced arbitrarily deep. For connections up
   and down the hierarchy, the reference to a subcomponent feature
   is limited to a single feature identifier.

Standard Properties

Required Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

Required Connection Quality Of Service : **inherit list of** Supported
Connection\_QoS

Connection Pattern: **list of list of** Supported Connection Patterns

Connection Set: **list of** Connection Pair

Semantics

 Feature connections represent connections between abstract features,
or between an abstract feature and a concrete feature. Connection
patterns are specified as described in Section 9.2.3.

Feature connections can be used to connect (nested) elements of
subcomponent feature groups. A feature in a subcomponent feature
group may be connected individually as well as by a feature group
connection of the enclosing feature group – resulting in separate
connections.

1. .. rubric:: Port Connections
  :name: port-connections

 Port connections represent directional transfer of data and control
between two concurrently executing components, i.e., between
threads, processors, and devices. They are feature connections,
whose source and destination are limited to ports and data access.

These connections are *semantic port connections*. A semantic port
connection is determined by a sequence of one or more individual
port connection declarations that follow the component containment
hierarchy in a fully instantiated system from an *ultimate source*
to an *ultimate destination*. In a partial AADL model the ultimate
source and destination of a semantic port connection are the ports
of leaf components in the containment hierarchy, i.e., a thread
group, process, or system without subcomponent.

 Semantic port connections are illustrated in Figure 14. The
*ultimate source* of a semantic port connection is an outgoing port
of a thread, virtual processor, processor, or device component, or
is a data component. The *ultimate destination* of a semantic port
connection is an incoming port of a thread, virtual processor,
processor, or device component, or is a data component. In the case
of a bidirectional connection between **in out** ports either port
can be the ultimate source or destination.

Port connection declarations follow the containment hierarchy of
threads, thread groups, processes and systems, or devices,
processors, and systems. An individual port connection declaration
links an outgoing port of one subcomponent to an incoming port of
another subcomponent, i.e., it connects sibling components at the
highest level in the component hierarchy required for the
connection. Alternatively, a port connection declaration maps an
outgoing port of a subcomponent to an outgoing port of a containing
component or an incoming port of a containing component to an
incoming port of a subcomponent. In other words, these connections
traverse up and down the containment hierarchy.

|image14|

Figure − Semantic Port Connection
 

 Semantic port connections also represent the sampling of a data
component content by a data or event data port, and updating a data
component with the output of a data or event data port. In other
words, the ultimate source or the ultimate destination of a semantic
port connection, but not both, can be a data component.

Semantic port connections may also route a raised event to a modal
component through a sequence of connection declarations. A mode
transition in such a component is the ultimate destination of the
connection, if the mode transition names an incoming event port in
the enclosing component, or an outgoing event port of one of the
subcomponents (see Section 12).

 Semantic port connections may exist between arrays of component
instances. In this case, the Connection\_Pattern or Connection\_Set
property specifies which source array elements have a semantic
connection to which destination array element (see section 9.2.3 for
more detail).

 This section defines the concepts of departure and arrival times of
port connection transmission for each type of *port connection*. The
transfer semantics between periodic threads can be defined such that
they ensure deterministic sampling of data. All other communication
can be defined to be sampling or data driven. The inputs and outputs
can be specified to occur at dispatch, any time during execution, at
completion, or at deadline.

Syntax

port\_connection ::=

**port**

*source\_*\ port\_connection\_reference

connection\_symbol

*destination\_*\ port\_connection\_reference

port\_connection\_refinement ::=

**port **

port\_connection\_reference ::=

-- port in the component type

*component\_type\_port*\ \_identifier

\|

-- port in a subcomponent

*subcomponent*\ \_identifier **.** *port*\ \_identifier

\|

-- port element in a feature group of the component type

*component\_type\_feature\_group*\ \_identifier .
*element\_port*\ \_identifier

\|

-- data element in aggregate data port

*component\_type\_port*\ \_identifier .
*data\_subcomponent*\ \_identifier

\|

-- requires data access in the component type

*component\_type\_requires\_data\_access*\ \_identifier

\|

-- data subcomponent

*data\_subcomponent*\ \_identifier

\|

-- data component provided by a subcomponent

*subcomponent*\ \_identifier **.**
*provides\_data\_access*\ \_identifier

\|

-- data access element in a feature group of the component type

*component\_type\_feature\_group*\ \_identifier .
*element\_data\_access*\ \_identifier

\|

-- access to element in a data subcomponent

*data\_subcomponent*\ \_identifier **.**
*data\_subcomponent*\ \_identifier

\|

-- processor port

[ **processor .** ] *processor\_port*\ \_identifier

\|

-- component itself as event or event data source

[ **self** . ] *internal\_event\_or\_event\_data\_*\ \_identifier

-- Note: **data port**, **event data port**, and **event port**
connections

-- are replaced by **port** connections in AADL V2

Naming Rules

1. The connection identifier in a port connection refinement declaration
   must refer to a named port or feature connection declared in an
   ancestor component implementation.

1. A source or destination reference in a port connection declaration
   must reference a port declared in the component type, a port of one
   of the subcomponents, or a port that is an element of a feature group
   in the component type, or it must refer to a requires data access in
   the component type, a provides data access in one of the
   subcomponents, or a data subcomponent.

2. The subcomponent reference may also consist of a reference to a
   subcomponent array.

3. The internal event or event data identifier must reference an
   internal event or event data declared in the component implementation
   or one of its ancestors.

Legality Rules

1. In the case of a directional port connection the connection end
   representing the source of the flow must be the source of the
   connection and the connection end representing the destination of
   the flow must be the destination of the connection.

2. In the case of a bidirectional port connection either connection end
   can be the source. If the bidirectional connection has
   directional connection ends, then the flow is determined by the
   direction of the connection ends. In the case of ports it is the
   port direction and in the case of data access features it is the
   access right.

3. If the source connection end is a data access feature it must have
   read access rights; if the destination connection end is a data
   access feature it must have write access rights.

1. The feature identifier of a subcomponent reference may refer to a
   feature array, if the subcomponent is a thread, device, or
   processor.

2. The following are acceptable sources and destinations of port
   connections. The left column shows connections between ports,
   while the right column shows connection between ports and data
   components.

+-------------------------------------------------------------+---------------------------------------------------------------+
| event port -> event port| data, data access -> data port, event data port, event port   |
| |   |
| data port -> data port, event data port, event port | data port, event data port -> data, data access   |
| |   |
| event data port -> event data port, data port, event port   |   |
+=============================================================+===============================================================+
+-------------------------------------------------------------+---------------------------------------------------------------+

The direction of the source port of a port connection declaration must
be compatible with the direction declared for the destination port as
defined by the following rules:

1. If the port connection declaration represents a connection between
   ports of sibling components, then the source must be an outgoing
   port and the destination must be an incoming port. If the source
   connection end is a data access feature, then it must be a
   **provides** access feature; if it is a destination connection
   end it must be a **requires** access feature.

2. If the port connection declaration represents a connection between
   ports up the containment hierarchy, then the source and
   destination must both be outgoing ports. If the source connection
   end is a data access feature, then it must be a **provides**
   access feature; if it is a destination connection end it must be
   a **requires** access feature.

3. If the port connection declaration represents a connection between
   ports down the containment hierarchy, then the source and
   destination must both be incoming ports. If the source connection
   end is a data access feature, then it must be a **requires**
   access feature; if it is a destination connection end it must be
   a **provides** access feature.

4. The individual connections of a semantic port connection must be
   bidirectional or have the same direction. The direction of the
   connection is determined by the direction of the source and
   destination feature and by the direction of the connection
   declarations.

5. **Self.**\ <identifier> may be referenced as the source or
   destination of a connection.

6. A data port cannot be the destination of more than one semantic port
   connection unless each semantic port connection is contained in a
   different mode.

7. A semantic connection cannot contain connection declarations with
   both immediate and delayed Timing property values.

8. For connections between data ports, event data ports and data access,
   the data classifier of the source port must match the data type
   of the destination port. The Classifier\_Matching\_Rule property
   specifies the rule to be applied to match the data classifier of
   a connection source to the data classifier of a connection
   destination.

9. The following rules are supported:

-  Classifier\_Match: The source data type and data implementation must
   be identical to the data type or data implementation of the
   destination. If the destination classifier is a component type, then
   any implementation of the source matches. This is the default rule.

-  Equivalence: An indication that the two classifiers of a connection
   are considered to match if they are listed in the
   Supported\_Classifier\_Equivalence\_Matches property. Acceptable data
   classifier matches are specified as
   Supported\_Classifier\_Equivalence\_Matches property with pairs of
   classifier values representing acceptable matches. Either element of
   the pair can be the source or destination classifier. Equivalence is
   intended to be used when the data types are considered to be
   identical, i.e., no conversion is necessary. The
   Supported\_Classifier\_Equivalence\_Matches property is declared
   globally as a property constant.

-  Subset: A mapping of (a subset of) data elements of the source port
   data type to all data elements of the destination port data type.
   Acceptable data classifier matches are specified as
   Supported\_Classifier\_Subset\_Matches property with pairs of
   classifier values representing acceptable matches. The first element
   of each pair specifies the acceptable source classifier, while the
   second element specifies the acceptable destination classifier. The
   Supported\_Classifier\_Subset\_Matches property is declared globally
   as a property constant. A virtual bus or bus must represent a
   protocol that supports subsetting, such as OMG DDS.

-  Conversion: A mapping of the source port data type to the destination
   port data type, where the source port data type requires a conversion
   to the destination port data type. Acceptable data classifier matches
   are specified as Supported\_Type\_Conversions property with pairs of
   classifier values representing acceptable matches. The first element
   of each pair specifies the acceptable source classifier, while the
   second element specifies the acceptable destination classifier. The
   Supported\_Type\_Conversions property may be declared globally as a
   property constant. A virtual bus or bus must support the conversion
   from the source data classifier to the destination classifier.

1. If more than one port connection declaration in a semantic port
   connection has a property association for a given connection
   property, then the resulting property values must be identical.

2. A processor port specification must only be used in event connections
   within threads and subprograms.

Consistency Rules

1. There cannot be cycles of immediate connections between threads,
   devices, and processors.

1. The processor port identifier of a processor port specification
   (**processor**.\ *processor\_port\_*\ identifier) must name a port of
   the processor that the thread is bound to.

2. The Supports\_Classifier\_Subset\_Matches property may be associated
   with a bus or virtual bus. This specifies the subset matches a
   particular protocol supports. Subset matches of connections bound to
   such a virtual bus or bus must be supported by the respective virtual
   bus or bus.

3. The Supports\_Type\_Conversions property may be associated with a bus
   or virtual bus. This specifies the subset matches a particular
   protocol supports. Subset matches of connections bound to such a
   virtual bus or bus must be supported by the respective virtual bus or
   bus.

Standard Properties

Timing : **enumeration** (sampled, immediate, delayed) **=>** sampled

Connection Pattern: **list of list of** Supported Connection Patterns

Connection Set: **list of** Connection\_Pair

Transmission Type: **enumeration** ( push, pull )

Required Connection Quality Of Service : **inherit list of** Supported
Connection QoS

Allowed Connection Binding\_Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

Allowed Connection Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, bus, virtual bus, device, memory, system)

Not Collocated: **record** (

Targets: **list** **of** **reference** (data, thread, process, system,
connection);

Location: **classifier** ( processor, memory, bus, system ); )

Actual Connection Binding: **inherit list of** **reference** (processor,
virtual processor, bus, virtual bus, device, system, memory)

Semantics

Port Connection Characteristics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 A semantic port connection represents directed flow of data and
control between threads, processors and devices. In the case of
event or event data ports the ultimate destination can be a mode
transition of a component.

A port connection can refine a feature connection. A port connection
can be refined by changing the direction from bidirectional to
directional, by adding a **in modes** subclause, and by adding
property associations for the connection in a connection refinement
declaration.

 A port connection declared with the optional
in\_modes\_and\_transitions subclause specifies whether the
connection is part of specific modes or is part of the transition
between two specific modes. The detailed semantics of this subclause
are defined in Section 12.

 While in a given mode, transmission over a port connection only
occurs if the connection is part of the current mode.

(5) During a mode switch, transmission over a data port connection only
occurs at the actual time of mode switch if the port connection is
declared to apply to the transition between two specific modes. The
actual mode switch initiates transmission. This allows data state to
be transferred between threads active in different modes. Similarly,
for event or event data ports it allows for transfer of queue
content.

(6) Port connections may refer to an event source specification
(**self**.event or event data name). An event source specification
indicates that the component itself is the source of an event. In
the case of a thread this may be due to a *Send\_Output* system call
or due to an event raised by the underlying runtime system, i.e.,
the processor. For all components it may represent the fact that a
component fault is the source of an event or a message (event data
port) with fault information to support diagnostics, as specified by
the Error Model Annex (see Annex Document C).

(7) A thread or device may be connected to the port of a processor. For
example, a health monitoring thread may receive the heart beat
events from several processors. In addition, a port connection may
refer to the port of the processor that an application software
component is bound to (**processor** . <portname> ). If a processor
or device is the data connection source, then the transmission is
initiated and completed when the destination thread or device is
dispatched. In this case a data port can model a processor or device
register that is sampled by a thread or device. If a device or
processor is the event connection source, then the occurrence of an
interrupt represents the initiation of an event transmission.

(8) AADL supports the following port connection declarations:

-  Event port, data port, event data port, data (data access) -> event
   port: port output or written data is recognized as event and queued
   in the event port.

-  Event data port, data port, data (data access) -> event data port:
   data output or written data is transferred and received as event data
   in a queued port.

-  Data port, event data port, data (data access) -> data port: Data
   output or written data is transferred and available upon receipt as
   most recent value of a data port variable, i.e., the data port
   samples data.

  A port connection to a mode transition is declared by naming the
 event port or event data port in the mode transition declaration
 (see Section 12).

 1. .. rubric:: Port Connection Topology
   :name: port-connection-topology

 The AADL supports n-to-n connectivity for event and event data
 ports. A port may have multiple outgoing connections, i.e., its
 content is transmitted to multiple destinations. This means that
 each destination port receives an instance of the event, or event
 data being transmitted. Similarly, event and event data ports can
 support multiple incoming connections resulting in sequencing and
 possibly queuing of incoming events and event data.

  For example, multiple threads may connect their outgoing event data
 port to an enclosing process event data port (fan-in), this port is
 connected to an incoming process event data port, and this port is
 connected to multiple thread ports (fan-out). This results in
 semantic connections from all ultimate source threads to all
 ultimate destination threads. If the port connections from multiple
 threads are declared to a feature group in the enclosing process, a
 feature group connection to a second process, and port connections
 from the feature group of the second process to its contained
 threads, the result is a collection of pair wise semantic
 connections from the ultimate source threads to the ultimate
 destination threads.

  Data ports are restricted to 1-n connectivity, i.e., a data port
 can have multiple outgoing connections, but only one incoming
 connection per mode. Since data ports hold a single data state
 value, multiple incoming connections would result in multiple
 sources overwriting each other’s values in the destination port
 variable.

(5)  Data ports can be used to model aggregate data ports. If data ports
 are declared with a data component type that has its data
 subcomponents externally visible through provides data access
 declarations, then a separate connection can be declared to each of
 these elements in the port of the enclosing component. For example,
 several periodic threads can deliver their data port results to a
 data port of the enclosing process that represents the aggregate of
 those data values as elements of its data component type. The set
 of threads whose output is considered are those whose dispatch
 aligns. Once they have produced their output, the aggregate output
 is sent to the recipients. The time at which the send is initiated
 is the latest completion of the source threads, i.e.,
 max(t\ :sub:`complete`), where t\ :sub:`complete` represents the
 value of t at completion time. Similarly, the recipient thread may
 receive an element of the aggregate data port of its enclosing
 process, or a subset of the elements. The latter is modeled by the
 classifier of the recipient data port satisfying the Subtype or
 Subset matching rules of the Classifier\_Matching\_Rule property.

(6)  Feature groups may have multiple outgoing and incoming connections
 unless any ports that are elements of a feature group place
 additional restrictions. A destination feature group may be a
 subset of the source feature group. Acceptable matches are
 specified via the Classifier\_Matching\_Rule property with values
 Subtype or Subset.

(7)  If a component has an **in out** port, this port may be the
 destination of a connection from one component and the source of a
 connection to another component. This can be expressed by two
 directional port connections. Bidirectional flow between two
 components is represented by a bidirectional port connection
 between the **in out** ports of two components.

 1. .. rubric:: Connection Patterns for Component Arrays and Feature
   Arrays
   :name: connection-patterns-for-component-arrays-and-feature-arrays

(8)  The Connection\_Pattern property specifies how semantic connections
 are replicated if the ultimate source and ultimate destination are
 component arrays. The ultimate source or destination is a component
 array if it or any enclosing component involved in the connection
 is declared as subcomponent array. The dimensions of the ultimate
 source and destination array is the sum of dimensions of the
 ultimate source/destination component and those of any enclosing
 component involved in the semantic connection. The order of the
 dimensions is from the ultimate source/destination component up the
 containment hierarchy.

(9)  The ultimate source or ultimate destination of a semantic
 connection may be a feature array. In this case, the dimension of
 the feature array is treated as an additional dimension (the first
 dimension if multiple dimensions exist).

(10) The Connection\_Pattern property is a multi-valued list of
 dimension pattern values. A dimension pattern value is a list
 itself with one value for each of the dimensions of component
 arrays. The number of dimension pattern values must correspond to
 the larger dimensionality of the source or destination component
 array. Each value specifies the intended connectivity for one
 dimension of the array. The following connection patterns are
 predefined for an array dimension:

-  An All\_To\_All value indicates that each array element of the
   ultimate source has a semantic connection to each element in the
   ultimate destination (broadcast). This connection pattern applies
   even if the two arrays have different sizes.

-  A One\_To\_One value indicates that elements of the ultimate source
   array and the ultimate destination array have pair wise semantic
   connections (Identity). This property value applies if the two arrays
   have identical sizes. If one range is a subset of the other then only
   the subset starting with the first element is connected.

-  A Next or Previous value indicates that elements of the ultimate
   source array dimension are connected to the next (previous) element
   in the ultimate destination array dimension. The last element does
   not connect in the case of next and the first element does not
   connect in the case of previous. This property value applies if the
   two arrays have identical sizes. Note that a Next value for two
   dimensions results in a diagonal connection.

-  A Cyclic\_Next or Cyclic\_Previous value indicates that elements of
   the ultimate source array dimension are connected to the next
   (previous) element in the ultimate destination array dimension. In
   the case of Cyclic\_Next the last element in the array connects to
   the first, and vice versa for Cyclic\_Previous. This property value
   applies if the two arrays have identical sizes. Note that a Next
   value for two dimensions results in a diagonal connection.

-  A One\_to\_All value indicates that a single element of the ultimate
   source has a semantic connection to each element in the ultimate
   destination. This connection pattern is used when the destination
   array has a higher dimensionality than the source array. It specifies
   that any connection according to the other dimensions is being
   replicated for each element in this destination dimension, i.e., the
   outputs are broadcast in this dimension.

-  An All\_to\_One value indicates that each array element of the
   ultimate source has a semantic connection to a single element in the
   ultimate destination. This connection pattern is used when the
   destination array has a lower dimensionality than the source array.
   It specifies that any connection according to the other dimensions is
   being replicated for each element in this source dimension, i.e., the
   outputs of this dimension are connected to a single fan-in point.

-  Even\_to\_Even value indicates that each even array element of the
   ultimate source has a semantic connection to the same even element in
   the ultimate destination. This connection pattern is used for a one
   to one mapping for even indices.

-  Odd\_to\_Odd value indicates that each odd array element of the
   ultimate source has a semantic connection to the same odd element in
   the ultimate destination. This connection pattern is used for a one
   to one mapping for odd indices.

-  A Next\_Next or Previous\_Previous value indicates that elements of
   the ultimate source array dimension are connected to the element
   after the next (previous) element in the ultimate destination array
   dimension. The last two elements do not connect in the case of
   next\_next and the first two element does not connect in the case of
   previous\_previous. This property value applies if the two arrays
   have identical sizes.

-  A Cyclic\_Next\_Next or Cyclic\_Previous\_Previous value indicates
   that elements of the ultimate source array dimension are connected to
   the element after next (previous) element in the ultimate destination
   array dimension. In the case of Cyclic\_Next\_Next the last two
   elements in the array connect to the first two, and vice versa for
   Cyclic\_Previous\_Previous. This property value applies if the two
   arrays have identical sizes.

 A list of Connection\_Pattern values permits more complex patterns
to be defined. Figure 15 illustrates the use of connection patterns
in a two-dimensional array. In more complex patterns, the value of
((Next,One\_To\_One), (One\_To\_One,Next), (Previous,One\_To\_One),
(One\_To\_One,Previous)) indicates connections to all horizontal and
vertical neighbors, while ((Next,One\_To\_One), (One\_To\_One,Next),
(Previous,One\_To\_One), (One\_To\_One,Previous),
(Previous,Previous), (Next,Previous), (Previous,Next), (Next,Next))
includes diagonal neighbors as well.

|image15|

Figure − Connection Patterns in 2-Dimensional Component Array
 

 The Connection\_Set property specifies each semantic connection
between elements of the source component array and destination
component array individually. The property has a list of pairs of
source and destination array indices. A source or destination array
index consists of a list **aadlinteger** values, one for each array
dimension. The first index is the value 1. The values for the
Connection\_Set property may be generated by a tool based on pattern
specification that is an annex extension of the core AADL.

If both Connection\_Pattern values and Connection\_Set values are
specified, the set of semantic connections is the union of both.

Examples

-- The first sensor array in Figure 15

**device** sensor

**features**

Incoming: **in event data port**;

Outgoing: **out event data port;**

**end** sensor;

**system** sensornet

**end** sensornet;

**system implementation** sensornet.impl

**subcomponents**

sensorarray: **device** sensor [10][10];

**connections**

rows: **port** sensorarray.outgoing -> sensorarray.incoming

{ Connection\_Pattern => ((One\_To\_One, Next));};

**end** sensornet.impl;

Port Communication Timing
~~~~~~~~~~~~~~~~~~~~~~~~~

  The content of incoming data, event, or event data ports is frozen
 for the duration of a thread execution, i.e., the port variable
 content as it is accessible to the component application code is
 not affected by the arrival of new data, events or event data. By
 default the input is frozen at the time of the dispatch, i.e., at
 the time when t = 0. If the Input\_Time property is specified it is
 determined by the property value as specified in Section (14). Any
 input arriving after the input time becomes available at the next
 input time. This may be the input time for the next dispatch, or
 the next input time in the same dispatch, if multiple input time
 values are specified.

 The output is transferred to other components at completion time,
 i.e., c = c\ :sub:`complete`, or as specified by the value of the
 Output\_Time property (see Section (14)). Output may be sent
 multiple times during the same dispatch; this is specified by
 multiple output time values.

  Event and event data ports may trigger the dispatch of a Sporadic,
 Aperiodic, or Timed thread as specified in Section 5.4.2. The
 content of data, event, and event data ports is processed at the
 dispatch rate. In case of event and event data ports, the input is
 the queued set of items at Input\_Time; each arrived event or event
 data is only processed once, and items can be processed one per
 dispatch or in batches.

  Arrival of events on event ports can also trigger a mode switch if
 the event port is named in a mode transition originating in the
 current mode (see Section 12). Events that trigger mode transitions
 are not queued at event ports.

(5)  In case of incoming data ports, the input is the most recently
 arrived value at input time. This may be the same as the value at
 the previous dispatch. The dispatch rate determines the rate at
 which a data stream is sampled.

(6)  An incoming data stream may be sampled periodically by a periodic
 thread, or it may be sampled at the rate at which Aperiodic,
 Sporadic, Hybrid, and Timed threads are dispatched. In this case
 the sampling thread samples the data stream at its dispatch rate
 independent of the dispatch and completion of the source thread. If
 the incoming port is a data port the most recent value is
 available. If the incoming port is an event port or event data port
 the content of the port queue may be sampled.

(7)  The actual transfer of data to data ports as ultimate destination
 is affected by the Transmission\_Type property, which specifies
 whether the ultimate source or ultimate destination initiate the
 transfer, with the default being the ultimate source.

(8)  The actual transfer of event data and events is affected by the
 Fan\_Out\_Policy property of ports with multiple outgoing
 connection declarations (see Section 9.2.6).

 1. .. rubric:: Sampled, Immediate, and Delayed Data Port
   Communication
   :name: sampled-immediate-and-delayed-data-port-communication

(9)  The source of a data stream may be a data or event data port of a
 periodic thread or device. When this data stream is sampled by a
 periodic thread or device, sampling, oversampling, and
 undersampling may occur non-deterministically due concurrency and
 preemption. This can lead latency jitter in the data and
 instability in control system behavior. AADL supports deterministic
 sampling of data streams between a periodic source and destination
 thread by specifying immediate and delayed timing of port
 connections.

(10) Sampled data port communication occurs when a periodic destination
 thread or device with an incoming data port samples a data stream.
 In a sampling semantic connection the recipient samples the output
 of the sender at dispatch time or as specified by the *Input\_Time*
 property of the recipient port. Since this sampling occurs
 independent of the dispatch and completion of the source thread the
 data stream is sampled non-deterministically as illustrated in
 Figure 16. It shows two threads executing concurrently at 10 Hz and
 20 Hz on different processor cores. The output of the first
 dispatch of Thread 1 is received by the second dispatch of Thread
 2. The output of the second dispatch of Thread 1 is received by the
 fifth dispatch of Thread 2, since the fourth dispatch of T2 occurs
 before the second dispatch of T1 completes.

Figure − Sampling Data Port Connection
  

 Port connections can also be declared to be deterministic, i.e.,
they are declared to have an *immediate* or *delayed* Timing
property value. If the ultimate source and destination of a semantic
connection are periodic threads or devices and the ultimate
destination is a data port, then a semantic connection with an
immediate or delayed Timing property value imposes special
communication timing semantics.

In an immediate semantic connection the sender always communicates
with the receiver mid-frame, i.e., in the same dispatch frame. In
this case the receiver, when dispatched at the same time or at the
first dispatch after the sender dispatch, waits for the sender to
complete its execution. The scheduler must ensure that the execution
of the receiver is aligned with the completion of the sender.

 In a delayed semantic connections the sender always communicates
with the recipient phase-delayed, i.e., in the next dispatch frame
of the recipient after the deadline of the sender. In this case, the
send and receipt times are specified in terms of clock time, thus,
ensuring determinism. The communication mechanism takes care of
delaying the communication and the scheduler is unaware of this
delay.

 Deterministic sampling is ensured within a synchronization domain.
In the case of asynchronous systems, deterministic sampling is not
guaranteed unless appropriate protocols are provided by the runtime
system to ensure logically coordinated sampling that accommodates
time drift across synchronization domains.

(5) Immediate and delayed connection timing are illustrated in Figure
17. Thread 1 and Thread 2 are two periodic threads executing at a
rate of 10Hz, i.e., they are logically dispatched every 100 ms.
Immediate connection timing semantics are shown on the left of the
figure, and delayed are shown on the right of the figure.

(6) For immediate connection timing the actual start of execution of the
receiving thread (Thread 2) will be delayed if its dispatch occurs
at the same time or after the dispatch of the sending thread (Thread
1) and before execution completion of the sending thread. At the
actual start time the sending thread **out** port data value is
transferred into the **in** port of the receiving thread. For
example, if Thread 2 executes at twice the rate of Thread 1, then
the execution of Thread 2 will be delayed every time the two periods
align to receive the data at completion of Thread 1. Every other
time Thread 2 will start executing at its dispatch time with the old
value in its data port.

(7) For delayed connection timing, the data is not made available to the
recipient until the deadline of the source thread. The data is
available at the destination port at the next dispatch of the
destination thread that occurs at or after the source thread
deadline. If the source deadline and the destination dispatch occur
at the same logical time instant, the transmission is considered to
occur within the same time instant. The output of Thread 1 is made
available to Thread 2 at the beginning of its next dispatch. Thread
1 producing a new output at the next dispatch does not affect this
value.

Note: The data transfer of a delayed connection may be initiated at the
time of send, but the runtime system must ensure through double
buffering that the data is not moved to the in port variable for receipt
by the recipient until after the deadline of the sender.

Figure − Timing of Immediate & Delayed Data Connections
   

  If the connection declarations that comprise a semantic port
 connection have an explicit Timing property association, then the
 value must be the same. The property is only interpreted if the
 source and destination are periodic and the destination feature is
 a data port.

 For immediate data port connections data transfer occurs at
 completion time of the sender (c:sub:`source` =
 c\ :sub:`complete,source`) and the receiver execution start time is
 delayed until the sender completes (c:sub:`destination` = 0 ∧
 c\ :sub:`source`\ ≤c\ :sub:`complete,source`). Both the source and
 destination must complete their execution by the deadline of the
 destination, i.e., (c:sub:`source` = c\ :sub:`complete,source` ∧
 c\ :sub:`destination` = c\ :sub:`complete,destination` ∧
 t\ :sub:`destination` ≤ Deadline\ :sub:`destination`). This rule is
 transitive for sequences of immediate semantic connections. Note
 that the output may occur at a time before completion as specified
 by the Output\_Time property. In this case,
 c\ :sub:`complete,source` becomes c\ :sub:`output\_time,source`.

  For delayed data port connections data transmission is initiated at
 the deadline of the source component (t:sub:`source` =
 Deadline\ :sub:`source`, i.e., the output time of the source data
 port is Deadline\_Time). The input time of the receiving component
 port is the Dispatch\_Time, i.e., the data is received at the next
 dispatch of the receiving component following or equal to the
 source deadline.

  For immediate connections the Input\_Time is assumed to be start
 time with zero offset and any other specified time is ignored. The
 Output\_Time is assumed to be completion time or must be specified
 as a single output time value.

(5)  For delayed connections the Input\_Time is assumed to be dispatch
 time and Output\_Time is assumed to be deadline.

(6)  If multiple transmissions occur for a data port connection from the
 source thread before the dispatch of the destination thread, then
 only the most recently transmitted data is available in the
 destination port. In other words, the destination thread
 *undersamples* the transmitted data. In the case of two connected
 periodic threads, this situation occurs when the source thread has
 a shorter period than the destination thread. In the case of a
 periodic thread connected to an aperiodic thread, this situation
 occurs if the aperiodic thread receives two dispatch events with an
 inter-arrival time larger than the period of the source thread. In
 the case of an aperiodic thread connected to a periodic thread,
 this situation occurs if the aperiodic thread has two successive
 completion times less than the period of the periodic thread.

(7)  If no transmission occurs on an in data port between two dispatches
 of the destination thread, then the thread receives the same data
 again, resulting in *oversampling* of the transmitted data. A
 status indicator is accessible to the source text of the thread as
 part of the port variable to determine whether the data is fresh.
 This allows a receiving thread to determine whether a connection
 source is providing data at the expected rate or fails to provide
 data.

 1. .. rubric:: Semantic Port Connections and Port Queues
   :name: semantic-port-connections-and-port-queues

(8)  If the ultimate destination of a semantic port connection is an
 event port or event data port, then this port has by default a
 queue of size one. The size of this queue can be changed by
 explicitly with the Queue\_Size property.

(9)  Ports that are part of the sequence of connection declarations of a
 semantic connection can have a fan-out policy called of OnDemand.
 Such a fan-out policy results in queuing of data and events for
 retrieval by the ultimate destination (see Section 8.3.3).

(10) This means that the output from the ultimate source of a semantic
 connection is passed into the queue of the first port with an
 OnDemand fan-out policy and more than one outgoing connection; in
 the case of a single outgoing connection the output can be passed
 on without queuing.

(11) If the port of the ultimate destination of a semantic connection
 does not receive input from a semantic connection, then it services
 the queue of the last port in its connection declaration sequence
 with an OnDemand fan-out policy, more than one outgoing connection,
 and at least one semantic connection that provides input into its
 queue. If a port with an OnDemand fan-out policy only receives
 input from one port with an OnDemand fan-out policy, then that port
 is the port to be serviced.

(12) If the ultimate destination is a thread or device with multiple
 ports that can trigger a dispatch, then they are serviced according
 to the rules specified for the Urgency property in Section 8.3.3.

(13) More complex queue processing patterns can occur if a port with
 OnDemand fan-out policy and more than one outgoing connection has
 input from an ultimate source of a semantic connection and from
 another port with OnDemand fan-out, or from multiple ports with
 OnDemand fan-out, In the former case, the other port queue is only
 serviced if the original queue is empty. In the latter case one of
 those port queues can be chosen according to a fan-in
 prioritization of the port.

(14) If the ultimate destination of a semantic port connection is a mode
 transition, then the arrival of an event or event data at the port
 queue results in immediately passing of a mode transition trigger
 event to the mode transition, in addition to its queuing in the
 port queue for the purpose of ultimate destination dispatch and
 input (see also Section 12).

Processing Requirements and Permissions

 The temporal semantics for port connections define several cases in
which the transmission initiation and completion times are
identical. While it is not possible to perform a transmission
instantaneously in a actual system, a method of implementing systems
must supply a thread execution schedule that preserves the temporal
and logical semantics of the model. In some cases, this may result
in a system where the actual sending thread completion time occurs
before the logical departure time of the transmission. Similarly,
the actual receiving thread may begin its execution after the
logical arrival of the transmission. Such an execution model is
acceptable if the observed causal order is identical to that of the
logical semantic model and all timing requirements specified in all
property associations are satisfied.

For port connections between periodic threads, the standard
semantics and default property associations result in undersampling
when the period of the sending thread is less than the period of the
receiving thread. Oversampling occurs when the period of the sending
thread is greater than the period of the receiving thread. A method
of implementing systems is permitted to provide an optimization
which may eliminate any physical transfers of data that can be
guaranteed to be overwritten, or that can be guaranteed to be
identical to previously transferred values. Error-free transmission
may be assumed when performing such an optimization.

 A method of building systems must include a runtime capability in
every system to detect an erroneous or failed transmission over a
data connection between periodic threads to the degree of assurance
required by an application. A method of building systems must
include a runtime capability in every system to report a failure to
perform a transmission by a sending periodic thread to all connected
recipients. A method of building systems must include a runtime
capability in every system to detect data errors in arriving
transmissions over any connection to the degree of assurance
required by an application. The source language annex to this
standard specifies the application program interface used to obtain
error information about transmissions. A method of building systems
may define additional error semantics and error detection mechanisms
and associated application programming interfaces.

 Port connections can have shared data components as source. This
requires the runtime system to monitor write operations to the data
component. A method of building systems may choose to not support
this capability.

(5) Deterministic communication expressed by immediate and delayed
connections must be guaranteed by the method of implementation
within a synchronization domain. Even if the transmission is
initiated and completed by explicit send and receive service calls
in the source text of the sending and receiving thread, the send and
receive order of the two communicating threads must be assured. A
method of implementation may choose not to support immediate and
delayed connections across synchronization domains.

NOTE: All data values that arrive at the data ports of a receiving
thread are immediately transferred at the logical arrival time into the
storage resources associated with those features in the source text and
binary image associated with that thread. A consequence of the semantic
rules for data connections is that the logical arrival time of a data
value to a data port contained in a thread always occurs either when
that thread is dispatchable or during an interval of time between a
dispatch event and a delayed start of execution, e.g., due to an
immediate connection. That is, data values will never be transferred
into a thread’s data ports between the time it starts executing and the
time it completes executing and suspends awaiting a new dispatch unless
such an input time is specified through the Input\_Time property.

Arriving event and event data values may be queued in accordance with
the queuing rules defined in the port features section. A consequence of
the semantic rules for event and event data connections is that there
will be exactly one dispatch of a receiving thread for each arriving
event or event data value that is not lost due to queue overflow, and
event data values will never be transferred into a thread between the
time it starts executing and the time it completes and suspends awaiting
a new dispatch.

Parameter Connections
---------------------

 Parameter connections represent flow of data between the parameters
of a sequence of subprogram calls in a thread. Parameter connections
may be declared from an incoming data or event data port of the
containing thread to an incoming parameter of a subprogram call.
Parameter connections also specify connections from an incoming
parameter of the containing subprogram to an incoming parameter of a
subprogram call, from an outgoing parameter of a subprogram call to
an outgoing parameter of the containing subprogram, and from an
outgoing parameter of a subprogram call to an incoming parameter of
a subprogram call or to an outgoing data or event data port of the
containing thread. In addition, parameters may be connected to data
components (either to data subcomponents or data access features) to
represent data flowing to and from static and local variables. In
summary, the parameter connections follow the containment hierarchy
of subprogram calls nested in other subprograms. This is illustrated
in Figure 18. The call order is from left to right.

|image16|

Figure − Parameter Connections
  

 For parameter connections, data transfer occurs at the time of the
subprogram call and call return. In the case of subprogram calls to
remote subprograms, the data is first transferred to a local proxy
and from there passed to the remote subprogram.

Syntax

parameter\_connection ::=

**parameter** *source\_*\ parameter\_reference

directional\_connection\_symbol *destination\_*\ parameter\_reference

parameter\_connection\_refinement ::=

**parameter**

parameter\_reference ::=

-- parameter in the thread or subprogram type

*component\_type\_parameter*\ \_identifier [ .
*data\_subcomponent*\ \_identifier ]

\|

-- parameter in another subprogram call

*subprogram\_call*\ \_identifier **.** *parameter*\ \_identifier

\|

-- data or event data port in the thread type

-- or an element of that port’s data

*component\_type\_port*\ \_identifier [ .
*data\_subcomponent*\ \_identifier ]

\|

-- data subcomponent in the thread or subprogram

*data\_subcomponent*\ \_identifier

\|

-- requires data access in the thread or subprogram type

*requires\_data\_access*\ \_identifier

\|

-- data access element in a feature group of the component type

*component\_type\_feature\_group*\ \_identifier .
*element\_data\_access*\ \_identifier

\|

-- port or parameter element in a feature group of the component type

*component\_type\_feature\_group*\ \_identifier .
*element\_port\_or\_parameter*\ \_identifier

Naming Rules

1. The connection identifier in a parameter connection refinement
   declaration must refer to a named parameter or feature connection
   declared in an ancestor component implementation.

1. A source (destination) reference in a parameter connection
   declaration must reference a parameter of a preceding (succeeding)
   subprogram call, a parameter declared in the component type of the
   containing subprogram, a data port or event data port declared in the
   component type of the enclosing thread, a data port or event data
   port that is an element of a feature group in the component type of
   the enclosing thread, a data subcomponent, or a requires data access
   to a data component.

Legality Rules

1. The source of a parameter connection must be an incoming data or
   event data port of the containing thread, an incoming parameter
   of the containing subprogram, or a data subcomponent or requires
   data access with read-only or read-write access rights, or an
   outgoing parameter of a previous subprogram call.

1. The following source/destination pairs are acceptable for parameter
   connection declarations:

+--------------------------------------------------+-----------------------------------------------------+
| threadport -> call.parameter | requiresdataaccess -> call.parameter|
+==================================================+=====================================================+
| threadfeaturegroup.port -> call.parameter| featuregroup.requiresdataaccess -> call.parameter   |
+--------------------------------------------------+-----------------------------------------------------+
| call.parameter -> threadport | call.parameter -> requiresdataaccess|
+--------------------------------------------------+-----------------------------------------------------+
| call.parameter -> threadfeaturegroup.port| call.parameter -> featuregroup.requiresdataaccess   |
+--------------------------------------------------+-----------------------------------------------------+
| call.parameter -> threadincompletefeaturegroup   | call.parameter -> datasubcomponent  |
+--------------------------------------------------+-----------------------------------------------------+
| containedcall.parameter -> parameter | datasubcomponent -> call.parameter  |
+--------------------------------------------------+-----------------------------------------------------+
| parameter -> containedcall.parameter | call.parameter -> call.parameter|
+--------------------------------------------------+-----------------------------------------------------+

1. A parameter cannot be the destination feature reference of more than
   one parameter connection declaration unless the source feature
   reference of each parameter connection declaration is contained
   in a different mode. In this case, the restriction applies for
   each mode.

2. The data classifier of the source and destination must match. The
   matching rules as specified by the Classifier\_Matching\_Rule
   property apply (see Section 9.2 (L13)). By default the data
   classifiers must be match.

Semantics

 Parameter connections represent sequential flow of data through
subprogram parameters in a sequence of subprogram calls being
executed by a thread.

Parameter connections are restricted to 1-n connectivity, i.e., a
data port or parameter can have multiple outgoing connections, but
only one incoming connection.

 If a subprogram has an **in out** parameter, this parameter may be
the destination of an incoming parameter connection and the source
of outgoing parameter connections.

 Parameter connections follow the call sequence order of subprogram
calls. In other words, parameter connection sources must be
preceding subprogram calls, and parameter connection destinations
must be successor subprogram calls.

(5) The optional in\_modes\_and\_transitions subclause specifies what
modes the parameter connection is part of. The detailed semantics of
this subclause are defined in Section 12.

Examples

NOTE: An example of parameter connections can be found in Section 5.2.

Access Connections
------------------

 Access connections represent access to shared data components by
concurrently executing threads or by subprograms executing within
thread. They also denote communication between processors, memory,
and devices by accessing a shared bus. Finally, they represent a
subprogram call to a specific instance of a subprogram or subprogram
group. If the subprogram or subprogram group resides in a different
thread, then the call is treated as a remote call. Access
connections for subprograms and subprogram groups are bidirectional.
Data, virtual bus, and bus access connections may be bidirectional
or directional. If they are directional they indicate write or read
access to the data component or bus component.

A *semantic access connection* is defined by a sequence of one or
more individual access connection declarations that follow the
component containment hierarchy from an *ultimate source* to an
*ultimate destination*.

 In the case of bidirectional connections either the subcomponent
being accessed or the feature that requires access can be the
ultimate source or destination. In the case of directional data
access connections the ultimate source is the source of the data
flow, e.g., the data component in the case of a read access and the
ultimate destination in the case of a write access. In the case of
partial AADL models, the ultimate source or destination may be a
provides access feature of a component without subcomponents.

 Figure 19 illustrates a semantic data connection from the data
component D to thread Q. The access connection is bidirectional,
thus, the data component could be the ultimate destination as well.

Figure − Semantic Access Connection For Data Components
   

 The flow of data of a semantic data, virtual bus, or bus access
connection can also be indicated by the Access\_Right property on
the shared component or the access feature. In the case of a
directional access connection, the connection direction must be
consistent with the access rights.

Syntax

access\_connection ::=

[ **bus** \| **virtual bus** \| **subprogram** \| **subprogram group**
\| **data** ] **access**

*source*\ \_access\_reference connection\_symbol
*destination\_*\ access\_reference

access\_connection\_refinement ::=

[ **bus** \| **virtual bus** \| **subprogram** \| **subprogram group**
\| **data** ] **access**

access\_reference ::=

-- requires or provides access feature in the component type

*requires\_access*\ \_identifier \| *provides\_access\_*\ identifier

\|

-- requires or provides access feature in a feature group of the
component type

*feature\_*\ group\_identifier **.** *requires\_access*\ \_identifier

\|

*feature\_*\ group\_identifier **.** *provides\_access*\ \_identifier

\|

-- provides or requires access in a subcomponent

*subcomponent*\ \_identifier **.** *provides\_access*\ \_identifier

\|

*subcomponent*\ \_identifier **.** *requires\_access*\ \_identifier

\|

*data\_subcomponent*\ \_identifier **.**
*data\_subcomponent*\ \_identifier

\|

*subprogram\_call*\ \_identifier **.**
*requires\_or\_provides\_access*\ \_identifier

-- data, subprogram, subprogram group or bus being accessed

*data\_subprogram\_subprogram\_group\_or\_bus\_subcomponent*\ \_identifier

\|

-- subprogram a processor being accessed

[ **processor .** ] *provides\_subprogram\_access*\ \_identifier

Naming Rules

1. The connection identifier in an access connection refinement
   declaration must refer to a named access or feature connection
   declared in an ancestor component implementation.

1. An access reference in an access connection declaration must
   reference an access feature of a subcomponent, subprogram call, or
   **processor**, an access feature in the component type of the
   containing component, an access feature in a feature group of the
   containing component type, or a data, bus, virtual bus, subprogram,
   or subprogram group subcomponent.

Legality Rules

1. The category of the source and the destination of a access connection
   declaration must be the same, i.e., they must both be **data**,
   **bus**, **virtual bus**, **subprogram**, or **subprogram
   group**, or their respective access feature.

1. In the case of a bidirectional semantic access connection either
   connection end can be the source.

2. In the case of a directional data, virtual bus, or bus access
   connection the connection end representing the component being
   accessed must be the source for read access, and the destination
   for write access, i.e., the direction declared for the access
   connection must be compatible with the direction of the data flow
   for reading or writing based on the access rights, if specified.

3. In a partial AADL model the ultimate source or destination may be a
   provides access feature of a component instead of the
   subcomponent.

The **provides** and **requires** indicators of source and destination
access features must satisfy the following rules:

1. If the access connection declaration represents an access connection
   between access features of sibling components, then the source
   must be a **provides** access, and the destination must be a
   **requires** access, or vice versa.

2. If the access connection declaration represents a feature mapping up
   the containment hierarchy, then one connection end must be a
   **provides** access of a subcomponent, or a data, subprogram, or
   bus subcomponent; and the other connection end must be a
   **provides** access feature of the enclosing component or a
   **provides** feature of a feature group of the enclosing
   component.

3. If the access connection declaration represents a feature mapping
   down the containment hierarchy, then one connection end must be a
   **requires** access of the enclosing component, a **requires**
   access of a feature group of the enclosing component, or a data,
   subprogram, virtual bus, or bus subcomponent; and the other
   connection end must be a **requires** access of a subcomponent.

4. An access connection must not lead to more than one virtual bus, bus,
   data, or subprogram subcomponent unless the connection is mode
   specific. In this case, the restriction applies for each mode.

5. For access connections the classifier of the provider access must
   match to the classifier of the requires access according to the
   Classifier\_Matching\_Rules property. By default the classifiers
   must be the same (see Section 9.1).

6. If more than one access feature in a semantic access connection has
   an Access\_Right property association, then the resulting
   property values must be compatible. This means that the provider
   must provide read-only or read-write access if the requirer
   specifies read-only. Similarly, the provider must provide
   write-only or read-write access if the requirer specifies
   write-only. The provider must provide read-write access if the
   requirer specifies read-write. Finally, the provider must provide
   by-method access if the requirer specifies by-method access.

7. The category of the access connection source and destination must be
   identical. If the component category is specified as part of the
   connection declaration, then it must be identical to that of the
   source and destination. **Bus**, **virtual bus**, **data**,
   **subprogram**, and **subprogram group** are acceptable
   categories.

Standard Properties

Connection Pattern: **list of list of** Supported Connection Patterns

Connection Set: **list of** Connection Pair

Semantics

 A data access connection represents access to a shared data
component by concurrently executing threads or by subprograms
executing within thread. A subprogram access or subprogram group
access connection represents access to subprogram code or a library
that calls can be made to. The call may be a remote call. A bus
access connection represents communication between processors,
memory, and devices by accessing a shared bus.

Access connections are restricted to 1-n connectivity, i.e., a data,
subprogram, or bus component can have multiple outgoing access
connections, but a **requires** access feature can only have one
connection. The restriction of one connection applies to each mode,
i.e., different incoming access connections may exist in different
modes, but not in the same mode.

 The optional in\_modes subclause specifies what modes the access
connection is part of. The detailed semantics of this subclause are
defined in Section 12.

 The actual data flow through data, virtual bus, and bus access
connections is determined by the value of the Access\_Right property
on the shared data or bus component and their provides and requires
access declarations. Read means flow of data from the shared
component to the component requiring access, and write means flow of
data from the component requiring access to the shared component.
The desired data flow can also be indicated by specifying the
direction in the access connection declaration.

(5) The connection patterns for component arrays described in Section
9.2.3 apply to access connections as well.

1. .. rubric:: Feature Group Connections
  :name: feature-group-connections

 Feature group connections represent a collection of connections
between groups of features of different components.

Syntax

-- connection between feature groups of two subcomponents or between

-- a feature group of a subcomponent and a feature group in the
component type

feature\_group\_connection ::=

**feature group** *source*\ \_feature\_group\_reference

connection\_symbol *destination*\ \_feature\_group\_reference

-- A feature group refinement can only add properties

-- The source and destination of the connection does not have to be
repeated

feature\_group\_connection\_refinement ::=

**feature group**

feature\_group\_reference ::=

-- feature group in the component type

*component\_type\_feature\_group*\ \_identifier

\|

-- feature group in a subcomponent

*subcomponent*\ \_identifier **.** *feature\_group*\ \_identifier {
**.** *nested\_feature\_*\ identifier }\*

\|

-- feature group element in a feature group of the component type

*component\_type\_feature\_group*\ \_identifier .

*element\_feature\_group\_*\ identifier

Naming Rules

1. The connection identifier in a feature group connection refinement
   declaration must refer to a feature group named connection declared
   in an ancestor component implementation.

1. A source or destination reference in a feature group connection
   declaration must reference a feature group declared in the component
   type, a feature group of one of the subcomponents, or feature group
   that is an element of a feature group in the component type. The
   subcomponent reference may also consist of a reference on a
   subcomponent array.

Legality Rules

1. If the feature group connection declaration represents a component
   connection between sibling components, the feature group types
   must be complements. This may be indicated by both feature group
   declarations referring to the same feature group type and one
   feature group declared as **inverse of**, by the feature group
   type of one feature group being declared as the **inverse of**
   the feature group type of the other feature group, or by the two
   referenced feature group types meeting the complement
   requirements as defined in Section 8.2.

1. The Classifier\_Matching\_Rule property specifies the rule to be
   applied to match the feature group classifier of a connection
   source to that of a connection destination.

2. The following rules are supported for feature group connection
   declarations that represent a connection up or down the
   containment hierarchy:

-  Classifier\_Match: The source feature group type must be identical to
   the feature group type of the destination. This is the default rule.

-  Equivalence: An indication that the two classifiers of a connection
   are considered to match if they are listed in the
   Supported\_Classifier\_Equivalence\_Matches property. Matching
   feature group types are specified by the
   Supported\_Classifier\_Equivalence\_Matches property with pairs of
   classifier values representing acceptable matches. Either element of
   the pair can be the source or destination classifier. Equivalence is
   intended to be used when the feature group types are considered to be
   identical, i.e., their elements match. The
   Supported\_Classifier\_Equivalence\_Matches property is declared
   globally as a property constant.

-  Subset: An indication that the two classifiers of a connection are
   considered to match if the inner feature group has outcoming features
   that are a subset of outgoing features of the outer feature group,
   and if the inner feature group has incoming features that are a
   subset of incoming features of the outer feature group. The pairs of
   features are expected to have the same name.

1. The following rules are supported for feature group connection
   declarations that represent a connection between two
   subcomponents, i.e., sibling component:

-  Classifier\_Match: The source feature group type must be the
   complement of the feature group type of the destination. This is the
   default rule.

-  Equivalence: An indication that the two classifiers of a connection
   are considered to match with opposite direction if they are listed in
   the Supported\_Classifier\_Equivalence\_Matches property. Matching
   feature group types are specified by the
   Supported\_Classifier\_Equivalence\_Matches property with pairs of
   classifier values representing acceptable matches. Either element of
   the pair can be the source or destination classifier. Equivalence is
   intended to be used when the feature group types are considered to be
   identical, i.e., their elements match with opposing direction. The
   Supported\_Classifier\_Equivalence\_Matches property is declared
   globally as a property constant.

-  Subset: An indication that the two classifiers of a connection are
   considered to match if each has incoming features that are a subset
   of outgoing features of the other. The pairs of features are expected
   to have the same name.

A feature group may have a direction declared; otherwise it is
considered bidirectional. The direction declared for the destination
feature group of a feature group connection declaration must be
compatible with the direction declared for the source feature group as
defined by the following rules:

1. If the feature group connection declaration represents a connection
   between feature groups of sibling components, then the source
   must be an outgoing feature group and the destination must be an
   incoming feature group.

2. If the feature group connection declaration represents a connection
   between feature groups up the containment hierarchy, then the
   source and destination must both be an outgoing feature group.

3. If the feature group connection declaration represents a connection
   between feature groups down the containment hierarchy, then the
   source and destination must both be an incoming feature group.

4. A feature group connection must be bidirectional or constrains the
   direction of the flow from the source and destination feature.

Standard Properties

Connection Pattern: **list of list of** Supported Connection Patterns

Connection Set: **list of** Connection Pair

Transmission Type: **enumeration** ( push, pull )

Allowed Connection Binding Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

Allowed Connection Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, bus, virtual bus, device, memory, system)

Not Collocated: **record** (

Targets: **list** **of** **reference** (data, thread, process, system,
connection);

Location: **classifier** ( processor, memory, bus, system ); )

Actual \_Connection\_Binding: **inherit list of** **reference**
(processor, virtual processor, bus, virtual bus, device, system, memory)

Semantics

 These connections represent a collection of *semantic connections*
between the individual features in each group. A semantic feature
connection is determined by a sequence of one or more individual
connection declarations that follow the component containment
hierarchy in a fully instantiated system from an *ultimate source*
to an *ultimate destination*. As the connection declarations follow
the component containment hierarchy they may involve the aggregation
of features into feature groups up the hierarchy and decomposition
into individual features down the hierarchy.

 Feature groups and feature group connections may impose a
restriction on the direction of the collection of features and
connections they represent.

Processing Requirements and Permissions

 When an AADL model with a thread architecture and fully detailed
feature groups is instantiated, each semantic connection of feature
group elements is included in the instance model. If the feature
groups of such a model are incomplete a connection instance between
the feature group instances is created. If the AADL model is not
fully detailed to the thread level, connection instances are created
between the leaf components in the system hierarchy.

Examples

**package** Example3

**public**

-- A simple example showing a system with two processes and threads.

**data** Alpha\_Type

**properties**

Data\_Size => 256 Bytes;

**end** Alpha\_Type;

**feature group** xfer\_plug

**features**

 Alpha : **out data port** Alpha\_Type;

Beta : **in data port** Alpha\_Type;

**end** xfer\_plug;

**feature group** xfer\_socket

**inverse of** xfer\_plug

**end** xfer\_socket;

**thread** P

**features**

Data\_Source : **out data port** Alpha\_Type;

**end** P;

**thread implementation** P.Impl

**properties**

Dispatch\_Protocol=>Periodic;

Period=> 10 ms;

**end** P.Impl;

**process** A

**features**

Produce : **feature group** xfer\_plug;

**end** A;

**process implementation** A.Impl

**subcomponents**

Producer : **thread** P.Impl;

Result\_Consumer : thread Q.Impl;

**connections**

daconn: **port** Producer.Data\_Source -> Produce.Alpha;

bdconn: **port** Produce.Beta -> Result\_Consumer.Data\_Sink;

**end** A.Impl;

**thread** Q

**features**

Data\_Sink : **in data port** Alpha\_Type;

**end** Q;

**thread implementation** Q.Impl

**properties**

Dispatch\_Protocol=>Periodic;

Period=> 10 ms;

**end** Q.Impl;

**process** B

**features**

Consume : **feature group** xfer\_socket;

**end** B;

**process implementation** B.Impl

**subcomponents**

Consumer : **thread** Q.Impl;

Result\_Producer : **thread** P.Impl;

**connections**

adconn: **port** Consume.Alpha -> Consumer.Data\_Sink;

dcconn: **port** Result\_Producer.Data\_Source -> Consume.Beta;

**end** B.Impl;

**system** Simple

**end** Simple;

**system implementation** Simple.Impl

**subcomponents**

pr\_A : process A.Impl;

pr\_B : process B.Impl;

**connections**

fgconn: **feature group** pr\_A.Produce <-> pr\_B.Consume;

**end** Simple.Impl;

**end** Example3;

Flows
=====

 A *flow* is a logical flow of data and control through a sequence of
threads, processors, devices, and port connections or data access
connections. A component can have a flow specification, which
specifies whether a component is a flow source, i.e., the flow
starts within the component, a flow sink, i.e., the flow ends within
the component, or there exists a flow path through the component,
i.e., from one of its incoming ports to one of its outgoing ports.

The purpose of providing the capability of specifying end-to-end
flows is to support various forms of flow analysis, such as
end-to-end timing and latency, reliability, numerical error
propagation, Quality of Service (QoS) and resource management based
on operational flows. To support such analyses, relevant properties
are provided for the end-to-end flow, the flow specifications of
components, and the ports involved in the flow to be analyzed. For
example, to deal with end-to-end latency the end-to-end flow may
have properties specifying its expected maximum latency and actual
latency. In addition, ports on individual components may have flow
specific properties, e.g., an **in** port property specifies the
expected latency of data relative to its sensor sampling time or in
terms of end-to-end latency from sensor to actuator to reflect the
latency assumption embedded in its extrapolation algorithm.

 Flows are represented by flow specification, flow implementation,
and end-to-end flow declarations.

A flow specification declaration in a component type specifies an
externally visible flow through a component’s ports, feature groups,
parameters, or data access features. The flow through a component is
called a *flow path*. A flow originating in a component is called
the *flow source*. A flow ending in a component is called the *flow
sink*.

 A flow implementation declaration in a component implementation
specifies how a flow specification is realized in the implementation
as a sequence of flows through subcomponents along connections from
the flow specification origin to the flow specification destination.
This is illustrated in Figure 20. The system type S1 is declared
with three ports and two flow specifications. These are the flows
through system S1 that are externally visible. In the example, both
flows are flow paths, i.e., they flow through the system. The ports
identified by the flow specification do not have to have the same
data type, nor do they have to be the same port type, i.e., one can
be an event port and the other an event data port. This allows flow
specifications to be used to describe logical flows of information.

 The system implementation for system S1 is shown on the right of
Figure 20. It contains two process subcomponents P1 and P2. Each has
two ports and a flow path specification as part of its process type
declaration. The flow implementation of flow path F1 is shown in
both graphical and textual form. It starts with port pt1, as
specified in the flow specification. It then follows a sequence of
connections and subcomponent flow specifications, modeled in the
figure as the sequence of connection C1, subcomponent flow
specification P2.F5, connection C3, subcomponent flow specification
P1.F7, connection C5. The flow implementation ends with port pt2, as
specified in the flow specification for F1.

|image17|

Figure − Flow Specification & Flow Implementation
 

 An end-to-end flow is a logical flow through a sequence of system
components, i.e., threads, devices and processors. An end-to-end
flow is specified by an end-to-end flow declaration. End-to-end flow
declarations are declared in component implementations, typically
the component implementation in the system hierarchy that is the
root of all threads, data components, processors, and devices
involved in an end-to-end flow. The subcomponent identified by the
first subcomponent flow specification referenced in the end-to-end
flow declaration contains the system component that is the starting
point of the end-to-end flow. Succeeding named subcomponent flow
specifications contain additional system components. In the example
shown in Figure 20, the flow specification F7 of process P1 may have
a flow implementation that includes flows through two threads which
is not included in this view of the model.

1. .. rubric:: Flow Specifications
  :name: flow-specifications

 A flow specification declaration indicates that information
logically flows from one of its incoming ports, parameters, or
feature groups to one of its outgoing ports, parameters, or feature
groups, or to and from data components via data access. The ports
can be event, event data, or data ports. A flow may start within the
component, called a *flow source*. A flow may end within the
component, called a *flow sink*. Or a flow may go through a
component from one of its **in** or **in out** ports or parameters
to one of its **out** or **in out** ports or parameters, called a
*flow path*. A flow may also follow read or write access to data
components. In the case of feature groups, there is a flow from a
feature group to its inverse.

 Multiple flow specifications can be defined involving the same
features. For example, data coming in through an **in** feature
group is processed and derived data from one of the feature group’s
contained ports is sent out through different **out** ports.

Syntax

flow\_spec ::=

flow\_source\_spec

\| flow\_sink\_spec

\| flow\_path\_spec

flow\_spec\_refinement ::=

flow\_source\_spec\_refinement

\| flow\_sink\_spec\_refinement

\| flow\_path\_spec\_refinement

flow\_source\_spec ::=

*defining\_flow\_*\ identifier **:** **flow source**
*out\_*\ flow\_feature\ *\_*\ identifier

[ **{** { property\_association }\ :sup:`+` **}** ]

[ in\_modes ] **;**

flow\_sink\_spec ::=

*defining\_flow\_*\ identifier **:** **flow sink**
*in\_*\ flow\_feature\_identifier

[ **{** { property\_association }\ :sup:`+` **}** ]

[ in\_modes ] **;**

flow\_path\_spec ::=

*defining\_flow\_*\ identifier **:** **flow path**
*in*\ \_flow\_feature\_identifier **->**

*out*\ \_flow\_feature\_identifier

[ **{** { property\_association }\ :sup:`+` **}** ]

[ in\_modes ] **;**

flow\_source\_spec\_refinement ::=

*defining\_flow\_*\ identifier **:**

**refined to flow source**

( ( **{** { property\_association }\ :sup:`+` **}** [
in\_modes\_and\_transitions ] ) \|

in\_modes\_and\_transitions ) **;**

flow\_sink\_spec\_refinement ::=

*defining\_flow\_*\ identifier **:**

**refined to flow sink**

( ( **{** { property\_association }\ :sup:`+` **}** [
in\_modes\_and\_Transitions ] ) \|

in\_modes\_and\_transitions ) **;**

flow\_path\_spec\_refinement ::=

*defining\_flow\_*\ identifier **:**

**refined to flow path **

( ( **{** { property\_association }\ :sup:`+` **}** [
in\_modes\_and\_transitions ] ) \|

in\_modes\_and\_transitions ) **;**

flow\_feature\_identifier ::=

*feature*\ \_identifier

\| *feature\_group*\ \_identifier

\| *feature\_group*\ \_identifier **.** *feature*\ \_identifier

\| *feature\_group*\ \_identifier **.** *feature\_group*\ \_identifier

\| subprogram\_access\_identifier . parameter\_identifier

Naming Rules

1. The defining flow identifier of a flow specification must be unique
   within the interface name space of the component type.

1. The flow feature identifier in a flow path must refer to a port,
   parameter, data access feature, or feature group in the component
   type, or to a port, data access feature, or feature group contained
   in a feature group in the component type, or to a parameter of a
   subprogram access feature.

2. The defining flow identifier of a flow specification refinement must
   refer to a flow specification or refinement in an ancestor component
   type.

Legality Rules

1. The direction declared for the *out\_flow* of a flow path
   specification declaration must be compatible with the direction
   declared for the *in\_flow* as defined by the following rules:

1.  If the *in\_flow* is a port or parameter, its direction must be must
be an **in** or an **in out**.

2.  If the *out\_flow* is a port or parameter, its direction must be an
**out** or an **in** **out**.

3.  If the *in\_flow* is a data access, its access right must be must be
Read\_Only or Read\_Write.

4.  If the *out\_flow* is a data access, its direction must be
Write\_Only or Read\_Write.

5.  If the *in\_flow* is a feature group then it must contain at least
one port or data access that satisfies the above rule, or it
must have an empty set of ports.

6.  If the *out\_flow* is a feature group then it must contain at least
one port or data access that satisfies the above rule, or it
must have an empty set of ports.

7.  The direction declared for the out flow of a flow source
specification declaration must be the same as for out flows in
flow paths.

8.  The direction declared for the *in\_flow* of a flow sink
specification declaration must be the same as for *in\_flow* of
flow paths.

9.  If the flow specification refers to a feature group then the feature
group must contain at least one port, data access, or parameter
that satisfies the above rules, or the feature group must have
an empty list of features.

10. If the *out\_flow* is a subprogram access feature parameter then its
direction must be **out** or **in out.**

11. If the *in\_flow* is a subprogram access feature parameter then its
direction must be **in** or **in out.**

Standard Properties

Latency: Time Range

NOTE: These properties are examples of properties for latency and
throughput analysis. Additional properties are also necessary on ports
to fully support throughput analysis, such as arrival rate and data
size. Appropriate properties for flow analysis may be defined by the
tool vendor or user (see Section 11).

Semantics

 A flow specification declaration represents a logical flow
originating from within a component, flowing through a component, or
ending within a component.

In case of a flow through a component, the component may transform
the input into a different form for output. In case of data or event
data port, the data type may change. Similarly the flow path may be
between different port types, e.g., an event port and a data port,
and between ports, parameters, data access features, and feature
groups. This permits end-to-end flows to be specified as logical
information flows through a system despite the fact that the
information is being manipulated and its representation changed.

Examples

**package** gps

**public**

**data** signal\_data

**end** signal\_data;

**data** commands

**end** commands ;

**data** position

**end** position;

**data** implementation position.radial

**end** position.radial;

**data** implementation position.cartesian

**end** position.cartesian;

**end** gps;

**package** FlowExample

**public**

**with** GPSLib;

**process** foo

**features**

Initcmd: **in event port**;

Signal: **in data port** GPSLib::signal\_data;

Result1: **out data port** GPSLib::position.radial;

Result2: **out data port** GPSLib::position.cartesian;

Status: **out event port**;

**flows**

-- two flows split from the same input

Flow1: **flow path** signal -> result1;

Flow2: **flow path** signal -> result2;

-- An input is consumed by process foo through its initcmd port

Flow3: **flow sink** initcmd;

-- An output is generated (produced) by process foo and made available

-- through its port Status;

Flow4: **flow source** Status;

**end** foo;

**thread** bar

**features**

p1 : **in data port** GPSLib::signal\_data;

p2 : **out data port** GPSLib::position.radial;

p3 : **out event port**;

reset : **in** **event** **port**;

**flows**

fs1 : **flow path** p1 -> p2;

fs2 : **flow source** p3;

**end** bar;

**thread implementation** bar.basic

**end** bar.basic;

**thread** baz

**features**

p1 : **in data port** GPSLib::position.radial;

p2 : **out data port** GPSLib::position.cartesian;

reset : **in event port**;

**flows**

fs1 : **flow path** p1 -> p2;

fsink : **flow sink** reset;

**end** baz;

**thread implementation** baz.basic

**end** baz.basic;

**end** FlowExample;

Flow Implementations
--------------------

 Component implementations must provide an implementation for each
flow specification. A flow implementation declaration identifies the
flow through its subcomponents. In case of a flow source
specification, it starts from the flow source of a subcomponent or
from the component implementation itself and ends with the port
named in the flow source specification. In case of a flow sink
specification, the flow implementation starts with the port named in
the flow sink specification declaration and ends within the
component implementation itself or with the flow sink of a
subcomponent. In case of a flow path specification, the flow
implementation starts with the *in\_flow* port and ends with the
*out\_flow* port. Flow characteristics modeled by properties on the
flow implementation are constrained by the property values in the
flow specification. Flow implementations can be declared to be
mode-specific.

Syntax

flow\_implementation ::=

( flow\_source\_implementation

\| flow\_sink\_implementation

\| flow\_path\_implementation )

[ in\_modes\_and\_transitions ] **;**

flow\_source\_implementation ::=

*flow*\ \_identifier : **flow source**

{ subcomponent\_flow\_identifier **->** *connection*\ \_identifier
**->** }\ :sup:`\*`

*out\_*\ flow\_feature\_identifier

flow\_sink\_implementation ::=

*flow*\ \_identifier : **flow sink**

*in\_*\ flow\_feature\_identifier

{ **->** *connection*\ \_identifier **->**
subcomponent\_flow\_identifier }\ :sup:`\*`

flow\_path\_implementation ::=

*flow*\ \_identifier : **flow path**

*in*\ \_flow\_feature\_identifier

[ { **->** *connection*\ \_identifier **->**
subcomponent\_flow\_identifier }\ :sup:`+`

**->** *connection*\ \_identifier ]

**->** *out*\ \_flow\_feature\_identifier

subcomponent\_flow\_identifier ::=

( *subcomponent*\ \_identifier [ **.** *flow\_spec*\ \_identifier ] )

\| data\_component\_reference

data\_component\_reference ::=

*data\_subcomponent\_*\ identifier \|
*requires\_data\_access*\ \_identifier

\| *provides\_data\_access*\ \_identifier

Naming Rules

1. The flow identifier of a flow implementation must name a flow
   specification in the component type. A flow implementation name may
   appear more than once in each component implementation, either as
   alternative flows under different modes or transitions (using **in
   modes**), or to represent multiple flows for the same flow
   specification such as replicated flows to support redundancy or
   different flows for different elements of feature groups.

1. The *in\_flow* and *out\_flow* feature identifier in a flow
   implementation must refer to the same *in\_flow* and *out\_flow*
   feature as the flow specification it implements.

2. The subcomponent flow identifier of a flow implementation must name a
   subcomponent and optionally a flow specification in the component
   type of the named subcomponent, or it must name a data component in
   the form of a data subcomponent, provides data access, or requires
   data access.

3. In case of a flow source implementation the flow identifier of the
   first subcomponent must refer to a flow source or a data component.

4. In case of a flow sink implementation the flow identifier of the last
   subcomponent must refer to a flow sink or a data component.

5. In all other cases the subcomponent flow identifier must refer to a
   flow path or a data component.

6. The connection identifier in a flow implementation must refer to a
   connection in the component implementation.

Legality Rules

1. The source of a connection named in a flow implementation declaration
   must be the same as the *in\_flow* feature of the flow
   implementation, or as the out flow feature of the directly
   preceding subcomponent flow specification, if present.

1. The destination of a connection named in a flow implementation
   declaration must be the same as the out flow feature of the flow
   implementation, or as the in\_flow feature of the directly
   succeeding subcomponent flow specification, if present.

2. The *in\_flow* feature of a flow implementation must be identical to
   the *in\_flow* feature of the corresponding flow specification.
   The *out\_flow* feature of a flow implementation must be
   identical to the *out\_flow* feature of the corresponding flow
   specification.

3. If the component implementation provides mode-specific flow
   implementations, as indicated by the **in** **modes** statement,
   then the set of modes in the **in** **modes** statement of all
   flow implementations for a given flow specification must include
   all the modes for which the flow specification is declared.

4. In case of a mode-specific flow implementation, the connections and
   the subcomponents named in the flow implementation must be
   declared at least for the modes listed in the **in** **modes**
   statement of the flow implementation.

5. Flow implementations may be declared from an *in\_flow* feature
   directly to an *out\_flow* feature.

6. Component type extensions may refine flow specifications and
   component implementation extensions may refine subcomponents and
   connections with **in** **modes** statements. A flow
   implementation that is inherited by the extension must be
   consistent with the modes of the refined flow specifications,
   subcomponents, and connections if named in the flow
   implementation according to rules (L4) and (L5). Otherwise, the
   flow implementation has to be defined again in the component
   implementation extension and satisfy rules (L4) and (L5).

7. Component implementation extensions may declare flow implementations
   for flow specifications that were already declared in the
   component implementation being extended.

Consistency Rules

1. If the component implementation has subcomponents, then a flow
   implementation declaration is required for each flow specification
   for a fully refined flow.

Semantics

 A flow implementation declaration represents the realization of a
flow specification in the given component implementation. Different
mode-specific flow implementations may be declared for the same flow
specification.

A flow path implementation starts with the port or data access named
in the corresponding flow specification, passes through zero or more
subcomponents, and ends with the port or data access named in the
corresponding flow specification (see Figure 20). A flow source
implementation ends with the port or data access named in the
corresponding flow specification. A flow sink implementation starts
with the port or data access named in the corresponding flow
specification. A flow path implementation may specify a flow that
goes directly from an *in\_flow* feature to an *out\_flow* feature
without any connections in between.

 A flow implementation may refer to subcomponents without identifying
a flow specification in the subcomponent. In this case, (a) an
unnamed flow specification is assumed to exist from the destination
port of the preceding connection to the source port of the
succeeding connection if no flow specification is declared for the
subcomponent, (b) it represents a separate flow for each of the flow
specifications of the subcomponent.

 A flow implementation within a thread may be modeled as flow through
subprogram calls via their parameters. If the call is to a
subprogram via a subprogram access feature, the flow goes to the
subprogram instance being called. This may be a subprogram in a
different thread that is called remotely.

(5) A flow through a component may transform the input into a different
form for output. In case of data, event data port, or data access,
the data type may change. Similarly the flow path may be between
different port types, between ports and feature groups, and between
data access and ports or feature groups. This permits end-to-end
flows to be specified as logical information flows through a system
despite the fact that the information is being manipulated and its
representation changed.

(6) The optional in\_modes\_and\_transitions subclause specifies what
modes the flow implementation is part of. The detailed semantics of
this subclause are defined in Section 12.

(7) Flow specifications can have component implementation specific and
mode specific property values. The respective property associations
can be declared in the properties subclause of the component
implementation declaration.

Examples

-- This is an AADL fragement

-- process foo is declared in the previous section

**process implementation** foo.basic

**subcomponents**

A: **thread** bar.basic;

-- bar has a flow path fs1 from port p1 to p2

-- bar has a flow source fs2 to p3

B: **thread** baz.basic;

-- baz has a flow path fs1 from port p1 to p2

-- baz has a flow sink fsink in port reset

**connections**

conn1: **port** signal -> A.p1;

conn2: **port** A.p2 -> B.p1;

conn3: **port** B.p2 -> result2;

conn4: **port** A.p2 -> result1;

conn6: **port** A.p3 -> status;

connToThread: **port** initcmd -> B.reset;

**flows**

Flow1: **flow path**

signal -> conn1 -> A.fs1 -> conn4 -> result1;

Flow2: **flow path**

signal -> conn1 -> A.fs1 -> conn2 ->

B.fs1 -> conn3 -> result2;

Flow3: **flow sink** initcmd -> connToThread -> C.fsink;

-- a flow source may start in a subcomponent,

-- i.e., the first named element is a flow source

Flow4: **flow source** A.fs2 -> conn6 -> status;

**end** foo.basic;

End-To-End Flows
----------------

 An end-to-end flow represents a logical flow of data and control
from a source to a destination through a sequence of threads that
process and possibly transform the data. In a complete AADL
specification, the source and destination can be threads, data
components, devices, and processors. In an incomplete AADL
specification, the source and destination are the leaf nodes in the
component hierarchy, which may be thread groups, processes, or
systems. If the AADL specification includes subprogram calls, the
end to end flow follows the semantic connections between threads,
processors, and devices, and the parameter connections within
threads.

Syntax

end\_to\_end\_flow ::= end\_to\_end\_flow\_spec \|
end\_to\_end\_flow\_implementation

end\_to\_end\_flow\_spec ::=

*defining\_end\_to\_end\_flow*\ \_identifier : **end to end** **flow**

*start*\ \_subcomponent\_flow\_identifier **->**
*end\_*\ subcomponent\_flow\_identifier

[ **{** ( property\_association }\ :sup:`+` **}** ]

[ in\_modes\_and\_transitions ] ;

end\_to\_end\_flow\_implementation ::=

*defining\_end\_to\_end\_flow*\ \_identifier : **end to end** **flow**

*start*\ \_subcomponent\_flow\_identifier

{ **->** *connection*\ \_identifier

**->** *flow\_path*\ \_subcomponent\_flow\_or\_etef\_identifier
}\ :sup:`+`

[ **{** ( property\_association }\ :sup:`+` **}** ]

[ in\_modes\_and\_transitions ] ;

end\_to\_end\_flow\_refinement ::=

*defining\_end\_to\_end\_flow\_*\ identifier **:**

**refined to end to end flow**

[*start*\ \_subcomponent\_flow\_identifier

{ **->** *connection*\ \_\ *identifier*

**->** *flow\_path*\ \_subcomponent\_flow\_or\_etef\_identifier
}\ :sup:`+` ]

( **{** { property\_association }\ :sup:`+` **}** [
in\_modes\_and\_transitions ]

\| in\_modes\_and\_transitions

) **;**

subcomponent\_flow\_or\_etef\_identifier ::=

subcomponent\_flow\_identifier \| *end\_to\_end\_flow\_*\ identifier

Naming Rules

1. The defining end-to-end flow identifier of an end-to-end flow
   declaration must be unique within the local name space of the
   component implementation containing the end-to-end flow declaration.

1. The connection identifier in an end-to-end flow declaration must
   refer to a connection in the component implementation.

2. The subcomponent flow identifier of an end-to-end flow declaration
   must name an optional flow specification in the component type of the
   named subcomponent or to a data component in the form of a data
   subcomponent, provides data access, or requires data access.

3. The end-to-end flow identifier referenced in an end-to-end flow
   declaration must name an end-to-end flow in the name space of the
   same component implementation.

4. The defining identifier of an end-to-end flow refinement must refer
   to an end-to-end flow spec, end to end flow implementation, or
   refinement in an ancestor component implementation.

Legality Rules

1. The flow specifications identified by the
   *flow\_path*\ \_subcomponent\_flow\_identifier must be flow
   paths, if present.

1. The *start*\ \_subcomponent\_flow\_identifier must refer to a
   subcomponent and optionally to a flow path or a flow source, or
   to a data component.

2. The *end*\ \_subcomponent\_flow\_identifier must refer to a
   subcomponent and optionally to a flow path or a flow sink, or to
   a data component.

3. If an end-to-end flow is referenced in an end-to-end flow
   declaration, then its first and last subcomponent flow must name
   the same port as the preceding or succeeding connection.

4. In case of a mode specific end-to-end flow declarations, the named
   connections and the subcomponents of the named flow
   specifications must be declared for the modes listed in the **in
   modes** statement.

5. An end-to-end flow refinement declaration that includes a sequence of
   connections and subcomponents must refine an end-to-end flow
   specification, but not an end-to-end flow implementation, i.e.,
   once defined the flow path of an end-to-end flow cannot be
   redefined.

Standard Properties

Actual Latency: Time Range

NOTE: These properties are examples of properties for latency and
throughput analysis. The expected property values represent constraints
that must be satisfied by the actual property values of the end-to-end
flow. The semantics of the constraint are analysis specific.

Semantics

 An end-to-end flow represents a logical flow of information through
a system instance. The end-to-end flow is declared in a component
implementation that is the common root of all components involved in
the flow. The declared end-to-end flow starts with a subcomponent
flow specification, followed by zero or more connections and
subcomponent flow specifications, and ends with a connection and a
subcomponent flow specification. The end-to-end flow can involve
active components such as threads, thread groups, processes,
systems, processors, and devices, as well as passive components in
the form of a data component.

An end-to-end flow may refer to subcomponents without identifying a
flow specification. In this case, the flow specification (a) refers
to an unnamed flow specification from the destination port of the
preceding connection and the source port of the succeeding
connection if the subcomponent has no declared flow specification,
or (b) represents a separate end-to-end flow for each of the flow
specifications of the subcomponent.

 An end-to-end flow may be specified as a composition of other
end-to-end flows, where the last element of the predecessor
end-to-end flow is connected with the first element of the successor
end-to-end flow.

 The corresponding end-to-end flow instance is determined by
expanding the flow specifications through their flow
implementations. The resulting end-to-end flow instance starts from
a device, processor, data component, or thread that is the leaf of
the component hierarchy of the first subcomponent, follows semantic
connections to intermediate threads and ends with a thread, data
component, device, or processor. For incomplete models or flow
implementations the components involved in the end-to-end flow
instance may be thread groups, processes, or systems.

(5) If the first subcomponent flow specification is a flow path, then
the end-to-end flow instance starts with the first component of the
flow path expanded through its flow implementation. Similarly, if
the last subcomponent flow specification is a flow path, then the
end-to-end flow instance ends with the last component of the flow
path expanded through its flow implementation.

(6) The optional in\_modes\_and\_transitions subclause specifies what
modes the end-to-end flow is part of. The detailed semantics of this
subclause are defined in Section 12.

Examples

-- This is an AADL fragment

-- process foo is declared in the previous section

**process implementation** foo.basic

**subcomponents**

A: **thread** bar.basic;

-- bar has a flow path fs1 from p1 to p2

-- bar has a flow source fs2 to p3

B: **thread** baz.basic;

-- baz has a flow path fs1

-- baz has a flow sink fsink

**connections**

conn1: **port** signal -> A.p1;

conn2: **port** A.p2 -> result1;

conn3: **port** B.p2 -> result2;

conn4: **port** A.p2 -> B.p1;

conn5: **port** A.p3 -> Status;

conn6: **port** A.p3 -> B.reset;

connToThread: **port** initcmd -> A.reset;

**flows**

Flow1: **flow path**

signal -> conn1 -> A.fs1 -> conn2 -> result1;

Flow3: **flow sink** initcmd -> connToThread -> B.fsink;

-- a flow source may start in a subcomponent,

-- i.e., the first named element is a flow source

Flow4: **flow source** A.fs2 -> conn5 -> status;

-- an end-to-end flow from a source to a sink

ETE1: **end to end flow**

A.fs2 -> conn6 -> B.fsink;

-- an end-to-end flow where the end points are not sources or sinks

ETE2: **end to end** **flow **

A.fs1 -> conn4 -> B.fs1;

**end** foo.basic;


