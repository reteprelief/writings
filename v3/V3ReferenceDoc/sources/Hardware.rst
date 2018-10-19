
Execution Platform Components
=============================

 This section describes the categories of execution platform
components that represent computing hardware: processor, virtual
processor, memory, bus, virtual bus; and the physical environment:
device.

  Processors can execute threads. Processors can contain memory
 subcomponents. Processors can access memories and communicate with
 devices and other processors over buses. Threads, thread groups,
 and processes are bound to processors.

 Virtual processors are logical execution platform elements that can
 execute threads. Threads, thread groups, and processes can be bound
 to virtual processors. Virtual processors must be bound to or
 contained in processors. This determines the binding of threads to
 processors.

  Memories represent randomly addressable storage capable of storing
 binary images in the form of data and code. Memories can be
 accessed by executing threads.

  Buses support physical communication between processors, devices,
 and memories. A bus provides the resources necessary to perform
 exchanges of control and data as specified by connections. These
 resources include bandwidth and protocols to perform the exchange.
 A connection may be bound to a sequence of buses and intermediate
 processors and devices in a manner that is analogous to the binding
 of threads to processors.

(5)  Virtual buses represent a logical bus abstraction to model
 protocols and virtual channels. Virtual buses can be contained in
 or bound to processors and buses. Connections can specify that they
 require specific protocols, or certain quality of service from
 protocols of platform components they are bound to.

(6)  Devices represent entities of the physical environment, e.g., an
 engine, or entities that interface with the physical environment,
 e.g., a sensor or an actuator. A device can interact with
 application software components through their port and provides
 subprogram access features. A device may interact with other
 execution platform components through bus access connections. A
 device may achieve its functionality through device internal
 software or may require device driver software to be executed by a
 processor. Binary images or threads cannot be bound to devices.

(7)  Processors may include software that implements the capability of
 the processor to schedule and execute threads and other services of
 the processor. Its source text and data in the form of binary
 images will be bound to memories accessible from that processor.
 The resource requirements of this software are reflected in
 processor properties.

(8)  Execution platform components can be assembled into execution
 platform systems, i.e., into systems of execution platform
 components to model complex physical computing hardware components
 and software/hardware computing systems, through the use of system
 components (see Section 7.1). The execution platform systems and
 their components may denote physical computing hardware for
 example, memory to represent a hard disk or RAM. Execution platform
 systems may also model abstracted storage, for example, a device or
 memory to represent a database, depending on the purpose of the
 model.

(9)  The hardware represented by the execution platform components may
 be modeled in a hardware description or simulation language.
 Alternatively, it may be represented using configuration data for
 programmable logic devices. A simulation may be used to
 characterize the components. Such descriptions can be associated
 with the component by property association.

(10) Execution platform components can represent high-level abstractions
 of physical and computing components. A detailed AADL model of
 their implementation can be represented by system implementations
 that are associated with the execution platform component by
 property (see Section 14). This effectively models a layered system
 architecture.

Processors
----------

 A processor is an abstraction of hardware and software that is
responsible for scheduling and executing threads and virtual
processors that are bound to it. A processor also may execute driver
software that is declared as part of devices that can be accessed
from that processor. Processors may contain memories and may access
memories and devices via buses.

Legality Rules

+-----------------+---------------------------------------+------------------------+
| **Category**| **Type**  | **Implementation** |
+-----------------+---------------------------------------+------------------------+
| **processor**   | Features: | Subcomponents: |
| |   ||
| | -  provides subprogram access | -  memory  |
| |   ||
| | -  provides subprogram group access   | -  bus |
| |   ||
| | -  port   | -  virtual processor   |
| |   ||
| | -  feature group  | -  virtual bus |
| |   ||
| | -  requires bus access| -  abstract|
| |   ||
| | -  provides bus access||
| |   ||
| | -  requires virtual bus access||
| |   ||
| | -  provides virtual bus access||
| |   ||
| | -  feature||
+-----------------+---------------------------------------+------------------------+

1. A processor component type can contain port, feature group, provides
   subprogram access, provides subprogram group access, virtual buss
   access, and bus access declarations. It may contain flow
   specifications, a modes subclause, as well as property
   associations.

1. A processor component implementation can contain declarations of
   memory, bus, virtual bus, virtual processor, and abstract
   subcomponents.

2. A processor implementation can contain a modes subclause, flows
   subclause, and a properties subclause.

3. A processor implementation can contain bus access, subprogram access,
   subprogram group access, port, feature, and feature group
   connection declarations.

4. A processor implementation must not contain a subprogram calls
   subclause.

Standard Properties

-- Hardware description properties

Hardware Description Source Text: **inherit** **list of aadlstring**

Hardware Source Language: Supported Hardware Source Languages

-- Properties related to source text that provides thread scheduling
services

Source Text: **inherit list of aadlstring**

Source Language: **inherit list of** Supported\_Source\_Languages

Code Size: Size

Data Size: Size

Stack Size: Size

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

-- Processor initialization properties

Startup Deadline: Time

Startup Execution Time: Time Range

-- Properties specifying provided thread execution support

Thread Limit: **aadlinteger** 0 .. Max Thread Limit

Allowed Dispatch Protocol: **list of** Supported Dispatch Protocols

Allowed Period: **list** **of** Time Range

Scheduling Protocol: **inherit list** **of** Supported Scheduling
Protocols

Scheduler Quantum : **inherit** Time

Slot Time: Time

Frame Period: Time

-- acceptable priority value on threads and mapping into RTOS priority
values

Priority Range: **range of aadlinteger**

Priority Map: **list of** Priority Mapping

Process Swap Execution Time: Time Range

Thread Swap Execution Time: Time Range

Supported Source Language: **list of** Supported Source Languages

-- Scaling of processor speed

Processor Capacity: **aadlreal** Processor Speed Units

Reference Processor: **inherit classifier** ( processor )

-- Properties related to data movement in memory

Assign Time: **record** (

Fixed: Time Range;

PerByte: Time Range; )

-- Properties related to the hardware clock

Reference Time: **inherit reference** (processor, device, bus, system,
abstract)

Clock Jitter: Time

Clock Period: Time

Clock Period Range: Time Range

