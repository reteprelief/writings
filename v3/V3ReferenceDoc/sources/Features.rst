Features and Shared Access
==========================


A *feature* represents an interaction point of a component that is visible to
other components.  
Software related features are: *ports* to support directional flow of events and data whose receipt is sampled or queued, 
*data access* feature for reading and writing shared data, *subprogram access* features for representing remote service calls, *subprogram group access* features for representing access to subprogram libraries, and *subprogram parameters*.
Hardware related features are: *bus access* features to represent hardware interactions via buses, and *binding points* are used in deployment specification of software components to the compute platform. 
*Abstract features* are used to represent general interaction points without specific interaction semantics.
*Named interfaces* represent collections of features defined in a component interface. The collection or individual elements can be specified to interact with other components.

A *feature* is a part of a component interface definition that specifies
the points through which the component interacts with other components in the system.

Features can be declared as one-dimensional feature arrays. Such
feature arrays complement component arrays and allow for connection
patterns that connect a feature of each of the component array
elements to a feature array element of one component. An example use
is redundant replicas of a component passing their output to a
voting component or a routing component.

Syntax
------

| feature ::=
|   ( port\_definition \| data\_access\_definition
|   \| sub\_program\_access\_definition \| subprogram\_group\_access\_definition \| subprogram\_parameter\_definition
|   \| bus\_access\_definition \| virtual\_bus\_access\_definition
|   \| abstract\_feature\_definition \| named\_interface\_definition )
|   [ array\_dimension ]
|   [ **{** \_property\_association { **;** property\_association [ **;**] }\ :sup:`*` **}** ] **;**
|
| feature\_reference ::= [ component\_path **.** ] feature\_path 
|
| component\_path ::= subcomponent\_identifier { **.** subcomponent\_identifier }\ :sup:`*`
|
| feature\_path ::= feature\_identifier { **.** feature\_identifier }\ :sup:`*`

Naming Rules
------------

1. The defining identifier of a feature is part of the local name space of a component interface.

#. A named interface declaration introduces a local name space for the content of the referenced interface.

#. A feature identifier of a feature path must exist in the name space of the preceding path element. The first feature identifier must exist in the name space of the subcomponent identified by the component path or in the name space of the context in which the reference is declared.


Legality Rules
--------------

1. For features with a classifier or data type reference, the reference can be configured with a configuration assignment. If the feature definition does not include such a reference any reference is acceptable.
If the definition includes such a reference, then it can be substituted by reference that is an extension, or by any reference when a configuration pattern is used.


Semantics
---------

A feature is referenced in one of two ways. Within the component
implementations a feature defined in its interface is referenced by a feature path, which identifies the feature possibly inside a named interface. 
A feature of a possibly nested subcomponent is referenced by subcomponent path followed by a feature path separated by a . (dot).

Features can be declared with an array dimension and size. Features in a feature array are referenced by an array index.

A feature may specify desired quality of service to be used for connections or bindings through the feature. 

Abstract Features
-----------------

Abstract features represent generic features without specific software related interaction semantics. It is often used in conceptual and
physical system models. 

Syntax
~~~~~~

| abstract\_feature\_definition ::=
|   *defining*\_identifier : [ **in** \| **out** ] **feature** 
|     [ data\_type\_reference ] 


Semantics
~~~~~~~~~

Abstract features represent generic features without specific software related interaction semantics. They are often used in conceptual and
physical system models. 

Abstract features may be specified without direction or they may indicate a direction.

Named Interfaces
----------------


Syntax
~~~~~~

| named\_interface\_definition ::= 
|   *defining*\_identifier : **interface** 
|     [ [ **reverse** ] component\_interface\_reference ] 

Naming Rules
~~~~~~~~~~~~

1. The named interface inherits the name space of the referenced component interface. 

Semantics
~~~~~~~~~

A named interface definition allows a collection of features defined in a component interface to be identified by a unique name. 
This allows users to define interface compositions in which the same component interface is included multiple times. 
Named interface declarations can also be used to address name conflicts between features of different interfaces in an interface composition.

Examples
~~~~~~~~

| package GPS\_Interface
|   import GPSLib::\*;
|
|   interface GPSInterface
|     Wakeup: in event port;
|     Observation: out data port position;
|   end;
|
|   process Satellite\_position
|     position: interface reverse GPS;
|   end ;
| 
|   process GPS\_System
|     position: interface GPS;
|   end;
|
|   system interface Satellite
|   end;
|
|   system Satellite.basicb
|     SatPos: process Satellite\_position;
|     MyGPS: process GPS\_System;
|     satconn: connection Satpos.position <-> MyGPS.position;
|   end;
| end;

