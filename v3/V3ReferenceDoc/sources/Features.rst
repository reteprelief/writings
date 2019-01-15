Features and Shared Access
==========================


A feature represents an interaction point of a component that is visible to
other components. Connections between features of different components represent exchange of control and data. 
Features include ports to support directional flow of data and control, shared data access, and remote procedure call interactions. Features also represent binding points between components in an application system and components in the hardware platform. 
Features also represent access to buses for modeling hardware interconnections via buses.

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


 A *feature* is a part of a component type definition that specifies
how that component interfaces with other components in the system.

 *Named interfaces* represent groups of component features. Feature
groups can contain feature groups. Feature groups can be used
anywhere features can be used. Within a component, the features of a
feature group can be connected to individually. Outside a component,
feature groups can be connected as a single unit. Feature groups can
be partially defined without specifying the elements to play the
role of an abstract feature. It can later be refined into a group of
one or more features.

*Port* features represent a communication interface for the exchange
of data and events between components. Ports are classified into
data ports, event ports, and event data ports.

 *Subprogram access* features represent access to a subprogram to be
called from other components, and the need for a component to call a
subprogram instance locally, i.e., a subprogram that is declared in
the containing process or one of its subcomponents, or to call a
subprogram remotely, i.e., a subprogram instance in another process.
When used in data components subprogram access features represent
subprograms (methods) through which the data component is
manipulated. When used in subprogram groups subprogram access
features represent the subprograms of a subprogram library.

 *Subprogram group access* features represent sharing and required
access to a subprogram library.

(5) *Parameter* features represent data values that can be passed into
and out of subprograms. Parameters are typed with a data classifier
reference.

(6) *Data access* represents communication via shared access to data
components. A data component declared inside a component
implementation is specified to be accessible to components outside
using a provides access feature declaration. A component may
indicate that it requires access to a data subcomponent declared
outside utilizing a requires access feature declaration.

(7) *Bus access* represents physical connectivity of processors, memory,
devices, and buses through buses. A bus component declared inside a
component implementation is specified to be accessible to components
outside using a provides access feature declaration. A component may
indicate that it requires access to a bus utilizing a requires
access feature declaration.

(8) Features can be declared as one-dimensional feature arrays. Such
feature arrays complement component arrays and allow for connection
patterns that connect a feature of each of the component array
elements to a feature array element of one component. An example use
is redundant replicas of a component passing their output to a
voting component or a routing component.

Syntax

feature ::=

( abstract\_feature\_spec \|

port\_spec \|

feature\_group\_spec \|

subcomponent\_access\_spec \|

parameter\_spec )

[ array\_dimension ]

[ **{** { *feature*\ \_contained\_property\_association \|
*feature*\ \_property\_association }\ :sup:`+` **}** ] **;**

subcomponent\_access\_spec ::=

subprogram\_access\_spec \| subprogram\_group\_access\_spec

\| data\_access\_spec \| bus\_access\_spec \| virtual bus access spec

feature\_refinement ::=

abstract\_feature\_refinement

port\_refinement \|

feature\_group\_refinement \|

subcomponent\_access\_refinement \|

parameter\_refinement

[ array\_dimension ]

[ **{** { *feature*\ \_contained\_property\_association \|
*feature*\ \_property\_association }\ :sup:`+` **}** ] **;**

subcomponent\_access\_refinement ::=

subprogram\_access\_refinement \| subprogram\_group\_access\_refinement

\| data\_access\_refinement \| bus\_access\_refinement \|
virtual\_bus\_access\_refinement

Naming Rules

1. The defining identifier of a feature must be unique within the
   namespace of the associated component type.

1. Thread features may not be declared using the predeclared ports names
   Complete or Error.

2. Each refining feature identifier that appears in a feature refinement
   declaration must also appear in a feature declaration of a component
   type being extended.

3. A feature is referenced in one of two ways. Within the component
   implementations for a component type, a feature declared in the type
   is named in the implementations by its identifier. Within component
   implementations that contain subcomponents with features, a
   subcomponent feature is named by the subcomponent identifier and the
   feature identifier separated by a . (dot).

4. The path of a contained property association for a feature must refer
   to an element of a feature group.


Naming Rules
^^^^^^^^^^^^

#. A named interface declaration introduces a local name space for the content of the referenced interface.

#. The content of a named interface is referenced by (recursively) qualifying its identifier with the identifier of the enclosing named interface.


Legality Rules

1. Each feature can be refined at most once in the same type extension.

1. A feature refinement declaration of a feature and the original
   feature must both be declared as port, parameter, access feature,
   or feature group, or the original feature must be declared as
   abstract feature.

2. Feature arrays must only be declared for thread, device, processor,
   memory, system, and abstract.

3. If the feature refinement specifies an array dimension, then the
   feature being refined must have an array dimension.

4. If the refinement specifies an array dimension size, then the feature
   being refined must not have an array dimension size.

5. A contained property association must only be used when the feature
   is a feature group.

6. In the case of a feature with a classifier reference, the classifier
   of the refined feature declaration in a component type extension
   must adhere to the classifier refinement rules as indicated by
   the Classifier\_Substitution\_Rule property (see Section 4.5). By
   default, the Classifier\_Match rule applies, i.e., an incomplete
   classifier reference can be completed.

Standard Properties

Acceptable Array Size: **list of** Size Range

Required Connection Quality Of Service : **inherit list of** Supported
Connection\_QoS

Required Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

Semantics

 A feature declaration specifies an interaction point with other
components. Features can be connected to features of other
components external to the component, and they can also be connected
to subcomponents within component implementations associated with
the component type with the feature declaration.

A refined feature declaration may complete an incomplete component
classifier reference and declare feature property associations. A
feature refinement may replace a classifier reference according to
the Classifier\_Substitution\_Rule property.

 Features can be declared with an array dimension, if the component
is a thread, data, device, memory, bus,or processor. In this case,
each element of the feature array is connected to a different
element of an ultimate source or destination component array.
Feature refinement declarations can complete the array dimension
with a size specification, but it cannot change the size. The array
dimension and size is inherited by the refined feature, if it is not
explicitly declared.

 For example, we may have a voting component that takes input from
multiple instances of the same processing component, as shown in
Figure 11. We can declare the processing component as an array of
subcomponents, and the single instance of the voting component with
a port array. We can then declare a connection from the outgoing
port of the processing subcomponent to the port of the voting
component declared with array dimensions. The connection can have a
Connection\_Pattern property of One\_To\_One (see Section 9.2.3) to
indicate that each processing component output is connected to a
separate port of the voting component. This is illustrated in the
example below.

Figure − Port Array in a Voting Pattern
   

 A feature may specify desired quality of service or a particular
protocol to be used for connections through the feature. This
property must be consistent with the same property associated with
the connection.

1. .. rubric:: Abstract Features
  :name: abstract-features

 Abstract features represent generic features in conceptual and
physical system models. They can also be used as placeholders for
concrete features, i.e., ports, parameters, and the different kinds
of access features.

Syntax

abstract\_feature\_spec ::=

*defining\_abstract\_feature\_*\ identifier :

( [ **in** \| **out** ] **feature** [
*component\_prototype*\ \_identifier

\| unique\_component\_classifier\_reference ] )

\| ( [ **in** \| **out** ] **prototype**
*feature\_prototype*\ \_identifier )

abstract\_feature\_refinement ::=

( *defining\ **\_**\ abstract\_feature\_*\ identifier : **refined to**

[ **in** \| **out** ] **feature** [ *component\_prototype*\ \_identifier

\| unique\_component\_classifier\_reference ] )

\| ( [ **in** \| **out** ] **prototype**
*feature\_prototype*\ \_identifier )

\| port\_refinement \| feature\_group\_refinement

\| subcomponent\_access\_refinement \| parameter\_refinement

Legality Rules

1. The feature direction in a refined feature declaration must be
   identical to the feature direction in the feature declaration
   being refined, or the feature being refined must not have a
   direction.

2. If the direction of an abstract feature is specified, then the
   direction must be satisfied by the refinement (see also the rules
   for feature prototypes in Section 4.7); in the case of ports the
   direction must be **in** or **out**; in the case of data access,
   the access right must be read-only for **in** and write-only for
   **out**; in the case of bus access, subprogram access and
   subprogram group access the direction must not be **in** nor
   **out**.

1. An abstract feature with a feature prototype identifier and the
   prototype being referenced must both specify the same direction
   or no direction.

2. An abstract feature refinement declaration of a feature with a
   feature prototype reference must only add property associations.

3. The unique component classifier reference of an abstract feature
   declaration must be to an abstract, data, bus, virtual bus,
   subprogram, subprogram group component classifier.

4. Abstract feature declarations with data component classifier
   reference must only be refined absto tract features, or concrete
   features with a data component classifier reference, i.e., data
   ports, event data ports, or data access features.

5. Abstract feature declarations with bus, virtual bus, subprogram, or
   subprogram group component classifier reference must only be
   refined to abstract features, or concrete features with the
   respective component classifier reference, e.g., bus access for
   bus classifiers.