-- Protocol support

Provided Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

Provided Connection Quality Of Service : **inherit list of** Supported
Connection QoS

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

Deactivation Policy: **enumeration** (inactive, unload) => inactive

-- runtime protection support of address spaces

Runtime Protection Support : **aadlboolean **

-- Virtual machine layering

Implemented As: **classifier** ( system implementation, abstract
implementation )

NOTE: The above is the list of the predefined processor properties.
Additional processor properties may be declared in user-defined property
sets. Candidates include properties that describe capabilities and
accuracy of a synchronized clock, e.g., drift rates, differences across
processors.

Semantics

 A processor is the execution platform component that is capable of
scheduling and executing threads. Threads will be bound to a
processor for their execution that supports the dispatch protocol
required by the thread. The Allowed\_Dispatch\_Protocol property
specifies the dispatch protocols that a processor supplies.

Support for thread execution may be embedded in the processor
hardware or it may require software that implements processor
functionality such as thread scheduling, e.g., an operating system
kernel or other software virtual machine. Such software must be
bound to a memory component that is accessible to the processor via
the Actual\_Memory\_Binding property. Services provided by such
software can be called through the provides subprogram access
features of a processor.

 The code that threads execute and the data they access must be bound
to a memory component that is accessible to the processor on which
the thread executes.

 If a processor executes device driver software associated with a
device, then the processor must have access to the corresponding
device component.

(5) Flow specifications model logical flow through processors. For
example, they may represent requests for operating system services
through subprograms or ports.

(6) The hardware description source text property may provide a
reference to source text that is a model of the hardware in a
hardware description language. This provides support for the
simulation of hardware.

(7) Modes allow processor components to have different property values
under different operational processor modes. Modes may be used to
specify different runtime selectable configurations of processor
implementations.

(8) Processor states and transitions are illustrated in the hybrid
automaton shown in Figure 9. The labels in this hybrid automaton
interact with labels in the system hybrid automaton (see Figure 22),
the virtual processor hybrid automaton (see Figure 10), and the
process hybrid automaton (see Figure 8).

-  The initial state of a processor is *stopped*.

-  When a processor is started, it enters the *processor starting*
   state. In this state, the processor hardware is initialized and any
   processor software that provides thread scheduling functionality is
   loaded into memory and initialized. Note that the virtual address
   space load images of processes may already have been loaded as part
   of a single load image that includes the processor or virtual
   processor software.

-  Once the processor and its software for handling virtual processors
   or processes is initialized, the processor transitions to the
   *processor operational* state with **started(processor)**. At this
   point virtual processor, process, and in turn thread initialization
   will start.

 While operational, a processor may be in different modes with
different processing characteristics reflected in appropriate
property values.

As a result of a processor abort, any threads bound to the processor
are aborted, as indicated by **abort(processor)** in the hybrid
automaton in Figure 9 and in the hybrid automata figures in Sections
5.4 and 5.6. After that any virtual processor bound to or contained
in a processor is aborted.

 A stop processor request results in a transition to the *processor
stopping* state at the next hyperperiod of the periodic threads
bound to the processor or to its virtual processors. The length of
the hyperperiod can be reduced by using the Synchronized\_Components
property to minimize the number of periodic threads that must be
synchronized within the hyperperiod (see Section 12).

 When the next hyperperiod begins, the virtual processors and
processes with threads bound to the processor are informed about the
stoppage request, as indicated by **stop(processor)** in the hybrid
automaton in Figure 9. The process hybrid automaton (see Figure 8)
in turn causes the thread hybrid automaton to respond, as indicated
with **stop(processor)** in the hybrid automaton in Figure 5. In
this case, any threads bound to the processor are permitted to
complete their dispatch executions and perform any finalization
before the process is stopped, which is indicated by
**stopped(process)** in Figure 8.

|image12|

Figure − Processor States and Actions
 

 A processor may have ports through which it reports information to
the application software, e.g., to report error conditions. Those
ports are identified in port connection declarations by the reserved
word **processor** (see Section 9.1).

A processor may have provide subprogram or subprogram group access
to represent services that can be called by the application. A
subprogram call identifies such a service by the subprogram
classifier and the binding to the specific processor is implicit
through the actual processor binding of the thread that contains the
call.

 A processor component can include protocols in its abstraction.
These protocols can be explicitly modeled as virtual bus
subcomponents to satisfy protocol requirements by connections. The
fact that a protocol is supported by a processor is specified by a
Provided\_Virtual\_Bus\_Class property.

 A processor can contain a bus subcomponent that it makes externally
accessible. This models a bus that is managed by the processor and
other components can connect to it. In this case the processor is
implicitly connected to this bus.

(5) Different processors may be different execution speeds. This affects
the execution time specified for threads and subprograms. The **in
binding** statement of property associations permits processor type
specific execution times to be declared. The execution time of a
thread or subprogram can also be scaled according to scaling factors
between different processors. The Processor\_Capacity property
specifies the speed of a processor for resource budget analysis.
This property is also used to determine a scaling factor for
execution time with respect to the processor capacity of a specified
Reference\_Processor.

(6) The processor is an abstraction of a hardware processor and possibly
a runtime system. If it is desirable, the internals of the processor
can be modeled by AADL as a system in its own right, i.e., an
application system and an execution platform. This system
implementation description can be associated with the processor
component declaration by the Implemented\_As property (see Section
14).

Processing Requirements and Permissions

 A method of implementation is not required to monitor the startup
deadline and report an overflow as an error.

A method of implementation may choose to enforce thread deadlines
and maximum compute execution time. Violations are reported as
thread recoverable errors.

1. .. rubric:: Virtual Processors
  :name: virtual-processors

 A virtual processor represents a logical resource that is capable of
scheduling and executing threads and other virtual processors bound
to them. Virtual processors can be declared as a subcomponent of a
processor or another virtual processor, i.e., they are implicitly
bound to the processor or virtual processor they are contained in.
Virtual processors can also be declared separately, that is as a
subcomponent of a system component, and explicitly bound to a
processor or virtual processor. This allows virtual processors to
represent hierarchical schedulers that schedule thread and/or
virtual processors bound to them. In a fully bound system every
virtual processor and thread is eventually bound to a physical
processor. Virtual processors can be interconnected via virtual
buses. This allows users to model virtual platforms.