Ports
-----

Ports represent interaction points for discrete directional message communication between components as specified by connections. 
Messages can be data with a specified data type (*data port* or *event data port*) or represent events without additional information (*event port*).

Messages may trigger the dispatch of the recipient to process its inputs. 
Messages may be queued at the recipient port (*event port* or *event data port*) or they may contain only the most recent message (*data port*).  

*Event data ports* are used to represent communication of data as messages that are queued. 
The recipient may process one message at a time or may periodically process the queue.

*Data ports* are typically used in control systems to communicate sensor readings and other state information as signal streams that are
sampled and processed in control loops. 

*Event ports* are typically used to represent
discrete events in the physical environment, such as a button push,
in the computing platform, such as a clock interrupt, or a logical
discrete event, such as an alarm.
 
During the execution of a recipient the received data is not affected by the arrival of new messages 
unless the recipient explicitly requests additional input. This ensures the integrity of data processing. Details of the communication timing semantics are described below.


Syntax
~~~~~~

| port\_definition ::=
|   *defining*\_port\_identifier : ( **in** \| **out** \| **in out** ) 
|   ( [ **event** ] **data port** [ data\_type\_reference ]
|     \| **event port** )


Legality Rules
~~~~~~~~~~~~~~

1. Each component category section specifies whether ports can be defined. 

#. Data and event data ports may be incompletely defined by not
   specifying the data type reference. The data type can be configured into the model by configuration assignment.

Standard Properties
~~~~~~~~~~~~~~~~~~~

| -- Input and output rate and time
|
|  Input Rate: Rate Spec => [ Value\_Range => 1.0 .. 1.0; Rate Unit => PerDispatch; Rate Distribution => Fixed; ]
|
| Input Time: list of IO Time\_Spec => ([ Time => Dispatch; Offset => 0.0 ns .. 0.0 ns;])
|
| Output Rate: Rate\_Spec => [ Value Range => 1.0 .. 1.0; Rate\_Unit => PerDispatch; Rate Distribution => Fixed; ]
|
| Output Time: list of IO Time Spec => ([ Time => Completion; Offset => 0.0 ns .. 0.0 ns;])
|
| -- port queue properties
|
| Overflow Handling Protocol: enumeration (DropOldest, DropNewest, Error) => DropOldest
|
| Queue Size: integer 0 .. Max\_Queue\_Size => 1
|
| Queue Processing Protocol: Supported Queue Processing Protocols => FIFO
|
| Dequeue Protocol: enumeration ( OneItem,  AllItems ) => OneItem
|
| Fan Out Policy: enumeration (RoundRobin, OnDemand)
|
| -- Optional property for device ports
|
| Device Register Address: integer

Semantics
~~~~~~~~~

Data, messages, and events that arrive at a port are made available to the recipient by default at dispatch time or at a user-specified time. 
Any newly arriving data, message, or event does not affect the value made to the recipient until the next recipient dispatch or an explicit receive operation by the recipient.
The *Input\_Time* property is used to explicitly specify an input time for ports with a list of values indicating multiple input receipts:

-  Dispatch: (the default value) input is made available at dispatch time.

-  Start, time range: input is made available at a specified amount of execution
   time from the beginning of execution. The time is within the
   specified time range. The time range must have positive values.
   Start\ :sub:`low` ≤ c ≤ Start\ :sub:`high`. 
   This specification is typically used when the application code makes explicit receive calls.

-  Completion, time range: input is made available at a specified amount of
   execution time relative to execution completion. The time is within
   the specified time range. A negative time range indicates execution
   time before completion. c\ :sub:`complete` + Completion\ :sub:`low` ≤
   c ≤ c\ :sub:`complete` + Completion\ :sub:`high`, where
   c\ :sub:`complete` represents the value of c at completion time. 
   This specification is typically used when the application code makes explicit receive calls.

-  NoIO: the port is excluded from making new input available to the source program. This allows users
   to specify that a subset of ports to provide input. The property
   value can be mode specific, i.e., a port can be excluded in one mode
   and included in another mode.

The *Queue\_Size* property specifies the size of the queue of incoming event or event data ports. By default, the queue size is 1.

The *Queue\_Processing\_Protocol* specifies how the queue content is processed. 
By default processing occurs in a first-in, first-out
order (FIFO). 

The *Overflow\_Handling\_Protocol* property determines what happens when the queue is full and a message arrives:
- DropOldest (default): the oldest element in the queue is dropped.
- DropNewest: the newly arriving element is dropped.