Semantics

 A component type can contain an abstract feature declaration. An
abstract feature may be specified with a direction; this direction
must be satisfied by any refinement according to legality rules. An
abstract reature may represent a generic feature in a conceptual or
physical system model. An abstract feature may later be refined into
a feature group, port feature, access feature, or parameter.

A component type can contain an abstract feature declaration with a
feature prototype reference. In that case it is a placeholder for
the feature that acts as a parameter to the component type. The
actual feature will be supplied as part of the prototype binding
when the component type is referenced, e.g., in a subcomponent
declaration.

1. .. rubric:: Feature Groups and Feature Group Types
  :name: feature-groups-and-feature-group-types

 Feature groups represent groups of component features or feature
groups. Within a component, the features of a feature group can be
connected to individually. Outside a component, feature groups can
be connected as a single unit. This grouping concept allows the
number of connection declarations to be reduced, especially at
higher levels of a system when a number of features from one
subcomponent and its contained subcomponents must be connected to
features in another subcomponent and its contained subcomponents.
The content of a feature group is declared through a feature group
type declaration. This declaration is then referenced when feature
groups are declared as component features. Feature groups can be
declared for any kind of feature, for ports, and for access
features.

Syntax

-- Defining the content structure of a feature group

feature\_group\_type ::=

**feature** **group** *defining*\ \_identifier

[ **prototypes** ( { prototype }\ :sup:`+` \| none\_statement ) ]

[ **features** { feature }\ :sup:`+` ]

[ **inverse of** unique\_feature\_group\_type\_reference ]

[ **properties** ( {
*feature\_group*\ \_contained\_property\_association \|

*feature\_group*\ \_property\_association }\ :sup:`+` \| none\_statement
) ]

{ annex\_subclause }\ :sup:`\*`

**end** *defining*\ \_identifier **;**

feature\_group\_type\_extension ::=

**feature** **group** *defining*\ \_identifier

**extends** unique\_feature\_group\_type\_reference [
prototype\_bindings ]

[ **prototypes** ( { prototype \| prototype\_refinement }\ :sup:`+` \|
none\_statement ) ]

[ **features** { feature \| feature\_refinement }\ :sup:`+` ]

[ **inverse of** unique\_feature\_group\_type\_reference ]

[ **properties** ( {
*feature\_group*\ \_contained\_property\_association }\ :sup:`+`

\| *feature\_group\_*\ property\_association \| none\_statement ) ]

{ annex\_subclause }\ :sup:`\*`

**end** *defining*\ \_identifier **;**

-- declaring a feature group as component feature

feature\_group\_spec ::=

*defining\_feature\_group*\ \_identifier : [ **in** \| **out** ]
**feature** **group **

[ [ **inverse of** ]

( unique\_feature\_group\_type\_reference \|
*feature\_group\_prototype­\_*\ identifier ) ]

feature\_group\_refinement ::=

*defining\_feature\_group*\ \_identifier : **refined to** [ **in** \|
**out** ] **feature** **group **

[ [ **inverse of** ]

( unique\_feature\_group\_type\_reference \|
*feature\_group\_prototype\_*\ identifier ) ]

unique\_feature\_group\_type\_reference ::=

[ package\_name **::** ] *feature\_group\_type*\ \_identifier

Naming Rules

1. The defining identifier of a feature group type must be unique within
   the package namespace of the package where the feature group type is
   declared.

1. Each feature group type provides a local namespace. The defining
   identifiers of prototype, feature, and feature group declarations in
   a feature group type must be unique within the namespace of the
   feature group type.

2. The local namespace of a feature group type extension includes the
   defining identifiers in the local namespace of the feature group type
   being extended. This means, the defining identifiers of prototype,
   feature, or feature group declarations in a feature group type
   extension must not exist in the local namespace of the feature group
   type being extended. The defining identifiers of prototype, feature,
   or feature group refinements in a feature group type extension must
   refer to a prototype, feature, or feature group in the local
   namespace of an ancestor feature group type.

3. The defining feature identifiers of feature group declarations must
   be unique in the local name space of the component type containing
   the feature group declaration.

4. The defining feature group identifier of feature\_refinement
   declarations in component types must exist in the local namespace of
   the component type being extended and must refer to a feature or
   feature group.

5. The package name of the unique feature group type reference must
   refer to a package name in the global namespace. The feature group
   type identifier of the unique feature group type reference must refer
   to a feature group type identifier in the named package.

6. The prototype reference in a feature group declaration must refer to
   a prototype of the component type or feature group type that contains
   the feature group declaration.

Legality Rules

1. A feature group type may contain zero or more elements, i.e., feature
   or feature groups. If it contains zero elements, then the feature
   group type may be declared to be the inverse of another feature
   group type.

1. A feature group type can be declared to be the inverse of another
   feature group type, as indicated by the reserved words **inverse
   of** and the name of a feature group type. Any feature group type
   named in an **inverse of** statement cannot itself contain an
   **inverse of** statement. This means that several feature groups
   can be declared to be the inverse of one feature group, e.g., B
   inverse of A and C inverse of A is acceptable. However, chaining
   of inverses is not permitted, e.g., B inverse of A and C inverse
   of B is not acceptable.

2. Only feature group types without **inverse of** or feature group
   types with features and **inverse of** can be extended.

3. A feature group type that is an extension of another feature group
   type without an **inverse of** cannot contain an **inverse of**
   statement.

4. The feature group type that is an extension of another feature group
   type with features and **inverse of** that adds features must
   have an **inverse of** to identify the feature group type whose
   inverse it is.

5. A feature group declaration with an **inverse of** statement must
   only reference feature group types without an **inverse of**
   statement.

6. A feature group refinement may be refined to only add property
   associations. In this case inclusion of the feature group type
   reference is optional.

Two feature group types are considered to complement each other if the
following holds:

1. The number of feature or feature groups contained in the feature
   group and its complement must be identical;

2. Each of the declared features or feature groups in a feature group
   must be a pair-wise complement with that in the feature group
   complement, with pairs determined by declaration order. In the
   case of feature group type extensions, the feature and feature
   group declarations in the extension are considered to be declared
   after the declarations in the feature group type being extended;

3. If both feature group types have zero features, then they are
   considered to complement each other;