Legality Rules

+-------------------------+---------------------------------------+------------------------+
| **Category**| **Type**  | **Implementation** |
+-------------------------+---------------------------------------+------------------------+
| **virtual processor**   | Features: | Subcomponents: |
| |   ||
| | -  provides subprogram access | -  virtual processor   |
| |   ||
| | -  provides subprogram group access   | -  virtual bus |
| |   ||
| | -  port   | -  abstract|
| |   ||
| | -  requires virtual bus access||
| |   ||
| | -  provides virtual bus access||
| |   ||
| | -  feature group  ||
| |   ||
| | -  feature||
+-------------------------+---------------------------------------+------------------------+

1. A virtual processor component type can contain port, feature group,
   provides subprogram access, and subprogram group access
   declarations. It may contain flow specifications, a modes
   subclause, as well as property associations.

1. A virtual processor component implementation can contain declarations
   of virtual bus, virtual processor, and abstract subcomponents.

2. A virtual processor implementation can contain a modes subclause,
   flows subclause, and a properties subclause.

3. A virtual processor implementation must not contain a subprogram
   calls subclause.

4. A virtual processor implementation can contain subprogram access,
   subprogram group access, port, feature, and feature group
   connections.

Consistency Rules

1. In a fully bound system every virtual processor must be directly or
   indirectly bound to, or directly or indirectly contained in a
   physical processor.

1. In a fully deployed system a requires virtual bus binding of a
   virtual processor specified by the Required\_Virtual\_Bus\_Class
   property must be satisfied by binding the virtual processor to a
   virtual processor or processor that provides this virtual bus. It is
   also satisfied if the virtual processor is contained in a processor
   and the respective virtual bus is bound to the processor.

2. There must not be cycles in bindings between virtual processors.

Standard Properties

-- Properties related to source text that provides thread scheduling
services

Source Text: **inherit list of aadlstring**

Source Language: **inherit list of** Supported Source Languages

Code Size: Size

Data Size: Size

Stack Size: Size

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

Allowed Processor Binding Class:

**inherit** **list** **of** **classifier** (processor, virtual
processor, device, system)

Allowed Processor Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, device, system)

Actual Processor Binding: **inherit** **list of** **reference**
(processor, virtual processor, device, system)

-- Virtual processor initialization properties

Startup Execution Time: Time Range

Startup Deadline: Time

-- Properties specifying provided thread execution support

Thread Limit: **aadlinteger** 0 .. Max Thread Limit

Allowed Dispatch Protocol: **list of** Supported Dispatch Protocols

Allowed Period: **list** **of** Time Range

Scheduling Protocol: **inherit list** **of** Supported Scheduling
Protocols

Slot Time: Time

Frame Period: Time

-- acceptable priority value on threads and mapping into RTOS priority
values

Priority Range: **range of aadlinteger**

Priority Map: **list of** Priority\_Mapping

Process Swap\_Execution Time: Time Range

Thread Swap Execution Time: Time Range

Supported Source Language: **list of** Supported Source Languages

-- Properties of the dispatch characterstics of this virtual processor

Period: **inherit** Time

Dispatch Protocol: Supported Dispatch Protocols

Execution Time: Time

-- Protocol support

Provided Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

Provided Connection Quality Of Service : **inherit list of** Supported
Connection QoS

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

Deactivation Policy: **enumeration** (inactive, unload) => inactive

-- need for and provision of address space protection

Runtime Protection : **inherit** **aadlboolean**

Runtime Protection Support : **aadlboolean **

-- Virtual machine layering

Implemented As: **classifier** ( system implementation, abstract
implementation )

NOTE: The above is the list of the predefined virtual processor
properties. Additional processor properties may be declared in
user-defined property sets. Candidates include properties that describe
capabilities and accuracy of a synchronized clock, e.g., drift rates,
differences across processors.

Semantics

 A virtual processor is the logical execution platform component that
is capable of scheduling and executing threads and other virtual
processors. Threads and virtual processors will be bound to a
virtual processor or processor for their execution. The
Allowed\_Dispatch\_Protocol property specifies the dispatch
protocols that a virtual processor supplies, i.e., only threads or
virtual processors whose dispatch protocol is supported can be
bound.

Support for thread execution may require software that implements
virtual processor functionality such as thread scheduling, e.g., an
operating system kernel or other software virtual machine. Such
software must be bound to a memory component that is accessible to
the processor via the Actual\_Memory\_Binding property. Services
provided by such software can be called through the provides
subprogram access features of a virtual processor.

 A virtual processor component can include protocols in its
abstraction. These protocols can be explicitly modeled as virtual
bus subcomponents to satisfy protocol requirements by connections.
The fact that a protocol is supported by a virtual processor can
also be specified by a Provided\_Virtual\_Bus\_Class property.

 A virtual processor can be declared as a subcomponent of a processor
or another virtual processor; it can also be declared separately in
a system component and then bound to a processor or another virtual
processor. The difference between the two uses of virtual processor
is that in the case of the subcomponent of a processor or virtual
processor the binding of the virtual processor is implicit in the
containment relationship.

(5) Flow specifications model logical flow through virtual processors.
For example, they may represent requests for operating system
services through subprograms or ports.

(6) Modes allow virtual processor components to have different property
values under different operational virtual processor modes. Modes
may be used to specify different runtime selectable configurations
of virtual processor implementations.

(7) Virtual processor states and transitions are illustrated in the
hybrid automaton shown in Figure 10. The hybrid automaton of a
virtual processor interacts with the hybrid automaton of the
processor or virtual processor that it is bound to. A virtual
processor is permitted to initialize itself after the processor and
any virtual processors are initialized, and before any processes or
threads are initialized. Similarly, virtual processors are stopped
after threads, processes, and virtual processors contained in them
are stopped.

|image13|

Figure − Virtual Processor States and Actions
 

 The virtual processor is an logical abstraction of a processor. If
it is desirable, the internals of the virtual processor can be
modeled by AADL as an application system in its own right. For
example, a virtual processor may represent an operating system that
can be described in terms of processes and threads. This system
implementation description can be associated with the device
component declaration by the Implemented\_As property (see Section
14).