The *Dequeue\_Protocol* property specifies different dequeuing options:
-  OneItem (default): a single message or event is dequeued and made available to the recipient. If the queue is empty no event or message is dequeued. 
The application code can determine the number of dequeued events or messages. 
-  AllItems: all messages or events are dequeued and made available to the recipient. The application code can determine the number of dequeued events or messages.

Incoming data ports effectively behave like event data ports with a queue size of 1. The application code can determine whether the available data is *fresh*, i.e., was made available at the current dispatch or at a previous dispatch.

A port may be serviced by multiple recipients. In this case the *Fan\_Out\_Policy* property indicates
how the content distributed:
- OnDemand (default): the recipient ready to accept event or message as input,
- RoundRobin: even distribution across recipients.


Messages on outgoing ports are send at completion time or at user-specified times through explicit send calls. 
The *Output\_Time* property is used to explicitly specify an
output time for ports with a list of values indicating multiple output sends:

-  Start, time range: output is transmitted at a specified amount of
   execution time relative to the beginning of execution. The time is
   within the specified time range. The time range must have positive
   values. Start\ :sub:`low` ≤ c ≤ Start\ :sub:`high`.

-  Completion, time range: output is transmitted at a specified amount
   of execution time relative to execution completion. The time is
   within the specified time range. A negative time range indicates
   execution time before completion. c\ :sub:`complete` +
   Completion\ :sub:`low` ≤ c ≤ c\ :sub:`complete` +
   Completion\ :sub:`high`, where c\ :sub:`complete` represents the value
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

The *Input\_Rate* and *Output\_Rate* properties specify the number of
times per dispatch (perDispatch) or per second (perSecond) at which
input and output is expected to occur at the port with the
associated property. By default the input and output rate of ports
is once per dispatch. The rate can be fixed or according to a
distribution. 
An input or output rate higher than once per dispatch indicates that
multiple inputs or multiple outputs are expected during a single
dispatch. An input or output rate lower than once per dispatch
indicates that inputs or outputs are not expected at every dispatch.

Processing Requirements and Permissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Ports may be represented by variables that are accessible to the application code. 
The runtime system may make the received data and number of received elements available through such a variable.

Events, messages, and data may be sent and received by application code calling on a set of service routines. 

The Code Generation Annex provides details of port variables and service routines.


Examples
~~~~~~~~

| -- a thread that gets input part way into execution and sends output before completion.
|
| package Navigation
|   type GPS { Data\_Size => 30 Bytes };
|   type INS { Data\_Size => 20 Bytes }; 
|   type Position { Data\_Size => 30 Bytes};
|
|   thread interface Blended\_Navigation
|     GPS\_Data : in data port GPS;
|     INS\_Data : in data port INS;
|     Position : out data port Position;
|     GPS\_Data#Input\_Time => ((Time=>Start;Offset=>2 us .. 3 us));
|     INS\_Data#Input\_Time => ((Time=>Start;Offset=>2 us .. 3 us));
|     Position#Output\_Time => ((Time=>Completion;Offset=>3 us .. 4 us));
|   end ;
| end ;
|
|
| -- An example with a port as feature array
| package Patterns
|  
|   thread interface Voter
|     Input: in data port [3];
|     Output: out data port;
|   end;
|  
|  thread interface Processing
|     Input: in data port;
|     Result: out data port;
|   end;
|  
|   thread group interface Redundant\_Processing
|     Input: in data port;
|     Result: out data port;
|   end;
|  
|   thread group Redundant\_Processing.basic
|     processing: thread Processing [3];
|     voting: thread voter;
|     voteconn: connection processing.Result -> voting.Input
|       { Connection\_Pattern => ((One\_To\_One)) };
|     procconn: connection Input -> processing.Input;
|     recon: connection voting.Output -> Result;
|   end;
| end;

Data Component Access
---------------------

Data component access definitions indicate that data components are accessible for reading and writing by components within the same component as the data component or
by components external to the containing component. 


Syntax
~~~~~~

| data\_access\_definition ::=
|   *defining*\_identifier **:** ( **provides** \| **requires** ) 
|   ( **read** \| **write** \| **readwrite** ) **access** [ data\_type\_reference ]

Legality Rules
~~~~~~~~~~~~~~

1. Each component category section specifies whether data access features can be defined. 

#. If the data access feature is defined in a data component interface and does have a data type reference, it must be to the data type of the component interface; if the data type reference is not present it is assumed to be that of the data component interface. 

#. The data type reference of a data access feature can be configured by configuration assignment. A configuration assignment may assign or override any data type. 


Standard Properties
~~~~~~~~~~~~~~~~~~~