4. Ports are pair-wise complementary if they satisfy the port connection
   rules specified in Section 9.2.1. This includes appropriate port
   direction and matching of data component classifier references
   according to classifier matching rules (see Section 9.5 legality
   rules (L3) and (L4);

5. Access features are pair-wise complementary if they satisfy the
   access connection rules in Section 9.4.

6. If an **in** or **out** direction is specified as part of a feature
   group declaration, then all features inside the feature group
   must satisfy this direction.

NOTE: Aggregate data ports can be modeled in AADL V2 by a data port with
a data component classifier that has data subcomponents for each of the
element ports. This replaces the Aggregate\_Data\_Port on port groups in
the original AADL standard.

Standard Properties

-- Port properties defined to be **inherit**, thus can be associated
with a

-- feature group to apply to all contained ports.

Source Text: **inherit list of aadlstring**

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

Semantics

 A feature group declaration represents groups of component features.
As such each feature group of a component type can represent a
separate interface to the component.

A feature group of a component can be connected to another component
through a single connection declaration. It represents a connection
for each of the feature inside the feature group. Feature groups can
contain feature groups. This supports nested grouping of features
for different levels of the modeled system.

 Within a component, the features of a feature group can be connected
to individually to subcomponents. The members of the feature group
are declared in a feature group type declaration that is referenced
by the feature group declaration. The referenced feature group type
determines the feature group compatibility for a feature group
connection.

 The **inverse of** reserved words of a feature group type
declaration indicate that the feature group type represents the
complement to the referenced feature group type. The legality of
feature group connections is affected by the complementary nature of
feature groups (see Section 9.5).

(5) Features can be declared without feature group types or with feature
group types without features. They are considered to be incomplete
feature group specifications. Feature group types can later be added
in a feature group refinement. Features can later be inserted
directly into the feature group type or the feature group type can
later be refined into feature group types with one or more features.

Examples

**package** GPS\_Interface

**public**

**with** GPSLib;

**feature group** GPSbasic\_socket

**features**

Wakeup: **in event port**;

Observation: **out data port** GPSLib::position;

**end** GPSbasic\_socket;

**feature group** GPSbasic\_plug

**features**

WakeupEvent: **out event port**;

ObservationData: **in data port** GPSLib::position;

-- the features must match in same order with opposite direction

**inverse of** GPSbasic\_socket

**end** GPSbasic\_plug;

**feature group** MyGPS\_plug

-- second feature group as inverse of the original

-- no chaining in inverse and

-- no pairwise inverse references are allowed

**inverse of** GPSbasic\_socket

**end** MyGPS\_plug;

**feature group** GPSextended\_socket **extends** GPSbasic\_socket

**features**

Signal: **out event port**;

Cmd: **in data port** GPSLib::commands;

**end** GPSextended\_socket;

**process** Satellite\_position

**features**

position: **feature group** GPSBasic\_socket;

**end** Satellite\_position;

**process** GPS\_System

**features**

position: **feature group** **inverse of** GPSbasic\_socket;

**end** GPS\_System;

**system** Satellite

**end** Satellite;

**system implementation** Satellite.others

**subcomponents**

SatPos: **process** Satellite\_position;

MyGPS: **process** GPS\_System;

**connections**

satconn: **feature group** Satpos.position <-> MyGPS.position;

**end** Satellite.others;

**end** GPS\_Interface;

Ports
-----

 Ports are logical connection points between components that can be
used for the transfer of control and data between threads or between
a thread and a processor or device. Ports are directional, i.e., an
output port is connected to an input port. Ports can pass data,
events, or both. Data transferred through ports is typed. From the
perspective of the application source text, data ports are
accessible in the source text as data variables. From the
perspective of the application source text, event ports represent
event queues whose size is accessible. Incoming events may trigger
thread dispatches or mode transitions, or they may simply be queued
for processing by the recipient. From the perspective of the
application source text, event data ports represent message queues
whose content can be retrieved.

The content of incoming ports are frozen at a specified time, by
default at dispatch time. This means that the content of the port
that is accessible to the recipient does not change during the
execution of a dispatch even though the sender may send new values.
Properties specify the input and output timing characteristics of
ports. Actual event and data transfer may be initiated by the
runtime system of the execution platform or by Send\_Output runtime
service calls in the application source text.

 AADL distinguishes between three port categories. *Event data ports*
are ports through which data is sent and received. The arrival of
data at the destination may trigger a dispatch or a mode switch. The
data may be queued if the destination component is busy. Event data
ports effectively represent message ports. *Data ports* are event
data ports with a queue size of one in which the newest arrival is
kept. By default arrival of data at data ports does not trigger a
dispatch. Data ports effectively represent unqueued ports that
communicate state information, such as signal streams that are
sampled and processed in control loops. *Event ports* are event data
ports with empty message content. Event ports effectively represent
discrete events in the physical environment, such as a button push,
in the computing platform, such as a clock interrupt, or a logical
discrete event, such as an alarm.

Syntax

port\_spec ::=

*defining\_port*\ \_identifier : ( **in** \| **out** \| **in out** )
port\_type

port\_refinement ::=

*defining\_port*\ \_identifier : **refined to**

( **in** \| **out** \| **in out** ) port\_type

port\_type ::=

**data port** [ *data*\ \_unique\_component\_classifier\_reference

\| *data\_component\_prototype*\ \_identifier ]

\| **event data port** [
*data*\ \_unique\_component\_classifier\_reference

\| *data\_component\_prototype\_*\ identifier ]

\| **event port **

Naming Rules

1. A defining port identifier must adhere to the naming rules specified
   for all features (see Section 8).

1. The defining identifier of a port refinement declaration must also
   appear in a feature declaration of a component type being extended
   and must refer to a port or an abstract feature.

2. The unique component type identifier of the data classifier reference
   must be the name of a data component type. The data implementation
   identifier, if specified, must be the name of a data component
   implementation associated with the data component type.

3. The prototype identifier of a prototype reference, if specified, must
   exist in the namespace of the component type or feature group type
   that contains the feature declaration.

Legality Rules

1. Ports can be declared in subprogram, thread, thread group, process,
   system, processor, virtual processor, and device component types.

1. Data and event data ports may be incompletely defined by not
   specifying the data component classifier reference or data
   component implementation identifier of a data component
   classifier reference. The port definition can be completed using
   refinement.

2. Data, event, and event data ports may be refined by adding a property
   association. The data component classifier declared as part of
   the data or event data port declaration being refined does not
   need to be included in this refinement.

3. The port category of a port refinement must be the same as the
   category of the port being refined, or the port being refined
   must be an abstract feature.

4. The port direction of a port refinement must be the same as the
   direction of the feature being refined. If the feature being
   refined is an abstract feature without direction, then all port
   directions are acceptable.

Standard Properties

-- Properties specifying the source text variable representing the port

Source Name: **aadlstring**

Source Text: **inherit list of aadlstring**

-- property indicating whether port connections are required or optional

Required Connection : **aadlboolean** **=>** **true**

-- The protocol the source text supporting the port is assumed to make
use of

Allowed Connection Binding\_Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

-- Optional property for device ports

Device Register Address: **aadlinteger**

-- data port connection timing

Timing : **enumeration** (sampled, immediate, delayed) **=>** sampled

-- Input and output rate and time

Input Rate: Rate Spec => [ Value\_Range => 1.0 .. 1.0; Rate Unit =>
PerDispatch; Rate Distribution => Fixed; ]

Input Time: **list of** IO Time\_Spec => ([ Time => Dispatch; Offset =>
0.0 ns .. 0.0 ns;])

Output Rate: Rate\_Spec => [ Value Range => 1.0 .. 1.0; Rate\_Unit =>
PerDispatch; Rate Distribution => Fixed; ]

Output Time: **list of** IO Time Spec => ([ Time => Completion; Offset
=> 0.0 ns .. 0.0 ns;])

-- Port specific compute entrypoint properties for event and event data
ports

Compute Entrypoint: **classifier** ( subprogram classifier )

Compute Execution Time: Time\_Range

Compute Deadline: Time

-- Properties specifying binding constraints for variables representing
ports

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

-- In port queue properties

Overflow Handling Protocol: **enumeration** (DropOldest, DropNewest,
Error)

=> DropOldest

Queue Size: **aadlinteger** 0 **..** Max\_Queue\_Size => 1

Queue Processing Protocol: Supported Queue Processing Protocols => FIFO

Fan Out Policy: **enumeration** (Broadcast, RoundRobin, Selective,
OnDemand)

Urgency: **aadlinteger** 0 **..** Max\_Urgency

Dequeued Items: **aadlinteger**

Dequeue Protocol: **enumeration** ( OneItem, MultipleItems, AllItems )
=> OneItem

Semantics

Port Categories
~~~~~~~~~~~~~~~

  A port specifies a logical connection point in the interface of a
 component through which incoming or outgoing data and events may be
 passed. Ports may be named in connection declarations. Ports that
 pass data are typed by naming a data component classifier
 reference.

 A data or event data port maps to a static variable in the source
 text that represents the data buffer or queue. By default the
 variable is accessible by the same name as the port name. A
 different name mapping can be specified with the Source\_Name and
 Source\_Text properties. The Allowed\_Memory\_Binding and
 Allowed\_Memory\_Binding\_Class properties indicate the memory (or
 device) hardware the port resources reside on.

  Event and event data ports may dispatch a port specific
 Compute\_Entrypoint. This permits threads with multiple event or
 event data ports to execute different source text sequences for
 events arriving at different event ports. If specified, the port
 specific Compute\_Execution\_Time and Compute\_Deadline takes
 precedence over those of the containing thread.

  Ports are directional. An **out** port represents output provided
 by the sender, and an **in** port represents input needed by the
 receiver. An **in out** port represents both an **in** port and an
 **out** port. Incoming connection(s) and outgoing connection(s) of
 an **in out** port may be connected to the same component or to
 different components. For a data port, the **in out** port maps to
 a port variable in the source text. This means that the source text
 will overwrite the existing incoming value of the port when writing
 the output value to the port variable. The queues of incoming event
 data ports and event ports may require a port variable that holds
 the queue content that is frozen during the execution of a thread.
 In the case of event data ports, the outgoing data in the
 implementation may utilize a separate port variable.

(5)  Ports that provide output, i.e., **out** ports or **in out** ports,
 are referred to as outgoing port. Ports that provide input, i.e.,
 **in** ports or **in out** ports, are referred to as incoming
 ports.

(6)  A port can require a connection or consider it as optional as
 indicated by the Required\_Connection property. In the latter case
 it is assumed that the component with this port can function
 without the port being connected.

(7)  Ports appear to the thread as input and output buffers, accessible
 in source text as port variables.

(8)  Data and event data ports are used to transmit data between
 threads.

(9)  Data ports are intended for transmission of state data such as
 sensor data streams. Therefore, no queuing is supported for data
 ports. A thread can determine whether the input buffer of an in
 data port has new data at this dispatch by checking the port status
 through a Get\_Count service call, which is accessible through the
 port variable through a Get\_Value service call. If no new data
 value has been received the old value is made available.

(10) Event data ports are intended for message transmission, i.e., the
 queuing of the event and associated data at the port of the
 receiving thread. A receiving thread can get access to one or more
 data element in the queue according to the Dequeue\_Protocol and
 Dequeued\_Items properties (see Section 8.3.3). The number of
 queued event data elements accessible to a thread can be determined
 through the port variable using the Get\_Count service call.
 Individual element of the queue can be retrieved via the port
 variable using the Get\_Value and Next\_Value service calls. If the
 queue is empty the most recent data value is available.

(11) Event ports are intended for event and alarm transmission, i.e.,
 the queuing of events at the port of the receiving thread, possibly
 resulting in a dispatch or mode transition. A receiving thread can
 get access to one or more events in the queue according to the
 Dequeue\_Protocol and the Dequeue\_Items property. The number of
 queued events accessible to a thread can be determined through the
 port variable by making a Get\_Count service call.

(12) The role of an aggregate data port is to make a collection of data
 from multiple outgoing data ports available in a time-consistent
 manner. Time consistency in this context means that if a set of
 periodic threads is dispatched at the same time to operate on data,
 then the recipients of their data see either all old values or all
 new values. This is accomplished by declaring a data port, whose
 data classifier has an implementation with data components
 corresponding to the data of the individual data ports. The
 functionality of an aggregate data port can be viewed as a thread
 whose only role is to collect the data values from several **in**
 data ports and make them available as an aggregate data record; on
 the receiving side an equivalent thread takes passes on the
 elements of the aggregate data record on to the respective **out**
 data ports of receiving threads. The function may be optimized by
 mapping the data ports of the individual threads into a data area
 representing the aggregate data port variable. This aggregate is
 then transferred as a single unit.

 1. .. rubric:: Port Input and Output Timing
   :name: port-input-and-output-timing

(13) Data, events, and event data arriving through incoming ports is
 made available to the receiving thread, processor, or device at a
 specified input time. For a data port the input that is available
 through a port variable is a data value, while for an event or
 event data port it can be one or more elements from the port queue
 according to a specified dequeuing protocol (see Section 8.3.3).
 From that point on any newly arriving data, event, or event data is
 not available to the receiving component until the next dispatch,
 i.e., the content of an incoming port that is accessible to the
 application code does not change while the thread completes its
 execution.

(14) By default, port input is frozen at dispatch time. For periodic
 threads or devices this means that input is sampled at fixed time
 intervals.

(15) The Input\_Time property can be used to explicitly specify an input
 time for ports. This can be done for all ports by specifying the
 property value for the thread, or it can be specified separately
 for each port.

(16) The following property values for Input\_Time are supported to
 specify the input time to be the dispatch time (Dispatch), any time
 during execution relative to the amount of execution time from the
 start (Start) or from the completion (Completion), and the fact
 that no input occurs (NoIO):

-  Dispatch\_Time: (the default value) input is frozen at dispatch time;
   the time reference is clock time t = 0.

-  Start, time range: input is frozen at a specified amount of execution
   time from the beginning of execution. The time is within the
   specified time range. The time range must have positive values.
   Start\ :sub:`low` ≤ c ≤ Start\ :sub:`high`.

-  Completion, time range: input is frozen at a specified amount of
   execution time relative to execution completion. The time is within
   the specified time range. A negative time range indicates execution
   time before completion. c\ :sub:`complete` + Completion\ :sub:`low` ≤
   c ≤ c\ :sub:`complete` + Completion\ :sub:`high`, where
   c\ :sub:`complete` represents the value of c at completion time.

-  NoIO: input is not frozen. In other words, the port is excluded from
   making new input available to the source program. This allows users
   to specify that a subset of ports to provide input. The property
   value can be mode specific, i.e., a port can be excluded in one mode
   and included in another mode.

 The Input\_Time property can have a list of values. In this case it
indicates that input is frozen multiple times for the execution of a
dispatch. If a thread has multiple input times specified, then the
content of an incoming port is frozen multiple times during a single
dispatch.

The input may be frozen at dispatch time (Input\_Time property value
of Dispatch) as part of the underlying runtime system, or it may be
frozen through a Receive\_Input service call in the source text
(Input\_Time property value of Start or Completion).

 The input of other ports that can trigger dispatch is not frozen.
Input of the remaining ports is frozen according to the specified
input time.

 If a dispatch condition is specified then the logic expression
determines the combination of event and event data ports that
trigger a dispatch, and whose input is frozen as part of the
dispatch. The input of other ports that can trigger dispatch is not
frozen. The input of other ports that can trigger dispatch is not
frozen. Input of the remaining ports is frozen according to the
specified input time.

(5) If an event port is associated with a component (including thread)
containing modes and mode transition, and the mode transition names
the event port, then the arrival of an event is a mode change
request and it is processed according to the mode switch semantics
(see Sections 12 and 13.6).

(6) By default, the output time, i.e., the time output is transmitted to
connected components, is the completion time for data ports. By
default, for event and event data ports the output time occurs
anytime during the execution through a Send\_Output service call.

(7) The Output\_Time property can be used to explicitly specify an
output time for ports. This can be done for all ports by specifying
the property value for the thread, or it can be specified separately
for each port.

(8) The following property values for Output\_Time are supported to
specify the output time to be the dispatch time (Dispatch), any time
during execution relative to the amount of execution time from the
start (Start) or from the completion (Completion) including at
completion time, the deadline (Deadline), and the fact that no input
occurs (NoIO):

-  Start, time range: output is transmitted at a specified amount of
   execution time relative to the beginning of execution. The time is
   within the specified time range. The time range must have positive
   values. Start\ :sub:`low` ≤ c ≤ Start\ :sub:`high`.

-  Completion, time range: output is transmitted at a specified amount
   of execution time relative to execution completion. The time is
   within the specified time range. A negative time range indicates
   execution time before completion. c\ :sub:`complete` +
   Completion\ :sub:`low` ≤ c ≤ c\ :sub:`complete` +
   Completion\ :sub:`high`, where c\ :sub:`complete` repesents the value
   of c at completion time. The default is completion time with a time
   range of zero, i.e., it occurs at c = c\ :sub:`complete`.

-  Deadline: output is transmitted at deadline time; the time reference
   is clock time rather than execution time. t = Deadline. This allows
   for static alignment of output time of one thread with the
   Dispatch\_Time input time of another thread with a Dispatch\_Offset.

-  NoIO: output is not transmitted. In other words, the port is excluded
   from transmitting new output from the source text. This allows users
   to specify that a subset of ports to provide output. The property
   value can be mode specific, i.e., a port can be excluded in one mode
   and included in another mode.

 The Output\_Time property can have a list of values. In this case it
indicates that output is transmitted multiple times as part of the
execution of a dispatch.

The output may be transmitted at completion time or deadline as part
of the underlying runtime system, or it may be transmitted through a
Send\_Output service call in the source text.

 If the output time of the originating port is Completion\_Time while
the input time of the receiving port is Dispatch and the sender and
receiver are in the same synchronization domain, then the output is
received at the next dispatch equal to or later than the deadline.
To accommodate the transfer the actual transfer may be initiated
before the deadline. In the case of the connection crossing
synchronization domains, the input is received at the dispatch
following the completion of the transfer.

 The Input\_Rate and Output\_Rate properties specify the number of
times per dispatch (perDispatch) or per second (perSecond) at which
input and output is expected to occur at the port with the
associated property. By default the input and output rate of ports
is once per dispatch. The rate can be fixed or according to a
distribution.

(5) An input or output rate higher than once per dispatch indicates that
multiple inputs or multiple outputs are expected during a single
dispatch. An input or output rate lower than once per dispatch
indicates that inputs or outputs are not expected at every dispatch.

(6) If an Input\_Time or Output\_Time property is specified, then the
values must be consistent with the rate. If the rate is specified in
terms of seconds and a period is specified for the thread or device
with the port, then the period value must also be consistent with
the other values. In the case of an Input\_Time or Output\_Time
property value of NoIO the rate value does not apply.

Examples

-- a thread that gets input partway into execution and sends output

-- before completion.

**thread** TightLoop

**features**

sensor: **in data** **port**

{Input\_Time => ((Time=>Start;Offset=>10 us .. 15 us;));} ;

actuator: **out data port**

{Output\_Time => ((Time=>Completion;Offset=>10 us .. 11 us;));} ;

**end** TightLoop;

Port Queue Processing
~~~~~~~~~~~~~~~~~~~~~

 Event and event data ports can have a queue associated with them. By
default, the incoming event ports and event data ports of threads,
devices, and processors have queues. The output from the ultimate
source of a semantic port connection is added into this queue, if
the ultimate destination component is actively processing. The
default port queue size is 1 and can be changed by explicitly
declaring a Queue\_Size property association for the port.

The Queue\_Size, Queue\_Processing\_Protocol, and
Overflow\_Handling\_Protocol port properties specify queue
characteristics. If an event arrives and the number of queued events
(and any associated data) is equal to the specified queue size, then
the Overflow\_Handling\_Protocol property determines the action. If
the Overflow\_Handling\_Protocol property value is Error, then an
error occurs for the thread. The thread can determine the port that
caused the error by calling the standard Dispatch\_Status runtime
service. For Overflow\_Handling\_Protocol property values of
DropNewest and DropOldest, the newly arrived or oldest event in the
queue event is dropped.

 Queues will be serviced according to the
Queue\_Processing\_Protocol, by default in a first-in, first-out
order (FIFO). When an aperiodic, sporadic, timed, or hybrid thread
declares multiple in event and event data ports in its type that can
be dispatch triggers and more than one of these queues are nonempty,
the port with the higher Urgency property value gets serviced first.
If several ports with the same Urgency are non-empty, then the
Queue\_Processing\_Protocol is applied across these ports and must
be the same for them. In the case of FIFO the oldest event will be
serviced (global FIFO). It is permitted to define and use other
algorithms for picking among multiple non-empty queues. Disciplines
other than FIFO may be used for managing each individual queue.

 By default, one item is dequeued and made available to the receiving
application through the port variable. The Dequeue\_Protocol
property specifies different dequeuing options.

-  OneItem: (default) a single frozen item is dequeued and made
   available to the source text unless the queue is empty. The
   Next\_Value service call has no effect.

-  AllItems: all items that are frozen at input time are dequeued and
   made available to the source text via the port variable, unless the
   queue is empty. Individual items become accessible as port variable
   value through the Next\_Value service call.

-  MultipleItems: multiple items can be dequeued one at a time from the
   frozen queue and made available to the source text via the port
   variable. One item is dequeued and its value made available via the
   port variable with each Next\_Value service call. Any items not
   dequeued remain in the queue and are available for the next dispatch.

 The Get\_Count service call indicates how many items have been made
available to the source text. A value of zero indicates that no new
item is available via a data port, event port, or event port
variable. A value greater than zero indicates that one or more fresh
values are available.

A port may have a Fan\_Out\_Policy property. This property indicates
how the content is transferred through outgoing connections. The
content can be passed to all recipients (Broadcast), or the output
is distributed evenly to the recipients (RoundRobin), to one
recipient based on content/routing information (Selective), or to
the next recipient ready to be dispatched (OnDemand). Broadcast,
RoundRobin, and Selective pass on data and events without queuing
it, while OnDemand requires a queue that is serviced by the
recipients. The size of the queue and other queue characteristics
are specified as properties of the port with the fan-out.

 An event or event data port with a fan-out policy of OnDemand allows
us to model a queue being serviced by multiple recipients. For
example, a queue on an incoming thread group port that is connected
to multiple threads allows sender output to be queued in a single
queue and be serviced by multiple threads (see also Section 9.2.6).

1. .. rubric:: Events and Subprograms
  :name: events-and-subprograms

 Any subprogram, thread, device, or processor with an outgoing event
port, i.e., **out event**, **out event data**, **in out event**,
**in out event data**, can be the source of an event. During a
single dispatch execution, a thread may raise zero or more events
and transmit zero or more event data through Send\_Output runtime
service calls. It may also raise an event at completion through its
reserved Complete port (see Section 5.4) and transmit event data
through event data ports that contain new values that have not been
transmitted through explicit Send\_Output runtime service calls.

(5) Events are received through **in event**, **in out event**, **in
event data**, and **in out event data** ports, i.e., incoming ports.
If such an incoming port is associated with a thread and the thread
does not contain a mode transition naming the port, then the event
or event data arriving at this port is added to the queue of the
port. If the thread is aperiodic or sporadic and does not have its
Dispatch event connected, then each event and event data arriving
and queued at any incoming ports of the thread results in a separate
request for thread dispatch.

Examples

**package** Patterns

**public**

**thread** Voter

**features**

Input: **in data port** [3];

Output: **out data port**;

**end** Voter;

**thread** Processing

**features**

Input: **in data port**;

Result: **out data port**;

**end** Processing;

**thread group** Redundant\_Processing

**features**

Input: **in data port**;

Result: **out data port**;

**end** Redundant\_Processing;

**thread group implementation** Redundant\_Processing.basic

**subcomponents**

processing: **thread** Processing [3];

voting: **thread** voter;

**connections**

voteconn: **port** processing.Result -> voting.Input
{Connection\_Pattern => ((One\_To\_One));};

procconn: **port** Input -> processing.Input;

recon: **port** voting.Output -> Result;

**end** Redundant\_Processing.basic;

**end** Patterns;

Runtime Support For Ports
~~~~~~~~~~~~~~~~~~~~~~~~~

 The application program interface for the following services is
defined in the applicable source language annex of this standard.
They are callable from within the source text.

A Send\_Output runtime service allows the source text of a thread to
explicitly cause events, event data, or data to be transmitted
through outgoing ports to receiver ports. The Send\_Output service
takes a port list parameter that specifies for which ports the
transmission is initiated. The send on all ports is considered to
occur logically simultaneously. Send\_Output is a non-blocking
service. An exception is raised if the send fails with exception
codes indicating the failing port and type of failure.

**subprogram** Send\_Output

**features**

OutputPorts: **in parameter** <implementation-dependent port list>;

-- List of ports whose output is transferred

SendException: **out event data**; -- exception if send fails to
complete

**end** Send\_Output;

NOTE: The Send\_Output runtime service replaces the Raise\_Event service
in the original AADL standard\ *. *

 A Put\_Value runtime service allows the source text of a thread to
supply a data value to a port variable. This data value will be
transmitted at the next Send\_Output call in the source text or by
the runtime system at completion time or deadline.

**subprogram** Put\_Value

**features**

Portvariable: **requires data access**; -- reference to port variable

DataValue: **in parameter**; -- value to be stored

DataSize: **in parameter**; - size in bytes (optional)

**end** Put\_Value;

 A Receive\_Input runtime service allows the source text of a thread
to explicitly request port input on its incoming ports to be frozen
and made accessible through the port variables. Any previous content
of the port variable is overwritten, i.e., any previous queue
content not processed by Next\_Value calls is discarded. The
Receive\_Input service takes a parameter that specifies for which
ports the input is frozen. Newly arriving data may be queued, but
does not affect the input that thread has access to (see Section
9.1). Receive\_Input is a non-blocking service.

**subprogram** Receive\_Input

**features**

InputPorts: **in parameter** <implementation-dependent port list>;

-- List of ports whose input is frozen

**end** Receive\_Input;

 In the case of data ports the value is made available without
requiring a Next\_Value call. The Get\_Count will return the value 1
if the value has been updated, i.e., is *fresh*. If the data is not
fresh, the value zero is returned.

In the case of event data ports each data value is retrieved from
the queue through the Next\_Value call and made available as port
variable value. Subsequent calls to Get\_Value or direct access of
the port variable will return this value until the next Next\_Value
call.

 In case of event ports and event data ports the queue is available
to the thread, i.e., Get\_Count will return the size of the queue.
If the queue size is greater than one the Dequeued\_Items property
and Dequeue\_Protocol property may specify that more than one
element is made accessible to the source text of a thread.

 A Get\_Value runtime service shall be provided that allows the
source text of a thread to access the current value of a port
variable. The service call returns the data value. Repeated calls to
Get\_Value result in the same value to be returned, unless the
current value is updated through a Receive\_Input call or a
Next\_Value call.

**subprogram** Get\_Value

**features**

Portvariable: **requires data access**; -- reference to port variable

DataValue: **out parameter**; -- value being retrieved

DataSize: **in parameter**; - size in bytes (optional)

**end** Get\_Value;

 A Get\_Count runtime service shall be provided that allows the
source text of a thread to determine whether a new data value is
available on a port variable, and in case of queued event and event
data ports, who many elements are available to the thread in the
queue. A count of zero indicates that no new data value is
available.

**subprogram** Get\_Count

**features**

Portvariable: **requires data access**; -- reference to port variable

CountValue: **out parameter** BaseTypes::Integer; -- content count of
port variable

**end** Get\_Count;

 A Next\_Value runtime service shall be provided that allows the
source text of a thread to get access to the next queued element of
a port variable as the current value. A NoValue exception is raised
if no more values are available.

**subprogram** Next\_Value

**features**

Portvariable: **requires data access**; -- reference to port variable

DataValue: **out parameter**; -- value being retrieved

DataSize: **in parameter**; -- size in bytes (optional)

NoValue: **out event port**; -- exception if no value is available

**end** Next\_Value;

 A Updated runtime service shall be provided that allows the source
text of a thread to determine whether input has been transmitted to
a port since the last Receive\_Input service call.

**subprogram** Updated

**features**

Portvariable: **in parameter** <implementation-dependent port
reference>;

-- reference to port variable

FreshFlag: **out parameter** BaseTypes::Boolean; -- true if new arrivals

**end** Updated;

Processing Requirements and Permissions

 For each data or event data port declared for a thread, a system
implementation method must provide sufficient buffer space within
the associated binary image to unmarshall the value of the data
type. Adequate buffer space must be allocated to store a queue of
the specified size for each event data port. The applicable source
language annex of this standard defines data variable declarations
that correspond to the data or event data features. Buffer variables
may be allocated statically as part of the source text data
declarations. Alternatively, buffer variables may be allocated
dynamically while the process is loading or during thread
initialization. A method of implementing systems may require the
data declarations to appear within source files that have been
specified in the source text property. In some implementations,
these declarations may be automatically generated for inclusion in
the final set of source text. A method of implementing systems may
allow direct visibility to the buffer variables. Runtime service
calls may be provided to access the buffer variables.

The type mark used in the source variable declaration must match the
type name of the port data component type. Language-specific annexes
to this standard may specify restrictions on the form of a source
variable declaration to facilitate verification of compliance with
this rule.

 For each event or event data port declared for a thread, a method of
implementing the system must provide a source name that can be used
to refer to that event within source text. The applicable source
language annex of this standard defines this name and defines the
source constructs used to declare this name within the associated
source text. A method of implementing systems may require such
declarations to appear within source files that have been specified
in the source text property. In some implementations, these
declarations may be automatically generated for inclusion in the
final set of source text.

 If any source text associated with a software component contains a
runtime service call that operates on an event, then the enumeration
value used in that service call must have a corresponding event
feature declared for that component.

(5) A method of processing specifications is permitted to use
non-standard property definitions and associations to define
alternative queuing disciplines.

(6) A method of implementing systems is permitted to optimize the number
of port variables necessary to perform the transmission of data
between ports as long as the semantics of such connections are
maintained. For example, the source text variable representing an
out data port and the source text variable representing the
connected in data port may be mapped to the same memory location
provided their execution lifespan does not overlap.

Examples

**package** Nav\_Types **public**

**data** GPS **properties** Data\_Size => 30 Bytes; **end** GPS;

**data** INS **properties** Data\_Size => 20 Bytes; **end** INS;

**data** Position\_ECEF \ **properties** Data\_Size => 30 Bytes; **end**
Position\_ECEF;

**data** Position\_NED \ **properties** Data\_Size => 30 Bytes; **end**
Position\_NED;

**end** Nav\_Types;

**package** Navigation

**public**

**process** Blended\_Navigation

**features**

GPS\_Data : **in data port** Nav\_Types::GPS;

INS\_Data : **in data port** Nav\_Types::INS;

Position\_ECEF : **out data port** Nav\_Types::Position\_ECEF;

Position\_NED : **out data port** Nav\_Types::Position\_NED;

**properties**

-- the input rate of GPS is twice that of INS

Input\_Rate => ( Value\_Range => 50.0 .. 50.0; Rate\_Unit => perSecond ,
Rate\_Distribution => Fixed ) **applies to** GPS\_Data;

Input\_Rate => (Value\_Range => 100.0 .. 100.0; Rate\_Unit => perSecond
, Rate\_Distribution => Fixed ) **applies to** INS\_Data;

**end** Blended\_Navigation;

**process** **implementation** Blended\_Navigation.Simple

**subcomponents**

Integrate : **thread**;

Navigate : **thread**;

**end** Blended\_Navigation.Simple;

**end** Navigation;

Subprogram and Subprogram Group Access
--------------------------------------

 Subprograms and subprogram groups can be made accessible to other
components. Components can declare that they require access to
subprograms and subprogram groups. Components may provide access to
their subprograms and subprogram groups. Subprogram access is used
to model binding of a subprogram call (local or remote) to the
subprogram instance being called.

Syntax

-- The requires and provides subprogram access subclause

subprogram\_access\_spec ::=

*defining\_subprogram\_access*\ \_identifier :

( **provides** \| **requires** ) **subprogram** **access**

[ *subprogram*\ \_unique\_component\_classifier\_reference

\| *subprogram\_component\_prototype\_*\ identifier ]

*subprogram*\ \_access\_refinement ::=

*defining\_subprogram\_access*\ \_identifier : **refined to **

( **provides** \| **requires** ) **subprogram** **access**

[ *subprogram*\ \_unique\_component\_classifier\_reference

\| *subprogram\_component\_prototype\_*\ identifier ]

-- The requires and provides subprogram group access subclause

subprogram\_group\_access\_spec ::=

*defining\_subprogram\_group\_access*\ \_identifier :

( **provides** \| **requires** ) **subprogram group** **access**

[ *subprogram\_group*\ \_unique\_component\_classifier\_reference

\| *subprogram\_group\_component\_prototype\_*\ identifier ]

*subprogram\_group*\ \_access\_refinement ::=

*defining\_subprogram\_group\_access*\ \_identifier : **refined to **

( **provides** \| **requires** ) **subprogram group** **access**

[ *subprogram\_group*\ \_unique\_component\_classifier\_reference

\| *subprogram\_group\_component\_prototype\_*\ identifier ]

Naming Rules

1. The defining identifier of a provides or requires subprogram or
   subprogram group access declaration must be unique within the
   namespace of the component type where the subcomponent access is
   declared.

1. The defining identifier of a provides or requires subprogram or
   subprogram group refinement must exist as a defining identifier of a
   provides or requires subprogram or subprogram group or an abstract
   feature in the namespace of the component type being extended.

2. The component type identifier or component implementation name of a
   subprogram or subprogram group access classifier reference, if
   present, must exist in the package namespace.

3. The prototype identifier of a subprogram or subprogram group access
   classifier reference, if present, must exist in the namespace of the
   classifier that contains the access declaration.

Legality Rules

1. If a subprogram access refers to a component classifier or a
   component prototype, then the category of the classifier or
   prototype must be **subprogram**.

1. If a subprogram group access refers to a component classifier or a
   component prototype, then the category of the classifier or
   prototype must be **subprogram group**.

2. An abstract feature can be refined into a subprogram access or a
   subprogram group access. In this case, the abstract feature must
   not have a direction specified.

3. A subprogram or subprogram group access declaration that does not
   specify a component classifier reference is incomplete. Such a
   reference can be added in a subprogram or subprogram group access
   refinement declaration.

4. A subprogram or subprogram group access declaration may be refined by
   adding a property association. Inclusion of the component
   classifier reference is optional.

5. A provides subprogram access cannot be refined to a requires
   subprogram access and a requires subprogram access cannot be
   refined to a provides subprogram access. Similarly, a provides
   subprogram group access cannot be refined to a requires
   subprogram group access and a requires subprogram group access
   cannot be refined to a provides subprogram group access.

Consistency Rules

1. A *provides subprogram access* feature indicates that a subprogram is
   made available to be referenced. A project may enforce a consistency
   rule that a subprogram access connection connects this feature to
   directly a subprogram subcomponent, or indirectly via a *requires
   subprogram (group) access* or *provides subprogram (group) access*.

Standard Properties

-- Subprogram call rate for subprogram access

Subprogram Call Rate: Rate\_Spec => [ Value Range => 1.0 .. 1.0;
Rate\_Unit => PerDispatch; Rate Distribution => Fixed; ]

Queue Size: **aadlinteger** 0 **..** Max Queue\_Size => 1

Queue Processing\_Protocol: Supported Queue Processing Protocols => FIFO

Overflow Handling\_Protocol: **enumeration** (DropOldest, DropNewest,
Error)

=> DropOldest

Urgency: **aadlinteger** 0 **..** Max Urgency

Semantics

 A *required subprogram (group) access* declaration indicates that a
component requires access to a externally declared subprogram
(group). Required subprogram (group) accesses are resolved to
subprogram (group) subcomponents through access connection
declarations.

A *provides subprogram (group) access* declaration indicates that a
component provides access to a subprogram (group) subcomponent
contained in the component. Provided subprogram (group) accesses can
be used to resolve required subprogram (group) accesses.

 A subprogram that is accessed by more than one component is shared
and must be reentrant. The shared subprogram may be called by
multiple threads. This may result in concurrent access to shared
data components.

 If a different thread provides access to a subprogram then the call
is remote, i.e., executed by the thread with the provides subprogram
access feature. Otherwise the call is a local call, i.e., executed
by the calling thread.

(5) In case of a remote subprogram call, the requesting thread calls a
local proxy that carries out the service request. The proxy may
marshall and unmarshall the parameters as necessary. The execution
time of the client proxy is determined by the
Client\_Subprogram\_Execution\_Time property. The actual call
results in communicating the subprogram execution request to the
remote thread. While the call is in progress, the calling thread is
blocked. Upon completion of the remote subprogram execution and
return of results, the calling thread resumes execution by entering
the ready state. A semi-synchronous remote call may be supported
where the calling thread may issue the call and wait for the result
at a later time by calling Await\_Result (see Section 5.4.8). In
this case the caller may issue multiple remote calls to be executed
concurrently.

(6) If the called subprogram raises events or event data and the
subprogram call is a remote call, then the raised event in the
subprogram is mapped to the corresponding event or event data port
of the caller subprogram proxy.

(7) Each provides subprogram access feature of a thread that represents
an entrypoint to a remotely callable code sequence in the source
text. A request for execution of such a subprogram is a dispatch
request to the thread containing the subprogram. Requests for
execution of subprograms may be queued if the thread is already
executing a dispatch request. A thread can have multiple subprogram
entrypoints, expressed by multiple subprogram access feature
declarations. Only one of these subprograms may be executed per
thread dispatch. Queuing and queue servicing follows the semantics
of event port queues.

(8) Execution of subprogram calls may get blocked for two reasons. A
call may get blocked if the call is remote to a thread that services
calls and it is currently executing a dispatch, or it may get
blocked because the called subprogram operates in a shared data
component. This is the case, if the called subprogram is a *provides
subprogram (group) access* feature of a data component that itself
has shared access, i.e., is an access method of a data object, or if
a shared data component is accessible to the subprogram through a
requires data access feature of the subprogram. In the former case
the thread servicing the calls assures mutual exclusion, while other
remote calls to subprograms of the thread are queued. In the latter
case, concurrent access to the data component is assured to be
mutually exclusive according to the Concurrency\_Control\_Protocol
property value and realized through the Get\_Resource service call
in the source text, while other mutually exclusive access attempts
to shared data components are queued.

(9) Call\_Rate specifies the number of times per dispatch or per second
at which a subprogram is called.

Examples

-- a remote procedure call from one thread to another thread

**package** RemoteCallExample

**public**

**system** simple

**end** simple;

**system implementation** simple.impl

**subcomponents**

A: **process** caller\_P.i;

B: **process** remote\_P.i;

**connections**

AtoB: **subprogram access** A.DoCalc -> B.DoCalc;

**end** simple.impl;

**process** remote\_P

**features**

DoCalc: **provides subprogram access** Calc;

**end** remote\_P;

**process implementation** remote\_P.i

**subcomponents**

t1: **thread** Remote;

-- other subcomponent declarations

**connections**

t1conn: **subprogram access** t1.MyCalc -> DoCalc;

**end** remote\_P.i;

**thread** Remote

**features**

MyCalc: **provides subprogram access** Calc;

**end** Remote;

**process** caller\_P

**features**

DoCalc: **requires subprogram access** Calc;

**end** caller\_P;

**process implementation** caller\_P.i

**subcomponents**

Q: **thread** caller;

**connections**

calcconn: **subprogram access** DoCalc -> Q.MyCalc;

**end** caller\_P.i;

**thread** caller

**features**

MyCalc: **requires subprogram access** Calc;

**end** caller;

**subprogram** Calc

**end** Calc;

**end** RemoteCallExample;

-- A Printer Server Example

**package** PrinterServerExample

**public**

**process** printers

**features**

printonServer: **provides subprogram access** print;

mainPrinter: **in event** **port**;

backupPrinter: **in event** **port**;

**end** printers;

**process implementation** printers.threaded

**subcomponents**

A : **thread** printer **in modes** ( modeA );

B : **thread** printer **in modes** ( modeB );

**connections **

printtoA: **subprogram access** A.print -> printonServer **in modes**
(modeA);

printtoB: **subprogram access** B.print -> printonServer **in modes**
(modeB);

**modes**

modeA: **initial mode**;

modeB: **mode**;

modeA -[ backupPrinter ]-> modeB;

modeB -[ mainPrinter ]-> modeA;

**end** printers.threaded;

**thread** printer

**features**

print : **provides subprogram access** print;

**end** printer;

**subprogram** print

**features**

filetoprint: **in** **parameter** file;

**end** print;

**data** file

**end** file;

**process** application

**features**

print: **provides subprogram access** print;

**system** ApplicationSystem

**end** ApplicationSystem;

**system implementation** ApplicationSystem.default

**subcomponents**

app: **process** Application;

printserver: **process** Printers.threaded

**connections**

appconn: **subprogram access** printserver.printonServer -> app.print;

**end** ApplicationSystem.default;

**end** PrinterServerExample;

Subprogram Parameters
---------------------

 Subprogram parameter declarations represent data values that can be
passed into and out of subprograms. Parameters are typed with a data
classifier reference representing the data type.

Syntax

parameter\_spec ::=

*defining\_parameter*\ \_identifier :

( **in** \| **out** \| **in out** ) **parameter**

[ *data\_unique\_component\_classifier\_reference* \|

*data\_component\_prototype\_identifier* ]

parameter\_refinement ::=

*defining\_parameter*\ \_identifier : **refined to**

( **in** \| **out** \| **in out** ) **parameter**

[ *data\_unique\_component\_classifier\_reference* \|

*data\_component\_prototype\_*\ identifier ]

Naming Rules

1. The defining identifier of a parameter must be unique within the
   namespace of the subprogram type containing the parameter
   declaration.

1. The defining parameter identifier of a parameter refinement
   declaration must also appear in a feature declaration of a component
   type being extended and must refer to a parameter or an abstract
   feature.

2. The data classifier reference must refer to a data component type or
   a data component implementation.

3. The prototype identifier, if present, must exist in the namespace of
   the subprogram classifier that contains the parameter declaration.

Legality Rules

1. Parameters can be declared for subprogram component types.

1. A parameter declaration that does not specify a data classifier
   reference is incomplete. Such a reference can be added in a
   parameter refinement declaration.

2. A parameter declaration may be refined by adding a property
   association. Inclusion of the data classifier reference is
   optional.

3. The parameter direction of a parameter refinement must be the same as
   the direction of the feature being refined. If the feature being
   refined is an abstract feature without direction, then all
   parameter directions are acceptable.

Standard Properties

-- Properties specifying the source text representation of the parameter

Source Name: **aadlstring**

Source Text: **inherit list of aadlstring**

Semantics

 A subprogram parameter specifies the data that are passed into and
out of a subprogram. The data type specified for the parameter and
the data type of the actual data passed to a subprogram must be
compatible according to the Classifier\_Matching\_Rule.

An **in out** parameter declaration represents a parameter whose
value is passed in and returned by value. Parameters passed by
reference are modeled using **requires data access**.

1. .. rubric:: Data Component Access
  :name: data-component-access

 Data component access is used to model shared data. Data components
can be made accessible outside their containment hierarchy.
Components can declare that they require access to externally
declared data components. Components may provide access to their
data components.

 The use of component access for data components is illustrated in
Figure 12. Data2, Thread1, and Thread2 are subcomponents of a
process implementation. Thread1 contains a data subcomponent called
Data1. Data1 is made accessible outside Thread1 through a **provides
data access** feature declaration in the thread type of Thread1. It
is being accessed by Thread2 as expressed by a **requires data
access** feature declaration in the thread type of Thread2. Thread1
accesses data component Data2.

Figure − Containment Hierarchy and Shared Access


Syntax

-- The requires and provides subcomponent access subclause

data\_access\_spec ::=

*defining\_data\_component\_access*\ \_identifier :

( **provides** \| **requires** ) **data** **access**

[ *data*\ \_unique\_component\_classifier\_reference

\| *prototype\_*\ identifier ]

data\_access\_refinement ::=

*defining\_data\_component\_access*\ \_identifier : **refined to **

( **provides** \| **requires** ) **data** **access**

[ *data*\ \_unique\_component\_classifier\_reference

\| *prototype*\ \_identifier ]

Naming Rules

1. The defining identifier of a provides or requires data access
   declaration must be unique within the namespace of the component type
   where the data access is declared.

1. The defining identifier of a provides or requires data access
   refinement must exist as a defining identifier of a provides or
   requires data access or as a defining identifier of an abstract
   feature in the namespace of the component type being extended.

2. The component type identifier or component implementation name of a
   data access classifier reference must exist in the package namespace.

3. The prototype identifier, if present, must exist in the namespace of
   the classifier that contains the data access declaration.

Legality Rules

1. If a data access refers to a component classifier or a component
   prototype, then the category of the classifier or prototype must
   be of category **data**.

1. A data access declaration may be refined by refining the data
   classifier, by adding a property association, or by doing both.
   If the refinement only adds a property association the classifier
   reference is optional.

2. A provides data access cannot be refined to a requires data access
   and a requires data access cannot be refined to a provides data
   access.

3. An abstract feature can be refined into a data access. In this case,
   the abstract feature must not have a direction specified.

Consistency Rules

1. A data access declaration that does not specify a data classifier
   reference is incomplete. Such a reference can be added in a data
   access refinement declaration.

1. If the source code of a component does access shared data, then the
   component type declaration must specify a requires data access
   declaration. In other words, for all components that access shared
   data their component type declaration must reflect that fact.

2. A data access refinement may refine an abstract feature declaration.
   If the abstract feature declaration specifies a direction of **in**,
   then the access right of the data access must be read-only. If the
   direction is **out**, then the access right of the data access must
   be write-only. If the abstract feature does not have a specified
   direction, then any access right is acceptable.

Standard Properties

Access\_Right : Access\_Rights => read\_write

Reference\_Time: **inherit reference** (processor, device, bus, system,
abstract)

-- access time range for data access

Access\_Time: **record** (

First: IO\_Time\_Spec ;

Last: IO\_Time\_Spec ; )

=> [ First =>[Time => Start; Offset => 0.0 ns .. 0.0 ns;];

Last => [Time => Completion; Offset => 0.0 ns .. 0.0 ns;]; ]

Semantics

 A *requires data access* declaration in the component type indicates
that a component requires access to a component declared external to
the component. Required data accesses are resolved to actual data
subcomponents through access connection declarations. For data
components different forms of required access, such as read-only
access, are specified by a Access\_Right property. Read-only access
can also be specified through a directional access connection from
the data component to the *requires data access* feature, while
write-only access is specified through a directional access
connection to the data component.

A *provides data access* declaration in the component type indicates
that a subcomponent provides access to a data component contained in
the component. Provided data accesses can be used to resolve
required subcomponent access. For data and bus components different
forms of provided access, such as read-only access, are specified by
a Access\_Right property or be directional access connections.

 If a data access feature is a refinement of an abstract feature,
then the direction of the abstract feature, if specified, imposes a
restriction on the data flow, i.e., **in** implies read-only, and
**out** implies write-only.

 Shared data may be accessed by multiple threads. Such potential
concurrent access is controlled according to the
Concurrency\_Control\_Protocol property.

(5) Access\_Time specifies the time range over which a component has
access to a shared data component. By default access is required for
the duration of the component execution. The value of a shared data
component is read or written through the use of a data variable that
represents the shared data component, or through Get\_Value and
Put\_Value service calls. Write access immediately updates the
shared data component.

Examples

**package** Example

**public**

**system** simple

**end** simple;

**system implementation** simple.impl

**subcomponents**

A: **process** pp.i;

B: **process** qq.i;

**connections**

**data access** A.dataset -> B.Reqdataset;

**end** simple.impl;

**process** pp

**features**

Dataset: **provides data access** dataset\_type;

**end** pp;

**process implementation** pp.i

**subcomponents**

Share1: **data** dataset\_type;

-- other subcomponent declarations

**connections**

**data access** Share1 <-> Dataset;

**end** pp.i;

**process** qq

**features**

Reqdataset: **requires data access** dataset\_type;

**end** qq;

**process implementation** qq.i

**subcomponents**

Q: **thread** rr;

**connections**

**data access** Reqdataset <-> Q.Req1;

**end** qq.i;

**thread** rr

**features**

Req1: **requires data access** dataset\_type;

**end** rr;

**data** dataset\_type

**end** dataset\_type

**end** Example;

Bus Component Access
--------------------

 Bus components can be made accessible to other components.
Components can declare that they require access to externally
declared buses. Components may provide access to their buses. Bus
access is used to model connectivity of execution platform
components through buses.

 Figure 13 illustrates the use of a shared bus. Bus1 provides the
connection between Processor1, Memory1, and Device1. In addition,
the bus is being made accessible outside System1.

Figure − Shared Bus Access
  

Syntax

-- The requires and provides bus access subclause

bus\_access\_spec ::=

*defining\_bus\_access*\ \_identifier :

( **provides** \| **requires** ) **bus** **access**

[ *bus*\ \_unique\_component\_classifier\_reference

\| *prototype*\ \_identifier ]

bus\_access\_refinement ::=

*defining\_bus\_access*\ \_identifier : **refined to **

( **provides** \| **requires** ) **bus** **access**

[ *bus*\ \_unique\_component\_classifier\_reference

\| *prototype*\ \_identifier ]

Naming Rules

1. The defining identifier of a provides or requires bus access
   declaration must be unique within the namespace of the component type
   where the bus access is declared.

1. The defining identifier of a provides or requires bus refinement must
   exist as a defining identifier of a requires or provides bus access
   or of an abstract feature in the namespace of the component type
   being extended.

2. The component type identifier or component implementation name of a
   bus access classifier reference must exist in the package namespace.

3. The prototype identifier, if present, must exist in the namespace of
   the classifier that contains the bus access declaration.

Legality Rules

1. If a bus access refers to a component classifier or a component
   prototype, then the category of the classifier or prototype must
   be of category **bus**.

1. A bus access declaration may be refined by refining the bus
   classifier, by adding a property association, or by doing both.
   If the refinement only adds a property association the bus
   classifier reference is optional.

2. A provides bus access cannot be refined to a requires bus access and
   a requires bus access cannot be refined to a provides bus access.

3. An abstract feature can be refined into a bus access.

Consistency Rules

1. A bus access declaration that does not specify a bus classifier
   reference is incomplete. Such a reference can be added in a bus
   access refinement declaration.

2. If a bus access feature is a refinement of an abstract feature, then
   the direction of the abstract feature, if specified, imposes a
   restriction on the access right, i.e., **in** implies read-only, and
   **out** implies write-only.

Standard Properties

Access\_Right : Access\_Rights => read\_write

Semantics

 A *required bus component access* declaration in the component type
indicates that a component requires access to a component declared
external to the component. Required bus accesses are resolved to
actual bus subcomponents through access connection declarations.

A *provides bus access* declaration in the component type indicates
that a subcomponent provides access to a bus contained in the
component. Provided bus accesses can be used to resolve required bus
access. For bus components different forms of provided access, such
as read-only access, are specified by a Access\_Right property or by
directional access connections.

 A bus that is accessed by more than one component is shared. The
shared bus is a common resource through which processor, memory, and
device components communicate.

Examples

**package** Example2

**public**

**with** Buses;

**system** simple

**end** simple;

**system implementation** simple.impl

**subcomponents**

A: **processor** PPC;

B: **device** DigCamera;

**connections**

**bus access** A.USB1 <-> B.USB2;

**end** simple.impl;

**processor** PPC

**features**

USB1: **provides bus access** Buses::USB;

**end** PPC;

**device** DigCamera

**features**

USB2: **requires bus access** Buses\ **::**\ USB;

**end** DigCamera;

**end** Example2;

 Virtual bus components can be connected to each other and to other
virtual platform components. These are virtual processor, device,
system, and abstract component. Components can declare that they
require access to virtual buses. Components may provide access to
virtual buses.

Syntax

virtual\_bus\_access\_spec ::=

*defining\_virtual\_bus\_access*\ \_identifier :

( **provides** \| **requires** ) **virtual bus** **access**

[ *virtual\_bus*\ \_unique\_component\_classifier\_reference

\| *prototype*\ \_identifier ]

virtual\_bus\_access\_refinement ::=

*defining\_virtual\_bus\_access*\ \_identifier : **refined to **

( **provides** \| **requires** ) **virtual bus** **access**

[ *virtual\_bus*\ \_unique\_component\_classifier\_reference

\| *prototype*\ \_identifier ]

Naming Rules

1. The defining identifier of a provides or requires virtual bus access
   declaration must be unique within the namespace of the component type
   where the virtual bus access is declared.

1. The defining identifier of a provides or requires virtual bus
   refinement must exist as a defining identifier of a requires or
   provides virtual bus access or of an abstract feature in the
   namespace of the component type being extended.

2. The component type identifier or component implementation name of a
   virtual bus access classifier reference must exist in the package
   namespace.

3. The prototype identifier, if present, must exist in the namespace of
   the classifier that contains the virtual bus access declaration.

Legality Rules

1. If a virtual bus access refers to a component classifier or a
   component prototype, then the category of the classifier or
   prototype must be of category **virtual bus**.

1. A virtual bus access declaration may be refined by refining the
   virtual bus classifier, by adding a property association, or by
   doing both. If the refinement only adds a property association
   the bus classifier reference is optional.

2. A provides virtual bus access cannot be refined to a requires virtual
   bus access and a requires virtual bus access cannot be refined to
   a provides virtual bus access.

3. An abstract feature can be refined into a virtual bus access.

Semantics

 A *requires virtual bus access* declaration in the component type
indicates that a component requires access to a virtual bus.

A *provides virtual bus access* declaration indicates that a virtual
bus is made accessible externally.

 Requires and provides virtual bus accesses are resolved to actual
virtual bus subcomponents through virtual bus access connection
declarations.

1. .. rubric:: Internal Features and Processor Features
  :name: internal-features-and-processor-features

 Internal features and processor features allow users to explicitly
declare placeholders used in connection declarations whose endpoints
include the keyword **self** or **processor**.

(5) Internal features represent sources of events and event data that
are internal to the component. They are referenced in connections
prefixed with the keyword **self**.

(6) Processor features act as proxies for features in processors that a
software component is bound to. Processor features are referenced in
connection declarations that represents calls to processor API
subprograms or event flows from processor ports to application
software.

Syntax

internal\_feature ::=

defining\_internal\_feature\_identifier :

( **event** \|

**event data** [ data\_unique\_component\_classifier\_reference ]

)

processor\_feature ::=

defining\_processor\_feature\_identifier :

( **port** [ data\_unique\_component\_classifier\_reference ] )

\|

**subprogram** [ subprogram\_unique\_component\_classifier\_reference ]

)

Naming Rules

1. The defining identifier of an internal feature or processor feature
   declaration must be unique within the namespace of the component
   implementation where it is declared.

1. The qualified name of a data or subprogram classifier reference must
   exist in the package namespace.

Semantics

 An *internal feature* declaration in the component implementation
indicates a component internal event or data source, e.g., the
detection point of an error condition as specified in the Error
Model Annex. Connection declarations in the component implementation
may refer to internal features by prefixing the reference with the
keyword **self**.

A *processor feature* declaration in the component implementation
acts as proxy for features in a processor the component may be bound
to and have connection declarations that refer to the features as
endpoints – prefixed with the keyword **processor**.