1. .. rubric:: Memory
  :name: memory

 A memory component represents an execution platform component that
stores code and data binaries. This execution platform component
consists of hardware such as randomly accessible physical storage,
e.g., RAM, ROM, or more complex permanent storage such as disks,
reflective memory, or logical storage. Memories have properties such
as the number and size of addressable storage locations.
Subprograms, data, and processes – reflected in binary images - are
bound to memory components for access by processors when executing
threads. A memory component may be contained in a processor or may
be accessible from a processor via a bus.

Legality Rules

+----------------+----------------------------------+----------------------+
| **Category**   | **Type** | **Implementation**   |
+----------------+----------------------------------+----------------------+
| **memory** | Features | Subcomponents:   |
||  |  |
|| -  requires bus access   | -  memory|
||  |  |
|| -  provides bus access   | -  bus   |
||  |  |
|| -  requires virtual bus access   | -  abstract  |
||  |  |
|| -  provides virtual bus access   |  |
||  |  |
|| -  feature group |  |
||  |  |
|| -  feature   |  |
||  |  |
|| -  port  |  |
+----------------+----------------------------------+----------------------+

1. A memory type can contain virtual bus access and bus access
   declarations, feature groups, a modes subclause, and property
   associations. It must not contain flow specifications.

1. A memory implementation can contain abstract, memory, and bus
   subcomponent declarations.

2. A memory implementation can contain a modes subclause and property
   associations.

3. A memory implementation can contain bus access connection
   declarations. Bus access connections can connect a memory
   subcomponent to a bus subcomponent or a requires bus access
   feature, as well as connect a provides bus access feature to a
   bus subcomponent.

4. A memory implementation must not contain flows subclause, or
   subprogram calls subclause.

Standard Properties

-- Properties related memory as a resource and its access

Memory Protocol: **enumeration** (execute only, read only, write only,
read write) =\ **>** read write

Word Size: Size **=>** 8 bits

Byte Count: **aadlinteger** 0 **..** Max\_Byte\_Count

Memory Size: Size

Word Space: **aadlinteger** 1 **..** Max\_Word\_Space **=>** 1

Base Address: **aadlinteger** **0** .. **Max** Base Address

Read Time: **record** **(**

Fixed: Time Range;

PerByte: Time Range; )

Write Time: **record** **(**

Fixed: Time Range;

PerByte: Time Range; )

-- Hardware description properties

Hardware Description Source Text: **inherit** **list of aadlstring**

Hardware Source Language: Supported Hardware Source Languages

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

-- Virtual machine layering

Implemented As: **classifier** ( system implementation, abstract
implementation )

Semantics

 Memory components are used to store binary images of source text,
i.e., code and data. These images are loaded into memory
representing the virtual address space of a process and are
accessible to threads contained in the respective processes bound to
the processor. Such access is possible if the memory is contained in
this processor or is accessible to this processor via a shared bus
component. Loading of binary images into memory may occur during
processor startup or the binary images may have been preloaded into
memory before system startup. An example of the latter case is PROM
or EPROM containing binary images.

A memory is accessible from a processor if the memory is contained
in a processor or is connected via a shared bus component and the
Allowed\_Physical\_Access property value for that bus includes
Memory\_Access.

 Memory components can have different property values under different
operational modes.

 The memory component is intended to be an abstraction of a physical
storage component. This can be a memory component such as RAM, or it
can represent a more abstract storage component such as a map
database. If it is desirable, the internals of the memory can be
modeled by AADL as a system in its own right, i.e., an application
system and an execution platform. For example, a map data base as a
memory component can be modeled as a set of processors and disks as
well as the database software. This system implementation
description can be associated with the memory component declaration
by the Implemented\_As property (see Section 14).

(5) Ports can be used as mode transition triggers in memory with modes.

1. .. rubric:: Buses
  :name: buses

 A bus component represents an execution platform component that can
exchange control and data between memories, processors, and devices.
This execution platform component represents a communication
channel, typically hardware together with communication protocols.
Supported communication protocols can be explicitly modeled through
virtual buses (see Section 6.5).

 Processors, devices, and memories can communicate by accessing a
shared bus. Such a shared bus can be located in the same system
implementation as the execution platform components sharing it or
higher in the system hierarchy. Memory, processor, and device types,
as well as the system type of systems they are contained in, can
declare a need for access to a bus through a requires bus reference.

Buses can be connected directly to other buses by one bus requiring
access to another bus. Buses connected in such a way can have
different bus classifier references.

 Connections between software components that are bound to different
processors transmit their information across buses whose protocol
supports the respective connection category.

Legality Rules

+----------------+----------------------------------+----------------------+
| **Category**   | **Type** | **Implementation**   |
+----------------+----------------------------------+----------------------+
| **bus**| Features | Subcomponents:   |
||  |  |
|| -  requires bus access   | -  virtual bus   |
||  |  |
|| -  provides bus access   | -  abstract  |
||  |  |
|| -  requires virtual bus access   |  |
||  |  |
|| -  provides virtual bus access   |  |
||  |  |
|| -  feature group |  |
||  |  |
|| -  feature   |  |
||  |  |
|| -  port  |  |
+----------------+----------------------------------+----------------------+

1. A bus type can have can have virtual bus and bus access declarations,
   a modes subclause, and property associations.

1. A bus type must not contain any flow specifications.

2. A bus implementation can contain virtual bus and abstract
   subcomponent declarations.

3. A bus implementation can contain a modes subclause and property
   associations.

4. A bus implementation must not contain flows subclause, or subprogram
   calls subclause.

Standard Properties

-- Properties specifying bus transmission properties

Allowed Connection Type: **list** **of** **enumeration **

(Sampled Data Connection, Immediate Data\_Connection,

Delayed Data Connection, Port Connection,

Data Access Connection,

Subprogram Access Connection)

Allowed Physical Access Class: **list** **of** **classifier** ( device,
processor, memory, bus )

Allowed Physical Access: **list** **of** **reference** ( device,
processor, memory, bus )

Allowed Message Size: Size Range

Transmission Type: **enumeration** ( push, pull )