| -- access time range for data access
|
| Access\_Time: **record** (
|   First: IO\_Time\_Spec ;
|   Last: IO\_Time\_Spec )

Semantics
~~~~~~~~~

Data access features are used to model access to shared data components. 

The **provides** keyword indicates that the component with the data access feature provides access to a data component, either itself if it is a data component or to a data component contained in the given component.

The **requires** keyword indicates that the component with the data access feature requires access to a data component that is external to the given component.

The **read**, **write** and **readwrite** keywords indicate whether the access is for reading or writing.

The data type indicates the type of the data component to be accessed.

The *Access\_Time* property specifies the time range over which a component has
access to a shared data component. By default access is required for
the duration of the component execution. 

Examples
~~~~~~~~

| package Sampling
|
| type Sample { #Data\_Size => 16 Bytes };
|
| type Sample\_Set { #Data\_Size => 1 MByte };
|
| end ;
|
| package SamplingTasks
|   with Sampling;
|
|   thread interface  Init\_Samples
|     OrigSet: requires read access Sample\_Set;
|     SampleSet: requires write access Sample\_Set;
|   end ;
|
|   thread interface  Collect\_Samples
|     Input\_Sample: in data port Sample;
|     SampleSet: requires write access Sample\_Set;
|   end ;
|
|   thread Collect\_Samples.Batch\_Update
|     Input\_Sample#Source\_Name => InSample;
|   end ;
|
|   thread interface Distribute\_Samples
|     SampleSet: requires read access Sample\_Set;
|     UpdatedSamples : out data port :Sample;
|   end ;
|
|   process Sample\_Manager
|     Input\_Sample: in data port Sample;
|     External\_Samples: requires read access Sample\_Set;
|     Result\_Sample: out data port Sample;
|   end ;
|
|   process Sample\_Manager.Slow\_Update
|     Samples: data Sample\_Set { rw: provides readwrite access Sample_Set};
|     Init\_Samples : thread Init\_Samples;
|     -- the required access is resolved to a subcomponent declaration
|     Collect\_Samples: thread Collect\_Samples.Batch\_Update;
|     Distribute: thread Distribute\_Samples;
|     ISSSConn: connection Init\_Samples.SampleSet -> Samples.rw;
|     ISOSConn: mapping External\_Samples => Init\_Samples.OrigSet;
|     CSSSConn: connection Collect\_Samples.SampleSet -> Samples.rw;
|     CSISConn: mapping Input\_Sample => Collect\_Samples.Input\_Sample;
|     DSSConn: connection Distribute.SampleSet -> Samples.rw;
|     DUSConn: mapping Result\_Sample => Distribute.UpdatedSamples ;
|   end ;
|
| end ;

Subprogram and Subprogram Group Access
--------------------------------------

Subprogram and subprogram group access definitions indicate that subprogram components and subprogram group (library) components are accessible to components external to the component with the access feature. 


Syntax
~~~~~~

| subprogram\_access\_definition ::=
|   *defining*\_identifier **:** ( **provides** \| **requires** ) 
|   ** subprogram** **access** [ subprogam\_interface\_reference ]
|
| subprogram\_group\_access\_definition ::=
|   *defining*\_identifier **:** ( **provides** \| **requires** ) 
|   ** subprogram group access** [ subprogam\_group\_interface\_reference ]

Legality Rules
~~~~~~~~~~~~~~

1. Each component category section specifies whether subprogram and subprogram group access features can be defined. 

#. The subprogram or subprogram group interface reference can be configured by configuration assignment. A configuration assignment may assign an interface reference if the original definition has none specified. 

Standard Properties
~~~~~~~~~~~~~~~~~~~

| -- Subprogram call rate for subprogram access
|
| Subprogram\_Call\_Rate: Rate\_Spec => [ Value Range => 1.0 .. 1.0;
|    Rate\_Unit => PerDispatch; Rate Distribution => Fixed; ]
|

Semantics
~~~~~~~~~

Subprogram access is used to model binding of a subprogram call to the
subprogram instance being called. This call may be a local call or a call to a service provided by another thread or collection of threads.  

The **provides** keyword indicates that the component with the subprogram or subprogram group access feature provides access to a subprogram or subprogram group component.

The **requires** keyword indicates that the component with the subprogram or subprogram group access feature requires access to a subprogram or subprogram group component that is external to the given component.


The *Subprogram\_Call\_Rate* property specifies the number of times per dispatch or per second
at which a subprogram is called.


Subprogram Parameters
---------------------

Subprogram parameter definitions represent parameters of subprograms.

Syntax
~~~~~~

| subprogram\_parameter\_definition ::=
|   *defining*\_identifier **:** ( **in** \| **out** \| **in out** ) **parameter** [ data\_type\_reference ]

Legality Rules
~~~~~~~~~~~~~~

1. Subprogram parameter features can be defined in subprogram interface definitions. 

#. The data type reference of a subprogram parameter feature can be configured by configuration assignment. A configuration assignment may assign or override any data type. 

Semantics
~~~~~~~~~

A subprogram parameter specifies the data that are passed into and
out of a subprogram. 


Bus Component Access
--------------------

Bus access definitions indicate that buses are accessible to other hardware components external to the containing component. 


Syntax
~~~~~~

| bus\_access\_definition ::=
|   *defining*\_identifier **:** ( **provides** \| **requires** ) 
|   **bus** **access** [ bus\_interface\_reference ]

Legality Rules
~~~~~~~~~~~~~~

1. Each component category section specifies whether bus access features can be defined. 

#. If the bus access feature is defined in a bus component interface and does have a bus interface reference, it must be to the enclosing bus interface; if the bus interface reference is not present it is assumed to be that of the enclosing bus interface. 

#. The bus interface reference of a bus access feature can be configured by configuration assignment. A configuration assignment may assign a bus interface if not present in the definition. 


Semantics
~~~~~~~~~

Bus access features are used to model connectivity of execution platform
components through buses. Multiple components may communicate through the same bus.

The **provides** keyword indicates that the component with the bus access feature provides access to a bus component, either itself if it is a bus component or to a bus component contained in the given component.

The **requires** keyword indicates that the component with the bus access feature requires access to a bus component that is external to the given component.

The bus interface reference indicates the type of the bus component to be accessed.

Examples
~~~~~~~~

| **package** Example2
|
| **system interface** simple **end** ;
|
| **system** simple.impl
|   PC: **processor** PPC;
|   Cam: **device** DigCamera;
|   usb1: **connection** PC.USB1 -> Cam.USB2;
| **end** ;
|
| **processor** PPC
|   USB1: **provides bus access** USB;
| **end** ;
| 
| **device** DigCamera
|   USB2: **requires bus access** USB;
| **end** ;
|
| **bus USB **end**;
|
| **end** ;


Virtual Bus Component Access
----------------------------

Virtual bus access definitions indicate that virtual buses are accessible to other components external to the containing component. 


Syntax
~~~~~~

| virtual\_bus\_access\_definition ::=
|   *defining*\_identifier **:** ( **provides** \| **requires** ) 
|   **virtual bus** **access** [ virtual\_bus\_interface\_reference ]

Legality Rules
~~~~~~~~~~~~~~

1. Each component category section specifies whether virtual bus access features can be defined. 

#. If the virtual bus access feature is defined in a virtual bus component interface and does have a virtual bus interface reference, it must be to the enclosing virtual bus interface; if the virtual bus interface reference is not present it is assumed to be that of the enclosing virtual bus interface. 

#. The virtual bus interface reference of a virtual bus access feature can be configured by configuration assignment. A configuration assignment may assign a virtual bus interface if not present in the definition. 


Semantics
~~~~~~~~~

Virtual bus access features are used to model connectivity of logical platform
components through virtual buses. Multiple components may communicate through the same virtual bus.

The **provides** keyword indicates that the component with the virtual bus access feature provides access to a virtual bus component, either itself if it is a virtual bus component or to a virtual bus component contained in the given component.

The **requires** keyword indicates that the component with the virtual bus access feature requires access to a virtual bus component that is external to the given component.

The virtual bus interface reference indicates the type of the virtual bus component to be accessed.


Binding Point
-------------

Binding point definitions indicate that components require to be bound to components providing a particular resource type. 


Syntax
~~~~~~

| binding\_point\_definition ::=
|   *defining*\_identifier **:** ( **provides** \| **requires** ) [ *resource*\_data\_type\_reference ]

Legality Rules
~~~~~~~~~~~~~~

1. Each component category section specifies whether binding points can be defined. 

#. The resource data type reference of a binding point can be configured by configuration assignment. A configuration assignment may assign a resource type if not present in the definition. 


Semantics
~~~~~~~~~

Binding points are used to model deployment binding of components to components in an underlying platform. Such components typically provide resources such as processing cycles or storage.
Multiple components may be bound to the same component binding point.

The **provides** keyword indicates that the component with the binding point provides resources of the specified type.

The **requires** keyword indicates that the component with the binding point requires resources of the specified type.

The resource data type reference indicates the type of resource involved in the binding.

The required or provided resource amount can be specified as part of the binding point.