Transmission Time: **record** (

Fixed: Time Range;

PerByte: Time Range; )

Prototype Substitution Rule: **inherit** **enumeration**
(Classifier\_Match, Type\_Extension, Signature Match)

-- Hardware description properties

Hardware Description Source Text: **inherit** **list of aadlstring**

Hardware Source Language: Supported Hardware Source Languages

Access\_Right : Access\_Rights => read\_write

-- Protocol support

Provided Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

Provided Connection Quality Of Service : **inherit list of** Supported
Connection QoS

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

-- Virtual machine layering

Implemented As: **classifier** ( system implementation, abstract
implementation )

Semantics

 A bus support physical communication between processors, memories,
and devices. This allows a processor to support execution of source
text in the form of code and data loaded as binary images into
memory components. A bus allows a processor to access device
hardware when executing device software. A bus may also support
different port and subprogram connections between thread components
bound to different processors. The Allowed\_Connection\_Type
property indicates which forms of access a particular bus supports.
The bus may constrain the size of messages communicated through data
or event data connections.

A bus component provides access between processors, memories, and
devices. It is a shared component, for which access is required by
each of the respective components. A device is accessible from a
processor if the two share a bus component. A memory is accessible
from a processor if the two share a bus component.

 Buses can be directly connected to other buses. This is represented
by one bus declaration specifying access to another bus in its
requires subclause.

 Physical access to a bus can be restricted to certain types of
devices, memory, buses, and processors. This is achieved with the
property Allowed\_Physical\_Access.

(5) Bus components can have different property values under different
operational modes.

(6) A bus component can include protocols in its abstraction. Protocols
provided by a bus can be specified with the
Provided\_Virtual\_Bus\_Class property. They are matched against
protocol requirements of connections and virtual buses as specified
by their Required\_Virtual\_Bus\_Class property. If desired
instances of protocols supported by a bus can be explicitly modeled
as virtual bus subcomponents. In that case the connection or virtual
bus requiring a specific protocol can be bound to the specific
virtual bus instance.

(7) Virtual buses (protocols) may be implemented in the bus hardware or
in software. The virtual bus software executes on processors
connected to the bus, whose bound threads communicate over
connections that require the protocol.

(8) The bus is intended to be an abstraction of a physical bus or
network. If it is desirable, the internals of the bus can be modeled
by AADL as a system in its own right, i.e., an application system
and an execution platform. This system implementation description
can be associated with the bus component declaration by the
Implemented\_As property (see Section 14).

(9) Ports can be used as mode transition triggers in buses with modes.

Processing Requirements and Permissions

 A method of implementation shall define how the final size of a
transmission is determined for a specific connection. Implementation
choices and restrictions such as packetization and header and
trailer information are not defined in this standard.

A method of implementation shall define the methods used for bus
arbitration and scheduling. If desired this can be modeled by a
notation of your choice and associated with a bus via property.
Alternatively, it can be modeled through an AADL system model and
associated with the bus through an Implemented\_As property.

Examples

**package** Hardware

**public**

**bus** VME

**end** VME;

**memory** Memory\_Card

**features**

Card\_Connector : **requires** **bus access** VME;

**end** Memory\_Card;

**processor** PowerPC

**features**

Card\_Connector : **requires bus access** VME;

**end** PowerPC;

**processor implementation** PowerPC.Linux

**end** PowerPC.Linux;

**system** Dual\_Processor **end** Dual\_Processor;

**system implementation** Dual\_Processor.PowerPC

**subcomponents**

System\_Bus: **bus** VME;

Left: **processor** PowerPC.Linux;

Right: **processor** PowerPC.Linux;

Shared\_Memory: **memory** Memory\_Card;

**connections**

LCConn: **bus access** System\_Bus <-> Left.Card\_Connector;

RCConn: **bus access** System\_Bus <-> Right.Card\_Connector;

SMConn: **bus access** System\_Bus <-> Shared\_Memory.Card\_Connector;

**end** Dual\_Processor.PowerPC;

**end** Hardware;

Virtual Buses
-------------

 A virtual bus component represents logical bus abstraction such as a
virtual channel or communication protocol.

Virtual buses can be bound to buses, virtual buses, processors,
virtual processors, devices, or memory. When bound to a bus, it may
represent multiple protocols supported by the bus or a virtual
channel with a subset of the bus bandwidth. When bound to a virtual
bus it may represent a hierarchy of protocols or virtual channels.
When bound to a processor it may represent protocols supported by a
processor. When bound to a sequence of hardware components it may
represent a virtual channel or flow across these components.

 Virtual buses can be connected to each other, to virtual processors,
and to devices. This allows users to model virtual platforms as a
collection of virtual components.

Virtual buses can be subcomponents of processors, buses, and other
virtual buses. This implies that they are bound to the processor,
bus, or virtual bus they are contained in.

 Connections and virtual buses can indicate by property the protocols
they require by identifying the appropriate virtual bus or bus
classifier. A connection can also indicate the desired level of
quality of service, which must be satisfied by the virtual bus or
bus the connection is bound to. Similarly, hardware components can
indicate by property the virtual buses they provide.

Legality Rules

+-------------------+----------------------------------+----------------------+
| **Category**  | **Type** | **Implementation**   |
+-------------------+----------------------------------+----------------------+
| **virtual bus**   | Features | Subcomponents:   |
|   |  |  |
|   | -  port  | -  virtual bus   |
|   |  |  |
|   | -  provides virtual bus access   | -  abstract  |
|   |  |  |
|   | -  requires virtual bus access   |  |
|   |  |  |
|   | -  feature   |  |
|   |  |  |
|   | -  feature group |  |
+-------------------+----------------------------------+----------------------+

1. A virtual bus type can have property associations.

1. A virtual bus type must not contain flow specifications.

2. A virtual bus implementation can contain virtual bus subcomponent
   declarations.

3. A virtual bus implementation can contain a modes subclause and
   property associations.

4. A virtual bus implementation must not contain a connections
   subclause, flows subclause, or subprogram calls subclause.

Consistency Rules

1. In a fully deployed system virtual buses must be directly or
   indirectly bound to processors or buses that support these virtual
   buses, or they must be subcomponents of buses and processors.

2. There must not be cycles in binding declarations between virtual
   buses.

Standard Properties

-- Properties specifying bus transmission properties

Allowed Connection\_Type: **list** **of** **enumeration **

(Sampled Data Connection, Immediate Data Connection,

Delayed Data Connection, Port Connection,

Data Access\_Connection,

Subprogram Access Connection)

Allowed Message Size: Size Range

Transmission Type: **enumeration** ( push, pull )

Transmission Time: **record** (

Fixed: Time Range;

PerByte: Time Range; )

Prototype Substitution Rule: **inherit** **enumeration** (Classifier
Match, Type Extension, Signature Match)

-- Protocol support

Provided Connection Quality Of Service : **inherit list of** Supported
Connection QoS

Provided Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

Required Connection Quality Of Service : **inherit list of** Supported
Connection QoS

Allowed Connection Binding\_Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

Allowed Connection Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, bus, virtual bus, device, memory, system)

Actual Connection Binding: **inherit list of** **reference** (processor,
virtual processor, bus, virtual bus, device, system, memory)

Required Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

-- Virtual machine layering

Implemented As: **classifier** ( system implementation, abstract
implementation )

Semantics

 A virtual bus represents a virtual channel or communication
protocol. The Provided\_Virtual\_Bus\_Class property is used to
indicate that a processor or bus supports a protocol. Similarly, the
Required\_Virtual\_Bus\_Class property is used to indicate that a
protocol is required by a connection or virtual bus.

A virtual bus can be a subcomponent of a virtual bus, bus, virtual
processor, processor, or system, it can be provided as indicated by
the Provided\_Virtual\_Bus\_Class property of a bus, virtual bus,
virtual processor, or processor. In all cases, this indicates that
the protocol represented by the virtual bus is supported on the bus
or processor.

 If a virtual bus requires another virtual bus as expressed by the
Required\_Virtual\_Bus\_Class property, this required access is
satisfied by binding the protocol to a processor or bus that
provides this virtual bus as specified with the
Provided\_Virtual\_Bus\_Class property.

 A virtual bus can represent a portion of the bandwidth capacity of a
bus. It can act as virtual channel that can make certain performance
guarantees.

(5) The Allowed\_Connection\_Type property indicates which forms of
access a particular virtual bus supports. The virtual bus may
constrain the size of messages communicated through data or event
data connections.

(6) Virtual bus components can have different property values under
different operational modes.

(7) If it is desirable, the internals of the virtual bus can be modeled
by AADL as an application system in its own right, e.g., as a sender
thread interacting with a receiver thread. This system
implementation description can be associated with the virtual bus
component declaration by the Implemented\_As property (see Section
14).

Processing Requirements and Permissions

 Ports can be used as mode transition triggers in virtual buses with
modes.

A method of implementation shall define how the final size of a
transmission is determined for a specific connection. Implementation
choices and restrictions such as packetization and header and
trailer information are not defined in this standard.

 A method of implementation shall define the methods used for bus
arbitration and scheduling. If desired this can be modeled by a
notation of your choice and associated with a virtual bus via a
property. Alternatively, it can be modeled through an AADL system
model and associated with the bus through an Implemented\_As
property.

Examples

**package** Hardware

**public**

**bus** Ethernet

**end** Ethernet;

**virtual bus** IP\_TCP

**end** IP\_TCP;

**virtual bus** HTTP

**properties**

Allowed\_Connection\_Binding\_Class => **classifier (**\ IP\_TCP);

**end** HTTP;

**processor** PowerPC

**end** PowerPC;

**processor implementation** PowerPC.Linux

**subcomponents**

IP\_TCP: **virtual bus** IP\_TCP;

**end** PowerPC.Linux;

**end** Hardware;

Devices
-------

 A device component represents a physical or mechanical component
within the system, entities in the external environment. A device is
logically connected to application software components and
physically connected to computing hardware. Examples of devices are
sensors, actucators, cameras, brakes. A device may represent a
physical entity or its (simulated) software equivalent. A device may
exhibit simple or complex behaviors. If the device has associated
software such as device drivers that must reside in a memory and
execute on a processor external to the device, this can be specified
through appropriate property values for the device.

 A device interacts with both execution platform components and
application software components. A device has logical connections to
application software components. Those logical connections are
represented by connection declarations between device ports and
application software component ports. A device also has physical
connections to processors via a bus. This models software executing
on a processor accessing the physical device. For any logical
connection between a device and a thread executing application
source code, there must be a physical connection in the execution
platform.

A device can be viewed to be as part of the application system. In
this case, it is natural to place the device together with the
application software components. The physical connection to
processors must follow the system hierarchy.

 Alternatively a device may be viewed to be part of the execution
platform. In this case, it is placed in proximity of other execution
platform components. The logical connections have to follow the
system hierarchy to connect to application software components.

 Examples of devices are sensors and actuators that interface with
the external physical world, or standalone physical systems (such as
a GPS) or dedicated hardware (such as counters or timers). Devices
communicate with embedded application systems through ports and
connections and with the computing hardware through bus access.

(5) Devices can be part of virtual platforms, i.e., they can be
connected to other virtual platform components, such as virtual, and
virtual buses through virtual bus access connections.

Legality Rules

+----------------+---------------------------------------+----------------------+
| **Category**   | **Type**  | **Implementation**   |
+----------------+---------------------------------------+----------------------+
| **device** | Features  | Subcomponents:   |
||   |  |
|| -  port   | -  bus   |
||   |  |
|| -  feature group  | -  virtual bus   |
||   |  |
|| -  provides subprogram access | -  data  |
||   |  |
|| -  provides subprogram group access   | -  abstract  |
||   |  |
|| -  requires bus access|  |
||   |  |
|| -  provides bus access|  |
||   |  |
|| -  provides virtual bus access|  |
||   |  |
|| -  requires virtual bus access|  |
||   |  |
|| -  feature|  |
+----------------+---------------------------------------+----------------------+

1. A device type can contain port, feature group, provides subprogram
   access, provides subprogram group access, bus access
   declarations, flow specifications, a modes subclause, as well as
   property associations.

1. A device component implementation must not contain a subprogram calls
   subclause.

2. A device implementation can contain abstract, data, virtual bus, and
   bus subcomponents, bus access connections, a modes subclause, a
   flows subclause, and property associations.

Standard Properties

-- Hardware description properties

Hardware Description Source Text: **inherit** **list of aadlstring**

Hardware Source Language: Supported Hardware Source Languages

-- Properties specifying device driver software that must be

-- executed by a processor

Source Text: **inherit list of aadlstring**

Source Language: **inherit list of** Supported Source Languages

Code Size: Size

Data Size: Size

Stack Size: Size

-- Properties specifying the execution properties of the device or its
driver

Dispatch Protocol: Supported Dispatch Protocols

Dispatch Trigger: **list of** **reference** (port)

Period: **inherit** Time

Compute Execution Time: Time Range

Deadline: **inherit** Time => Period

Reference Time: **inherit reference** (processor, device, bus, system,
abstract)

-- scheduling properties

Time Slot: **list of aadlinteger **

Priority: **inherit** **aadlinteger**

-- Properties specifying constraints for processor and memory binding

-- for the device driver software

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

Actual Processor Binding: **inherit** **list of** **reference**
(processor, virtual processor, device, system)

Allowed Processor Binding Class:

**inherit** **list** **of** **classifier** (processor, virtual
processor, device, system)

Allowed Processor Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, device, system)

-- protocol support

Provided Virtual Bus Class : **inherit list of** **classifier** (virtual
bus)

Provided Connection Quality Of Service : **inherit list of** Supported
Connection QoS

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

-- Virtual machine layering

Implemented As: **classifier** ( system implementation, abstract
implementation )

Semantics

  AADL device components model dedicated hardware or physical
 entities in the external environment, e.g., a GPS system, or
 entities that interface with an external environment, e.g., sensors
 and actuators as interface between a physical plant and a control
 system. Devices may represent a physical entity or its (simulated)
 software equivalent. They may exhibit simple behavior, e.g., a
 timer, sensor, or actuator, or complex behaviors, e.g., a camera,
 GPS, or an engine. In the latter case sensors or actuators may also
 be represented by device ports. Devices are logically connected to
 application software components and physically connected to
 processors. The device functionality may be fully embedded in the
 device hardware, or it may be provided by device-specific driver
 software.

 A device is accessible from a processor if the device is connected
 via a shared bus component.

  A device declaration can include flow specifications that indicate
 that a device is a flow source, a flow sink, or a flow path exists
 through a device.

  Device components can have different property values under
 different operational modes.

(5)  A device component can contain a data subcomponent to represent
 persistent state. This data subcomponent cannot be made accessible
 via data access. Device behavior can be specified via Behavior
 Annex subclauses, which can refer to the data subcomponent.

(6)  Embedded application software interacts with devices through port
 connections and through subprogram calls. Data ports can be used to
 represent device registers. Event and event data ports can be used
 to represent signals and interrupts that trigger execution of
 software or that initiate a reaction by the device. Subprogram
 calls reflect functionality executed by the device. This
 functionality may be fully implemented in the device hardware or
 through a device driver.

(7)  The Dispatch\_Protocol property determines the execution behavior
 of the device. A device can execute periodically or event-driven.
 Periodic execution means that the device polls the external
 environment periodically to produce a periodic data stream to the
 application, or that it samples input from the application
 periodically. Event-driven execution means that the device
 generates events, e.g., a clock or timer, that it observes events
 in the external environment and passes them to the application,
 e.g., flipping of a switch, or that it responds to events or event
 data being sent by the application, e.g., a signal to turn on a
 light. The input or output rate on device ports can be specified
 through the Output\_Rate property. The Dispatch\_Trigger property
 can be used to specify a subset of event or event data ports that
 can trigger a device dispatch.

(8)  The interface to a device may be through a device driver. The
 features of the device type may represent the interface to the
 device via the device driver. The execution behavior of the device
 driver is specified by the device dispatch protocol.

(9)  The device driver may execute as part of the underlying operating
 system kernel. In this case, we can specify the driver
 characteristics as properties on the device itself, such as the
 code size, data size, and stack size. Binding properties specify
 the processor whose runtime system includes the driver.

(10) The device driver may execute in a separate address space from the
 operating system kernel. In this case, the binding property may
 specify a virtual processor as target.

(11) A device driver may execute as an application thread. In this case,
 the driver is modeled by an AADL thread. This thread provides the
 device interface to the application and interfaces with the device
 registers of the physical device, represented by the AADL device
 component.

(12) A device can contain a bus subcomponent that it makes externally
 accessible. This models a bus that is managed by the device and
 other components can connect to it. In this case the device is
 implicitly connected to this bus.

(13) A device can support communication protocols as expressed by the
 Provided\_Virtual\_Bus\_Class property. Instances of such protocols
 can also be explicitly represented by virtual bus subcomponents.

(14) The device is intended to be an abstraction of a physical component
 in the embedded system environment. If it is desirable, the
 internals of the device can be modeled by AADL as a system in its
 own right, i.e., an application system and an execution platform.
 For example, a digital camera as a device can be modeled as a set
 of processors and application software that handles the taking of
 images and their transfer from the camera to a processor via a USB
 bus connection. This system implementation description can be
 associated with the device component declaration by the
 Implemented\_As property (see Section 14).

Processing Requirements and Permissions

 This software must reside as a binary image on memory components and
is executed on a processor component. The executing processor that
has access to the device must be connected to the device via a bus.
The memory storing the binary image must be accessible to the
processor. Device driver software may be modeled implicitly through
the Source\_Text and related properties, or it may be modeled
explicitly by a separate thread declaration.

In the implicit model the execution of the device driver software
may be considered to be part of the runtime system of a processor to
which the device is connected, or it may be treated as an implicitly
declared thread.

Examples

\ **Package** Equipment

**with** Buses, UserTypes, *Simulation*;

**device** Camera

**features**

usbport: **requires bus access** Buses::USB.USB2;

image: **out event data port** UserTypes::imageformat.jpg;

**end** Camera;

**device** temperature\_sensor

**features**

serialline: **requires bus access** Buses::RS232;

temp\_reading: **out data port** UserTypes::Temperature.Celsius;

**end** temperature\_sensor;

**device implementation** temperature\_sensor.hardware

**properties**

Hardware\_Description\_Source\_Text => (
TemperatureSensorHardwareModel.mdl );

**end** temperature\_sensor.hardware;

**device implementation** temperature\_sensor.simulation

**properties**

Simulation::SensorReadings => SensorTrace1.xls;

**end** temperature\_sensor.simulation;

**device** Timer

**features**

SignalWire: **requires bus access** Wire.Gauge12;

SetTime: **in event data port** UserTypes::Time;

TimeExpired: **out event port**;

**end** Timer;

**end** Equipment;

System Composition
==================

 Systems are organized into a hierarchy of components to reflect the
structure of actual systems being modeled. This hierarchy is modeled
by *system* declarations to represent a composition of components
into composite components. A *system instance* models an instance of
an application system and its binding to a system that contains
execution platform components.

1. .. rubric:: Systems
  :name: systems

 A system represents an assembly of interacting application software,
execution platform, and system components. Systems can have multiple
modes, each representing a possibly different configuration of
components and their connectivity contained in the system. Systems
may require access to data and bus components declared outside the
system and may provide access to data and bus components declared
within. Systems may be hierarchically nested.

Legality Rules

+----------------+---------------------------------------+------------------------+
| **Category**   | **Type**  | **Implementation** |
+----------------+---------------------------------------+------------------------+
| **system** | Features: | Subcomponents: |
||   ||
|| -  port   | -  data|
||   ||
|| -  feature group  | -  subprogram  |
||   ||
|| -  provides subprogram access | -  subprogram group|
||   ||
|| -  requires subprogram access | -  process |
||   ||
|| -  provides subprogram group access   | -  processor   |
||   ||
|| -  requires subprogram group access   | -  virtual processor   |
||   ||
|| -  provides bus access| -  memory  |
||   ||
|| -  requires bus access| -  bus |
||   ||
|| -  provides virtual bus access| -  virtual bus |
||   ||
|| -  requires virtual bus access| -  device  |
||   ||
|| -  provides data access   | -  system  |
||   ||
|| -  requires data access   | -  abstract|
||   ||
|| -  feature||
+----------------+---------------------------------------+------------------------+

1. A system component type can contain subprogram, subprogram group,
   data and bus access declarations, port, feature group
   declarations. It can also contain flow specifications as well as
   property associations.

1. A system component implementation can contain abstract, data,
   subprogram, subprogram group, process, and system subcomponent
   declarations as well as execution platform components, i.e.,
   processor, virtual processor, memory, bus, virtual bus, and
   device.

2. A system implementation can contain a modes subclause, a connections
   subclause, a flows subclause, and property associations.

3. A thread group must not contain a subprogram calls subclause.

Standard Properties

-- Properties related to source text

Source Text: **inherit list of aadlstring**

Source Language: **inherit list of** Supported Source Languages

-- Process property that can be specified at the system level as well

-- Runtime enforcement of address space boundaries

Runtime\_Protection : **inherit** **aadlboolean**

-- Inheritable thread properties

Synchronized Component: **inherit** **aadlboolean** => **true**

Active Thread Handling Protocol:

**inherit** Supported Active Thread Handling Protocols => abort

Period: **inherit** Time

Deadline: **inherit** Time => Period

-- execution time related properties

Reference Processor: **inherit classifier** ( processor )

-- scheduling properties

Time Slot: **list of aadlinteger **

Priority: **inherit** **aadlinteger**

-- Properties related binding of software component source text in

-- systems to processors and memory

Allowed Processor Binding\_Class:

**inherit** **list** **of** **classifier** (processor, virtual
processor, device, system)

Allowed Processor Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, device, system)

Actual Processor Binding: **inherit** **list of** **reference**
(processor, virtual processor, device, system)

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

Allowed Connection Binding Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

Allowed Connection Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, bus, virtual bus, device, memory, system)

Actual Connection Binding: **inherit list of** **reference** (processor,
virtual processor, bus, virtual bus, device, system, memory)

Collocated: **record** (

Targets: **list** **of** **reference** (data, thread, process, system,
connection);

Location: **classifier** ( processor, memory, bus, system ); )

Not Collocated: **record** (

Targets: **list** **of** **reference** (data, thread, process, system,
connection);

Location: **classifier** ( processor, memory, bus, system ); )

-- Properties related systems as execution platforms

Hardware Source Language: Supported Hardware Source Languages

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

-- Properties related to startup of processor contained in a system

Startup Deadline: Time

Startup Execution Time: Time Range

-- Properties related to system load times

Load Time: Time Range

Load Deadline: Time

-- Properties related to the hardware clock

Clock Jitter: Time

Clock Period: Time

Scheduling Protocol: **inherit list** **of** Supported Scheduling
Protocols

Clock Period Range: Time Range

Semantics

 A system component represents an assembly of software and execution
platform components. All subcomponents of a system are considered to
be contained in that system.

Some system components consist of purely software components all of
which must be bound to execution platform components outside the
system itself. An example is an application software system. Some
system components consist purely of computing hardware components.
They represent aggregations of processor, memory, and bus components
that act as the hardware platform. Some system component is a
composition of devices and buses that represent the physical
environment that the embedded software system interacts with. Some
system components may be combinations of the above. Some system
components are self-contained in that all contained software
components are bound to execution platform components contained
within the same system. Such self-contained systems may have
external connectivity in the form of logical connection points
represented by ports and physical connection points in the form of
required or provided bus access. Examples, of such systems are
database servers, GPS receivers, and digital cameras. Such
self-contained systems with an external interface may represent the
implementation of devices. The device representation takes a
black-box perspective, while the system representation takes a
white-box perspective and is associated with the device through the
Implemented\_As property.

 A system component can contain a modes subclause. Each mode can
represent an alternative system configuration of contained
subcomponents and their connections. The transition between modes is
determined by the mode transition declarations of specific property
associations.

Processing Requirements and Permissions

 Processing methods may restrict data, subprogram, and subprogram
group subcomponents to be part of only one process address space. In
that case they may require those subcomponents to be placed inside a
process, thread group, or thread, and not be allowed in system
implementations.



