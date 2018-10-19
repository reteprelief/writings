Modes and Mode Transitions
==========================

 A *mode* represents an operational mode state, which manifests
itself as a configuration of contained components, connections, and
mode-specific property value associations. Modes can be defined for
different components in a system and modes can be inherited from the
enclosing component. A configuration may be an execution platform
configuration in the form of a set of processors, memories, buses,
and devices; an application system configuration in the form of a
set of threads communicating through connections or calls within or
across processes and systems; or a source text operational mode
within a thread, i.e., an execution behavior embedded in the source
text itself. When multiple modes are declared for a component, a
mode transition behavior declaration identifies which event and
event data arrivals cause a mode switch and the new mode, i.e., a
change to a different configuration. Exactly one mode is considered
the current mode. The current mode determines the set of threads
that are considered *active*, i.e., ready to respond to dispatches,
and the connections that are available to transfer data and control.
A set of modes may be inherited from the containing component.

 Mode transitions model dynamic operational behavior that represents
switching between configurations and changes in components internal
characteristics. Such transitions are initiated by mode transition
triggering events, i.e., the arrival of events or event data on
ports named in a mode transition or an event raised by the component
itself (**self**). When declared for processes and systems, mode
transitions model the switch between alternative configurations of
active threads. When declared for execution platforms, mode
transitions model the change between different execution platform
configurations. When declared for threads and subprograms, modes may
represent application logic that is encoded in the source text and
may result in different associated property values for the thread or
data component.

Syntax

modes\_subclause ::=

**modes** ( { mode \| mode\_transition }\ :sup:`+` \| none\_statement )

requires\_modes\_subclause ::=

**requires modes** ( { mode }\ :sup:`+` \| none\_statement )

mode ::=

*defining\_mode*\ \_identifier : [ **initial** ] **mode**

[ **{** { *mode*\ \_property\_association }\ :sup:`+` **}** ]\ **;**

mode\_transition ::=

[ *defining\_\_mode\_transition*\ \_identifier \ **:** ]

*source\_mode*\ \_identifier

**-[** mode\_transition\_trigger { **,** mode\_transition\_trigger
}\ :sup:`\*` **]-> **

*destination\_mode*\ \_identifier

[ **{** { *mode\_transition*\ \_property\_association }\ :sup:`+` **}**
] **;**

mode\_transition\_trigger ::=

unique\_feature\_identifier \| [ **self** . ]
*internal\_feature\_*\ identifier

\| [ **processor .** ] *processor\_feature\_*\ identifier

unique\_feature\_identifier ::=

[ *subcomponent\_or\_feature\_group\_or\_subprogram\_call*\ \_identifier
**.** ] *feature*\ \_identifier

in\_modes ::=

**in modes (** *mode*\ \_identifier { **,** *mode*\ \_identifier
}\ :sup:`\*` **)**

component\_in\_modes ::=

**in modes (** mode\_name { **,** mode\_name }\ :sup:`\*` **)**

mode\_name ::= *local\_mode*\ \_identifier [ **=>**
*subcomponent\_mode\_*\ identifier ]

in\_modes\_and\_transitions ::=

**in modes (** mode\_or\_transition { **,** mode\_or\_transition
}\ :sup:`\*` **)**

mode\_or\_transition ::=

*mode*\ \_identifier \| *mode\_transition*\ \_identifier

Naming Rules

1. The defining mode and mode transition identifiers must be unique
   within the local namespace of the component classifier that contains
   the mode subclause. This means that modes declared in component types
   and in component implementations for the same type must have
   different identifiers.

1. The identifiers in a mode transition that refer to modes must exist
   in the local namespace of the component classifier that contains the
   mode subclause. In other words, mode transitions of a component can
   only refer to modes of the same component.

2. The unique feature identifier must be either incoming feature
   identifier in the namespace of the associated component type, or an
   outgoing feature identifier in the namespace of the component type
   associated with the named subcomponent. The internal feature
   identifier or processor feature identifier must exist in the
   namespace of the component implementation that contains the mode
   subclause.

3. The mode and mode transition identifiers named in an **in modes**
   statement of a subcomponent or connection must refer to modes or mode
   transitions declared in the local namespace of the component
   implementation that contains the subcomponent or connection. The
   modes and mode transitions in the local name space are those declared
   in the modes\_subclause or requires\_modes\_subclause of the
   component implementation or the component type.

4. The subcomponent\_mode\_identifier in a component\_in\_modes must
   refer to a required mode of the subcomponent for which the **in
   modes** is declared. If the subcomponent’s component type has a
   **requires modes** clause and no mapping to a subcomponent mode
   identifier is specified, then for each such mode in the subcomponent
   there must be a mode with the same name in the containing component.

5. An **in modes** statement in a subcomponent or connection refinement
   declaration may be used to specify mode membership to replace the
   one, if any, in the declaration being refined. A subcomponent or
   connection refinement declaration without an **in modes** statement
   specifies that the connection is active in all modes.

Special rules apply for **in modes** in property associations (see
Section 11.3).

1. For a contained property association with an **in modes** statement,
   the identifier must refer to modes or mode transitions of the last
   subcomponent named in the dot-separated identifier list of the
   **applies to** subclause, or to modes and mode transitions of the
   component containing the property association if the **applies to**
   subclause does not start with a subcomponent identifier.

Legality Rules

1. A mode or mode transition can be declared in any of the component
   categories.

1. If a component classifier contains mode declarations, one of those
   modes must be declared with the reserved word **initial**. If the
   component classifier extends another component classifier, the
   initial mode must have been declared in one of the ancestor
   component classifier. This rule does not apply to **requires
   modes** subclauses.

2. The set of transitions declared within a single component
   implementation must define a deterministic transition function.
   For each mode, there must exist exactly one transition, which can
   cause transition to another mode.

3. If a component has been declared with a requires\_mode\_clause, then
   a subcomponent declaration referencing this component classifier
   may include a component\_in\_modes that specifies a mapping for
   the inherited modes by specifying the subcomponent’s mode
   identifier after the optional **=>**; if the mapping is not
   specified the same mode identifier is assumed.



Legality Rules
^^^^^^^^^^^^^^

1.  The category of the component implementation must be identical to
the category of the component interface for which the component
implementation is declared, or the category of the component interface must be *component* or not specified.

3.  If the component interface of the component implementation contains a
requires\_modes\_subclause then the component implementation
must not contain any mode or mode transition declarations.

4.  If modes are declared in the component interface, then modes cannot be
declared in component implementations.

5.  If modes or mode transitions are declared in the component interface,
then mode transitions can be added in the component
implementation. These mode transitions may be triggered by ports of the component interface or by ports of subcomponents.


Standard Properties

Mode Transition Response: **enumeration** ( emergency, planned )

Semantics

**Mode Sensitive Architecture**

  The mode semantics described here focus on a single mode subclause.
 A system instance that represents the runtime architecture of an
 operational system can contain multiple components with their own
 mode transitions. The semantics of system-wide mode transitions are
 discussed in Section 13.6.

 A mode represents an operational state that is represented as a
 runtime configuration of contained components, connections, and
 mode-specific property value associations.

  A mode may represent a runtime configuration of systems, processes,
 thread groups and threads and their connections for a given
 operational state. In this case the modes are declared in thread
 groups, processes and systems, and **in modes** clauses indicate
 which subcomponents and connections are active in a given mode. In
 this case, only the threads that are part of the current mode are
 in the *suspended awaiting dispatch* state – responding to dispatch
 requests. All other threads are in the *suspended awaiting mode*
 state or *thread halted* state.

  A mode may also represent logical execution state within a thread.
 In this case the mode logic is embedded in the source text and may
 be represented as thread modes and mode-specific thread property
 values or mode-specific call sequences through the use of the **in
 modes** clause. Thread and subprogram internal modes effectively
 represent a set of behavioral states, whose state transition
 behavior can be specified through annex subclauses of the Behavior
 Annex notation (see Annex Document D).

(5)  System and execution platform component declarations can have
 subcomponents with **in modes** clauses. In this case, only the
 execution platform components that are part of the current mode are
 accessible to software components. For example, only the processors
 and memories that are part of the current mode can be the target of
 bindings of application components active in that mode.

(6)  For execution platform components a mode can represent an
 operational mode that is internal to the execution platform
 component. For example, a processor may execute at two execution
 speeds. The different execution speeds are reflected in
 mode-specific property values

(7)  A component type or component implementation may contain several
 declared modes. Exactly one of those modes is the *current* mode.
 Initially, the initial mode is the current mode.

(8)  A component that has modes itself can be a subcomponent of another
 component with modes. As a result, the component can be deactivated
 and activated as result of mode transitions in the enclosing
 component. On activation of a components with modes the
 Resumption\_Policy property determines whether the initial mode is
 entered or the mode from the last deactivation is resumed.

(9)  The **in modes** statement is declared as part of subcomponent
 declarations, subprogram call sequences, flow implementations, and
 property associations. It specifies the modes for which these
 declarations and property values hold. The mode identifiers refer
 to mode declarations in the modes subclause of the component
 classifier. If the **in modes** statement is not present, then the
 subcomponent, subprogram call sequence, flow implementation, or
 property association is part of all modes. If a property
 association has both mode-specific declarations and a declaration
 without an **in modes** statement, then the declaration without the
 **in modes** statement applies to those modes not covered by the
 mode-specific declarations.

(10) A component type may specify through a **requires modes**
 declaration that a subcomponent should inherit the modes of its
 containing component. In this case the **in modes** declared as
 part of a subcomponent defines a mapping of the modes of the
 containing component to the inherited modes of the subcomponent.
 The modes of both may have the same names, or they may be
 different. Multiple modes of the containing component may be mapped
 to the same subcomponent mode.

(11) The **in modes** statement declared as part of connection
 declarations specify the modes or mode transitions for which these
 connection declarations hold. The mode identifiers refer to mode
 declarations in the modes subclause of the component
 implementation. If a connection is declared to be part of a mode
 transition, then the content of the ultimate source port is
 transferred to the ultimate destination port at the actual mode
 switch time. If the **in modes** statement contains only mode
 transitions, then the connection is part of the specified mode
 transitions, but not part of any particular mode. If the **in
 modes** statement is not present, then the connection is part of
 all modes.

(12) If the **in modes** statement is declared as part of a refinement,
 the newly named modes replace the modes specified in the
 declaration being refined.

**Mode Transition**

  The modes subclause declares a state machine describing the dynamic
 mode transition behavior between modes. The states of the state
 machine represent the different modes and the transitions specify
 the event(s) that can trigger a transition to the destination mode.
 Only one mode alternative represents the current mode at any one
 time.

 The arrival of an event or event data through an event or event
 data port that is named in one of the transitions is called a mode
 transition *trigger event*. A mode transition is triggered if one
 of the ports named in a mode transition out of the state
 representing the current mode causes the trigger event. If such a
 trigger event occurs and there is no transition out of the current
 mode naming the port with the trigger event, the trigger event is
 ignored with respect to mode transitions of the receiving modal
 component.

  A mode transition may actually be performed immediately if it is
 considered an *emergency*, or it may be performed in a *planned*
 fashion after currently executing threads complete their execution
 and the dispatches of a set of critical thread are aligned (see
 Section 13.6 for more detail). This is indicated by the
 Mode\_Transition\_Response property on the mode transition with the
 default being planned.

  If several trigger events occur logically simultaneously and affect
 different mode transitions out of the current mode, the order of
 arrival for the purpose of determining the mode transition is
 implementation dependent. If an Urgency property is associated with
 each port named in mode transitions, then the mode transition with
 the highest port urgency takes precedence. If several ports have
 the same urgency then the mode transition is chosen
 non-deterministically.

(5)  Any change of the current mode has the effect of changing the
 property value in property associations with mode-specific values –
 as expressed by the **in modes** statement.

(6)  A thread may execute different source text sequences under
 different modes declared within a thread. This may be represented
 by different compute execution times and different entrypoints or
 call sequences for different modes. The current mode at dispatch
 time determines the source text sequence to be executed. In other
 words, a thread-internal mode represents a behavioral state, in
 which the thread completes execution of one dispatch and awaits the
 next dispatch.

(7)  The semantics of mode transitions between modes declared inside
 threads have been described in Section 5.4.5.

(8)  Mode transitions within an execution platform component occur as a
 result of external or internal events, e.g., a processor may switch
 to a different execution speed. A mode transition within a thread
 or execution platform component does not affect the set of active
 threads, processors, devices, buses, or memories, nor does it
 affect the set of active connections external to the thread or
 execution platform component.

(9)  A mode transition within a system, process, or thread group
 implementation has the effect of deactivating and activating
 threads to respond to dispatches, and changing the pattern of
 connections between components. Deactivated threads transition to
 the *suspended awaiting mode* state. Background threads that are
 not part of the new mode suspend performing their execution.
 Activated threads transition to the *suspended awaiting dispatch*
 state and start responding to dispatches. Previously suspended
 background threads that are part of the new mode resume performing
 execution once the transition into the new mode is complete.
 Threads that are part of both the old and new mode of a mode
 transition continue to respond to dispatches and perform execution.
 Ports that were connected in the old mode, may not be connected in
 the new mode and vice versa.

(10) Threads that are active in both the old and the new mode are
 dispatched in their usual manner; in the case of background
 threads, they continue in the *execute* state.

(11) At the time of the actual mode switch, any threads that were active
 in the old mode and are inactive in the new mode execute their
 Deactivate\_Entrypoint.

(12) At the time of the actual mode switch, any threads that were
 inactive in the old mode and are active in the new mode execute
 their Activate\_Entrypoint.

NOTE: Mode transitions can only be triggered by the arrival of an event
or event data on a named port or a **self** event. This means that
trigger conditions based on the content of any data cannot be expressed
in the mode transition declaration. Instead, they have to be connected
to a thread whose source text interprets the data portion to identify
the error type and then raise an appropriate event through an out event
port that triggers the appropriate mode transition. Such a thread
typically plays the role of a system health monitor that makes system
reconfiguration decisions based on the nature and frequency of detected
faults.

The default mode transition trigger conditions based on a set of
ports is a disjunction. Other conditions can be modeled through the
use of the Behavior Annex (see Annex Document D).

Processing Permissions and Requirements

 Every method for processing specifications must parse mode
transition declarations and check the legality rules defined in this
standard. However, a method of processing specifications need not
define how to build a system from a specification that contains mode
transition declarations. That is, complex behaviors that may have
multiple modes of operation may be rejected by a method of building
systems as an unsupported capability.

In an actual distributed system, exact simultaneity among multiple
events cannot be achieved. A system realization must use
synchronization protocols sufficient to ensure that the causal
ordering of event and data transfers defined by the logical temporal
semantics of this standard are satisfied by the actual system, to
the degree of assurance required by an application.

 A method of implementation is permitted to provide preservation of
queue content for aperiodic and sporadic threads on a mode switch
until the next activation. This is specified using the thread
property Active\_Thread\_Queue\_Handling\_Protocol.

 A method of implementation is permitted to support a subset of the
described protocols to handle threads that are in the performing
computation state at the time instant of actual mode switch. They
must document the chosen subset and its semantic behavior as part of
the Supported\_Active\_Thread\_Handling\_Protocol property.

(5) A method of implementation may enforce that there is a mode-specific
property value defined for each mode.

Examples

-- This is an AADL fragment inside a package

**data** Position\_Type

**end** Position\_Type;

**process** Gps\_Sender

**features**

Position: **out** **data port** Position\_Type;

-- if connected secondary position information is used to recalibrate

SecondaryPosition: **in data port** Position\_Type

{ Required\_Connection => **false**;};

**end** Gps\_Sender;

**process implementation** Gps\_Sender.Basic

**end** Gps\_Sender.Basic;

**process** **implementation** Gps\_Sender.Secure

**end** Gps\_Sender.Secure;

**process** GPS\_Health\_Monitor

**features**

Backup\_Stopped: **out** **event** **port**;

Main\_Stopped: **out** **event** **port**;

All\_Ok: **out** **event** **port**;

Run\_Secure: **out** **event** **port**;

Run\_Normal: **out** **event** **port**;

**end** GPS\_Health\_Monitor;

**system** Gps

**features**

Position: **out** **data port** Position\_Type;

Init\_Done: **in** **event** **port**;

**end** Gps;

**system implementation** Gps.Dual

**subcomponents**

Main\_Gps: **process** Gps\_Sender.Basic **in modes** (Dualmode,
Mainmode);

Backup\_Gps: **process** Gps\_Sender.Basic **in modes** (Dualmode,
Backupmode);

Monitor: **process** GPS\_Health\_Monitor;

**connections**

conn1 : **port** Main\_Gps.Position -> Position **in modes** (Dualmode,
Mainmode);

conn2 : **port** Backup\_Gps.Position -> Position **in modes**
(Backupmode);

conn3 : **port** Backup\_Gps.Position -> Main\_Gps.SecondaryPosition

**in modes** (Dualmode);

**modes**

Initialize: **initial mode**;

Dualmode : **mode**;

Mainmode : **mode**;

Backupmode: **mode**;

Started: Initialize –[ Init\_Done ]-> Dualmode;

Dualmode –[ Monitor.Backup\_Stopped ]-> Mainmode;

Dualmode –[ Monitor.Main\_Stopped ]-> Backupmode;

Mainmode –[ Monitor.All\_Ok ]-> Dualmode;

Backupmode –[ Monitor.All\_Ok ]-> Dualmode;

**end** Gps.Dual;

**system implementation** Gps.Secure **extends** Gps.Dual

**subcomponents**

Secure\_Gps: **process** Gps\_Sender.Secure **in modes** ( Securemode );

**connections**

secureconn: **port** Secure\_Gps.Position -> Position **in modes** (
Securemode );

**modes**

Securemode: **mode**;

SingleSecuremode: **mode**;

Dualmode –[ Monitor.Run\_Secure ]-> Securemode;

Securemode –[ Monitor.Run\_Normal ]-> Dualmode;

Securemode –[ Monitor.Backup\_Stopped ]-> SingleSecuremode;

SingleSecuremode –[ Monitor.Run\_Normal ]-> Mainmode;

Securemode –[ Monitor.Main\_Stopped ]-> Backupmode;

**end** Gps.Secure;

The following example illustrates inherited modes. A process declares a
high fidelity and a low fidelity mode. All threads in the process
respond to these two modes by performing high or low fidelity
computation.

**thread** Calculate

**features**

Incoming: **in event data port**;

Outgoing: **out event data port**;

**requires modes**

HighFidelity: **initial mode**;

LowFidelity: **mode**;

**end** Calculate;

**process** Altitude

**features **

doHigh: **in event port**;

doLow: **in event port**;

Incoming: **in event data port**;

Outgoing: **out event data port**;

**end** Altitude;

**process implementation** Altitude.impl

**subcomponents**

calc: **thread C**\ alculate **in modes** (LowFid => LowFidelity, HiFid
=> HighFidelity);

**connections**

inconn: **port** Incoming -> calc.Incoming;

outconn: **port** calc.Outgoing -> Outgoing;

**modes**

HiFid: **initial mode**;

LowFid: **mode**;

HiFid –[ doLow ]-> LowFid;

LowFid –[ doHigh ]-> HiFid;

**end** Altitude.impl;

Operational System
==================

 Component type and component implementation declarations are
architecture design elements that define the structure and
connectivity of a actual system architecture. They are component
classifiers that must be instantiated to create a complete system
instance. A complete system instance that represents the containment
hierarchy of the actual system is created by instantiating a root
component classifier. This typically is a component implementation
and then recursively instantiating the subcomponents and their
subcomponents. Once instantiated, a system instance can be
completely bound, i.e., each thread is bound to a processor; each
source text, data component, and port is bound to memory; and each
connection is bound to a bus if necessary.

 A completely instantiated and bound system acts as a blueprint for a
system build. Binary images are created and configured into load
instructions according to the system instance specification in AADL.

1. .. rubric:: System Instances
  :name: system-instances

 A system instance represents the runtime architecture of a actual
system that consists of application software components and
execution platform components.

 A system instance is *completely instantiable* if the system
implementation being instantiated is completely specified and
completely resolved.

A system instance is *completely instantiated and bound* if all
threads are ultimately bound to a processor, all source text making
up process address spaces are bound to memory, connections are bound
to buses if their ultimate source and destinations are bound to
different processors, and subprogram calls are bound to remote
subprograms as necessary.

 A set of contained property associations can reflect property values
that are specific to individual instances of components, ports,
connections, provided and required access. These properties may
represent the actual binding of components, as well as results of
analysis, simulation, or actual execution of the completely
instantiated and bound system. Thus, multiple sets of contained
property associations can be associated with the same system
instance to represent different system configurations.

Consistency Rules

1. A complete system instance must not contain incompletely specified
   subcomponents, ports, and subprograms. All processes in a completely
   instantiable system must contain at least one thread.

1. The Required\_Connection property may be used to indicate that a port
   connection is required. In the case of the reserved Error, Stop,
   Abort, and Complete ports (see Section 5.4), connections are
   optional.

2. In a complete system instance, the required ports of all threads,
   devices, and processors must be the ultimate source or destination of
   semantic connections.

3. In a completely instantiable system, the subprogram calls of all
   threads must either be local calls or be bound to a remote subprogram
   whose thread is part of the same mode.

4. In a completely instantiable system, for every mode that is the
   source of mode transitions, there must be at least one mode
   transition that is the ultimate destination of a semantic connection
   whose ultimate source is part of the mode.

5. In a complete system instance, aperiodic and sporadic threads that
   are part of a given mode must have at least one connection to one of
   their **in** event ports or **in** event data ports.

6. For instantiable systems, all threads must be bindable to processors
   and all components representing source text must be bindable to
   memory.

7. The source text associated with all contained components of the
   system instance must be compliant with the specified component type,
   component implementation, and property associations.

8. In order to be considered complete, system instance must contain at
   least one thread, one processor and one memory component in its
   containment hierarchy to represent an application system that is
   executable on an execution platform, i.e., a processor with memory
   containing the application code and data.

9. If a system instance has processors of different types, then the
   execution time of threads must be specified for each processor type
   using the **in binding** statement, or a reference processor type
   must be identified through the Reference\_Processor property.

Semantics

 A system instance represents an operational actual system. That
actual system may be a stand-alone system or a system of systems. A
system instance consists of application software and execution
platform components. The component configuration, i.e., the
hierarchical structure and interconnection topology of these
components is statically known. The mode concept describes
alternative statically known component configurations. The runtime
behavior of the system allows for switching between these
alternative configurations according to a mode transition
specification.

The actual system denoted by a component implementation can be built
if the system is instantiable and if source text exists for all
components whose properties refer to source text. This source text
must be compliant with the AADL specification and the source text
language semantics. Source text is compiled and linked to generate
binary images. The binary images are loaded into memory and made
accessible to threads in virtual address spaces of processes.

 In addition, there exists a kernel address space for every
processor. This address space contains binary images of processor
software and device driver software bound to the processor.

 Execution time of threads and subprograms is specified in units of
time. If a system instance has processors of different types, then
the execution time may vary from processor type to processor type.
Execution time specific to a processor type can be specified using
the **in binding** statement in a property association.
Alternatively, the execution time can be specified with respect to a
reference processor. The reference processor is specified through
the Reference\_Processor property. This property is declared as
**inherit**, i.e., it can be declared at the system level and apply
to all threads in a system. When a thread is bound to a specific
processor type, its execution time, if not explicitly defined for
this processor, will be adjusted according to a scaling factor
between the reference processor and the target processor.

1. .. rubric:: System Binding
  :name: system-binding

 This section defines how binary images produced by compiling source
units are assigned to and loaded onto processor and memory
resources, taking into account requirements for component sharing
and the interconnect topology between processors, memories and
devices. The decisions and methods required to combine the
components of a system to produce a actual system implementation are
collectively called bindings.

Naming Rules

1. The Allowed\_Processor\_Binding property values must evaluate to a
   processor, virtual processor, or a system that contains a processor
   in its component containment hierarchy.

1. The Allowed\_Memory\_Binding property values must evaluate to a
   memory, a processor that contains memory or a system that contains a
   memory or a processor containing memory in its component containment
   hierarchy.

Legality Rules

1. The memory requirements of ports and data components are specified as
   property values of their data types or by properties on the port
   feature or data subcomponent. Those property associations can
   have binding-specific values.

Consistency Rules

1. Every mode-specific configuration of a system instance must have a
   binding of every process component to a (set of) memory component(s),
   and a binding of every thread component to a (set of) processor(s).

1.  In the case of dynamic process loading, the actual binding may
change at runtime. In the case of tightly coupled multi-processor
configurations, such as dual core processors, the actual thread
binding may change between members of an actual binding set of
processors as these processors service a common set of thread ready
queues.

2.  Multiple software components may be bound to a single memory
component.

3.  A software component may be bound to multiple memory components.

4.  A thread must be bound to a one or more processors. If it is bound
to multiple processors, the processors share a ready queue, i.e.,
the thread executes on one processor at a time.

5.  Multiple threads can be bound to a single processor.

6.  All software components for a process must be bound to memory
components that are all accessible from every processor to which any
thread contained in the process is bound. That is, every thread is
able to access every memory component into which the process
containing that thread is loaded.

7.  A shared data component must be bound to memory accessible by all
processors to which the threads sharing the data component are
bound.

8.  For all threads in a process, all processors to which those threads
are bound must have identical component types and component
implementations. That is, all threads that are contained in a given
process must all be executing on the same kind of processor, as
indicated by the processor classifier reference value of the
Allowed\_Processor\_Binding\_Class property associated with the
process. Furthermore, all those processors must be able to access
the memory to which the process is bound.

9.  The complete set of software components making up the functionality
of the processor must be bound to memory that is accessible by that
processor.

10. Each thread must be bound to a processor satisfying the
Allowed\_Processor\_Binding\_Class and Allowed\_Processor\_Binding
property values of the thread.

11. The Allowed\_Processor\_Binding property may specify a single
processor, thus specifying the exact processor binding. It may also
specify a list of processor components or system components
containing processor components, indicating that the thread is
bindable to any of those processor components.

12. Each process must be bound to a memory satisfying the
Allowed\_Memory\_Binding\_Class and Allowed\_Memory\_Binding
property values of the process. The Allowed\_Memory\_Binding
property may specify a single memory component, thus specifying the
exact memory binding. It may also specify a list of memory
components or system components containing memory components,
indicating that the process is bindable to any of those memory
components.

13. The memory requirements of the binary images and the runtime memory
requirements of threads and processes bound to a memory component
must not exceed that memory’s capacity. The execution time
requirements of all threads bound to a processor must not exceed the
schedulable cycles required to ensure that all thread timing
requirements are met. These two constraints may be checked
statically or dynamically. Runtime detection of such a memory
capacity or timing requirements violation results in an error that
the application system can choose to recover from.

Semantics

 A complete system instance is instantiated and bound by identifying
the actual binding of all threads to processors, all binary images
reflected in processes and other components to memory, and all
connections to buses if they span multiple processors. The actual
binding can be recorded for each component in the containment
hierarchy by property associations declared within the system
implementation.

The actual binding must be determined within specified binding
constraints. Binding constraints of application components to
execution platform components are expressed by the allowed binding
and allowed binding class properties for memory, processor, and bus.
In the case of an allowed binding property, the execution platform
component is identified by a sequence of ‘.’ (dot) separated
subcomponent names. This sequence starts with the subcomponent
contained in the component implementation for which the property
association is declared. Or the sequence begins with the
subcomponent contained in the component implementation of the
subcomponent or system implementation for which the property
association is declared. This means that the property association
representing the binding constraint or the actual binding may have
to be declared as a component instance property association of a
component that represents a common root of the components to be
bound.

Processing Requirements and Permissions

 A method of building systems is permitted to require bindings of
selected kinds to be fixed at development time, or to be fixed at
the time of actual system construction. A method of building systems
is permitted to allow bindings of selected kinds to change
dynamically at runtime. For example, a method of building systems
may require process to memory binding and loading to be fixed during
actual system construction, and may require thread to processor
bindings to be fixed at mode changes. Other choices are possible and
permitted.

A method of building systems must check and enforce the semantics
and legality rules defined in this standard. Property associations
may impose constraints on allowed bindings. The access semantics
impose a number of constraints on allowed bindings for processes and
threads to execution platform systems, and ultimately to processors
and memories. In general, the semantic constraints depend on the
particular software and hardware architecture interconnect
topologies. In particular, for most hardware and operating system
configurations all threads contained in a process must execute on
the same processor. Such additional restrictions must be taken into
account by the method of building systems. A method of building
systems is otherwise permitted to make any partitioning and binding
choices that are consistent with the semantics and legality rules of
this standard.

NOTE: If multiple processes share a component, then the physical memory
to which the shared component is bound will appear in the virtual
address space of all those processes. This physical memory is not
necessarily addressed using either the same virtual address in different
processes or the same physical address from different processors. An
access property association may be used to specify different addresses
used to access the same component from different processors.

The AADL supports binding-specific property values. This allows
different property values to be specified for a component property
whose values are sensitive to binding decisions.

Examples

**package** Deployment

**system** smp

**end** smp;

**system implementation** smp.s1

-- a multi-processor system

**subcomponents**

p1: **processor** cpu.u1;

p2: **processor** cpu.u1;

p3: **processor** cpu.u1;

**end** smp.s1;

**process** p1

**end** p1;

**process implementation** p1.i1

**subcomponents**

ta: **thread** t1.i1;

tb: **thread** t1.i1;

**end** p1.i1;

**thread** t1

**end** t1;

**thread** **implementation** t1.i1

**end** t1.i1;

**processor** cpu

**end** cpu;

**processor** **implementation** cpu.u1

**end** cpu.u1;

**system** S

**end** S;

**system implementation** S.I

-- a system combining application components

-- with execution platform components

**subcomponents**

p\_a: **process** p1.i1;

p\_b: **process** p1.i1;

up1: **processor** cpu.u1;

up2: **processor** cpu.u1;

ss1: **system** smp.s1;

**properties**

Allowed\_Processor\_Binding => ( **reference** (up1), **reference**
(up2) )

**applies to** p\_a.ta;

Allowed\_Processor\_Binding => ( **reference** (up1), **reference**
(up2) )

**applies to** p\_a.tb;

 -- ta is restricted to a subset of processors that tb can be bound to;

-- since ta and tb are part of the same process they must be bound to
the

-- same processor in most hardware configurations

Allowed\_Processor\_Binding => **reference** (ss1.p3 ) **applies to**
p\_b.ta;

Allowed\_Processor\_Binding => **reference** (ss1 ) **applies to**
p\_b.tb;

**end** S.I;

**end** Deployment;

NOTE: Binding properties are declared in the system implementation that
contains in its containment hierarchy both the components to be bound
and the execution platform components that are the target of the
binding. Binding properties can also be declared separately for each
instance of such a system implementation, either as part of the system
instantiation or as part of a subcomponent declaration.

System Startup
--------------

 On system startup the system instance transitions from *system
offline* to *system starting* (see Figure 22). **Start(system)**
initiates the initialization of the execution platform components.
In the case of processors, the binary images of the kernel address
space are loaded into memory of each processor, and execution is
started to initialize the execution platform software (see Figure
9). Loading into memory may take zero time, if the memory can be
preloaded, e.g., PROM or flash memory.

 Once a processor is initialized (*Processor operational*), each
processor initiates the initialization of virtual processors bound
to the processor, if any (see Figure 9). When the virtual processors
have completed their initialization they are operational (see Figure
10).

When processors and virtual processors have entered their
operational state (**started(processor)** and
**started(vprocessor)**), the loading of the binary images of
processes bound to the specific processor or its virtual processors
into memory is initiated (see Figure 8). Process binary images are
loaded in the memory component to which the process and its
contained software components are bound. In a static process loading
scenario, all binary images must be loaded before execution of the
application system starts, i.e., thread initialization is initiated.
In a dynamic process loading scenario, binary images of all the
processes that contain a thread that is part of the current mode
must be loaded

 The maximum system initialization time can be determined as

-  max(Startup\_Deadline) of all processors and other hardware
   components

-  + max(Startup\_Deadline) of all virtual processors

-  + max(Load\_Deadline) of all processes

-  + max(Process\_Startup\_Deadline) of all processes

-  + max(Initialize\_Deadline) of all threads.

 All software components for a process must be bound to memory
components that are all accessible from every processor to which any
thread contained in the process is bound. That is, every thread is
able to access every memory component into which the binary image of
the process containing that thread is loaded.

Data components shared across processes must be bound to memory
accessible by all processors to which the processes sharing the data
component are bound.

 Thread initialization must be completed by the next hyperperiod of
the initial mode. Once all threads are initialized, threads that are
part of the initial mode enter the await dispatch state. If loaded,
threads that are not part of the initial mode enter the suspend
awaiting mode state (see Figure 5). At their first dispatch, the
initial values of connected **out** or **in out** ports are made
available to destination threads in their **in** or **in out**
ports.

 This initialization model assumes independent initialization of
threads, i.e., no ordering requirement (other than processes must be
initialized first). If there is an ordering requirement the user can
introduce an initialization mode, in which they can utilize the full
power of AADL to specify thread execution dependencies.

Figure − System Instance States, Transitions, and Actions
 

Normal System Operation
-----------------------

 Normal operation, i.e., the execution semantics of individual
threads and transfer of data and control according to connection and
shared access semantics, have been covered in previous sections. In
this section we focus on the coordination of such execution
semantics throughout a system instance.

 A system instance is called synchronized if all components use a
globally synchronized reference time. A system instance is called
asynchronous if different components use separate clocks with the
potential for clock drift. The clock drift in asynchronous systems
may be bounded, e.g., by resynchronizing the clocks.

In a synchronized system, periodic threads are dispatched
simultaneously with respect to a global clock. The hyperperiod of a
set of periodic threads is defined to be the least common multiple
of the periods of those threads.

 In a synchronized system, a raised event logically arrives
simultaneously at the ultimate destination of all semantic
connections whose ultimate source raised the event. In a
synchronized system, two events are considered to be raised
logically simultaneously if they occur within the granularity of the
globally synchronized reference time. If several events are
logically raised simultaneously and arrive at the same port or at
different transitions out of the current mode in the same or
different components, the order of arrival is
implementation-dependent.

 In an asynchronous system the above hold within a synchronization
domain, i.e., periodic threads are dispatched simultaneously and
events arrive logically simultaneously. A method of implementing a
system may provide coordination protocols for asynchronous system to
provide simultaneity guarantees within a certain time granularity.
Otherwise, the dispatches and event arrivals are considered
independent across synchronization domains.

1. .. rubric:: System Operation Modes
  :name: system-operation-modes

 The set of all modes specified for all components of a system
instance form a set of concurrent mode state machines, whose
composite state is called *system operation mode* (SOM). The set of
possible SOMs is the cross product of the sets of modes for each
component. That is, a SOM is a set of component modes, one mode for
each component of the system. The initial SOM is the set of initial
modes for each component.

The set of possible SOMs can be reduced by taking into account mode
combinations that are not reachable. For example, if a component
instance with modes is itself not active under certain modes of one
of its containing components, then these mode combinations can be
excluded. Similarly, of a component inherits modes from a containing
component, then only the mode combinations of the containing
component need to be considered. Finally, reachability analysis of
modes taking into account mode transition conditions may further
reduce the set of SOMs to be considered.

 The discrete variable **Mode** denotes a SOM. That is, the variable
**Mode** denotes a compound discrete state that is defined by the
mode hybrid semantic diagrams for each component instance in the
system. Note that the value of **Mode** will in general change at
various instants of time during system operation, although not in a
continuous time-varying way.

1. .. rubric:: System Operation Mode Transitions
  :name: system-operation-mode-transitions

 A SOM transition is initiated whenever a mode transition trigger
event (see also Section 12) occurs in any modal component in the
system instance. A single sent event or event data can trigger a
mode switch request in one or more components. In a synchronized
system, this trigger event occurs logically simultaneously for all
components and the resulting component mode transition requests are
treated as a single SOM transition.

 Several events may occur logically simultaneously and may originate
from different ports of the same component, e.g., through a
Send\_Output service call with multiple ports as parameter, or from
different components at the same time. If they are semantically
connected to transitions in different components that lead out of
their current mode or to different transitions out of the same mode
in one component, then events are treated as independent SOM
transition initiations.

If multiple SOM transition initiations occurs logically
simultaneously, one is chosen in an implementation-dependent manner.
If an Urgency property is associated with each port named in mode
transitions, then the mode transition with the highest port urgency
takes precedence. If several ports have the same urgency then the
mode transition is chosen non-deterministically.

 A mode transition of a thread internal mode, i.e., a mode declared
in the thread or one of its subprograms, that is triggered by the
component itself or is triggered by an event or data arriving ata
port of the thread, takes place at the next thread dispatch. For
further detail see Section 5.4.5.

 A mode transition of a processor, device, bus, or memory internal
mode, i.e., a mode declared in the component implementation, that is
triggered by the component itself or is triggered by an external
event, takes place immediately.

The next paragraphs address mode transitions that involve activation and
deactivation of threads and connections.

 If a mode transition has a Mode\_Transition\_Response property value
of Emergency, the mode\_transition\_in\_progress state is entered in
zero time and the actual SOM transition is performed immediately. In
this case all threads in the performing state that have to be
deactivated are aborted.

After an event triggering a SOM transition request has arrived, the
actual SOM transition occurs as a Planned response, if the mode
transition does not have a Mode\_Transition\_Response property value
of Emergency. In this case, execution continues under the old SOM
until the dispatches of a critical set of periodic components
(threads and devices), which are active in the old SOM, align at
their hyperperiod. At that point the mode\_transition\_in\_progress
state is entered. A component is considered critical if it has a
Synchronized\_Component property value of true. If the set of
critical components is empty, the mode\_transition\_in\_progress
state is entered immediately. This is indicated in Figure 23.

 by the guard of the function Hyper(\ **Mode**) on the transition
from the current\_system\_operation\_mode state to the
mode\_transition\_in\_progress state.

 Until the mode\_transition\_in\_progress state is entered, any mode
transition trigger event with the same or lower urgency that would
result in a SOM transition from the current SOM is ignored. A
trigger event with higher urgency supersedes the event under
consideration for determining the SOM transition at the time the
mode\_transition\_in\_progress state is entered.

(5) A runtime transition between SOMs requires a non-zero interval of
time, during which the system is said to be in transition between
two system modes of operation. While a system is in transition,
excluding the instants of time at the start and end of a transition,
all mode transition trigger events are ignored with respect to mode
transitions.

(6) The system is in a *mode transition in progress state* for a limited
amount of time. This is determined by the maximum time required to
perform all deactivations and activations of threads and connections
(see below), increased to the next multiple of the hyperperiod of a
non-zero set of critical components that continue to execute, i.e.,
periodic threads that are active in the old and in the new SOM and
have a Synchronized\_Component property value of true. This is shown
in Figure 23.

(7) by the guard of the function Hyper(\ **Mode**)\* on the transition
from the mode\_transition\_in\_progress state to the
current\_system\_operation\_mode state. After that period of time,
the component is considered to operate in the new mode and the
active threads in the new mode start to execute.

(8) At the time the mode\_transition\_in\_progress state is entered
there are several kinds of threads

-  *Continuing threads*: threads that continue to execute because they
   are active in the old mode and active in the new mode;

-  *Activated threads*: threads that are inactive in the old mode and
   are active in the new mode;

-  *Deactivated threads*: threads that are active in the old mode, not
   active in the new mode, and whose dispatches align at the
   hyperperiod;

-  *Zombie threads*: aperiodic, sporadic, timed, or hybrid threads as
   well as periodic threads that are active in the old mode and not
   active in the new mode and whose dispatches are not aligned at the
   hyperperiod, i.e., they may still be in the perform thread
   computation state (see Figure 5) and may have events or event data in
   their port queues.

  At the instant of time the mode\_transition\_in\_progress state is
 entered, connections that are part of the old SOM and not part of
 the new SOM are disabled.

 While in the mode\_transition\_in\_progress state, for all
 connections between data ports that are declared to be active in
 the current mode transition and whose source threads have completed
 execution at the time the mode\_transition\_in\_progress state is
 entered, data is transferred from the **out** data port such that
 its value becomes available at the first dispatch of the receiving
 thread.

  At the instant in time the mode\_transition\_in\_progress state is
 exited, connections that are not part of the old SOM and are part
 of the new SOM are enabled. At that time the new current SOM is
 entered and the following hold for data port connections. The data
 value of the **out** data port of the source thread is transferred
 into the **in** data port variable of the newly enabled thread,
 unless the **in** data port of the destination thread is the
 destination of a connection active in the current mode transition.

  While in the mode\_transition\_in\_progress state, all continuing
 threads continue to get dispatched and execute, but will only use
 connections that are active in both the old and the new SOM.

(5)  When the mode\_transition\_in\_progress state is entered, *thread*
 **exit(Mode)** is triggered to deactivate all threads that are part
 of the old mode and not part of the new mode, i.e., the set of
 deactivated threads. This results in the execution of deactivation
 entrypoints for those threads (see Figure 5).

(6)  When the mode\_transition\_in\_progress state is entered, *thread*
 **enter(Mode)** is triggered to activate threads that are part of
 the new mode and not part of the old mode, i.e., the set of
 activated threads. This permits those threads to execute their
 activation entrypoints (see Figure 5).

(7)  There is no requirement that all deactivation entrypoint executions
 complete before any activation entrypoint executions start. The
 maximum execution time for the deactivations and activations is the
 maximum deadline of the respective entrypoints.

(8)  Zombie threads may be stopped at actual mode switch time, they may
 be suspended, or they may complete their execution while mode
 transition is in progress. The Active\_Thread\_Handling\_Protocol
 property specifies for each such thread what action is to be taken
 at the time the mode\_transition\_in\_progress state is entered.

(9)  The default action is to stop the execution of the zombie thread
 and execute its recover entrypoint indicating an stop for
 deactivation - effectively handling them like deactivated threads.
 This permits the thread to recover to a consistent state for future
 activation, including the release of resources held by the thread.
 Upon completion of the recover entrypoint, execution the thread
 enters the suspended awaiting mode state; event and event data port
 queues of the thread are flushed by default or remain in the queue
 until the thread is activated again as specified by the
 Active\_Thread\_Queue\_Handling\_Protocol property. If the thread
 was executing a remotely called subprogram, the current dispatch
 execution of the calling thread of a call in progress or queued
 call is also aborted. In this case the recover entrypoint deadline
 is taken into account when determining the duration of the
 mode\_transition\_in\_progress state.

(10) Other actions are project-specific implementor provided action and
 may include:

-  Suspend the execution of the zombie thread for resumption the next
   time the thread is part of a new mode. This action is only safe to
   use if the thread does not hold the lock to a shared resource.

-  Complete\_one permits the thread to complete the execution of its
   current dispatch and invoke its deactivation entrypoint. Any
   remaining queued events, or event data may be flushed, or remain in
   the queue until the thread is activated again, as specified by the
   Active\_Thread\_Queue\_Handling\_Protocol property. The output is
   held and communicated according to the connections when the thread is
   reactivated. In this case the execution of the zombie thread competes
   for execution platform resources.

-  Complete\_all permits the thread to finish processing all events or
   event data in its queues at the time the actual mode switch state is
   entered. The output is held and communicated according to the
   connections when the thread is reactivated. When execution completes
   the deactivation entrypoint is executed. In this case the execution
   of the zombie thread competes for execution platform resources.

 At the next multiple of the SOM transition hyperperiod the system
instance enters current\_system\_operation\_mode state and starts
responding to new requests for SOM transition.

Figure − System Mode Transition Semantics
 

 The synchronization scope for **enter(Mode)** consists of all
threads that are contained in the system instance that were inactive
and are about to become active. The synchronization scope for
**exit(Mode)** contains all threads that are contained in the system
instance that were active and are to become inactive. The edge
labels **enter(Mode)** and **exit(Mode)** also appear in the set of
concurrent semantic automata derived from the mode declarations in a
specification. That is, **enter(Mode)** and **exit(Mode)**
transitions for threads occur synchronously with a transition from
the current\_system\_operation\_mode state to the
mode\_transition\_in\_progress state.

The description of this time-coordinated transitioning of system
operation mode assumes a synchronous system, i.e., a single
synchronization domain. In the case of an asynchronous system,
multiple synchronization domains exist in a system. In this case,
the coordinated activation and deactivation of threads and
connections as part of a system operation mode transition must be
ensured within each synchronization domain. In the case of an
asynchronous system, coordination protocols may be supported to
coordinate system operation mode transition across synchronization
domains within bounded time.

System-wide Fault Handling, Shutdown, and Restart
-------------------------------------------------

 Thread unrecoverable errors result in transmission of event data on
the Error port of the appropriate thread, processor, or device. The
ultimate destination of this semantic connection can be a thread or
set of threads whose role is that of a system health monitor and
system configuration manager. Such threads make decisions about
appropriate fault handling actions to take. Such actions include
raising of events to trigger mode switches, e.g., to request SOM
transitions.

Runtime Services

 Several runtime services are provided to control starting and
stopping of the system, processors, virtual processors, and
processes. They may be explicitly called by application source code,
or they may be called by an AADL runtime system that is generated
from an AADL model.

Current\_System\_Mode returns a value corresponding to the active
system operation mode (SOM).

**subprogram** Current\_System\_Mode

**features**

ModeID: **out parameter** <implementor-specific>; -- ID of the mode

**end** Current\_System\_Mode;

 Set\_System\_Mode indicates the next SOM the runtime system must
switch to. System modes are assumed to have identifying numbers.

**subprogram** Set\_System\_Mode

**features**

ModeID: **in parameter** <implementor-specific>; -- ID of the SOM

**end** Set\_System\_Mode;

 The following subprograms take a process, virtual processor, or
processor ID as parameter. The ID representation is determined by
the runtime system.

Stop\_Process is called for a controlled shut-down of a process,
i.e., all of its contained threads, whether executing, awaiting a
dispatch, or not part of the current mode, are given a chance to
execute their finalize entrypoint before being halted.

 Abort\_Process is called for a shut-down of a process due to an
anomaly. All of its contained threads are terminated immediately.

 Stop\_Virtual\_Processor is called to initiate a transition to the
virtual processor stopping state at the next hyperperiod. This has
the effect of initiating Stop\_Virtual\_Processor for all virtual
processors bound to the virtual processor, and Stop\_Process for all
processes bound to the virtual processor.

(5) Abort\_Virtual\_Processor is called for a shut-down of a virtual
processor due to an anomaly. All virtual processors and processes
bound to the virtual processor are aborted.

(6) Stop\_Processor is called to initiate a transition to the processor
stopping state at the next hyperperiod. This has the effect of
initiating Stop\_Virtual\_Processor for all virtual processors bound
to the processor, and Stop\_Process for all processes bound to the
processor.

(7) Abort\_Processor is called for a shut-down of a processor due to an
anomaly. All virtual processors and processes bound to the processor
are aborted.

(8) Stop\_System is called to initiate a transition to the system
stopping state, which will initiate a Stop\_Processor for all
processors in the system.

(9) Abort\_System is called for a shut-down of the system due to an
anomaly. All processors in the system are aborted.

Processing Requirements and Permissions

  This standard does not require that source text be associated with
 a software or execution platform category. However, a method of
 implementing systems may impose this requirement as a precondition
 for constructing a actual system from a specification.

 A system instance represents the runtime architecture of an
 application system that is to be analyzed and processed. A system
 instance is identified to a tool by a component classifier
 reference to an instantiable system implementation. For example, a
 tool may allow a system classifier reference to be supplied as a
 command line parameter. Any such externally identified component
 specification must satisfy all the rules defined in this
 specification for system instances.

  A method of building systems is permitted to only support static
 process loading.

  A method of building systems is permitted to create any set of
 loadable binary images that satisfy the semantics and legality
 rules of this standard. For example, a single load image may be
 created for each processor that contains all processes and threads
 executed by that processor and all source text associated with
 devices and buses accessible by that processor. Or a separate load
 image may be created for each process to be loaded into memory to
 make up the process virtual address space, in addition to the
 kernel address space created for each processor.

(5)  A process may define a source namespace for the purpose of
 compiling source programs, define a virtual address space, and
 define a binary image for the purpose of loading. A method of
 building systems is permitted to separate these functions. For
 example, processes may be compiled and pre-linked as separate
 programs, followed by a secondary linking to combine the process
 binary images to form a load image.

(6)  A method of building systems is permitted to compile, link and load
 a process as a single source program. That is, a method of building
 systems is permitted to impose the additional requirement that all
 associated source text for all threads contained in a process form
 a legal program as defined in the applicable programming language
 standard.

(7)  If two software components that are compiled and linked within the
 same namespace have identical component types and implementations,
 or the intersection of their associated source text compilation
 units is non-empty, then this must be detected and reported.

(8)  A method of building systems is permitted to omit loading of
 processor, device driver, and bus protocol software in a processor
 kernel address space if none of the threads bound to that processor
 need to access or execute that software.

(9)  This standard supports static virtual memory management, i.e.,
 permits the construction of systems in which binary images of
 processes are loaded during system initialization, before a system
 begins operation.

(10) Also permitted are methods of dynamic virtual memory management or
 dynamic library linking after process loading has completed and
 thread execution has started. However, any method for implementing
 a system must assure that all deadline properties will be satisfied
 for each thread.

(11) An alternative implementation of the process and thread state
 transition sequences is permitted in which a process is loaded and
 initialized each time the system changes to a mode of operation in
 which any of the containing threads in that process are active.
 This process load and initialize replaces the perform thread
 activate action in the thread state transition sequence as well as
 the process load action in the process state transition sequence.
 These alternative semantics may be adopted for any designated
 subset of the processes in a system. All threads contained in a
 process must obey the same thread semantics.

Layered System Architectures
============================

 Layering of system architectures is supported in AADL in several
ways:

-  Hierarchical containment of components;

-  Layered use of threads for processing and services;

-  Layered virtual machine abstraction of the execution platform.

 The hierarchical containment of components is the result of modeling
a system as a component architecture and decomposing its
capabilities hierarchically. This is supported by the abstract
component category as well as the software components, the hardware
components, and the system components. Interaction between
components at different levels of the system hierarchy is managed
and controlled by the component features. Connections between the
components must follow the component hierarchy.

Threads and devices represent active components in a system. Threads
and devices interact with other threads and devices through
directional flow of events and data, representing a pipeline
architecture pattern. Threads also may call on services of other
threads through subprogram service calls. These thread services
often are organized into service layers in that threads of one layer
call on services of the same or lower layer, but not higher layers.
Multi-tiered e-business architectures are examples of this
architecture pattern. Threads and related components of different
service layer can be organized into separate packages and access to
packages can be restricted by convention to follow the layering
hierarchy. Packages or components such as threads can be attributed
with a Service\_Layer property that can be used by a design rule
checker to enforce such layering.

 Execution platform components virtual processor and virtual bus can
represent virtual machine abstractions. For example, a virtual
processor may represent a language runtime system such as the Ada
runtime system, which is responsible for scheduling the execution of
Ada tasks. This runtime system may be implemented on top of another
virtual processor, namely a real-time operating system. Virtual
processors and processors provide the runtime service calls
described in *Runtime Support* in Section 5.4, and Section 8.3, as
well as services declared explicitly as part of the features of a
(virtual) processor type. Similarly, a virtual bus may represent a
protocol that is implemented in terms of a lower level protocol.

 AADL can be used to represent this system layering as follows: The
implementation of an execution platform component can be described
by system components. For example, a processor that represents
hardware and an operating system can be described by an application
software system to represent the operating system and a lower level
execution platform system that includes a lower-level processor
abstraction. Similarly, the internal implementation of a device,
e.g., a digital camera, can be described as a system that consists
of image processing and transfer software as well as processors,
internal memory, and USB bus interface, and CCD sensors as devices.
A map data base may be a highly abstracted memory component type,
whose implementation consists of database software and data
components that represent the database content together with
physical disks as lower-level memory components and a processor that
acts as dedicated database server.

(5) System implementations can be associated with their respective
processor, virtual processor, memory, bus, virtual bus, and device
types and implementations through the Implemented\_As property,
which takes classifiers as its value.

(6) In the case of processors, virtual processors, and devices, these
system implementations can be used as plug-replaceable patterns for
the original platform components, when an instance model is created.
The pattern is called plug-replaceable because the system component
implementing the execution platform element implements the interface
defined by the execution platform element type.

(7) In the case of a virtual bus or bus a connection bound to a bus or
virtual bus is interpreted during model instantiation as rerouting
the connection from its source to a source feature in the system
implementation of the bus or virtual bus, and a second connection
from a predeclared destination feature to the destination of the
original connection. For example, the system implementation
realizing a virtual bus may consist of a sender thread with a
predeclared source port and a receiver thread with a predeclared
destination port. Alternatively, the connection can be mapped into
Send\_Output and Receive\_Input service calls (see *Runtime Support*
in Section 8.3) to reflect an API-based functional service
architecture.

(8) In the case of a memory component, read and write accesses to data
components that are bound to a memory component can be mapped into
read and write accesses to data components in the system
implementation of the memory abstraction, or interpreted as retrieve
and store service calls to the subprogram access features of the
system component.

Lexical Elements
================

 The text of an AADL description consists of a sequence of lexical
elements, each composed of characters. The rules of composition are
given in this section.

1. .. rubric:: Character Set
  :name: character-set

 The only characters allowed outside of comments are the
graphic\_characters and format\_effectors.

Syntax

character ::= graphic\_character \| format\_effector

\| other\_control\_character

graphic\_character ::= identifier\_letter \| digit \|
space\_character

\| special\_character

Semantics

 The character repertoire for the text of an AADL specification
consists of the collection of characters called the Basic
Multilingual Plane (BMP) of the ISO 10646 Universal Multiple-Octet
Coded Character Set, plus a set of format\_effectors and, in
comments only, a set of other\_control\_functions; the coded
representation for these characters is implementation defined (it
need not be a representation defined within ISO-10646-1).

The description of the language definition in this standard uses the
graphic symbols defined for Row 00: Basic Latin and Row 00: Latin-1
Supplement of the ISO 10646 BMP; these correspond to the graphic
symbols of ISO 8859-1 (Latin-1); no graphic symbols are used in this
standard for characters outside of Row 00 of the BMP. The actual set
of graphic symbols used by an implementation for the visual
representation of the text of an AADL specification is not
specified.

 The categories of characters are defined as follows:

*identifier\_letter *

upper\_case\_identifier\_letter \| lower\_case\_identifier\_letter

*upper\_case\_identifier\_letter *

Any character of Row 00 of ISO 10646 BMP whose name begins Latin
Capital Letter.

*lower\_case\_identifier\_letter *

Any character of Row 00 of ISO 10646 BMP whose name begins Latin
Small Letter.

*digit *

One of the characters 0, 1, 2, 3, 4, 5, 6, 7, 8, or 9.

*space\_character *

The character of ISO 10646 BMP named Space''.

*special\_character *

Any character of the ISO 10646 BMP that is not reserved for a
control function, and is not the space\_character, an
identifier\_letter, or a digit.

*format\_effector *

The control functions of ISO 6429 called character tabulation (HT),
line tabulation (VT), carriage return (CR), line feed (LF), and form
feed (FF).

*other\_control\_character *

Any control character, other than a format\_effector, that is
allowed in a comment; the set of other\_control\_functions allowed
in comments is implementation defined.

 The following names are used when referring to certain
special\_characters:

+--------------+---------------------+--------------+------------------------+
| **Symbol**   | **Name**| **Symbol**   | **Name**   |
+--------------+---------------------+--------------+------------------------+
| "| quotation mark  | :| colon  |
+--------------+---------------------+--------------+------------------------+
| #| number sign | ;| semicolon  |
+--------------+---------------------+--------------+------------------------+
| =| equals sign | (| left parenthesis   |
+--------------+---------------------+--------------+------------------------+
| )| Right parenthesis   | \_   | underline  |
+--------------+---------------------+--------------+------------------------+
| +| plus sign   | [| left square bracket|
+--------------+---------------------+--------------+------------------------+
| ,| Comma   | ]| right square bracket   |
+--------------+---------------------+--------------+------------------------+
| -| Minus   | {| left curly bracket |
+--------------+---------------------+--------------+------------------------+
| .| Dot | }| right curly bracket|
+--------------+---------------------+--------------+------------------------+

Implementation Permissions

 In a nonstandard mode, the implementation may support a different
character repertoire; in particular, the set of symbols that are
considered identifier\_letters can be extended or changed to conform
to local conventions.

NOTE: Every code position of ISO 10646 BMP that is not reserved for a
control function is defined to be a graphic\_character by this standard.
This includes all code positions other than 0000 - 001F, 007F - 009F,
and FFFE - FFFF.

Lexical Elements, Separators, and Delimiters
--------------------------------------------

Semantics

 The text of an AADL specification consists of a sequence of separate
*lexical elements*. Each lexical element is formed from a sequence
of characters, and is either a delimiter, an identifier, a reserved
word, a numeric\_literal, a character\_literal, a string\_literal,
or a comment. The meaning of an AADL specification depends only on
the particular sequences of lexical elements that form its
compilations, excluding comments.

 The text of an AADL specification is divided into lines. In general,
the representation for an end of line is implementation defined.
However, a sequence of one or more format\_effectors other than
character tabulation (HT) signifies at least one end of line.

In some cases an explicit *separator* is required to separate
adjacent lexical elements. A separator is any of a space character,
a format\_effector, or the end of a line, as follows:

-  A space character is a separator except within a comment, a
   string\_literal, or a character\_literal.

-  Character tabulation (HT) is a separator except within a comment.

-  The end of a line is always a separator.

 One or more separators are allowed between any two adjacent lexical
elements, before the first, or after the last. At least one
separator is required between an identifier, a reserved word, or a
numeric\_literal and an adjacent identifier, reserved word, or
numeric\_literal.

A *delimiter* is either one of the following special characters

**( ) [ ] { } , . : ; = \* + - **

 or one of the following *compound delimiters* each composed of two
or three adjacent special characters

**:: => +=> -> <-> .. −[ ]−> {\*\* \*\*}**

 Each of the special characters listed for single character
delimiters is a single delimiter except if that character is used as
a character of a compound delimiter, or as a character of a comment,
string\_literal, character\_literal, or numeric\_literal.

The following names are used when referring to compound delimiters:

+-----------------+----------------------------+
| **Delimiter**   | **Name**   |
+-----------------+----------------------------+
| **::**  | qualified name separator   |
+-----------------+----------------------------+
| =>  | association|
+-----------------+----------------------------+
| +=> | additive association   |
+-----------------+----------------------------+
| ->  | directional connection |
+-----------------+----------------------------+
| <-> | bidirectional connection   |
+-----------------+----------------------------+
| ..  | interval   |
+-----------------+----------------------------+
| -[  | left step bracket  |
+-----------------+----------------------------+
| ]-> | right step bracket |
+-----------------+----------------------------+
| {\*\*   | begin annex|
+-----------------+----------------------------+
| \*\*}   | end annex  |
+-----------------+----------------------------+

Processing Requirements and Permissions

 An implementation shall support lines of at least 200 characters in
length, not counting any characters used to signify the end of a
line. An implementation shall support lexical elements of at least
200 characters in length. The maximum supported line length and
lexical element length are implementation defined.

1. .. rubric:: Identifiers
  :name: identifiers

 Identifiers are used as names. Identifiers are case insensitive.

Syntax

identifier ::= identifier\_letter {[underline] letter\_or\_digit}\*

letter\_or\_digit ::= identifier\_letter \| digit

-  An identifier shall not be a reserved word.

-  For the lexical rules of identifiers, the rule of whitespace as token
   separator does not apply. In other words, identifiers do not contain
   spaces or other whitespace characters.

Legality Rules

1. An identifier must be distinct from the reserved words of the AADL.

Semantics

 All characters of an identifier are significant, including any
underline character. Identifiers differing only in the use of
corresponding upper and lower case letters are considered the same.

Processing Requirements and Permissions

 In a nonstandard mode, an implementation may support other
upper/lower case equivalence rules for identifiers, to accommodate
local conventions.

In non-standard mode, a method of implementation may accept
identifier syntax of any programming language that can be used for
software component source text.

Examples

Count X Get\_Symbol Ethelyn Garçon

Snobol\_4 X1 Page\_Count Store\_Next\_Item Verrűckt

Numerical Literals
------------------

 There are two kinds of numeric literals, real and integer. A
real\_literal is a numeric\_literal that includes a point; an
integer\_literal is a numeric\_literal without a point.

Syntax

numeric\_literal ::= integer\_literal \| real\_literal

integer\_literal ::= decimal\_integer\_literal \|
based\_integer\_literal

real\_literal ::= decimal\_real\_literal

Decimal Literals
~~~~~~~~~~~~~~~~

 A decimal literal is a numeric\_literal in the conventional decimal
notation (that is, the base is ten).

Syntax

decimal\_integer\_literal ::= numeral [ positive\_exponent ]

decimal\_real\_literal ::= numeral **.** numeral [ exponent ]

numeral ::= digit {[underline] digit}\ :sup:`\*`

exponent ::= E [+] numeral \| E – numeral

positive\_exponent ::= E [+] numeral

Semantics

 An underline character in a numeral does not affect its meaning. The
letter E of an exponent can be written either in lower case or in
upper case, with the same meaning.

An exponent indicates the power of ten by which the value of the
decimal literal without the exponent is to be multiplied to obtain
the value of the decimal literal with the exponent.

Examples

12 0 1E6 123\_456 *-- integer literals*

12.0 0.0 0.456 3.14159\_26 *-- real literals*

Based Literals
~~~~~~~~~~~~~~

 A based literal is a numeric\_literal expressed in a form that
specifies the base explicitly.

Syntax

based\_integer\_literal ::= base **#** based\_numeral **#** [
positive\_exponent ]

base ::= digit [ digit ]

based\_numeral ::= extended\_digit {[underline] extended\_digit}

extended\_digit ::= digit \| A \| B \| C \| D \| E \| F \| a \| b \|
c \| d \| e \| f

Legality Rules

1. The base (the numeric value of the decimal numeral preceding the
   first #) shall be at least two and at most sixteen. The
   extended\_digits A through F represent the digits ten through
   fifteen respectively. The value of each extended\_digit of a
   based\_literal shall be less than the base.

Semantics

 The conventional meaning of based notation is assumed. An exponent
indicates the power of the base by which the value of the based
literal without the exponent is to be multiplied to obtain the value
of the based literal with the exponent. The base and the exponent,
if any, are in decimal notation.

The extended\_digits A through F can be written either in lower case
or in upper case, with the same meaning.

Examples

2#1111\_1111# 16#FF# 016#0ff# *-- integer literals of value 255*

2#1110\_0000# 16#E#E1 8#340# *-- integer literals of value 224*

String Literals
---------------

 A string\_literal is formed by a sequence of graphic characters
(possibly none) enclosed between two quotation marks used as string
brackets.

Syntax

string\_literal ::= "{string\_element}\*"

string\_element ::= "" \| non\_quotation\_mark\_graphic\_character

 A string\_element is either a pair of quotation marks (""), or a
single graphic\_character other than a quotation mark.

Semantics

 The sequence of characters of a string\_literal is formed from the
sequence of string\_elements between the bracketing quotation marks,
in the given order, with a string\_element that is "" becoming a
single quotation mark in the sequence of characters, and any other
string\_element being reproduced in the sequence.

A null string literal is a string\_literal with no string\_elements
between the quotation marks.

NOTE: An end of line cannot appear in a string\_literal.

Examples

"Message of the day:"

"" *-- a null string literal*

" " "A" """" *-- three string literals of length 1*

"Characters such as $, %, and } are allowed in string literals"

Comments
--------

 A comment starts with two adjacent hyphens and extends up to the end
of the line.

Syntax

comment ::= *--{non\_end\_of\_line\_character}\**

-  A comment may appear on any line of a program.

Semantics

 The presence or absence of comments has no influence on whether a
program is legal or illegal. Furthermore, comments do not influence
the meaning of a program; their sole purpose is the enlightenment of
the human reader.

Examples

*-- this is a comment*

**end**; *-- processing of Line is complete*

*-- a long comment may be split onto*

*-- two or more consecutive lines*

*---------------- the first two hyphens start the comment*

Reserved Words
--------------

 The following are the AADL reserved words. Reserved words are case
insensitive.

+-------------------+-------------------+----------------------+---------------------+
| **aadlboolean**   | **aadlinteger**   | **aadlreal** | **aadlstring**  |
+===================+===================+======================+=====================+
| **abstract**  | **access**| **all**  | **and** |
+-------------------+-------------------+----------------------+---------------------+
| **annex** | **applies**   | **binding**  | **bus** |
+-------------------+-------------------+----------------------+---------------------+
| **calls** | **classifier**| **compute**  | **connections** |
+-------------------+-------------------+----------------------+---------------------+
| **constant**  | **data**  | **delta**| **device**  |
+-------------------+-------------------+----------------------+---------------------+
| **end**   | **enumeration**   | **event**| **extends** |
+-------------------+-------------------+----------------------+---------------------+
| **false** | **feature**   | **features** | **flow**|
+-------------------+-------------------+----------------------+---------------------+
| **flows** | **group** | **implementation**   | **in**  |
+-------------------+-------------------+----------------------+---------------------+
| **inherit**   | **initial**   | **inverse**  | **is**  |
+-------------------+-------------------+----------------------+---------------------+
| **list**  | **memory**| **mode** | **modes**   |
+-------------------+-------------------+----------------------+---------------------+
| **none**  | **not**   | **of**   | **or**  |
+-------------------+-------------------+----------------------+---------------------+
| **out**   | **package**   | **parameter**| **path**|
+-------------------+-------------------+----------------------+---------------------+
| **port**  | **private**   | **process**  | **processor**   |
+-------------------+-------------------+----------------------+---------------------+
| **properties**| **property**  | **prototypes**   | **provides**|
+-------------------+-------------------+----------------------+---------------------+
| **public**| **range** | **record**   | **reference**   |
+-------------------+-------------------+----------------------+---------------------+
| **refined**   | **renames**   | **requires **| **self**|
+-------------------+-------------------+----------------------+---------------------+
| **set**   | **sink**  | **source**   | **subcomponents**   |
+-------------------+-------------------+----------------------+---------------------+
| **subprogram**| **system**| **thread**   | **to**  |
+-------------------+-------------------+----------------------+---------------------+
| **true**  | **type**  | **units**| **virtual** |
+-------------------+-------------------+----------------------+---------------------+
| **with**  |   |  | |
+-------------------+-------------------+----------------------+---------------------+

NOTE: The reserved words appear in lower case boldface in this standard.
Lower case boldface is also used for a reserved word in a
string\_literal used as an operator\_symbol. This is merely a convention
– AADL specifications may be written in whatever typeface is desired and
available.

A. Predeclared Property Sets

Normative

 The set of predeclared property sets Deployment\_Properties,
Thread\_Properties, Timing\_Properties, Memory\_Properties,
Programming\_Properties, and Modeling\_Properties is part of every
AADL specification. It defines properties for AADL model elements
that are defined in the core of the AADL. These property sets may
not be modified by the modeler. Deployment\_Properties contains
properties related to the deployment of the embedded application on
the execution platform. Thread\_Properties contains properties that
characterize threads and their features. Timing\_Properties contains
properties related to execution timing. Memory\_Properties contains
properties related to memory as storage, data access, and device
access. Programming\_Properties contains properties for relating
AADL models to application programs. Modeling\_Properties contains
properties related to the AADL model itself.

 The property set AADL\_Project is a part of every AADL
specification. It defines property enumeration types and property
constants that can be tailored for different AADL projects and site
installations. These definitions allow for tailoring of the
predeclared properties through changes to these predeclared property
types and property constants.

The property types, property definitions, and property constants of
these predeclared property sets can be named with or without
property set name qualification.

NOTE: All predeclared properties and user-defined properties are
applicable to components of the category **abstract** without listing
this category in the **applies to** statement of the property
definition.

NOTE: In accordance with the naming rules for references to items
defined in the predeclared property sets, the declarations in these
property set refer to enumeration types and property constants declared
in the AADL\_Project property set without a qualifying property set
name.

Predeclared Deployment Properties
---------------------------------

 The properties of the predeclared property set named
Deployment\_Properties record binding constraints and actual
bindings of software components to hardware components, i.e., of
threads and virtual processors to virtual processors and processors,
of processes and data components to memory, and of connections to
virtual buses and buses, and of virtual buses to virtual buses,
buses, virtual processors, and processors.

**Property** **set** Deployment\_Properties **is **

+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Processor\_Binding\_Class: |
| |
| **inherit** **list** **of** **classifier** (processor, virtual processor, device, system)   |
| |
| **applies** **to** (thread, thread group, process, system, virtual processor, device);  |
| |
| The Allowed\_Processor\_Binding\_Class property specifies a set of virtual processor, processor and system classifiers. These component classifiers constrain the set of candidate virtual processors and processors for binding to the subset that satisfies the component classifier. |
| |
| The value may be inherited from the containing component.   |
| |
| If this property has no associated value, then all processors specified in the Allowed\_Processor\_Binding are acceptable candidates.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Processor\_Binding: **inherit** **list** **of** **reference** (processor, device, virtual processor, system)   |
| |
| **applies** **to** (thread, thread group, process, system, virtual processor, device);  |
| |
| The Allowed\_Processor\_Binding property specifies the set of virtual processors and processors that are available for binding. The set is specified by a list of virtual processor, processor and system component names. System names represent the processors contained in them. |
| |
| If the property is specified for a thread, the thread can be bound to any of the specified set of virtual processors or processors for execution. If the property is specified for a thread group, process, or system, then it applies to all contained threads, i.e., the contained threads inherit the property association unless overridden. If this property is specified for a device, then the thread associated with the device driver code can be bound to one of the set of processors for execution. The Allowed\_Processor\_Binding property may specify a single processor, thus specifying the exact processor binding.   |
| |
| The allowed binding may be further constrained by the processor classifier reference specified in the Allowed\_Processor\_Binding\_Class property.  |
| |
| If this property has no associated value, then all processors declared in n AADL specification are acceptable candidates.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Actual\_Processor\_Binding: **inherit** **list of** **reference** (processor, virtual processor, device, system)|
| |
| **applies** **to** (thread, thread group, process, system, virtual processor, device);  |
| |
| A thread is bound to the processor specified by the Actual\_Processor\_Binding property. The process of binding threads to processors determines the value of this property. If there is more than one processor listed, a scheduler will dynamically assign the thread to one at a time. This allows modeling of multi-core processors without explicit binding to one of the cores.   |
| |
| If a device is bound to a processor this indicates the binding of the device driver software.   |
| |
| A virtual processor may be bound to a processor. This indicates that the virtual processor executes on the processor it is bound to.|
| |
| Threads, devices, and virtual processors can be bound to virtual processors, which in turn are bound to virtual processors or processors.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Memory\_Binding\_Class:|
| |
| **inherit** **list** **of** **classifier** (memory, system, processor, virtual processor)   |
| |
| **applies** **to** (thread, thread group, process, system, device, data, data port, event data port, subprogram, subprogram group, processor, virtual processor);   |
| |
| The Allowed\_Memory\_Binding\_Class property specifies a set of memory, device, and system classifiers. These classifiers constrain the set of memory components in the Allowed\_Memory\_Binding property to the subset that satisfies the component classifier.|
| |
| The value of the Allowed\_Memory\_Binding property may be inherited from the component that contains the component or feature.  |
| |
| If this property has no associated value, then all memory components specified in the Allowed\_Memory\_Binding are acceptable candidates.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Memory\_Binding: **inherit list** **of** **reference** (memory, system, processor, virtual processor)   |
|  |
| **applies** **to** (thread, thread group, process, system, device, data, data port, event data port, subprogram, subprogram group, processor, virtual processor);|
|  |
| Code and data produced from source text can be bound to the set of memory components that is specified by the Allowed\_Memory\_Binding property. The set is specified by a list of memory and system component names. System names represent the memories contained in them. The Allowed\_Memory\_Binding property may specify a single memory, thus specifying the exact memory binding.|
|  |
| The allowed binding may be further constrained by the memory classifier specified in the Allowed\_Memory\_Binding\_Class.|
|  |
| The value of the Allowed\_Memory\_Binding property may be inherited from the component that contains the component or feature.   |
|  |
| If this property has no associated value, then all memory components declared in an AADL specification are acceptable candidates.|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Actual\_Memory\_Binding: **inherit** **list of** **reference** (memory, system, processor, virtual processor)|
|  |
| **applies** **to** (thread, thread group, process, system, processor, device, data, data port, event data port, subprogram, subprogram group, virtual processor);|
|  |
| Code and data from source text is bound to the memory specified by the Actual\_Memory\_Binding property. The property can hold a list of values to reflect the possibility of code and data being bound to different memory components, for example. |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Connection\_Binding\_Class: |
|  |
| **inherit** **list** **of** **classifier**\ (processor, virtual processor, bus, virtual bus, device, memory, system) |
|  |
| **applies** **to** (feature, connection, thread, thread group, process, system, virtual bus);|
|  |
| The Allowed\_Connection\_Binding\_Class property specifies a set of execution platform classifiers to constrain the binding of connections and virtual buses. The named component classifiers must belong to a processor, virtual processor, bus, virtual bus, device, or memory category, i.e., any execution platform component that supports communication between threads. When specified for a feature it indicates a binding constraint for all connections through that feature, e.g., any protocol assumptions a component makes about its communication through the port.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Connection\_Binding: **inherit** **list** **of** **reference** (processor, virtual processor, bus, virtual bus, device, memory, system) |
|  |
| **applies** **to** (feature, connection, thread, thread group, process, system, virtual bus);|
|  |
| The Allowed\_Connection\_Binding property specifies a set of execution platform components to constrain the binding of connections and virtual buses. The named components must belong to a processor, virtual processor, bus, virtual bus, device, or memory category. When specified for a feature such as a port it indicates a binding constraint for all connections through that feature, e.g., any protocol assumptions a component makes about its communication through the port.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
|  Actual\_Function\_Binding: **inherit list of** **reference** (processor, virtual processor, bus, virtual bus, device, system, memory, thread, process, feature, abstract)   |
|  |
| **applies** **to** (subprogram, thread, thread group, process, system, abstract, feature, virtual bus, virtual processor);   |
|  |
| This property allows users to define mappings from functional architectures to system architectures. |
|  |
| .|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Subprogram\_Call: **list of** **reference** (subprogram)|
|  |
| **applies** **to** (subprogram access);  |
|  |
| A subprogram call can be bound to any member of the set of subprograms specified by the Allowed\_Subprogram\_Call property. These may be remote subprograms, i.e., subprogram instances in other threads, or local subprogram instances. In the latter case the property identifies a specific code instance. If no value is specified, then subprogram call must be a local call.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Actual\_Subprogram\_Call: **reference** (subprogram) |
|  |
| **applies** **to** (subprogram access);  |
|  |
| The Actual\_Subprogram\_Call property specifies the subprogram instance whose code is servicing the subprogram call. These may be remote subprograms, i.e., subprogram instances in other threads, or local subprogram instances. In the latter case the property identifies a specific local code instance, i.e., it can model sharing of subprogram. If no value is specified, the subprogram call is a local call.|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Subprogram\_Call\_Binding:  |
|  |
| **inherit** **list** **of** **reference** (bus, processor, device)   |
|  |
| **applies** **to** (subprogram, thread, thread group, process, system);  |
|  |
| Remote subprogram calls can be bound to the physical connection of an execution platform that is specified by the Allowed\_Subprogram\_Call\_Binding property. If no value is specified, then subprogram call may be a local call or the binding may be inferred from the bindings of the caller and callee. |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Actual\_Subprogram\_Call\_Binding: **list of** **reference** (bus, processor, memory, device)|
|  |
| **applies** **to** (subprogram); |
|  |
| The Actual\_Subprogram\_Call\_Binding property specifies the bus, processor, device, or memory to which a remote subprogram call is bound. If no value is specified, the subprogram call is a local call or the binding can be inferred from the binding of the caller and callee.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Provided\_Virtual\_Bus\_Class : **inherit list of** **classifier** (virtual bus) |
|  |
| **applies to** (bus, virtual bus, processor, virtual processor, device, memory, system); |
|  |
| The Provided\_Virtual\_Bus\_Class property specifies the set of virtual bus classifiers (protocols) supported by a bus, virtual bus, virtual processor, device, or processor. The property indicates that a component with a binding requirement for a virtual bus classifier can be bound to a component whose Provided\_Virtual\_Bus\_Class property value includes the desired virtual bus classifier. Note that the component with this property is not required to have virtual bus subcomponents.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Required\_Virtual\_Bus\_Class : **inherit list of** **classifier** (virtual bus) |
|  |
| **applies to** (virtual bus, connection, port, thread, thread group, process, system, device);   |
|  |
| The Required\_Virtual\_Bus\_Class property specifies the set of virtual bus classifiers (protocols) that this connection or virtual bus needs to be bound to, i.e., that it requires to be bound to one instance of each of the specified classifiers. This property complements the Allowed\_Connection\_Binding\_Class property, which specifies that the connection binding must be to components of one of the specified classifiers.|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Provided\_Connection\_Quality\_Of\_Service : **inherit list of** Supported\_Connection\_QoS  |
|  |
| **applies to** (bus, virtual bus, processor, virtual processor, system, device, memory); |
|  |
| The Provided\_Connection\_Quality\_Of\_Service property specifies the quality of service provided by a protocol, i.e., a virtual bus, bus, virtual processor, or processor supporting protocols, for its transmission.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Required\_Connection\_Quality\_Of\_Service : **inherit list of** Supported\_Connection\_QoS  |
|  |
| **applies to** (port, connection, virtual bus, thread, thread group, process, system, device);   |
|  |
| The Required\_Connection\_Quality\_Of\_Service property specifies that a connection or virtual bus expects a certain quality of service from the protocol that is used for its transmission. |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Not\_Collocated: **record** (|
|  |
| Targets: **list** **of** **reference** (data, thread, process, system, connection);  |
|  |
| Location: **classifier** ( processor, memory, bus, system ); )   |
|  |
| **applies** **to** (process, system);|
|  |
| The Not\_Collocated property specifies that hardware resources used by several software components must be distinct. The components referenced by Target must not be bound to the same hardware of the type specified in the Location field. If the Location is a system component, then they may not be collocated to any component contained in the system component.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Collocated: **record** ( |
|  |
| Targets: **list** **of** **reference** (data, thread, process, system, connection);  |
|  |
| Location: **classifier** ( processor, memory, bus, system ); )   |
|  |
| **applies** **to** (process, system);|
|  |
| The Collocated property specifies that several software components must be bound to the same hardware. The components referenced by Target must be bound to the same hardware of the type specified in the Location field. If the Location is a system component, then they must be collocated on any component contained in the system component.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

 The next set of properties specify characteristics of the computing
hardware as it relates to the deployment of software.

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Connection\_Type: **list** **of** **enumeration **   |
|   |
| (Sampled\_Data\_Connection, Immediate\_Data\_Connection,  |
|   |
| Delayed\_Data\_Connection, Port\_Connection,  |
|   |
| Data\_Access\_Connection, |
|   |
| Subprogram\_Access\_Connection)   |
|   |
| **applies** **to** (bus, device); |
|   |
| The Allowed\_Connection\_Type property specifies the categories of connections a bus supports. That is, a connection may only be legally bound to a bus if the bus supports that category of connection.  |
|   |
| If a list of allowed connection protocols is not specified for a bus, then any category of connection can be bound to the bus.|
+=======================================================================================================================================================================================================================================================================================================+
| Allowed\_Dispatch\_Protocol: **list of** Supported\_Dispatch\_Protocols   |
|   |
| **applies** **to** (processor, virtual processor);|
|   |
| The Allowed\_Dispatch\_Protocol property specifies the thread dispatch protocols are supported by a processor. That is, a thread may only be legally bound to the processor if the specified thread dispatch protocol of the processor corresponds to the dispatch protocol required by the thread.   |
|   |
| If a list of allowed scheduling protocols is not specified for a processor, then a thread with any dispatch protocol can be bound to and executed by the processor.   |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Period: **list** **of** Time\_Range  |
|   |
| **applies** **to** (processor, system, virtual processor);|
|   |
| The Allowed\_Period property specifies a set of allowed periods for periodic tasks bound to a processor.  |
|   |
| The period of every thread bound to the processor must fall within one of the specified ranges.   |
|   |
| If an allowed period is not specified for a processor, then there are no restrictions on the periods of threads bound to that processor.  |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Physical\_Access\_Class: **list** **of** **classifier** ( device, processor, memory, bus )   |
|   |
| **applies** **to** (bus); |
|   |
| The Allowed\_Physical\_Access\_Class property specifies the classifiers of processors, devices, memory, and buses that are allowed to be connected to the bus, i.e., whose connection is supported by the bus.|
|   |
| If the property is not specified for a bus, then the bus may be used to connect both devices and memory to the processor. |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Physical\_Access: **list** **of** **reference** ( device, processor, memory, bus )   |
|   |
| **applies** **to** (bus); |
|   |
| The Allowed\_Physical\_Access property specifies the classifiers of processors, devices, memory, and buses that are allowed to be connected to the bus, i.e., whose connection is supported by the bus.   |
|   |
| If the property is not specified for a bus, then the bus may be used to connect both devices and memory to the processor. |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Memory\_Protocol: **enumeration** (execute\_only, read\_only, write\_only, read\_write) => read\_write|
|   |
| **applies** **to** (memory);  |
|   |
| The Memory\_Protocol property specifies memory access and storage behaviors and restrictions. Writeable data produced from software source text may only be bound to memory components that have the write\_only or read\_write property value.   |
+===================================================================================================================================================================================================================================================================================================+
| Runtime\_Protection\_Support : **aadlboolean **   |
|   |
| **applies** **to** (processor, virtual processor);|
|   |
| The Runtime\_Protection\_Support property specifies whether the processor or virtual processor is able to support runtime enforcement of protected address spaces. Processes and virtual processors bound to virtual processors and processors specify the demand for such runtime enforcement.   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Scheduling\_Protocol: **inherit** **list of** Supported\_Scheduling\_Protocols|
|   |
| **applies** **to** (virtual processor, processor, system);|
|   |
| The Scheduling\_Protocol property specifies what scheduling protocol the thread scheduler of the processor uses. The core standard does not prescribe a particular scheduling protocol.   |
|   |
| Scheduling protocols may result in schedulers that coordinate scheduling of threads across multiple processors.   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Preemptive\_Scheduler : **aadlboolean**   |
|   |
|      **applies to** (processor);  |
|   |
| This property specifies if the processor can preempt a thread during its execution. By default, if this property is not specified, the processor owns a preemptive scheduler. |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Thread\_Limit: **aadlinteger** 0 .. Max\_Thread\_Limit|
|   |
| **applies** **to** (processor, virtual processor);|
|   |
| The Thread\_Limit property specifies the maximum number of threads supported by the processor.|
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Priority\_Map: **list of** Priority\_Mapping  |
|   |
| **applies** **to** (processor);   |
|   |
| The Priority\_Map property specifies a mapping of AADL priorities into priorities of the underlying real-time operating system. This map consists of a list of aadlinteger pairs. |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Priority\_Mapping: **type** **record** (  |
|   |
| Aadl\_Priority: **aadlinteger**;  |
|   |
| RTOS\_Priority: **aadlinteger;** );   |
|   |
| The Priority\_Mapping property specifies a mapping of a single AADL priority value into a single priority value of the underlying real-time operating system. This property is used to define the elements of a consists of a Priority\_Map.  |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------+
| Priority\_Range: **range of aadlinteger**  |
||
| **applies** **to** (processor, virtual processor); |
||
| The Priority\_Range property specifies the range of thread priority values that are acceptable to the processor.   |
||
| The property type is range of aadlinteger. |
+====================================================================================================================+
+--------------------------------------------------------------------------------------------------------------------+

**end** Deployment\_Properties;

Predeclared Thread Properties 
------------------------------

 The properties of the predeclared property set named
Thread\_Properties record information related to threads and
devices, i.e., active application components. They address
dispatching, concurrency, and mode transition.

**Property** **set** Thread\_Properties **is **

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Dispatch\_Protocol: Supported\_Dispatch\_Protocols   |
|  |
| **applies** **to** (thread, device, virtual processor);  |
|  |
| The Dispatch\_Protocol property specifies the dispatch behavior for a thread.|
|  |
| A method used to construct a actual system from a specification is permitted to support only a subset of the standard scheduling protocols. A method used to construct a actual system is permitted to support additional non-standard scheduling protocols. |
+==================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Dispatch\_Trigger: **list of** **reference** (port)  |
|  |
| **applies** **to** (device, thread); |
|  |
| The Dispatch\_Trigger property specifies the list of ports that can trigger the dispatch of a thread or device.  |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Dispatch\_Able: **aadlboolean ** |
|  |
| **applies** **to** (thread); |
|  |
| The Dispatch\_Able property specifies whether a thread should be dispatched. Threads can be activated for dispatch in given modes, which is specified as part of the subcomponent declaration of the component using the thread. In some cases the thread itself may have modes and that mode determines whether the thread is active or idle. For example, various combinations of low level control threads may be active or idle at various points in time.   |
|  |
| Expressing this through modes in the enclosing component would lead to possibly having to model many mode combinations of subcomponents. Specification of zero compute\_execution\_time for a thread indicates that thread is dispatched and its application code decides there is nothing to do.|
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| POSIX\_Scheduling\_Policy : **enumeration** (SCHED\_FIFO, SCHED\_RR, SCHED\_OTHERS)  |
|  |
| **applies to** (thread, thread group);   |
|  |
| The POSIX\_Scheduling\_Policy property is used for the modeling of the scheduling protocols defined by POSIX 1003.1b. Such a property specifies the policy assign to a given thread. The policy may be either SCHED\_FIFO, SCHED\_RR or SCHED\_OTHER. In a POSIX 1003.1b architecture, the policy allows the scheduler to choose the thread to run when several threads have the same fixed priority. If a thread does not define the POSIX\_Scheduling\_Policy property, it has by default the SCHED\_FIFO policy. The policy semantics are :   |
|  |
| -  SCHED\_FIFO : this policy implements a FIFO scheduling protocol on the set of equal fixed priority : a thread stays on the processor until it has terminated or until a highest priority thread is released.  |
|  |
| -  SCHED\_RR : this policy is similar to SCHED\_FIFO except that the quantum is used. At the end of the quantum, the running thread is pre-empted from the processor and a equal priority thread has to be released. |
|  |
| -  SCHED\_OTHER : its semantic is defined by POSIX policy implementers. This policy usually implements a timing sharing scheduling protocol. |
+==================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Priority: **inherit** **aadlinteger**|
|  |
| **applies** **to** (thread, thread group, process, system, device, data access, data);   |
|  |
| The Priority property specifies the priority of the thread that is taken into consideration by some scheduling protocols in scheduling the execution order of threads. When applied to data or data access, it specifies priority for concurrency control protocols that use priority based conflict resolution. A larger value represents a higher priority.|
|  |
| The property type is aadlinteger. Its value is expected to be within the range of priority values supported by a given processor.|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Criticality: **aadlinteger** |
|  |
| **applies** **to** (thread, thread group);   |
|  |
| This property specifies the criticality level of a thread. This property is used by maximum urgency first scheduling protocols. Such a property can also be used by any project specific scheduling protocols.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Time\_Slot: **list of aadlinteger ** |
|  |
| **applies** **to** (thread, thread group, process, virtual processor, system);   |
|  |
| The Time\_Slot property specifies statically allocated slots on a timeline. This property is used by scheduling protocols with a time slot allocation approach, such as the protocol for scheduling partitions on a static timeline. |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Concurrency\_Control\_Protocol: Supported\_Concurrency\_Control\_Protocols   |
|  |
| **applies** **to** (data);   |
|  |
| The Concurrency\_Control\_Protocol property specifies the concurrency control protocol used to ensure mutually exclusive access, i.e., a critical region, to a shared data component. If no value is specified the default value is None\_Specified, i.e., no concurrency control protocol.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Urgency: **aadlinteger** 0 **..** Max\_Urgency   |
|  |
| **applies** **to** (port, subprogram);   |
|  |
| The Urgency property specifies the urgency with which an event at an in port is to be serviced relative to other events arriving at or queued at other in ports of the same thread. A numerically larger number represents higher urgency.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Dequeue\_Protocol: **enumeration** ( OneItem, MultipleItems, AllItems ) => OneItem|
|   |
| **applies** **to** (event port, event data port); |
|   |
| The Dequeue\_Protocol property specifies different dequeuing options. |
|   |
| -  OneItem: (default) a single frozen item is dequeued at input time and made available to the source text unless the queue is empty. The Next\_Value service call has no effect. |
|   |
| -  AllItems: all items that are frozen at input time are dequeued and made available to the source text via the port variable, unless the queue is empty. Individual items become accessible as port variable value through the Next\_Value service call. Any element in the frozen queue that are not retrieved through the Next\_Value service call are discarded, i.e., are removed from the queue and are not available at the next input time.   |
|   |
| -  MultipleItems: multiple items can be dequeued one at a time from the frozen queue and made available to the source text via the port variable. One item is dequeued and its value made available via the port variable with each Next\_Value service call. Any items not dequeued remain in the queue and are available at the next input time.|
|   |
| If the Dequeued\_Items property is set, then it imposes a maximum on the number of elements that are made accessible to a thread at input time when the Dequeue\_Protocol property is set to AllItems or MultipleItems.   |
|   |
| The default property value is OneItem.|
+=======================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Dequeued\_Items: **aadlinteger**  |
|   |
| **applies** **to** (event port, event data port); |
|   |
| The Dequeued\_Items property specifies the maximum number of items that are made available to the application via a port variable for event or event data ports when the input is frozen at input time. Its value cannot exceed that of the Queue\_Size property for the same port. See also Dequeue\_Protocol property.  |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Mode\_Transition\_Response: **enumeration** ( emergency, planned )|
|   |
| **applies** **to** (mode transition); |
|   |
| The Mode\_Transition\_Response property specifies whether the mode transition occurs immediately due to an emergency, or whether it is planned in that the completion of thread execution can be coordinated before performing the mode transition. If not specified the mode transition is considered to be planned. |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Resumption\_Policy: **enumeration** ( restart, resume )   |
|   |
| **applies** **to** (thread, thread group, process, system, device, processor, memory, bus, system, virtual processor, virtual bus, subprogram);   |
|   |
| The Resumption\_Policy property specifies whether as result of a mode transition activation a component that has modes itself starts in the initial mode or resumes in the current mode at the time of its deactivation.  |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Active\_Thread\_Handling\_Protocol:   |
|   |
| **inherit** Supported\_Active\_Thread\_Handling\_Protocols => abort   |
|   |
| **applies to** (thread, thread group, process, system);   |
|   |
| The Active\_Thread\_Handling\_Protocol property specifies the protocol to use to handle execution at the time instant of an actual mode switch. The available choices are implementation defined. The default value is abort. |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Active\_Thread\_Queue\_Handling\_Protocol:|
|   |
| **inherit enumeration** (flush, hold) => flush|
|   |
| **applies to** (thread, thread group, process, system);   |
|   |
| The Active\_Thread\_Queue\_Handling\_Protocol property specifies the protocol to use to handle the content of any event port or event data port queue of a thread at the time instant of an actual mode switch. The available choices are flush and hold. Flush empties the queue. Hold keeps the content in the queue of the thread being deactivated until it is reactivated. The default value is flush.   |
+===============================================================================================================================================================================================================================================================================================================================================================================================================+
| Deactivation\_Policy: **enumeration** (inactive, unload) => inactive  |
|   |
| **applies** **to** (thread, process, virtual processor, processor);   |
|   |
| The Deactivation\_Policy property specifies whether a process is to be unloaded when it is deactivated. If the policy is unload, then the process is unloaded on deactivate and loaded on activate. In the case of threads, the property indicates whether thread state is saved. |
|   |
| The default is that the process is loaded during startup and is not unloaded when deactivated.|
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Runtime\_Protection : **inherit** **aadlboolean** |
|   |
| **applies to** (process, system, virtual processor);  |
|   |
| This property specifies whether a process requires runtime enforcement of address space protection. If no value is specified the default is assumed to be **true**.   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Subprogram\_Call\_Type: **enumeration** (Synchronous, SemiSynchronous)|
|   |
| => Synchronous|
|   |
| **applies** **to** (subprogram);  |
|   |
| The Subprogram\_Call\_Type property specifies whether the call is to be performed synchronous or semi-synchronous. In case of a semi-synchronous call the user of the result is may be suspended until the result is available. The default is Synchronous if no property value is specified. |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Synchronized\_Component: **inherit** **aadlboolean** => **true**  |
|   |
| **applies to** (thread, thread group, process, system);   |
|   |
| The Synchronized\_Component property specifies whether a periodic thread will be synchronized with transitions into and out of a mode. In other words, the thread affects the hyperperiod for mode switching of the property value is true.   |
|   |
| The default value is true.|
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**end** Thread\_Properties;

Predeclared Timing Properties
-----------------------------

 The predeclared property set named Timing\_Properties contains
execution time related property definitions regarding threads,
devices, and runtime system support for thread execution.

**Property** **set** Timing\_Properties **is **

+----+
+----+

 These properties record information related to the timing of thread
and device execution timing.

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Activate\_Deadline: Time|
| |
| **applies** **to** (Thread);|
| |
| Activate\_Deadline specifies the maximum amount of time allowed for the execution of a thread’s activation sequence. The numeric value of time must be positive.|
| |
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).|
+=========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Activate\_Execution\_Time: Time\_Range  |
| |
| **applies** **to** (thread);|
| |
| Activate\_Execution\_Time specifies the minimum and maximum execution time, in the absence of runtime errors, that a thread will use to execute its activation sequence, i.e., when a thread becomes active as part of a mode switch. The specified execution time includes all time required to execute any service calls that are executed by a thread, but excludes any time spent by another thread executing remote procedure calls in response to a remote subprogram call made by this thread.   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Compute\_Deadline: Time  |
|  |
| **applies** **to** (thread, device, subprogram, subprogram access, event port, event data port); |
|  |
| The Compute\_Deadline specifies the maximum amount of time allowed for the execution of a thread’s compute sequence. If the property is specified for a subprogram, event port, or event data port feature, then this compute execution time applies to the dispatched thread when the corresponding call, event, or event data arrives. When specified for a subprogram access feature, the Compute\_Deadline applies to the thread executing the remote procedure call in response to the remote subprogram call. The Compute\_Deadline specified for a feature must not exceed the Compute\_Deadline of the associated thread. The numeric value of time must be positive.|
|  |
| The values specified for this property for a thread are bounds on the values specified for specific features.|
|  |
| The Deadline property places a limit on Compute\_Deadline and Recover\_Deadline: Compute\_Deadline + Recover\_Deadline ≤ Deadline.   |
|  |
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours). |
+==================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Compute\_Execution\_Time: Time\_Range|
|  |
| **applies** **to** (thread, device, subprogram, event port, event data port);|
|  |
| The Compute\_Execution\_Time property specifies the amount of time that a thread will execute after a thread has been dispatched, before that thread begins waiting for another dispatch. If the property is specified for a subprogram, event port, or event data port feature, then this compute execution time applies to the dispatched thread when the corresponding call, event, or event data initiates a dispatch. When specified for a subprogram (access) feature, it applies to the thread executing the remote procedure call in response to a remote subprogram call. The Compute\_Execution\_Time specified for a feature must not exceed the Compute\_Execution\_Time of the associated thread.   |
|  |
| The range expression specifies a minimum and maximum execution time in the absence of runtime errors. The specified execution time includes all time required to execute any service calls that are executed by a thread, but excludes any time spent by another thread executing remote procedure calls in response to a remote subprogram call made by the thread. |
|  |
| The values specified for this property for a thread are bounds on the Compute\_Execution\_Time values specified for ports or subprogram access that dispatch execution.  |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Client\_Subprogram\_Execution\_Time: Time\_Range |
|  |
| **applies** **to** (subprogram); |
|  |
| The Client\_Subprogram\_Execution\_Time property specifies the length of time it takes to execute the client portion of a remote subprogram call.|
|  |
| The property type is Time\_Range. The standard units are ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours). The numeric value must be a positive number.   |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Deactivate\_Deadline: Time|
|   |
| **applies** **to** (thread);  |
|   |
| The Deactivate\_Deadline property specifies the maximum amount of time allowed for the execution of a thread’s deactivation sequence. The numeric value of time must be positive. |
|   |
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).  |
+===========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Deactivate\_Execution\_Time: Time\_Range  |
|   |
| **applies** **to** (thread);  |
|   |
| The Deactivate\_Execution\_Time property specifies the amount of time that a thread will execute its deactivation sequence, i.e., when the thread is deactivated as part of a mode switch.|
|   |
| The range expression specifies a minimum and maximum execution time in the absence of runtime errors. The specified execution time includes all time required to execute any service calls that are executed by a thread, but excludes any time spent by another thread executing remote procedure calls in response to a remote subprogram call made by this thread. |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Deadline: **inherit** Time => Period  |
|   |
| **applies** **to** (thread, thread group, process, system, device, virtual processor);|
|   |
| The Deadline property specifies the maximum amount of time allowed between a thread dispatch and the time that thread begins waiting for another dispatch. Its numeric value must be positive.|
|   |
| The Deadline property places a limit on Compute\_Deadline and Recover\_Deadline: Compute\_Deadline + Recover\_Deadline ≤ Deadline |
|   |
| The Deadline property may not be specified for threads with background dispatch protocol. |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| First\_Dispatch\_Time **: inherit** Time  |
|   |
| **applies** **to** (thread, thread group);|
|   |
| This property specifies the time of the first dispatch request.   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Dispatch\_Jitter: **inherit** Time|
|   |
| **applies** **to** (thread, thread group);|
|   |
| The Dispatch\_Jitter property specifies a maximum bound on the lateness of a thread dispatching. In the case of a periodic thread for instance, the thread is supposed to be dispatched according to a fixed delay called the period. However, for many reasons, a periodic thread dispatching event can be delayed. The Dispatch\_Jitter property can be used to specify such a delay. The Dispatch\_Jitter property can be specified on any thread which can be dispatched several times (e.g.,. Periodic, Sporadic).   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Dispatch\_Offset: **inherit** Time|
|   |
| **applies** **to** (thread);  |
|   |
| The Dispatch\_Offset property specifies a dispatch time offset for a thread. The offset indicates the amount of clock time by which the dispatch of a thread is offset relative to its period. This property applies only to periodic threads.|
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Execution\_Time: Time   |
| |
| **applies** **to** (virtual processor); |
| |
| The Execution\_Time property specifies the amount of execution time allocated to a virtual processor by the processor it is bound to. This is the amount of execution time the virtual processor can make available to threads or virtual processors it schedules. It is the equivalent of the compute\_execution\_time for a thread.   |
+=========================================================================================================================================================================================================================================================================================================================================================================+
| Finalize\_Deadline: Time|
| |
| **applies** **to** (thread);|
| |
| The Finalize\_Deadline property specifies the maximum amount of time allowed for the execution of a thread’s finalization sequence. The numeric value of time must be positive. |
| |
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).|
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Finalize\_Execution\_Time: Time\_Range  |
| |
| **applies** **to** (thread);|
| |
| The Finalize\_Execution\_Time property specifies the amount of time that a thread will execute its finalization sequence.   |
| |
| The range expression specifies a minimum and maximum execution time in the absence of runtime errors. The specified execution time includes all time required to execute any service calls that are executed by a thread, but excludes any time spent by another thread executing remote procedure calls in response to a remote subprogram call made by this thread.   |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Initialize\_Deadline: Time  |
| |
| **applies** **to** (thread);|
| |
| The Initialize\_Deadline property specifies the maximum amount of time allowed between the time a thread executes its initialization sequence and the time that thread begins waiting for a dispatch. The numeric value of time must be positive.   |
| |
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).|
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Initialize\_Execution\_Time: Time\_Range|
| |
| **applies** **to** (thread);|
| |
| The Initialize\_Execution\_Time property specifies the amount of time that a thread will execute its initialization sequence.   |
| |
| The range expression specifies a minimum and maximum execution time in the absence of runtime errors. The specified execution time includes all time required to execute any service calls that are executed by a thread, but excludes any time spent by another thread executing remote procedure calls in response to a remote subprogram call made by this thread.   |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Load\_Deadline: Time|
| |
| **applies** **to** (process, system);   |
| |
| The Load\_Deadline property specifies the maximum amount of elapsed time allowed between the time the process begins and completes loading. Its numeric value must be positive. |
| |
| The property type is Time. The standard units are ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).  |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Load\_Time: Time\_Range |
| |
| **applies** **to** (process, system);   |
| |
| The Load\_Time property specifies the amount of execution time that it will take to load the binary image associated with a process. The numeric value of time must be positive.|
| |
| When applied to a system, the property specifies the amount of time it takes to load the binary image of data components declared within the system implementation and shared across processes (and their address spaces).  |
| |
| The range expression specifies a minimum and maximum load time in the absence of runtime errors.|
+=========================================================================================================================================================================================================================================================================================================================================================================+
| Period: **inherit** Time|
| |
| **applies** **to** (thread, thread group, process, system, device, virtual processor, bus, virtual bus);|
| |
| The Period property specifies the time interval between successive dispatches of a thread whose scheduling protocol is *periodic*, or the minimum interval between successive dispatches of a thread whose scheduling protocol is *sporadic*.   |
| |
| The property type is Time. The standard units are ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours). The numeric value must be a single positive number.  |
| |
| A Period property association is only allowed if the thread scheduling protocol is either periodic or sporadic, timed or hybrid.|
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Recover\_Deadline: Time |
| |
| **applies** **to** (thread);|
| |
| Recover­\_Deadline specifies the maximum amount of time allowed between the time when a detected error occurs and the time a thread begins waiting for another dispatch. Its numeric value must be positive.|
| |
| The Recover\_Deadline property may not be specified for threads with background dispatch protocol.  |
| |
| The Recover\_Deadline must not be greater than the specified period for the thread, if any. |
| |
| The Deadline property places a limit on Compute\_Deadline and Recover\_Deadline: Compute\_Deadline + Recover\_Deadline ≤ Deadline.  |
| |
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).|
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Recover\_Execution\_Time: Time\_Range   |
| |
| **applies** **to** (thread);|
| |
| The Recover\_Execution\_Time property specifies the amount of time that a thread will execute after an error has occurred, before it begins waiting for another dispatch.   |
| |
| The range expression specifies a minimum and maximum execution time in the absence of runtime errors. The specified execution time includes all time required to execute any service calls that are executed by a thread, but excludes any time spent by another thread executing remote procedure calls in response to a remote subprogram call made by this thread.   |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Startup\_Deadline: Time|
||
| applies to (processor, virtual processor, process, system);|
||
| The Startup\_Deadline property specifies the deadline for processor, virtual processor, process, and system initialization.|
||
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours). The numeric value must be a single positive number.   |
+================================================================================================================================================================================================================================+
| Startup\_Execution\_Time: Time\_Range  |
||
| applies to (virtual processor, processor, process, system);|
||
| The Startup\_Execution\_Time property specifies the execution time for initialization of a virtual processor or process. Initialization time for threads is accounted for through its initialize entrypoint.   |
||
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours). The numeric value must be a single positive number.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

 The following properties specify timing information related to the
computing platform executing threads.

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Clock\_Jitter: Time   |
|   |
| **applies** **to** (processor, system);   |
|   |
| The Clock\_Jitter property specifies a time unit value that gives the maximum time between the start of clock interrupt handling on any two processors in a multi-processor system.   |
|   |
| The property type is Time. The standard units are ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours). The numeric value must be a positive number.   |
+=======================================================================================================================================================================================================+
| Clock\_Period: Time   |
|   |
| **applies** **to** (processor, system);   |
|   |
| The Clock\_Period property specifies a time unit value that gives the time interval between two clock interrupts. |
|   |
| The property type is Time. The standard units are ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours). The numeric value must be a positive number.   |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Clock\_Period\_Range: Time\_Range |
|   |
| **applies** **to** (processor, system);   |
|   |
| The Clock\_Period\_Range property specifies a time range value that represents the minimum and maximum value assignable to the Clock\_Period property.|
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Process\_Swap\_Execution\_Time: Time\_Range  |
|  |
| **applies to** (processor);  |
|  |
| The Process\_Swap\_Execution\_Time property specifies the amount of execution time necessary to perform a context swap between two threads contained in different processes. |
|  |
| The range expression specifies a minimum and maximum swap time in the absence of runtime errors. |
+======================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Reference\_Processor: **inherit classifier** ( processor )   |
|  |
| **applies** **to** (subprogram, subprogram group, thread, thread group, process, device, system);|
|  |
| The Reference\_Processor property specifies the processor based on which the execution time is specified. When code is bound to a different processor type, the Processor\_Capacity of that processor is used to determine the execution, unless a binding specific execution time value is associated.  |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Processor\_Capacity: **aadlreal** Processor\_Speed\_Units|
|  |
| **applies to** (processor, virtual processor, system);   |
|  |
| This property specifies the capacity/speed of a processor in terms of instructions per time unit. The ratio of processor capacity between processors is used to determine a scaling factor for adjusting execution times to specific processors. |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Reference\_Time: **inherit reference** (processor, device, bus, system, abstract)|
|  |
| **applies to** (processor, device, memory, bus, virtual processor, virtual bus, system); |
|  |
| This property identifies the reference time being used by the component the property applies to. The reference time is represented by an instance of a processor, device, bus, or system component type. Note that abstract component types are always allowed   |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Scheduler\_Quantum : **inherit** Time|
|  |
| **applies to** (processor);  |
|  |
| This property specifies the quantum of a given processor. The quantum is a maximum bound on the time a thread can hold the processor without being preempted. A quantum is typically used in time sharing scheduling and in POSIX 1003.1b scheduling (with the SCHED\_RR policy). The quantum can be used with any user-defined schedulers. If the quantum is not specified for a given processor, the quantum has a positive infinitesimal value.   |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Thread\_Swap\_Execution\_Time: Time\_Range   |
|  |
| **applies** **to** (processor, system);  |
|  |
| The Thread\_Swap\_Execution\_Time property specifies the amount of execution time necessary for performing a context swap between two threads contained in the same process. |
|  |
| The range expression specifies a minimum and maximum swap time in the absence of runtime errors. |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Frame\_Period: Time  |
|  |
| **applies** **to** (processor, virtual processor);   |
|  |
| The Frame\_Period property specifies the time period of a major frame in a static scheduling protocol, such as a cyclic executive.   |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Slot\_Time: Time |
|  |
| **applies** **to** (processor, virtual processor);   |
|  |
| The Slot\_Time property specifies the time period of a slot in major frame in a static scheduling protocol, such as a cyclic executive, if the protocol uses fixed slot times.   |
+==================================================================================================================================================================================+
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**end** Timing\_Properties;

Predeclared Communication Properties
------------------------------------

 The predeclared property set named Communication\_Properties defines
communication related properties specify connection topology and
queuing characteristics.

**Property** **set** Communication\_Properties **is **

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Fan\_Out\_Policy: **enumeration** (Broadcast, RoundRobin, Selective, OnDemand) |
||
| **applies** **to** (feature);  |
||
| The Fan\_Out\_Policy property specifies how the output is distributed to multiple recipients of a port with multiple outgoing connections. Broadcast sends to all recipients, RoundRobin to one recipient at a time in order, Selective sends to one recipient based on data content, and OnDemand to the next recipient waiting on a port for dispatch.   |
||
| Note that Broadcast, RoundRobin, and Selective pass on data and events without queuing it, while OnDemand requires a queue that is serviced by the recipients. |
+============================================================================================================================================================================================================================================================================================================================================================+
| Data\_Rate: **aadlinteger** **units** Data\_Rate\_Units|
||
| **applies to** ( feature, connection, bus, virtual bus, system, processor, virtual processor, device, memory );|
||
| The Data\_Rate property type specifies a property for the rate of data per time unit. The numeric value of the property must be positive.  |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Connection\_Pattern: **list of list of** Supported\_Connection\_Patterns   |
||
| **applies** **to** (connection);   |
||
| The Connection\_Pattern property specifies how an individual connection between arrays of ports looks like. If the property is not set the One\_to\_One pattern applies. The outer list has an element for each array dimension. The inner list specifies one or more patterns for that dimension. |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Connection\_Set: **list of** Connection\_Pair  |
||
| **applies** **to** (connection);   |
||
| The Connection\_Set property specifies a list of specific source element and destination element of a semantic connection by their array indices.  |
||
| Connection\_Pair: **type record** (|
||
| src: **list of aadlinteger**;  |
||
| dst: **list of aadlinteger;**);|
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Overflow\_Handling\_Protocol: **enumeration** (DropOldest, DropNewest, Error)   |
| |
| => DropOldest   |
| |
| **applies** **to** (event port, event data port, subprogram access);|
| |
| The Overflow\_Handling\_Protocol property specifies the runtime behavior of a thread when an event arrives and the queue is full. DropOldest removes the oldest event from the queue and adds the new arrival. DropNewest ignores the newly arrived event. Error causes the thread’s error recovery to be invoked. The default value is DropOldest. |
+=========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Queue\_Processing\_Protocol: Supported\_Queue\_Processing\_Protocols => FIFO|
| |
| **applies** **to** (event port, event data port, subprogram access);|
| |
| The Queue\_Processing\_Protocol property specifies the protocol for processing elements in the queue.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Queue\_Size: **aadlinteger** 0 **..** Max\_Queue\_Size => 1 |
| |
| **applies** **to** (event port, event data port, subprogram access);|
| |
| The Queue\_Size property specifies the size of the queue for an event, event data port, of a subprogram access feature,. In the case of a subprogram access it represents the queue for remote subprogram calls.|
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Required\_Connection : **aadlboolean** **=>** **true**  |
| |
| **applies to** (feature);   |
| |
| The Required\_Connection property specifies whether the port or subprogram requires a connection. If the value of this property is false, then it is assumed that the component can function without this port or subprogram access feature being connected.|
| |
| The default value is that a connection is required. |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Timing : **enumeration** (sampled, immediate, delayed) **=>** sampled   |
| |
| **applies to** (port connection);   |
| |
| The Timing property specifies the timing of port connections. By default the interaction is sampled, i.e., the receiving component samples at dispatch or during execution. |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Transmission\_Type: **enumeration** ( push, pull )  |
| |
| **applies to** (feature, connection, bus, virtual bus); |
| |
| The Transmission\_Type property specifies whether the transmission across a data port connection is initiated by the sender (push) or by the receiver (pull). By default the transmission is initiated by the sender. A pull transmission type results in data being transmitted at the rate of the receiver. In the case of event data port or event ports, a pull transmission type results in events or event data queued with the sender to be transmitted upon receiver request.   |
| |
| When associated with a connection the property represents the transmission type the connection expects. When associated with a port the property represents the transmission type expected by the port. When associated with a bus or virtual bus the property represents the transmission type that is provided by the bus or protocol.|
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

 The following communication properties specify input and output
characteristics of port based communication.

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Input\_Rate: Rate\_Spec => [ Value\_Range => 1.0 .. 1.0; Rate\_Unit => PerDispatch; Rate\_Distribution => Fixed; ]  |
| |
| **applies** **to** (feature);   |
| |
| The Input\_Rate property specifies the number of inputs per dispatch or per second of data, events, event data, or subprogram calls. If no Input\_Rate is specified the default is one input per thread dispatch.   |
| |
| If no distribution function is specified it is assumed to be Fixed. |
+=========================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Input\_Time: **list of** IO\_Time\_Spec => ([ Time => Dispatch; Offset => 0.0 ns .. 0.0 ns;])   |
| |
| **applies** **to** (feature);   |
| |
| The Input\_Time property specifies the amount of execution time that can pass after dispatch before the input is frozen on a given port. The property value is a pair of Time a time range Offset. The default input time is Dispatch with zero Offset. A typical property value is a time offset in terms of Start.|
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| IO\_Time\_Spec : **type record** (  |
| |
| Offset : Time\_Range;   |
| |
| Time : IO\_Reference\_Time; |
| |
| );  |
| |
| The IO\_Time\_Spec property specifies the amount of execution time Offset relative to a Time at which input or output occurs. The value consists of a reference point and time range pair.  |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| IO\_Reference\_Time : **type enumeration** (Dispatch, Start, Completion, Deadline, NoIO, Dynamic);  |
| |
| The IO\_Reference\_Time property specifies the time reference point to be used for specifying when input or output is available. The reference points are dispatch time (typically with zero time offset), start time (zero or more time), completion time (amount of execution time until completion), and deadline (typically with zero time offset). NoIO indicates that no I/O occurs. Dynamic indicates that, if I/O occurs, the time can be anytime during execution without specifying a specific offset relative to start or completion time.   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Output\_Rate: Rate\_Spec => [ Value\_Range => 1.0 .. 1.0; Rate\_Unit => PerDispatch; Rate\_Distribution => Fixed; ] |
| |
| **applies** **to** (feature);   |
| |
| The Output\_Rate property specifies the number of outputs per dispatch or per second of data, events, event data, or initiations of subprogram calls. The default is one output per thread dispatch and the default distribution is Fixed.  |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Output\_Time: **list of** IO\_Time\_Spec => ([ Time => Completion; Offset => 0.0 ns .. 0.0 ns;])|
| |
| **applies** **to** (feature);   |
| |
| The Output\_Time property specifies the amount of execution time until completion at which output becomes available. The property value is a pair of Time and Offset. The default Output\_Time is Completion with zero Offset. For data ports with a delayed connection the default output time is Deadline.|
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Rate\_Spec : **type record** (|
|   |
| Value\_Range : **range of aadlreal** **;**|
|   |
| Rate\_Unit **: enumeration** (PerSecond, PerDispatch);|
|   |
| Rate\_Distribution : Supported\_Distributions;|
|   |
| );|
|   |
| The Rate\_Spec property specifies the number of input or output occurrences per Rate\_Unit, i.e., per dispatch or per second. The default Rate\_Distribution is Fixed.|
+===============================================================================================================================================================================================+
| Subprogram\_Call\_Rate: Rate\_Spec => [ Value\_Range => 1.0 .. 1.0; Rate\_Unit => PerDispatch; Rate\_Distribution => Fixed; ] |
|   |
| **applies** **to** (subprogram access);   |
|   |
| The Subprogram\_Call\_Rate property specifies the number of subprogram calls per dispatch or per second. The default is one call per thread dispatch and the default distribution is Fixed.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

 The following are communication timing related properties.

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Transmission\_Time: **record** ( |
|  |
| Fixed: Time\_Range;  |
|  |
| PerByte: Time\_Range; )  |
|  |
| **applies** **to** (bus);|
|  |
| The Transmission\_Time property specifies the parameters for a linear model for the time interval between the start and end of a transmission of a sequence of N bytes onto a bus. The transmission time is the time between the transmission of the first bit of the message onto the bus and the transmission of the last bit of the message onto the bus. This time excludes arbitration, queuing, or any other times that are a function of how bus contention and scheduling are performed for a bus.   |
|  |
| The associated expressions must evaluate to a record of two ranges of nonnegative numbers.   |
|  |
| The time required to transmit a message of N Bytes over the bus is Fixed + N \* PerByte, where Fixed is any number that falls in the range of the Fixed record field and PerByte is any number that falls in the range of the PerByte record field.  |
+==============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Actual\_Latency: Time\_Range |
|  |
| **applies** **to** (flow, connection, bus, virtual bus, device, processor, virtual processor, system, memory, feature);  |
|  |
| The Actual\_Latency property specifies the actual latency as determined by the implementation of the end-to-end flow through semantic connections. Its numeric value must be positive.   |
|  |
| The property type is Time\_Range. The standard units are ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Latency: Time\_Range |
|  |
| **applies** **to** (flow, connection, bus, virtual bus, device, processor, virtual processor, system, memory, feature);  |
|  |
| The Latency property specifies the minimum and maximum amount of elapsed time allowed between the time the data or events enter the connection or flow and the time it exits. Its numeric value must be positive.|
|  |
| The property type is Time. The standard units are ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**end** Communication\_Properties;

Predeclared Memory Properties 
------------------------------

 The predeclared property set named Memory\_Properties defines
properties related to memory as storage, memory and device access.

**Property** **set** Memory\_Properties **is **

+----+
+----+

 These properties record information related to memory and access of
data.

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Access\_Right : Access\_Rights => read\_write|
|  |
| **applies to** (Data, Bus, Data Access, Bus Access); |
|  |
| The Access\_Right property specifies the form of access that is permitted for a component. If associated with a requires access clause it specifies the intended access to the component being accessed. If associated with a provides access clause it specifies the type of access that is permitted to the component for which access is provided. This access may be direct through read and write access or indirect through subprograms provided with the data type. The provided access Access\_Right must not exceed the access right specified for the component itself. The required access Access\_Right must not exceed the access right specified by the provides access or the component itself.   |
+======================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Access\_Rights : **type** **enumeration** (read\_only, write\_only, read\_write, |
|  |
| by\_method); |
|  |
| The Access\_Rights property type specifies the literals used to indicate the form of access that is permitted for a component.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Access\_Time: **record** (   |
|  |
| First: IO\_Time\_Spec ;  |
|  |
| Last: IO\_Time\_Spec ; ) |
|  |
| => [ First =>[Time => Start; Offset => 0.0 ns .. 0.0 ns;];   |
|  |
| Last => [Time => Completion; Offset => 0.0 ns .. 0.0 ns;]; ] |
|  |
| **applies** **to** (data access);|
|  |
| The Access\_Time property specifies the range of execution time during which the data component is being accessed.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Allowed\_Message\_Size: Size\_Range   |
|   |
| **applies** **to** (bus); |
|   |
| The Allowed\_Message\_Size property specifies the allowed range of sizes for a block of data that can be transmitted by the bus hardware in a single transmission (in the absence of packetization).  |
|   |
| The expression defines the range of data message sizes, excluding any header or packetization overheads added due to bus protocols, that can be sent in a single transmission over a bus. Messages whose sizes fall below this range will be padded. Messages whose sizes fall above this range must be broken into two or more separately transmitted packets.   |
+===================================================================================================================================================================================================================================================================================================================================================================+
| Assign\_Time: **record** (|
|   |
| Fixed: Time\_Range;   |
|   |
| PerByte: Time\_Range; )   |
|   |
| **applies** **to** (processor);   |
|   |
| The Assign\_Time property specifies a time unit value used in a linear estimation of the execution time required to move a block of bytes on a particular processor. The time required is (Number\_of\_Bytes \* PerByte) + Fixed  |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Base\_Address: **aadlinteger** 0 **..** Max\_Base\_Address|
|   |
| **applies** **to** (memory, data, data access, port); |
|   |
| The Base\_Address property specifies the address of the first word in the memory. The addresses used to access successive words of memory are Base\_Address, Base\_Address + Word\_Space, …Base\_Address + (Word\_Count-1) \* Word\_Space.|
|   |
| In the case of a memory component it indicates what its starting address is. In the case of a data component, data access, or port it indicates what its starting address is in the memory it is bound to.|
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Device\_Register\_Address: **aadlinteger**|
|   |
| **applies** **to** (port, feature group); |
|   |
| The Device\_Register\_Address property specifies the address of a device register that is represented by a port associated with a device. This property is optional. Device ports may be represented by a source text variable as part of the device driver software. |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Read\_Time: **record** (  |
|   |
| Fixed: Time\_Range;   |
|   |
| PerByte: Time\_Range; )   |
|   |
| **applies** **to** (memory);  |
|   |
| The Read\_Time property specifies a time unit value used in a linear estimation of the execution time required to read a block of bytes from memory. The time required is (Number\_of\_Bytes \* PerByte) + Fixed  |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Code\_Size: Size   |
||
| **applies** **to** (data, thread, thread group, process, system, subprogram, processor, virtual processor, device);|
||
| The Code\_Size property specifies the size of the static code and read-only data that results when the associated source text is compiled, linked, bound and loaded in the final system.   |
||
| The property type is Size. The standard units are bits, Bytes (bytes), KByte (kilobytes), MByte (megabytes) and GByte (gigabytes). |
+============================================================================================================================================================================================================================================================================================================+
| Data\_Size: Size   |
||
| **applies** **to** (data, subprogram, thread, thread group, process, system, processor, virtual processor, device);|
||
| The Data\_Size property specifies the size of the readable and writeable data that results when the associated source text is compiled, linked, bound and loaded in the final system. In the case of data types, it specifies the maximum size required to hold a value of an instance of the data type.   |
||
| The property type is Size. The standard units are bits, Bytes (bytes), KByte (kilobytes), MByte (megabytes) and GByte (gigabytes). |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Heap\_Size: Size   |
||
| **applies to** (thread, subprogram);   |
||
| The Heap\_Size property specifies the maximum heap size requirements of a thread or subprogram.|
||
| The property type is Size. The standard units are bits, Bytes (bytes), KByte (kilobytes), MByte (megabytes) and GByte (gigabytes). |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Stack\_Size: Size  |
||
| **applies** **to** (thread, subprogram, processor, virtual processor, device); |
||
| The Stack\_Size property specifies the maximum size of the stack used by a processor executive, a device driver, a thread or a subprogram during execution.|
||
| The property type is Size. The standard units are bits, Bytes (bytes), KByte (kilobytes), MByte (megabytes) and GByte (gigabytes). |
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Byte\_Count: **aadlinteger** 0 **..** Max\_Byte\_Count |
||
| **applies** **to** (memory);   |
||
| The Byte\_Count property specifies the number of bytes in the memory. Note: Deprecated. Please use Memory\_Size|
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Memory\_Size: Size |
||
| **applies** **to** (memory, processor, virtual processor, system); |
||
| The Memory\_Size property specifies the size of the memory.|
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Word\_Size: Size **=>** 8 bits |
||
| **applies** **to** (memory);   |
||
| The Word\_Size property specifies the size of the smallest independently readable and writeable unit of storage in the memory. |
||
| The property type is Size. The standard units are bits, Bytes (bytes), KByte (kilobyte), MByte (megabyte) and GByte (gigabyte).|
+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Word\_Space: **aadlinteger** 1 **..** Max\_Word\_Space **=>** 1|
||
| **applies** **to** (memory);   |
||
| The Word\_Space specifies the interval between successive addresses used for successive words of memory. A value greater than 1 means the addresses used to access words are not contiguous integers.  |
||
| The default value is 1.|
+====================================================================================================================================================================================================================+
| Write\_Time: **record** (  |
||
| Fixed: Time\_Range;|
||
| PerByte: Time\_Range; )|
||
| **applies** **to** (memory);   |
||
| The Write\_Time property specifies a time unit value used in a linear estimation of the execution time required to write a block of bytes to memory. The time required is (Number\_of\_Bytes \* PerByte) + Fixed   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**end** Memory\_Properties;

Predeclared Programming Properties
----------------------------------

 The predeclared property set named Programming\_Properties contains
properties that record information related to the implementation of
application components and hardware components in an implementation
language.

**Property** **set** Programming\_Properties **is **

+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Activate\_Entrypoint: **classifier** ( subprogram classifier )  |
| |
| **applies** **to** (thread, device, processor, virtual processor);  |
| |
| The Activate\_Entrypoint property specifies the name of a subprogram classifier. This classifier represents a subprogram in the source text that will execute when a thread or device are activated as part of a mode switch.   |
| |
| The subprogram in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.   |
+=============================================================================================================================================================================================================================================================================================================================+
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Activate\_Entrypoint\_Call\_Sequence: **reference** ( subprogram call sequence )   |
||
| **applies** **to** (thread, device);   |
||
| The Activate\_Entrypoint\_Call\_Sequence property specifies the name of a subprogram call sequence. This call sequence will execute after a thread has been dispatched. If the property is specified for a provided subprogram, event port, or event data port feature, then this entrypoint is chosen when the corresponding call, event, or event data arrives instead of the compute entrypoint specified for the containing thread.|
||
| The named call sequence must exist in the source text as parameterless function, which performs the calls and passes the appropriate port and parameter values as actual parameters. This function may be generated from AADL. This function must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.   |
+============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Activate\_Entrypoint\_Source\_Text: **aadlstring** |
||
| **applies** **to** (thread, device, processor, virtual processor); |
||
| The Activate\_Entrypoint\_Source\_Text property specifies the name of a source text code sequence that will execute when a thread is activated.|
||
| The named code sequence in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable source language. The source language annex of this standard defines acceptable parameter and result signatures for the entrypoint subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Compute\_Entrypoint: **classifier** ( subprogram classifier )  |
||
| **applies** **to** (thread, device, provides subprogram access, event port, event data port);  |
||
| The Compute\_Entrypoint property specifies the name of a subprogram classifier. This classifier represents a subprogram in the source text that will execute after a thread has been dispatched. If the property is specified for a provided subprogram, event port, or event data port feature, then this entrypoint is chosen when the corresponding call, event, or event data arrives instead of the compute entrypoint specified for the containing thread.   |
||
| The subprogram in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Compute\_Entrypoint\_Call\_Sequence: **reference** ( subprogram call sequence )|
||
| **applies** **to** (thread, device, provides subprogram access, event port, event data port);  |
||
| The Compute\_Entrypoint\_Call\_Sequence property specifies the name of a subprogram call sequence. This call sequence will execute after a thread has been dispatched. If the property is specified for a provided subprogram, event port, or event data port feature, then this entrypoint is chosen when the corresponding call, event, or event data arrives instead of the compute entrypoint specified for the containing thread. |
||
| The named call sequence must exist in the source text as parameterless function, which performs the calls and passes the appropriate port and parameter values as actual parameters. This function may be generated from AADL. This function must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Compute\_Entrypoint\_Source\_Text: **aadlstring**  |
||
| **applies** **to** (thread, device, provides subprogram access, event port, event data port);  |
||
| The Compute\_Entrypoint\_Source\_Text property specifies the name of a source text code sequence that will execute after a thread has been dispatched. If the property is specified for a provided subprogram, event port, or event data port feature, then this entrypoint is chosen when the corresponding call, event, or event data arrives instead of the compute entrypoint specified for the containing thread. |
||
| The named code sequence in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram. |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Deactivate\_Entrypoint: **classifier** ( subprogram classifier )   |
||
| **applies** **to** (thread, device);   |
||
| The Deactivate\_Entrypoint property specifies the name of a subprogram classifier. This classifier represents a subprogram in the source text that will execute when a thread is deactivated as part of a mode switch. |
||
| The subprogram in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Deactivate\_Entrypoint\_Call\_Sequence: **reference** ( subprogram call sequence ) |
||
| **applies** **to** (thread, device);   |
||
| The Deactivate\_Entrypoint\_Call\_Sequence property specifies the name of a subprogram call sequence. This call sequence will execute when a thread is deactivated as part of a mode switch.   |
||
| The named call sequence must exist in the source text as parameterless function, which performs the calls and passes the appropriate port and parameter values as actual parameters. This function may be generated from AADL. This function must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Deactivate\_Entrypoint\_Source\_Text: **aadlstring**   |
||
| **applies** **to** (thread);   |
||
| The Deactivate\_Entrypoint\_Source\_Text property specifies the name of a source text code sequence that will execute when a thread is deactivated.|
||
| The named code sequence in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram. |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Finalize\_Entrypoint: **classifier** ( subprogram classifier ) |
||
| **applies** **to** (thread, device);   |
||
| The Finalize\_Entrypoint property specifies the name of a subprogram classifier. This classifier represents a subprogram in the source text that will execute when a thread is finalized.  |
||
| The subprogram in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.  |
+============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Finalize\_Entrypoint\_Call\_Sequence: **reference** ( subprogram call sequence )   |
||
| **applies** **to** (thread, device);   |
||
| The Finalize\_Entrypoint\_Call\_Sequence property specifies the name of a subprogram call sequence. This call sequence will execute when a thread is finalized.|
||
| The named call sequence must exist in the source text as parameterless function, which performs the calls and passes the appropriate port and parameter values as actual parameters. This function may be generated from AADL. This function must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Finalize\_Entrypoint\_Source\_Text: **aadlstring** |
||
| **applies** **to** (thread, device);   |
||
| The Finalize\_Entrypoint\_Source\_Text property specifies the name of a source text code sequence that will execute when a thread is finalized.|
||
| The named code sequence in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entrypoint subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Initialize\_Entrypoint: **classifier** ( subprogram classifier )   |
||
| **applies** **to** (thread, device);   |
||
| The Initialize\_Entrypoint property specifies the name of a subprogram classifier. This classifier represents a subprogram in the source text that will execute when a thread is initialized.  |
||
| The subprogram in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Initialize\_Entrypoint\_Call\_Sequence: **reference** ( subprogram call sequence ) |
||
| **applies** **to** (thread, device);   |
||
| The Initialize\_Entrypoint\_Call\_Sequence property specifies the name of a subprogram call sequence. This call sequence will execute when a thread is initialized.|
||
| The named call sequence must exist in the source text as parameterless function, which performs the calls and passes the appropriate port and parameter values as actual parameters. This function may be generated from AADL. This function must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Initialize\_Entrypoint\_Source\_Text: **aadlstring**   |
||
| **applies** **to** (thread, device);   |
||
| The Initialize\_Entrypoint\_Source\_Text property specifies the name of a source text code sequence that will execute when a thread is to be initialized.  |
||
| The named code sequence in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entrypoint subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Recover\_Entrypoint: **classifier** ( subprogram classifier )  |
||
| **applies** **to** (thread, device);   |
||
| The Recover\_Entrypoint property specifies the name of a subprogram classifier. This classifier represents a subprogram in the source text that will execute when a thread is in recovery. |
||
| The subprogram in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Recover\_Entrypoint\_Call\_Sequence: **reference** ( subprogram call sequence )|
||
| **applies** **to** (thread, device);   |
||
| The Recover\_Entrypoint\_Call\_Sequence property specifies the name of a subprogram call sequence. This call sequence will execute when a thread is in recovery.   |
||
| The named call sequence must exist in the source text as parameterless function, which performs the calls and passes the appropriate port and parameter values as actual parameters. This function may be generated from AADL. This function must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entry point subprogram.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Recover\_Entrypoint\_Source\_Text: **aadlstring**  |
||
| **applies** **to** (thread, device);   |
||
| The Recovery\_Entrypoint\_Source\_Text property specifies the name of a source text code sequence that will execute when a thread is recovering from a fault.  |
||
| The named code sequence in the source text must be visible in and callable from the outermost program scope, as defined by the scope and visibility rules of the applicable implementation language. The source language annex of this standard defines acceptable parameter and result signatures for the entrypoint subprogram.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Source\_Language: inherit list of Supported\_Source\_Languages |
||
| applies to (subprogram, data, thread, thread group, process, system,   |
||
| bus, device, processor, virtual bus, virtual processor);   |
||
| The Source\_Language property specifies an applicable programming language.|
||
| The property type is enumeration. The standard enumeration literals are Ada, C, and Simulink\ :sup:`®` for software categories. Other enumeration literals are permitted.  |
||
| There is no default value for the source language property.|
||
| Where a source language is specified for a component, the source text associated with that component must comply with the applicable programming language standard. Where a source language is not specified, a method of processing may infer a source language from the syntax of the source text pathname. A method of processing may establish a default value for the source language property.   |
||
| Where a source language property is specified for a component, any specified source language and any specified source text for the complete set of software subcomponents of that component must comply with the applicable language standard. |
||
| Multiple source languages, and source text written in those languages, are compliant with a specified source language if the applicable language standard permits mixing source text units of those languages within the same program. |
||
| A method of processing may accept data produced by processing a source text file, for example an object file produced by compiling source text may be considered compliant with the applicable language standard. A method of processing may accept non-standard source languages. A method of processing may restrict itself to specific source languages, either standard or non-standard.   |
||
| The source language associated with processor or virtual processor indicates the source language of any software that implements virtual processor or processor functionality. |
+========================================================================================================================================================================================================================================================================================================================================================================================================+
| Source\_Name: **aadl**\ string |
||
| applies to (data, port, subprogram, parameter, virtual bus, virtual processor);|
||
| The Source\_Name property specifies a source declaration or source name within the associated source text that corresponds to a feature defining identifier.   |
||
| The default value is defined in the source language annex of this standard.|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Source\_Text: **inherit list of aadlstring**   |
||
| **applies to** (data, port, subprogram, thread, thread group, process, system, |
||
| virtual bus, virtual processor, memory, bus, device, processor, parameter, feature group, package);|
||
| The Source\_Text property specifies a list of files that contain source text.  |
||
| Each string is interpreted as a POSIX pathname and must satisfy the syntax and semantics for path names as defined in the POSIX standard. Extensions to the standard POSIX pathname syntax and semantics are permitted. For example, environment variables and regular expressions are permitted. Special characters may be used to assist in configuration management and version control.|
||
| If the first character of a pathname is **.** (dot) then the path is relative to the directory in which the file containing the AADL specification text is stored. Otherwise, the method of processing may define the default directory used to locate the file designated by a pathname.  |
||
| There is no standard default value for the source text property.   |
||
| The combined source text contained in all files named in the associated expression must form one or more separately compileable units as defined in the applicable source language standard.   |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Source\_Language: list of Supported\_Source\_Languages  |
||
| applies to (processor, virtual processor, system); |
||
| The Supported\_Source\_Language property specifies the source language(s) supported by a processor.|
||
| The source language of every software component that may be accessed by any thread bound to a processor must appear in the list of allowed source languages for that processor.|
||
| If an allowed source language list is not specified, then there are no restrictions on the source. |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Type\_Source\_Name: **aadl**\ string   |
||
| applies to (data, port, subprogram);   |
||
| The Type\_Source\_Name property specifies a source declaration or source name within the associated source text that corresponds to a feature defining identifier. |
||
| The default value is defined in the source language annex of this standard.|
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Hardware\_Description\_Source\_Text: **inherit** **list of aadlstring**|
||
| **applies to** (memory, bus, device, processor, system);   |
||
| The Hardware\_Description\_Source\_Text property specifies a list of files that contain source text of a hardware description in a hardware description language.  |
||
| Each string is interpreted as a POSIX pathname and must satisfy the syntax and semantics for path names as defined in the POSIX standard. Extensions to the standard POSIX pathname syntax and semantics are permitted. For example, environment variables and regular expressions are permitted. Special characters may be used to assist in configuration management and version control.|
||
| If the first character of a pathname is **.** (dot) then the path is relative to the directory in which the file containing the AADL specification text is stored. Otherwise, the method of processing may define the default directory used to locate the file designated by a pathname.  |
+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Hardware\_Source\_Language: Supported\_Hardware\_Source\_Languages  |
| |
| applies to (memory, bus, device, processor, system);|
| |
| The Hardware\_Source\_Language property specifies an applicable hardware description language.  |
| |
| The hardware description source text associated with a hardware component is written in a hardware description language.|
+=================================================================================================================================================================================+
| Device\_Driver: classifier (abstract implementation)|
| |
| applies to (device);|
| |
| The Device\_Driver property specifies a reference to an abstract component implementation that contains an instance of a driver subprogram or an instance of an (ISR) thread.   |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**end** Programming\_Properties;

Predeclared Modeling Properties 
--------------------------------

 The predeclared property set named Modeling\_Properties defines
properties related to the AADL model itself.

**Property** **set** Modeling\_Properties **is **

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Acceptable\_Array\_Size: **list of** Array\_Size\_Range  |
|  |
| **applies** **to** (subcomponent, feature);  |
|  |
| The Acceptable\_Array\_Size property specifies acceptable values for the array size in each dimension. This property can be used to impose a constraint on the array dimension size if it has not been specified to be refined separately.   |
|  |
| Array\_Size\_Range: **type range of aadlinteger** ;  |
+==============================================================================================================================================================================================================================================+
| Classifier\_Matching\_Rule: **inherit** **enumeration** (Classifier\_Match, Equivalence, Subset, Conversion, Complement) |
|  |
| **applies** **to** (connection, component implementation);   |
|  |
| The Classifier\_Matching\_Rule property specifies acceptable matches of classifiers between the source and the destination of a connection. The semantics of each of the rules are defined in sections 9.1 and 9.5.  |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Classifier\_Substitution\_Rule: **inherit** **enumeration** (Classifier\_Match, Type\_Extension, Signature\_Match)   |
|  |
| **applies** **to** (classifier, subcomponent, feature);  |
|  |
| The Classifier\_Substitution\_Rule property specifies acceptable substitutions of classifiers in a **refined to** declaration.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Implemented\_As: **classifier** ( system implementation, abstract implementation ) |
||
| **applies to** (memory, bus, virtual bus, device, virtual processor, processor, system);   |
||
| The Implemented\_As property specifies a system implementation that describes how the internals of a execution platform component are realized. This allows systems to be modeled as a layered architecture using the execution platform as a layering abstraction (see Section 14).   |
+========================================================================================================================================================================================================================================================================================+
| Prototype\_Substitution\_Rule: **inherit** **enumeration** (Classifier\_Match, Type\_Extension, Signature\_Match)  |
||
| **applies** **to** (prototype, classifier);|
||
| The Prototype\_Substitution\_Rule property specifies acceptable classifiers supplied as actual in a prototype binding. |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**end** Modeling\_Properties;

Project-Specific Property Set
-----------------------------

 There is a set of enumeration property types and property constants
for which enumeration literals and property constant values can be
defined for different systems modeled in AADL. This set of property
types is declared in a property set named AADL\_Project. All of the
property enumeration types listed in this section must be declared
in this property set. The set of enumeration literals may vary. This
property set is a part of every AADL specification.

 The set of values for the property types in this property set are to
be provided by tool suppliers for the AADL and can be tailored by
AADL users on a project by project basis to reflect those
capabilities provided by tool suppliers that are actually being made
use of in a particular project.

NOTE: The label <project-specified> indicates that actual values are to
be supplied by the person providing a project-specific property set. The
actual values do not include the < and > symbols.

**Property** **set** AADL\_Project **is **

+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Active\_Thread\_Handling\_Protocols: **type enumeration**|
| |
| (abort, *<project-specified>*); |
| |
| -- The following are other example protocols.   |
| |
| -- (suspend, complete\_one, complete\_all); |
| |
| The Supported\_Active\_Thread\_Handling\_Protocols property enumeration type specifies the set of possible actions that can be taken to handle threads that are in the state of performing computation at the time instant of actual mode switch.   |
+=====================================================================================================================================================================================================================================================+
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Connection\_Patterns: **type enumeration** ( One\_To\_One, |
|   |
| All\_To\_All, |
|   |
| One\_To\_All, |
|   |
| All\_To\_One, |
|   |
| Next, |
|   |
| Previous, |
|   |
| Cyclic\_Next, |
|   |
| Cyclic\_Previous );   |
|   |
| The Supported\_Connection\_Patterns property enumeration type specifies the set of patterns that are supported to connect together a source component array and a destination component array.|
|   |
| One\_To\_One represents the case when each item of the source component array is connected to the corresponding item of the destination component array. This property can only be used if the source and the destination component arrays of the connection have the same dimension and same dimension size. |
|   |
| All\_To\_All represents the case when each item of the source component array is connected to each item of the destination component array.   |
|   |
| A Next or Previous value indicates that elements of the ultimate source array dimension are connected to the next (previous) element in the ultimate destination array dimension without wrapping between the first and last. |
|   |
| A Cyclic\_Next or Cyclic\_Previous value indicates that elements of the ultimate source array dimension are connected to the next (previous) element in the ultimate destination array dimension. |
|   |
| A One\_to\_All value indicates that a single element of the ultimate source has a semantic connection to each element in the ultimate destination.|
|   |
| A All\_to\_One value indicates that each array element of the ultimate source has a semantic connection to a single element in the ultimate destination.  |
+===============================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================================+
| Supported\_Concurrency\_Control\_Protocols: **type** **enumeration** (None\_Specified, Interrupt\_Masking, Maximum\_Priority, Priority\_Inheritance,  |
|   |
| -- Priority\_Ceiling, Spin\_Lock, Semaphore, *<project-specified>*);  |
|   |
| -- The following are example concurrency control protocols:   |
|   |
| -- (Interrupt\_Masking, Maximum\_Priority, Priority\_Inheritance, |
|   |
| -- Priority\_Ceiling, Spin\_Lock, Semaphore)  |
|   |
| The Supported\_Concurrency\_Control\_Protocols property enumeration type specifies the set of concurrency control protocols that are supported.   |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Dispatch\_Protocols: **type enumeration** (Periodic, Sporadic, Aperiodic, Timed, Hybrid, Background, *<project-specified>*);   |
|   |
| -- The following are protocols for which the semantics are defined:   |
|   |
| -- (Periodic, Sporadic, Aperiodic, Timed, Hybrid, Background);|
|   |
| The Supported\_Dispatch\_Protocols property enumeration type specifies the set of thread dispatch protocols that are supported.   |
|   |
| Periodic represents periodic dispatch of threads with deadlines. Sporadic represents event-triggered dispatching of threads with soft deadlines. Aperiodic represents event-triggered dispatch of threads with hard deadlines. Timed represents threads that are dispatched after a given time unless they are dispatched by arrival of an event or event data. Hybrid represents threads that are dispatched by both an event or event data arrival and periodically. Background represents threads that are dispatched once and execute until completion. The Period is required for Periodic, Timed, and Hybrid threads.   |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Queue\_Processing\_Protocols: **type enumeration** (Fifo, |
|  |
| *<project-specified>*);  |
|  |
| The Supported\_Queue\_Processing\_Protocols property enumeration type specifies the set of queue processing protocols that are supported.|
|  |
| Fifo represents first-in first-out processing of queues. Other prototcols are project specific.  |
+==================================================================================================================================================================================================================================================================================================================================================================================+
| Supported\_Hardware\_Source\_Languages: **type enumeration** (VHDL, *<project-specified>*);  |
|  |
| -- The following is an example hardware description language:|
|  |
| -- (VHDL)|
|  |
| The Supported\_Hardware\_Source\_Languages property enumeration type specifies the set of hardware description languages that are supported. |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Connection\_QoS : **type** **enumeration** (GuaranteedDelivery, OrderedDelivery, SecureDelivery, <project specific>); |
|  |
| The Supported\_Connection\_QoS property specifies the quality of services.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Scheduling\_Protocols: **type** **enumeration** (FixedTimeline, Cooperative, RMS, EDF, SporadicServer, SlackServer, ARINC653, *<project-specified>*); |
|  |
| -- The following are examples of scheduling protocols:   |
|  |
| -- (FixedTimeline, Cooperative, RMS, EDF, SporadicServer, SlackServer, ARINC653) |
|  |
| The Supported\_Scheduling\_Protocols property enumeration type specifies the set of scheduling protocols that are supported. |
|  |
| Scheduling protocols that can be provided by implementers include:   |
|  |
| -  None (single thread). |
|  |
| -  Interrupt-driven (handling of interrupt service routines (ISR)).  |
|  |
| -  For periodic task sets: fixed timeline, cooperative (cyclic executive), deadline monotonic, least laxity. |
|  |
| -  For hybrid task set:  |
|  |
|-  Fixed priority server based on Rate Monotonic Scheduling (RMS) (polling server, deferrable server, sporadic server, slack stealer).|
|  |
|-  Dynamic priority server based on Earliest Deadline First (EDF) (dynamic polling server, dynamic deferrable server, dynamic sporadic server, total bandwidth server, constant bandwidth server).|
|  |
| Scheduling protocols have a policy for scheduling periodic threads, for aperiodic/sporadic threads, and for background threads. In the case of RMS, the periodic thread policy is priority assignment according to decreasing rate, aperiodic and sporadic threads according to their minimum inter-arrival time, and background as FIFO. Others have similar characteristics.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Source\_Languages: **type** **enumeration** (Ada95, Ada2005, C, Simulink\_6\_5, *<project-specified>*);   |
|  |
| -- The following are example software source languages:  |
|  |
| -- ( Ada95, Ada2005, C, Simulink\_6\_5 ) |
|  |
| The Supported\_Source\_Languages property enumeration type specifies the set of software source languages that are supported.|
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Supported\_Distributions: **type** **enumeration** (Fixed, Poisson, *<project-specified>*); |
| |
| -- The following are example distributions: |
| |
| -- ( Fixed, Poisson )   |
| |
| The Supported\_Distribution property enumeration type specifies the set of distribution functions that are supported.   |
+=========================================================================================================================================================================================================================================================================================================================================+
| Supported\_Classifier\_Substitutions: **type** **enumeration** (Classifier\_Match, Type\_Extension, Signature\_Match, *<project-specified>*);   |
| |
| The Supported\_Classifier\_Substitutions property enumeration type specifies the set of classifier substitutions that are supported for prototypes and refinement declarations. Three kinds of substitutions are defined as part of the standard. A candidate for additional substitution rules signature matching with name mapping.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Data\_Rate\_Units: **units** ( bitsps, Bytesps => bitsps \* 8,  |
| |
| KBytesps => Bytesps \* 1000,|
| |
| MBytesps => KBytesps \* 1000,   |
| |
| GBytesps => MBytesps \* 1000 ); |
| |
| The Data\_Rate\_Units property type units for the rate of data per time unit as a property type. The predeclared unit literals are expressed in terms of seconds as time unit.  |
| |
| Note: Conversion factor of 1000 consistent with ISO.|
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Time: **type** **aadlinteger units** Time\_Units;   |
| |
| The Time property type specifies a property type for time that is expressed as numbers with predefined time units.  |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Time\_Range: **type** **range** **of** Time;|
| |
| The Time\_Range property type specifies a property type for a closed range of time, i.e., a time span including the lower and upper bound.  |
| |
| The property type is Time. The standard units are ps (picoseconds), ns (nanoseconds), us (microseconds), ms (milliseconds), sec (seconds), min (minutes) and hr (hours).|
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Size: **type** **aadlinteger** **units** Size\_Units;   |
| |
| Memory size as integers with predefined size units. |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Size\_Range: **type** **range** **of** Size;|
| |
| The Size\_Range property specifies a closed range of memory size values, i.e., a memory size range including the lower and upper bound. |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Aadlinteger: **constant** **aadlinteger** => *<project-specified-integer-literal>*;|
| |
| The property constant Max\_Aadlinteger specifies the largest machine representable integer value that may be used as the maximum value in property associations. This property does not specify the maximum integer representation on a target processor.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Target\_Integer: **constant** **aadlinteger** => *<project-specified-integer-literal>*;|
| |
| The property constant Max\_Target\_Integer specifies the largest machine representable integer value on a target processor. |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Base\_Address: **constant** **aadlinteger** => *<project-specified-integer-literal>*;  |
| |
| The property constant Max\_Base\_Address specifies the maximum value that can be declared in for the Base\_Address property.|
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Memory\_Size: **constant** Size => *<project-specified-aadl-integer> <Size\_Units\_Literal>*;  |
| |
| The property constant Max\_Memory\_Size specifies the maximum memory size that can be declared in for the Size property, expressed in the specified unit of Size.   |
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Queue\_Size: **constant** **aadlinteger** => *<project-specified-integer-literal>*;|
| |
| The property constant Max\_Queue\_Size specifies the maximum value that can be declared in for the Queue\_Size property.|
+-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Thread\_Limit: **constant** **aadlinteger** => *<project-specified-integer-literal>*;   |
|  |
| The property constant Max\_Thread\_Limit specifies the maximum value that can be declared in for the Thread\_Limit property. |
+==================================================================================================================================================================================+
| Max\_Time: **constant** Time => *<project-specified-integer-literal>*;   |
|  |
| The property constant Max\_Time specifies the maximum value that can be declared in for the Time property, expressed in the specified unit of Time.  |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Urgency: **constant** **aadlinteger** => *<project-specified-integer-literal>*; |
|  |
| The property constant Max\_Urgency specifies the maximum value that can be declared in for the Urgency property. |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Byte\_Count: **constant** **aadlinteger** => *<project-specified-integer-literal>*; |
|  |
| The property constant Max\_Byte\_Count specifies the maximum value that can be declared in for the Byte\_Count property. |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Max\_Word\_Space: **constant** **aadlinteger** => *<project-specified-integer-literal>*; |
|  |
| The property constant Max\_Word\_Space specifies the maximum value that can be declared in for the Word\_Space property. |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Size\_Units: **type units** (bits, Bytes => bits \* 8, KByte => Bytes\* 1000,|
|  |
| MByte => KByte\* 1000, GByte => MByte \* 1000,   |
|  |
| TByte => GByte \* 1000 );|
|  |
| The type Size\_Units defines a measurement of size that is available for use in other property definitions. Users may append to this type.   |
|  |
| Note: conversion factor of 1000 consistent with ISO/SI.  |
|  |
| Note: B, KB, etc. in AADL 2004 have been replaced by Byte, Kbyte etc. in AADL V2. Since AADL is case insensitive, Kb could have been interpreted a K bits rather than K bytes.   |
|  |
| Note: There is a proposal to add the binary units as defined by IEC, i.e., KiByte => Bytes \* 1024, MiByte => KiByte \* 1024, etc.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Time\_Units: **type units** (ps, ns => ps \* 1000, us => ns \* 1000, ms => us \* 1000,   |
|  |
| sec => ms \* 1000, min => sec \* 60, hr => min \* 60);   |
|  |
| The type Time\_Units defines a measurement of time that is available for use in other property definitions. Users may append to this type.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Processor\_Speed\_Units: **type units** (KIPS, MIPS => KIPS \* 1000, GIPS => MIPS \* 1000 ); |
|  |
| The type Processor\_Speed\_Units defines a measurement of the speed at which a processor operates.   |
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

**end** AADL\_Project;

Predeclared Runtime Services 
-----------------------------

 Two sets of runtime services are predeclared. The first set declares
service subprograms that can be called by the application source
text directly. The second set declares service subprograms that are
intended to be called by an AADL runtime executive that can be
generated from an AADL model, thus is not expected to be used
directly by application component developers.

 **Application Runtime Services**

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

 **Runtime Executive Services**

The following are subprograms may be explicitly called by
application source code, or they may be called by an AADL runtime
system that is generated from an AADL model.

 The Get\_Resource and Release\_Resource  runtime services represent
an abstract interface for functions that perform locking and
unlocking of resources according to the specified concurrency
control protocol. The method may lock multiple resources
simultaneously.

**subprogram**  Get\_Resource

**features**

resource: **in parameter** <implementation-specific representation of
one or more resources>; 

**end** Get\_Resource;

**subprogram**  Release\_Resource 

**features**

resource: **in parameter** <implementation-specific representation of
one or more resources>; 

**end** Release\_Resource;

 The Await\_Dispatch runtime service is called to suspend the thread
execution at completion of its dispatch execution. It is the point
at which the next dispatch resumes. The service call takes several
parameters. It takes a DispatchPort list and an optional trigger
condition function to identify the ports and the condition under
which the dispatch is triggered. If the condition function is not
present any of the ports in the list can trigger the dispatch. It
takes a DispatchedPort as out parameter to return the port(s) that
triggered the dispatch. It takes OutputPorts and InputPorts as port
lists. OutputPorts, if present, identifies the set of ports whose
sending is initiated at completion of execution, equivalent to an
implicit Send\_Output service call. InputPorts, if present,
identifies the set of ports whose content is received at the next
dispatch, equivalent to an implicit Receive\_Input service call.

**subprogram ** Await\_Dispatch 

**features**

-- List of ports whose output is sent at completion/deadline

OutputPorts: **in parameter** <implementation-defined port list>;

-- List of ports that can trigger a dispatch

DispatchPorts: **in parameter** <implementation-defined port list>;

-- list of ports that did trigger a dispatch

DispatchedPort: **out parameter** < implementation-defined port list>;

-- optional function as dispatch guard, takes port list as parameter

DispatchConditionFunction: **requires** **subprogram access**;

-- List of ports whose input is received at dispatch

InputPorts: **in parameter** <implementation-defined port list>;

**end** Await\_Dispatch;

 A Raise\_Error runtime service shall be provided that allows a
thread to explicitly raise a thread recoverable or thread
unrecoverable error. Raise\_Error takes an error type identifier as
parameter.

**subprogram**  Raise\_Error 

**features**

errorID: **in parameter** <implementation-defined error type>;

**end** Raise\_Error;

 A Get\_Error\_Code runtime service shall be provided that allows a
recover entrypoint to determine the type of error that caused the
entrypoint to be invoked.

**subprogram** Get\_Error\_Code

**features**

errorID: **out parameter** <implementation-defined error type>;

**end** Get\_Error\_Code;

 Subprograms have event ports but do not have an error port. If a
Raise\_Error is called, it is passed to the error port of the
enclosing thread. If a Raise\_Error is called by a remotely called
subprogram, the error is passed to the error port of the thread
executing the remotely called subprogram. The Raise\_Error method is
permitted to have an error identification as parameter value. This
error identification can be passed through the error port as the
data value, since the error port is defined as event data port.

A Await\_Result runtime service shall be provided that allows an
application to wait for the result of a semi-synchronous subprogram
call.

**subprogram** Await\_Result

**features**

CallID: **in parameter** <implementation-defined call ID>;

**end** Await\_Result;

 Current\_System\_Mode returns a value corresponding to the active
system operation mode (SOM).

**subprogram** Current\_System\_Mode

**features**

ModeID: **out parameter** <implementor-specific>; -- ID of the mode

**end** Current\_System\_Mode;

 Set\_System\_Mode indicates the next SOM the runtime system must
switch to. System modes are assumed to have identifying numbers.

**subprogram** Set\_System\_Mode

**features**

ModeID: **in parameter** <implementor-specific>; -- ID of the SOM

**end** Set\_System\_Mode;

 Current\_System\_Mode returns a value corresponding to the active
system operation mode (SOM).

**subprogram** Current\_System\_Mode

**features**

ModeID: **out parameter** <implementor-specific>; -- ID of the mode

**end** Current\_System\_Mode;

 Set\_System\_Mode indicates the next SOM the runtime system must
switch to. System modes are assumed to have identifying numbers.

**subprogram** Set\_System\_Mode

**features**

ModeID: **in parameter** <implementor-specific>; -- ID of the SOM

**end** Set\_System\_Mode;

 The following subprograms take a process, virtual processor, or
processor ID as parameter. The ID representation is determined by
the runtime system.

Stop\_Process is called for a controlled shut-down of a process,
i.e., all of its contained threads, whether executing, awaiting a
dispatch, or not part of the current mode, are given a chance to
execute their finalize entrypoint before being halted.

 Abort\_Process is called for a shut-down of a process due to an
anomaly. All of its contained threads are terminated immediately.

 Stop\_Virtual\_Processor is called to initiate a transition to the
virtual processor stopping state at the next hyperperiod. This has
the effect of initiating Stop\_Virtual\_Processor for all virtual
processors bound to the virtual processor, and Stop\_Process for all
processes bound to the virtual processor.

(5) Abort\_Virtual\_Processor is called for a shut-down of a virtual
processor due to an anomaly. All virtual processors and processes
bound to the virtual processor are aborted.

(6) Stop\_Processor is called to initiate a transition to the processor
stopping state at the next hyperperiod. This has the effect of
initiating Stop\_Virtual\_Processor for all virtual processors bound
to the processor, and Stop\_Process for all processes bound to the
processor.

(7) Abort\_Processor is called for a shut-down of a processor due to an
anomaly. All virtual processors and processes bound to the processor
are aborted.

(8) Stop\_System is called to initiate a transition to the system
stopping state, which will initiate a Stop\_Processor for all
processors in the system.

(9) Abort\_System is called for a shut-down of the system due to an
anomaly. All processors in the system are aborted.

A. Glossary

Informative

 Definitions of terms used from other standards, such as the IEEE
*Standard Glossary of Software Engineering Terminology* [IEEE Std.
610.12-1990], ISO/IEC 9945-1:1996 [IEEE/ANSI Std 1003.1, 1996
Edition], *Information Technology – Portable Operating System
Interface (POSIX)*, Unified Modeling Language Specification [UML
2004, version 1.4.2], or the IFIP WG10.4 *Dependability: Basic
Concepts and Terminology* [IFIP WG10.4-1992] are so marked. Terms
not defined in this standard are to be interpreted according to the
Webster's Third New International Dictionary of the English
Language.

-  *Actual System*: [UML] 1) Subject of a model; 2) A collection of
   connected physical units, which can include software, hardware, and
   people, that are organized to accomplish a specific purpose. [AADL]
   The system being modeled by an AADL specification. This includes
   software components, execution platform components, including
   interfaces to the external environment, and system components.

-  *Aggregate Data Port:* [AADL] An aggregate data port makes a
   collection of data from multiple thread **out** data available in a
   time-consistent manner.

-  *Ancestor*: [AADL] An ancestor in the component extension hierarchy
   is a component type or component implementation that (directly or
   indirectly) has been extended to the given component type or
   implementation.

-  *Annex Document*: [AADL] An approved addition to the core AADL
   standard in the form of additional standardized properties and/or a
   sublanguage notation to be used in annex subclauses and libraries.

-  *Annex Subclause*: [AADL] An annex-specific subclause expressed in a
   sublanguage notation. Annex subclauses can be declared in component
   types and component implementations.

-  *Annex Library*: [AADL] A library of reusable declaration expressed
   in a sublanguage notation. Annex libraries can be declared in
   packages.

-  *Anomaly:* [IFIP, AADL] An *anomaly* occurs when a component is in an
   erroneous or failed state that does not result in a standard
   exception.

-  *Application*: [IEEE] Software designed to fulfill specific needs of
   a user.

-  *Architecture*: [IEEE] An *architecture* represents the
   organizational structure of a system or component.

-  *Binding-dependent Implementation*: [AADL] A *binding-dependent
   implementation* of a subcomponent is determined after the binding to
   an execution platform component is fixed.

-  *Behavior*: [AADL] A *behavior* is a description of sequencing of
   various kinds that may occur in a component. Examples are execution
   sequences of subprograms, end-to-end sequencing of data and event
   flow, the interaction between components, or a specification of mode
   transitions. Behaviors describe how various fault events cause a
   component to transition between various error states. Behaviors are
   nameable objects in specifications and have properties.

-  *Binding:* [AADL] A *binding* establishes a many-to-one
   hosted/allocated relationship between components using resources and
   those representing resources. In particular, it maps application
   components to execution platform components.

-  *Bus*: [AADL] A *bus* is a *category* of component used to model a
   communication channel, typically hardware together with communication
   protocols that can exchange control and data between memories,
   processors, and devices. Depending on the properties of the bus, a
   bus may be used to access a memory or device. A bus may be used to
   send data messages between processors. Connections may be bound to
   buses.

-  *Component*: [AADL] A *component* is a hardware, software, or system
   unit used as a part of some system. A component belongs to a
   *component category*. Each component has a name (also called its
   *defining* *identifier*) and has category–specific properties that
   can be assigned values. A fully defined component is an instance of a
   component type and has an implementation.

-  *Component Category*: [AADL] A component category is either the name
   of a kind of execution platform component (e.g., processor), a
   software component (e.g., process), or a composite component (e.g.,
   system). The AADL defines several categories for software components:
   *process, thread group,* *thread*, *subprogram,* *data.* Several
   categories are also defined for execution platform components:
   *processor*, *memory*, *bus*, and *device*. The composite category
   only defines the *system* component category.

-  *Component Classifier*: [AADL] A *component classifier* is a language
   element that defines one or more *components* regarded as forming a
   group by reason of the same structure and behavior. The *classifier*
   is a reusable description of the structure and behavior used to
   define one or more *components*. There are two kinds of classifiers:
   *component type* and *component implementation*. A component
   classifier belongs to a particular *component category*.

-  *Component Classifier Reference:* [AADL] A component classifier
   reference is a uniquely named reference to a component type or to a
   component type and implementation pair.

-  *Component Extension*: [AADL] A *component extension* allows a
   component classifier to be defined in terms of an existing component
   classifier through addition of features, subcomponents, property
   associations, connections, and behaviors, while inheriting the
   elements of the component being extended.

-  *Component Implementation*: [AADL] A *component implementation* is a
   *component classifier* that describes an internal structure
   (subcomponents, features, connections), properties, and behaviors of
   a component. A *component type* can have several implementations.

-  *Component Instance*: [AADL] Another name for *component*.

-  *Component Type*: [AADL] A *component type* is a kind of *component
   classifier* that describes a component’s visible functional and
   behavioral interface. The *component type* describes the provided
   features and accessible components of an instance of the type that
   other components may use. The *component type* also specifies the
   features and components required to create each instance of itself.

-  *Configuration*: [AADL] The set of components and connections that
   are active in the current mode.

-  *Connection*: [AADL] A *connection* is a link between the *features*
   of two components over which data, events, or subprogram calls can be
   exchanged. Connections are directional.

-  *Data*: [IEEE] A representation of facts, concepts, or instructions
   in a manner suitable for communication, interpretation, or processing
   by humans or automatic means.

-  *Data Component*: [AADL] A *data component* represents static data in
   the source text. This data may have shared access by multiple
   threads.

-  *Data Port*: [AADL] A *data port* is a port through which data is
   transferred in unqueued form. It represents a variable declared in
   source text.

-  *Data Subprogram:* [AADL] A subprogram that provides an interface to
   a data component.

-  *Data Type:* [IEEE] A class of data, characterized by the members of
   the class and the operations that can be applied to them.

-  *Declaration:* [AADL] A language statement that introduces a
   component classifier, component instance, feature, connection, or
   property.

-  *Delayed Connection*: [AADL] A connection between two data ports,
   where the data is transferred at the deadline of the originating
   thread. This results in deterministic communication of state data and
   receipt of data in a phase-delayed manner.

-  *Descendent*: [AADL] A *descendent* in the component extension
   hierarchy is a component type or component implementation that
   (directly or indirectly) extends the given component type or
   implementation.

-  *Deterministic*: [IEEE] Pertaining to a process, model, or variable
   whose outcome, result, or value does not depend on chance.

-  *Deterministic Communication*: [AADL] Communication of state data
   between periodic threads in predictable order.

-  *Device*: [AADL] A *device* models components that interface with an
   external environment, i.e., exhibit complex behaviors that require a
   nontrivial interface to application software systems. A device cannot
   directly store and execute binary images associated with software
   process specifications. Software components can communicate with
   devices via ports. Examples of devices are sensors and actuators that
   interface with the external physical world, or standalone systems
   such as a GPS. Devices interact with the embedded application system
   through port connections and with the computing hardware through bus
   access.

-  *Dispatch Time*: [AADL] For periodic threads: any non-negative
   integral multiple of the period following the transition out of the
   loaded process state. For sporadic and aperiodic threads: the arrival
   of an event or event data at an event or event data port of the
   thread, or the arrival of a remote subprogram call at a subprogram
   (access) feature of the thread following the transition out of the
   loaded process state.

-  *Error*: [AADL] An *error* in a component occurs when an existing
   fault causes the internal state of the component to deviate from its
   nominal or desired operation. For example, a component error may
   occur when an add instruction produces an incorrect result because a
   transistor in the adding circuitry is faulty.

-  *Event*: [Webster] An outcome or occurrence.

-  *Event Port*: [AADL] Port through which events are passed between
   components. Events may be queued at the receiving end.

-  *Event Data Port:* [AADL] Port through which data is passed between
   components, whose transfer implies the passing of an event. Event
   data may be queued at the receiving end.

-  *Exception:* [IFIP] An *exception* represents an error condition that
   changes the behavior of a component. An exception may occur for an
   erroneous or failed component when that error or failure is detected,
   either by the component itself or by another component with which it
   interfaces.

-  *Execution Platform*: [AADL] A combination of hardware and software
   that supports the execution of application software through its
   ability to schedule threads, store source text code and data, and
   perform communication modeled by connections. This combination would
   typically include runtime support (O/S or language runtime
   environment) and the specific hardware.

-  *Execution Platform Component*: [AADL] A component of the execution
   platform.

-  *Failure*: [IFIP] A *failure* in a physical component occurs when an
   error manifests itself at the component interface. A component fails
   when it does not perform its nominal function for the other parts of
   the system that depend on that component for their nominal operation.

-  *Fault*: [IFIP] A fault is defined to be an anomalous undesired
   change in thread execution behavior, possibly resulting from an
   anomalous undesired change in data being accessed by that thread or
   from violation of a compute time or deadline constraint. A *fault* in
   a physical component is a root cause that may eventually lead to a
   component error or failure. A fault is often a specific event such as
   a transistor burning out or a programmer making a coding mistake.

-  *Feature:* [AADL] A *feature* models a characteristic or a component
   that is externally visible as part of the component type. A feature
   is used to establish interaction with other components. Features
   include: ports and feature groups to support flow of data and
   control, access to data components to support coordination of access
   to shared data components, subprogram access to bind subprogram
   calls, and parameters to represent the data values that can be passed
   into and out of subprograms.

-  *Immediate Connection*: [AADL] A connection between two data ports,
   where the execution of the receiving thread is delayed until the
   sending thread completes execution and data is transferred at that
   time. This results in deterministic communication of state data and
   receipt of data in mid-frame, i.e., within the same dispatch of both
   the sending and receiving thread.

-  *Implementation*: [IEEE] The result of translating a design, i.e.,
   architecture, components, interfaces, and other characteristics of a
   system or component, into hardware components, software components,
   or both.

-  *Instance*: [AADL] An individual object of a class that is indicated
   by a component classifier.

-  *Instantiation*: [IEEE] The process of substituting specific data,
   instructions, or both into a generic program unit to make it usable
   in a computer program.

-  *Interface*: [IEEE] 1) A shared boundary across which information is
   passed; 2) A hardware or software component that connects two or more
   other components for the purpose of passing information from one to
   the other; 3) To connect two or more components for the purpose of
   passing information from one to the other; 4) To serve as a
   connecting or connected component as in (2). [UML] A named set of
   operations that characterize the behavior of an element.

-  *Memory*: [AADL] A *memory* is a category of component used to model
   randomly addressable physical storage or logical storage. A memory
   component may be contained in a platform, or may be accessible from a
   platform via a bus.

-  *Mode*: [AADL] A *mode* models an operational mode in the form of an
   alternative set of active threads and connections and alternative
   execution behavior within threads. A mode behavior models dynamic
   mode changes by declaring runtime transitions between operational
   modes as the result of events. Mode specifications can be
   hierarchical; sub-modes and concurrent reconfiguration of different
   subsystems can be specified.

-  *Model*: [UML] An abstraction of a actual system, with a certain
   purpose.

-  *Noncompliance*: [AADL] *Noncompliance* of a component with its
   specification is a kind of design fault. This may be handled by
   run-time fault-tolerance in an implemented actual system. A developer
   is permitted to classify such components as anomalous rather than
   noncompliant.

-  *Package:* [AADL] A *package* defines a namespace for component type
   and component implementation declarations and allows them to be
   logically organized into a hierarchy. Packages can control visibility
   of declarations by declaring them public or private.

-  *Port*: [AADL] *Ports* are connection points between components that
   are used for directional transfer of data, events, or both. Data
   transferred through ports is typed.

-  *Port Connection:* [AADL] *Port connections* represent directional
   transfer of data and control between data, event, and event data
   ports of components. The endpoints of such a connection are threads,
   devices, or processors.

-  *Feature Group:* [AADL] A grouping of features or feature groups.
   Outside a component a feature group is treated as a single unit.
   Inside a component the features of a feature group can be accessed
   individually. Feature groups allow collections of features to be
   connected with a single connection.

-  *Predictable*: [Webster] Determinable in advance.

-  *Private:* [AADL] Declarations contained in a package that can only
   be referenced from within the package.

-  *Process*: [IEEE] An executable unit managed by an operating system
   scheduler; [POSIX] a conceptual object with an address space, one or
   more threads of control executing within that address space, a
   collection of system resources required for execution, and possibly
   other attributes. [AADL]: A process represents a space partition in
   terms of virtual address spaces containing source text that forms
   complete programs. A process contains threads as concurrent units of
   execution scheduled to execute on processors.

-  *Processor*: [AADL] A *processor* is an abstraction of hardware and
   software that is responsible for scheduling and executing threads.
   Processors may contain memory and may access memory and devices via
   buses.

-  *Program:* [AADL] A *program* is a body of *source* text written in a
   standard programming language that can be compiled, linked and loaded
   as an executable image. A program has a self-contained namespace (no
   source names appear that are not declared within that program or are
   part of the language standard). The complete set of source files
   associated with a process must form a legal program as defined by the
   applicable programming language standard.

-  *Property*: [AADL] A *property* is a named value associated with an
   AADL component, connection, flow, or feature. Every property has a
   name and can be assigned a value of a specific type in a
   specification. There is a standard set of properties for each
   category of component, connection, flow or feature, e.g., thread
   components have a Period property that can be assigned a time value.
   Implementations, components, features, connections, and behaviors
   have properties. Non-standard property definitions may be defined and
   used in a specification, this is the primary means available to
   create non-standard extensions to the language.

-  *Property Set:* [AADL] A new set of properties and property types as
   extension to existing categories of components, features, behaviors,
   and connections.

-  *Property Type*: [AADL] The type of values that can be associated
   with a property.

-  *Property Value Association*: [AADL] A *property value association*
   is a statement in the language that associates a value with a named
   *property*. In some cases a property value may be overridden.

-  *Public:* [AADL] Declarations contained in a package that can be
   referenced from within another package or component.

-  *Qualified Name:* [AADL] A qualified name uniquely identifies the
   component classifier identifier with a package name separated by a
   double colon (::). Only the public package namespace is used to
   resolve these references.

-  *Reconfiguration:* [AADL] Transition from one mode to another
   resulting in a change from one component and connection configuration
   to another.

-  *Remote Subprogram:* [AADL] A subprogram that executes in its own
   thread and can be called remotely.

-  *Sampling Connection*: [AADL] A connection between two data ports,
   where the periodic recipient thread samples the output of another
   thread. This may result in non-deterministic communication of state
   data.

-  *Shared Component*: [AADL] A component that is directly accessible to
   two or more components.

-  *Software Component*: [AADL] A *software component* is a component
   that represents software, i.e., source text describing a program or
   procedure in an implementation language, and represents the units of
   concurrency of an application in terms of its threads and processes.

-  *Source Text*: [AADL] Refers to a body of text written in a
   conventional programming language such as Ada or C, written in a
   domain-specific modeling language such as
   MatLab\ :sup:`®`/Simulink:sup:`®`, or written in a hardware
   description language such as VHDL.

-  *Specification:* [AADL] An AADL source file that specifies, in a
   complete or legally partial manner, a set of requirements, design,
   behavior, or other characteristics of a system or component.

-  *Subcomponent:* [AADL] A component declared inside a component
   implementation that represents the application system or the
   execution platform. A subcomponent that belongs to the system
   category is also known as a *subsystem*.

-  *Subprogram:* [AADL] A *subprogram* is a category of component used
   to model source text and images that have a single entry point that
   can be called by a thread. Subprograms have no statically allocated
   variables. A data subprogram can be declared as a feature of a data
   component. A subprogram may be declared as a feature of a thread,
   thread group, process, processor or system.

-  *System*: [AADL] A *system* is an integrated composite that consists
   of one or more hardware, software, or system components. System is
   the name of a *component category*.

-  *Task*: [IEEE] 1) A sequence of instructions treated as a basic unit
   of work by the supervisory program of an operating system; 2) A
   software component that can operate in parallel with other software
   components.

-  *Thread:* [AADL] A *thread* is used to model a physical thread of
   execution. A thread communicates with other threads through ports.
   Threads can have different dispatch protocols such as periodic,
   aperiodic, sporadic, and background.

-  *Type:* [AADL] Qualities common to a number of individuals that
   distinguish them as an identifiable class. In this standard the term
   type is used to characterize three classes: component type, property
   type, data type.

A. Syntax Summary

Informative

 This appendix provides a summary of the containment restrictions
imposed by different component categories, a summary of the syntax
rules, and a summary of the property owners for the AADL core
language.

1. .. rubric:: Constraints on Component Containment
  :name: constraints-on-component-containment
  :class: appendix2

 The AADL has the following restrictions on the constructs that are
permitted in the component type and component implementation of each
component category.

+------------------+---------------------------------------+------------------------+
| **Category** |   ||
+------------------+---------------------------------------+------------------------+
| **abstract** | Features: | Subcomponents: |
|  |   ||
|  | -  port   | -  data|
|  |   ||
|  | -  feature group  | -  subprogram  |
|  |   ||
|  | -  provides data access   | -  subprogram group|
|  |   ||
|  | -  provides subprogram access | -  thread  |
|  |   ||
|  | -  provides subprogram group access   | -  thread group|
|  |   ||
|  | -  provides bus access| -  process |
|  |   ||
|  | -  provides virtual bus access| -  processor   |
|  |   ||
|  | -  requires data access   | -  virtual processor   |
|  |   ||
|  | -  requires subprogram access | -  memory  |
|  |   ||
|  | -  requires subprogram group access   | -  bus |
|  |   ||
|  | -  requires bus access| -  virtual bus |
|  |   ||
|  | -  requires virtual bus access| -  device  |
|  |   ||
|  | -  feature| -  system  |
|  |   ||
|  |   | -  abstract|
+------------------+---------------------------------------+------------------------+
| **data** | Features: | Subcomponents: |
|  |   ||
|  | -  provides subprogram access | -  data|
|  |   ||
|  | -  requires subprogram access | -  subprogram  |
|  |   ||
|  | -  requires subprogram group access   | -  abstract|
|  |   ||
|  | -  provides data access   ||
|  |   ||
|  | -  feature group  ||
|  |   ||
|  | -  feature||
+------------------+---------------------------------------+------------------------+
| **subprogram**   | Features: | Subcomponents: |
|  |   ||
|  | -  out event port | -  data|
|  |   ||
|  | -  out event data port| -  abstract|
|  |   ||
|  | -  feature group  | -  subprogram  |
|  |   ||
|  | -  requires data access   ||
|  |   ||
|  | -  requires subprogram access ||
|  |   ||
|  | -  requires subprogram group access   ||
|  |   ||
|  | -  parameter  ||
|  |   ||
|  | -  feature||
+------------------+---------------------------------------+------------------------+

+------------------------+---------------------------------------+-----------------------+
| **subprogram group**   | Features: | Subcomponents:|
||   |   |
|| -  provides subprogram access | -  subprogram |
||   |   |
|| -  requires subprogram access | -  subprogram group   |
||   |   |
|| -  requires subprogram group access   | -  data   |
||   |   |
|| -  provides subprogram group access   | -  abstract   |
||   |   |
|| -  feature group  |   |
||   |   |
|| -  feature|   |
+------------------------+---------------------------------------+-----------------------+
| **thread** | Features: | Subcomponents:|
||   |   |
|| -  port   | -  data   |
||   |   |
|| -  feature group  | -  subprogram |
||   |   |
|| -  provides data access   | -  subprogram group   |
||   |   |
|| -  requires data access   | -  abstract   |
||   |   |
|| -  provides subprogram access |   |
||   |   |
|| -  requires subprogram access |   |
||   |   |
|| -  provides subprogram group access   |   |
||   |   |
|| -  requires subprogram group access   |   |
||   |   |
|| -  feature|   |
+------------------------+---------------------------------------+-----------------------+
| **thread group**   | Features: | Subcomponents:|
||   |   |
|| -  port   | -  data   |
||   |   |
|| -  feature group  | -  subprogram |
||   |   |
|| -  provides data access   | -  subprogram group   |
||   |   |
|| -  requires data access   | -  thread |
||   |   |
|| -  provides subprogram access | -  thread group   |
||   |   |
|| -  requires subprogram access | -  abstract   |
||   |   |
|| -  provides subprogram group access   |   |
||   |   |
|| -  requires subprogram group access   |   |
||   |   |
|| -  feature|   |
+------------------------+---------------------------------------+-----------------------+
| **process**| Features: | Subcomponents:|
||   |   |
|| -  port   | -  data   |
||   |   |
|| -  feature group  | -  subprogram |
||   |   |
|| -  provides data access   | -  subprogram group   |
||   |   |
|| -  requires data access   | -  thread |
||   |   |
|| -  provides subprogram access | -  thread group   |
||   |   |
|| -  requires subprogram access | -  abstract   |
||   |   |
|| -  provides subprogram group access   |   |
||   |   |
|| -  requires subprogram group access   |   |
||   |   |
|| -  feature|   |
+------------------------+---------------------------------------+-----------------------+

+-------------------------+---------------------------------------+------------------------+
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
| **memory**  | Features  | Subcomponents: |
| |   ||
| | -  requires bus access| -  memory  |
| |   ||
| | -  provides bus access| -  bus |
| |   ||
| | -  requires virtual bus access| -  abstract|
| |   ||
| | -  provides virtual bus access||
| |   ||
| | -  feature group  ||
| |   ||
| | -  feature||
| |   ||
| | -  port   ||
+-------------------------+---------------------------------------+------------------------+
| **bus** | Features  | Subcomponents: |
| |   ||
| | -  requires bus access| -  virtual bus |
| |   ||
| | -  provides bus access| -  abstract|
| |   ||
| | -  requires virtual bus access||
| |   ||
| | -  provides virtual bus access||
| |   ||
| | -  feature group  ||
| |   ||
| | -  feature||
| |   ||
| | -  port   ||
+-------------------------+---------------------------------------+------------------------+
| **virtual bus** | Features  | Subcomponents: |
| |   ||
| | -  port   | -  virtual bus |
| |   ||
| | -  provides virtual bus access| -  abstract|
| |   ||
| | -  requires virtual bus access||
| |   ||
| | -  feature||
| |   ||
| | -  feature group  ||
+-------------------------+---------------------------------------+------------------------+
| **device**  | Features  | Subcomponents: |
| |   ||
| | -  port   | -  bus |
| |   ||
| | -  feature group  | -  virtual bus |
| |   ||
| | -  provides subprogram access | -  data|
| |   ||
| | -  provides subprogram group access   | -  abstract|
| |   ||
| | -  requires bus access||
| |   ||
| | -  provides bus access||
| |   ||
| | -  provides virtual bus access||
| |   ||
| | -  requires virtual bus access||
| |   ||
| | -  feature||
+-------------------------+---------------------------------------+------------------------+
| **system**  | Features: | Subcomponents: |
| |   ||
| | -  port   | -  data|
| |   ||
| | -  feature group  | -  subprogram  |
| |   ||
| | -  provides subprogram access | -  subprogram group|
| |   ||
| | -  requires subprogram access | -  process |
| |   ||
| | -  provides subprogram group access   | -  processor   |
| |   ||
| | -  requires subprogram group access   | -  virtual processor   |
| |   ||
| | -  provides bus access| -  memory  |
| |   ||
| | -  requires bus access| -  bus |
| |   ||
| | -  provides virtual bus access| -  virtual bus |
| |   ||
| | -  requires virtual bus access| -  device  |
| |   ||
| | -  provides data access   | -  system  |
| |   ||
| | -  requires data access   | -  abstract|
| |   ||
| | -  feature||
+-------------------------+---------------------------------------+------------------------+

AADL Core Language Syntax Rules
-------------------------------

 The following is a summary of the AADL syntax as specified in the
different sections of the document.

AADL\_specification ::=

( package\_spec \| property\_set )\ :sup:`+`

package\_spec ::=

**package** *defining*\ \_package\_name

( **public** package\_declarations [ **private** package\_declarations ]

\| **private** package\_declarations )

[ **properties** ( { basic\_property\_association }\ :sup:`+` \|

none\_statement ) ]

**end** *defining*\ \_package\_name ;

package\_declarations ::=

{ name\_visibility\_declaration }\* { AADL\_declaration }\*

package\_name ::=

{ *package*\ \_identifier **::** }\ :sup:`\*` *package*\ \_identifier

none\_statement ::= **none ;**

AADL\_declaration ::=

classifier\_declaration

\| annex\_library

classifier\_declaration ::=

component\_classifier\_declaration \|
feature\_group\_classifier\_declaration

component\_classifier\_declaration ::=

component\_type \| component\_type\_extension \|

component\_implementation \| component\_implementation\_extension

feature\_group\_classifier\_declaration ::=

feature\_group\_type \| feature\_group\_type\_extension

name\_visibility\_declaration ::=

import\_declaration \|

alias\_declaration

import\_declaration ::=

**with** ( package\_name \| *property\_set*\ \_identifier )

{ **,** ( package\_name \| *property\_set*\ \_identifier ) }\ :sup:`\*`
**; **

alias\_declaration ::=

( *defining\_*\ identifier **renames package** package\ *\_*\ name **;**
) \|

( [ *defining*\ \_identifier ] **renames **

( component\_category unique\_component\_type\_reference \|

**feature group** unique\_feature\_group\_type\_reference ) **;** ) \|

( **renames** package\_name::\ **all ;** )

NOTE: The **properties** subclause of the package is optional, or
requires an explicit empty subclause declaration. The latter is provided
to accommodate AADL modeling guidelines that require explicit
documentation of empty subclauses. An empty subclause declaration
consists of the reserved word of the subclause and a none statement (
**none ;** ).

component\_type ::=

component\_category *defining\_component\_type\_*\ identifier

[ **prototypes** ( { prototype }\ :sup:`+` \| none\_statement ) ]

[ **features** ( { feature }\ :sup:`+` \| none\_statement ) ]

[ **flows** ( { flow\_spec }\ :sup:`+` \| none\_statement ) ]

[ modes\_subclause \| requires\_modes\_subclause ]

[ **properties** (

{ *component\_type*\ \_property\_association \|
contained\_property\_association }\ :sup:`+`

\| none\_statement ) ]

{ annex\_subclause }\ :sup:`\*`

**end** *defining\_component\_type*\ \_identifier \ **;**

component\_type\_extension ::=

component\_category *defining\_component\_type\_*\ identifier

**extends** unique\_component\_type\_reference [ prototype\_bindings ]

[ **prototypes** ( { prototype \| prototype\_refinement }\ :sup:`+` \|
none\_statement ) ]

[ **features** ( { feature \| feature\_refinement }\ :sup:`+` \|
none\_statement ) ]

[ **flows** ( { flow\_spec \| flow\_spec\_refinement }\ :sup:`+` \|
none\_statement ) ]

[ modes\_subclause \| requires\_modes\_subclause ]

[ **properties** (

{ *component\_type*\ \_property\_association \|
contained\_property\_association }\ :sup:`+`

\| none\_statement ) ]

{ annex\_subclause }\ :sup:`\*`

**end** *defining\_component\_type*\ \_identifier \ **;**

component\_category ::=

abstract\_component\_category

\| software\_category

\| execution\_platform\_category

\| composite\_category

abstract\_component\_category ::= **abstract**

software\_category ::= **data** \| **subprogram** \| **subprogram
group** \|

**thread** \| **thread group** \| **process **

execution\_platform\_category ::=

**memory** \| **processor** \| **bus** \| **device** \| **virtual
processor** \| **virtual bus**

composite\_category ::= **system**

unique\_component\_type\_reference ::=

[ package\_name **::** ] *component\_type*\ \_identifier

NOTE: The above grammar rules characterize the common syntax for all
component categories. The sections defining each of the component
categories will specify further restrictions on the syntax.

The **prototypes**, **features**, **flows**, **modes**, **annex**, and
**properties** subclauses of the component type are optional, or require
an explicit empty subclause declaration. The latter is provided to
accommodate AADL modeling guidelines that require explicit documentation
of empty subclauses. An empty subclause declaration consists of the
reserved word of the subclause and a none statement ( **none ;** ).

component\_implementation ::=

component\_category **implementation**

*defining\_*\ component\_implementation\_name [ prototype\_bindings ]

[ **prototypes** ( { prototype }\ :sup:`+` \| none\_statement ) ]

[ **subcomponents** ( { subcomponent }\ :sup:`+` \| none\_statement ) ]

[ **internal features** { internal\_feature }\ :sup:`+` ]

[ **processor features** { processor\_feature }\ :sup:`+` ]

[ **calls** ( { subprogram\_call\_sequence }\ :sup:`+` \|
none\_statement ) ]

[ **connections** ( { connection }\ :sup:`+` \| none\_statement ) ]

[ **flows** ( { flow\_implementation \|

end\_to\_end\_flow }\ :sup:`+` \| none\_statement ) ]

[modes\_subclause ]

[ **properties** ( { property\_association \|
contained\_property\_association }\ :sup:`+`

\| none\_statement ) ]

{ annex\_subclause }\ :sup:`\*`

**end** *defining\_*\ component\_implementation\_name **;**

component\_implementation\_name ::=

*component\_type*\ \_identifier .
*component\_implementation*\ \_identifier

component\_implementation\_extension ::=

component\_category **implementation**

*defining\_*\ component\_implementation\_name

**extends** unique\_component\_implementation\_reference [
prototype\_bindings ]

[ **prototypes** ( { prototype \| prototype\_refinement }\ :sup:`+` \|
none\_statement ) ]

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

end\_to\_end\_flow \| end\_to\_end\_flow\_refinement }\ :sup:`+`

\| none\_statement ) ]

[modes\_subclause ]

[ **properties** ( { property\_association \|
contained\_property\_association }\ :sup:`+`

\| none\_statement ) ]

{ annex\_subclause }\ :sup:`\*`

**end** *defining\_*\ component\_implementation\_name ;

unique\_component\_implementation\_reference ::=

[ package\_name **::** ] component\_implementation\_name

NOTE: The above grammar rules characterize the common syntax for all
component categories. The sections defining each of the component
categories will specify further restrictions on the syntax.

The **prototypes**, **subcomponents**, **connections**, **calls**,
**flows**, **modes**, and **properties** subclauses of the component
implementation are optional or if used and empty, require an explicit
empty declaration. The latter is provided to accommodate AADL modeling
guidelines that require explicit documentation of empty subclauses. An
empty subclause declaration consists of the reserved word of the
subclause and a none statement ( **none ;** ).

The annex\_subclause of the component implementation is optional.

The **refines type** subclause of AADL V1 has been removed. Its role was
to allow association of implementation-specific property values to
features and flow specifications declared in the component type. The
same can be achieved by contained property associations whose **applies
to** subclause names the feature or flow specification.

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

numeral \| unique\_property\_constant\_identifier \|
unique\_property\_identifier

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

annex\_subclause ::=

**annex** *annex*\ \_identifier (

( **{\*\*** annex\_specific\_language\_constructs **\*\*}** ) \|
**none** )

[ in\_modes ] ;

annex\_library ::=

**annex** *annex*\ \_identifier

(( **{\*\*** annex\_specific\_reusable\_constructs **\*\*}** ) \|
**none** ) ;

subprogram\_call\_sequence ::=

*defining\_call\_sequence*\ \_identifier **:**

**{** { subprogram\_call }\ :sup:`+` **}**

[ **{** { *call\_sequence*\ \_property\_association }\ :sup:`+` **}** ]
[ in\_modes ] **;**

subprogram\_call ::=

*defining\_call*\ \_identifier **:** **subprogram** called\_subprogram

[ **{** { *subcomponent\_call*\ \_property\_association }\ :sup:`+`
**}** ] **;**

called\_subprogram ::=

-- identification by classifier

*subprogram*\ \_unique\_component\_classifier\_reference

\| ( *data*\ \_unique\_component\_type\_reference

**.** *data\_provides\_subprogram\_access*\ \_identifier )

\| ( *subprogram\_group*\ \_unique\_component\_type\_reference

**.** *provides\_subprogram\_access*\ \_identifier )

\| ( *abstract*\ \_unique\_component\_type\_reference

**.** *provides\_subprogram\_access*\ \_identifier )

\| ( *feature\_group\_*\ identifier

**.** *requires\_subprogram\_access*\ \_identifier )

-- identification by prototype

\| *component\_prototype*\ \_identifier

-- identification by processor subprogram access feature

\| ( **processor** . *provides\_subprogram\_access\_*\ identifier )

-- identification by subprogram instance

\| *subprogram\_subcomponent*\ \_identifier

\| ( *subprogram\_group\_subcomponent*\ \_identifier **. **

*provides\_subprogram\_access*\ \_identifier )

\| *requires\_subprogram\_access\_*\ identifier

\| ( *requires*\ \_\ *subprogram\_group\_access*\ \_identifier **. **

*provides\_subprogram\_access*\ \_identifier )

NOTE: Subprogram type and implementation declarations follow the syntax
rules for component types and implementations. Subprogram instances may
be implied by subprogram calls referring to subprogram classifiers, or
subprogram instances may be declared explicitly as subprogram
subcomponents and made accessible to calls through provides and requires
subprogram access declarations.

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

\| data\_access\_spec \| bus\_access\_spec \| virtual\_bus\_access\_spec

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

abstract\_feature\_spec ::=

*defining\_abstract\_feature\_*\ identifier :

( [ **in** \| **out** ] **feature** [
*component\_prototype*\ \_identifier

\| unique\_component\_classifier\_reference ] )

\| ( [ **in** \| **out** ] **prototype**
*feature\_prototype*\ \_identifier )

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

parameter\_spec ::=

*defining\_parameter*\ \_identifier :

( **in** \| **out** \| **in out** ) **parameter**

[ *data*\ \_unique\_component\_classifier\_reference \|

*data\_component\_prototype\_*\ identifier ]

parameter\_refinement ::=

*defining\_parameter*\ \_identifier : **refined to**

( **in** \| **out** \| **in out** ) **parameter**

[ *data*\ \_unique\_component\_classifier\_reference \|

*data\_component\_prototype\_*\ identifier ]

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

*data\_subcomponent*\ \_identifier
**.**\ *data\_subcomponent*\ \_identifier

\|

-- processor port

[ **processor .** ] *processor\_port*\ \_identifier

\|

-- component itself as event or event data source

[ **self** . ] *internal\_event\_or\_event\_data\_*\ \_identifier

-- Note: the keywords **processor** and **self**

-- are nor required but optional for backward compatibility

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

\| *data\_subcomponent*\ \_identifier **.**
*data\_subcomponent*\ \_identifier

\|

*subprogram\_call*\ \_identifier **.**
*requires\_or\_provides\_access*\ \_identifier \|

*subprogram\_call*\ \_identifier **.**
*requires\_or\_provides\_access*\ \_identifier

-- data, subprogram, subprogram group or bus being accessed

*data\_subprogram\_subprogram\_group\_or\_bus\_subcomponent*\ \_identifier

\|

-- subprogram a processor being accessed

[ **processor .** ] *provides\_subprogram\_access*\ \_identifier

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

*subcomponent*\ \_identifier **.** *feature\_group*\ \_identifier

\|

-- feature group element in a feature group of the component type

*component\_type\_feature\_group*\ \_identifier .

*element\_feature\_group\_*\ identifier

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

flow\_implementation ::=

( flow\_source\_implementation

\| flow\_sink\_implementation

\| flow\_path\_implementation )

[ **{** { property\_association }\ :sup:`+` **}** ]

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

end\_to\_end\_flow ::= end\_to\_end\_flow\_spec \|
end\_to\_end\_flow\_implementation

end\_to\_end\_flow\_spec ::=

*defining\_end\_to\_end\_flow*\ \_identifier : **end to end** **flow**

*start*\ \_subcomponent\_flow\_identifier **->**
*end\_*\ subcomponent\_flow\_identifier

[ **{** ( property\_association }\ :sup:`+` **}** ]

[ in\_modes\_and\_transitions ] ;

end\_to\_end\_flow\_ refinement ::=

*defining\_end\_to\_end\_*\ identifier **:**

**refined to end to end flow**

[*start*\ \_subcomponent\_flow\_identifier

{ **->** *connection*\ \_identifier

**->** *flow\_path*\ \_subcomponent\_flow\_or\_etef\_identifier
}\ :sup:`+` ]

( **{** { property\_association }\ :sup:`+` **}** [
in\_modes\_and\_transitions ]

\| in\_modes\_and\_transitions

) **;**

subcomponent\_flow\_or\_etef\_identifier ::=

subcomponent\_flow\_identifier \| *end\_to\_end\_flow\_*\ identifier

property\_set ::=

**property set** *defining\_­property\_set*\ \_identifier **is**

{ import\_declaration }\ :sup:`\*`

{ property\_type\_declaration

\| property\_definition\_declaration

{ annex\_subclause }\ :sup:`\*`

**end** *defining\_property\_set*\ \_identifier ;

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

**record (** record\_field

{ *record\_field* }\ :sup:`\*` **)**

record\_field ::=

*defining\_field*\ \_identifier **:** [ **list of** ]
property\_type\_designator **;**

property\_type\_designator ::=

unique\_property\_type\_identifier \|

property\_type

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

Note: Different from AADL V1 the **access** keyword is no longer used in
property definitions. The fact that a property applies to an access
feature is already specified in the applies to clause.

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

modes\_subclause ::=

**modes** ( { mode \| mode\_transition }\ :sup:`+` \| none\_statement )

requires\_modes\_subclause ::=

**requires modes** ( { mode }\ :sup:`+` \| none\_statement )

mode ::=

*defining\_mode*\ \_identifier : [ **initial** ] **mode**

[ **{** { *mode*\ \_property\_association }\ :sup:`+` **}** ]\ **;**

mode\_transition ::=

[ *defining\_\_mode\_transition*\ \_identifier \ **:** ]

*source\_mode*\ \_identifier

**-[** mode\_transition\_trigger { **,** mode\_transition\_trigger
}\ :sup:`\*` **]-> **

*destination\_mode*\ \_identifier

[ **{** { *mode\_transition*\ \_property\_association }\ :sup:`+` **}**
] **;**

mode\_transition\_trigger ::=

unique\_feature\_identifier \| [ **self** . ]
*internal\_feature\_*\ identifier

\| [ **processor .** ] *processor\_feature\_*\ identifier

unique\_feature\_identifier ::=

[ *subcomponent\_or\_feature\_group\_or\_subprogram\_call*\ \_identifier
**.** ] *feature*\ \_identifier

in\_modes ::=

**in modes (** *mode*\ \_identifier { **,** *mode*\ \_identifier
}\ :sup:`\*` **)**

component\_in\_modes ::=

**in modes (** ( mode\_name { **,** mode\_name }\ :sup:`\*` ) **)**

mode\_name ::= *local\_mode*\ \_identifier [ **=>**
*subcomponent\_mode\_*\ identifier ]

in\_modes\_and\_transitions ::=

**in modes (** ( mode\_or\_transition { **,** mode\_or\_transition
}\ :sup:`\*` **)**

mode\_or\_transition ::=

*mode*\ \_identifier \| *mode\_transition*\ \_identifier

AADL Core Language Meta Model Element Identifiers
-------------------------------------------------

 The following are AADL core language model elements that can be
identified in **applies to** subclauses of property definitions as
property owners, as acceptable classifiers in classifier property
values, and as referable elements in reference property values. They
correspond to classes in the AADL meta model. The indentation
partially indicates the subclass hierarchy. For example,
*classifier* and the list *subcomponent*, *feature*, etc. are
subclasses of *named element*, and *component classifier* and
*feature group type* are subclasses of *classifier*. Subclasses may
inherit from more than one class. For example, the class *abstract
classifier* is a subclass of both *abstract* and of *classifier*.
The second inheritance is not explicitly shown, but can be inferred
from the name. The AADL Meta Model Annex document defines the full
class hierarchy.

The meta model class name is determined by concatenating the
identifier names, each converted to starting in upper case and
followed by lower case letters.

+----------------------------------------------------------------------------------------------------+
| named element  |
||
| classifier |
||
| component classifier   |
||
| component type, component implementation   |
||
| feature group type |
||
| classifier feature |
||
| subcomponent, feature, connection, mode, mode transition, flow, instance   |
+====================================================================================================+
| abstract   |
||
| abstract classifier|
||
| abstract type, abstract implementation |
||
| abstract subcomponent, abstract instance   |
||
| prototype  |
+----------------------------------------------------------------------------------------------------+
| thread |
||
| thread classifier  |
||
| thread type, thread implementation |
||
| thread subcomponent, thread instance   |
+----------------------------------------------------------------------------------------------------+
| thread group   |
||
| thread group classifier|
||
| thread group type, thread group implementation, thread group subcomponent, thread group instance   |
+----------------------------------------------------------------------------------------------------+
| process|
||
| process classifier |
||
| process type, process implementation   |
||
| process subcomponent, process instance |
+----------------------------------------------------------------------------------------------------+
| system |
||
| system classifier  |
||
| system type, system implementation |
||
| system subcomponent, system instance   |
+----------------------------------------------------------------------------------------------------+
| data   |
||
| data classifier|
||
| data type, data implementation |
||
| data subcomponent, data instance,  |
||
| data port, event data port |
||
| data access|
+----------------------------------------------------------------------------------------------------+
| subprogram |
||
| subprogram classifier  |
||
| subprogram type, subprogram implementation,|
||
| subprogram subcomponent, subprogram instance   |
||
| subprogram call sequence, subprogram call  |
||
| subprogram access  |
+----------------------------------------------------------------------------------------------------+
| subprogram group   |
||
| subprogram group classifier|
||
| subprogram group type, subprogram group implementation |
||
| subprogram group subcomponent, subprogram group instance   |
||
| subprogram group access|
+----------------------------------------------------------------------------------------------------+
| processor  |
||
| processor classifier   |
||
| processor type, processor implementation   |
||
| processor subcomponent, processor instance |
+----------------------------------------------------------------------------------------------------+
| virtual processor  |
||
| virtual processor classifier   |
||
| virtual processor type, virtual processor implementation   |
||
| virtual processor subcomponent, virtual processor instance |
+----------------------------------------------------------------------------------------------------+
| memory |
||
| memory classifier  |
||
| memory type, memory implementation |
||
| memory subcomponent, memory instance   |
+----------------------------------------------------------------------------------------------------+

+-------------------------------------------------------------------------------------------------------------------+
| bus   |
|   |
| bus classifier|
|   |
| bus type, bus implementation  |
|   |
| bus subcomponent, bus instance|
|   |
| bus access|
+===================================================================================================================+
| virtual bus   |
|   |
| virtual bus classifier|
|   |
| virtual bus type, virtual bus implementation  |
|   |
| virtual bus subcomponent, virtual bus instance|
|   |
| virtual bus access|
+-------------------------------------------------------------------------------------------------------------------+
| device|
|   |
| device classifier |
|   |
| device type, device implementation|
|   |
| device subcomponent, device instance  |
+-------------------------------------------------------------------------------------------------------------------+
| feature   |
|   |
| port  |
|   |
| event port, data port, event data port|
|   |
| access|
|   |
| subprogram access, subprogram group access, data access, bus access, virtual bus access   |
+-------------------------------------------------------------------------------------------------------------------+
| parameter |
|   |
| feature instance, port instance, access instance  |
|   |
| internal feature, processor feature   |
|   |
| feature group |
|   |
| feature group instance|
+-------------------------------------------------------------------------------------------------------------------+
| connection|
|   |
| port connection, feature group connection |
|   |
| access connection |
|   |
| bus access connection, data access connection, subprogram access connection, subprogram group access connection   |
|   |
| connection instance   |
|   |
| port connection instance, access connection instance, mode transition connection instance |
+-------------------------------------------------------------------------------------------------------------------+
| mode, mode transition, mode instance, mode transition instance, mode transition connection instance   |
+-------------------------------------------------------------------------------------------------------------------+
| flow  |
|   |
| flow specification|
|   |
| flow source specification, flow sink specification, flow path specification   |
|   |
| end to end flow, flow specification instance, end to end flow instance|
+-------------------------------------------------------------------------------------------------------------------+
| package   |
+-------------------------------------------------------------------------------------------------------------------+

A. Graphical AADL Notation

Normative

Scope
-----

 This appendix defines a set of graphical symbols for the graphical
AADL notation. To be compliant with the graphical AADL notation you
these iconic symbols must be used. These graphical symbols represent
components and relationships between components in an AADL model.
Graphical AADL diagrams are legal in accordance with the AADL core
standard if the AADL model being presented graphically is legal and
if the correct graphical symbols are used. For example, a graphical
editor is not permitted to create a connection whose source and
destination are not connected. Graphical presentations of AADL
models are permitted to show subsets of legal AADL models. For
example, property values may be entered through a property sheet or
dialog box. The figures in this appendix present different views of
an AADL model. These views are not prescriptive, but intended to
illustrate possible views and layouts.

1. .. rubric:: AADL Graphical Symbols
  :name: aadl-graphical-symbols
  :class: appendix2

 AADL graphical symbols are shown with the AADL logo placed as
decorator in the upper left corner of the symbol. This logo may be
omitted if its absence does not cause confusion with other graphical
notations.

 In this section we define the graphical symbols for component
categories, ports and port connections, shared access to data and
buses, subprograms and subprogram calls, execution platforms and
binding of application components to execution platforms. We also
define the representation for component types, component
implementations, and component instances. These symbols are provided
as a standard set to be used in toolsets implementing the graphical
AADL. If the standardized set is not fully supported a graphical
toolset then, the toolset must define its mapping with respect to
the standardized set.

**Component Categories**

 Figure 24 shows the graphical symbols of the component categories in
AADL.

|image19|

Figure − AADL Components Graphical Symbols
  

 These graphical symbols are used to describe a system instance and
its components. The same basic symbol is used to represent component
types, component implementations, subcomponents, and component
instances of a particular category.

|image20|

Figure − Decorators on Threads
  

 Decorators may be attached to graphical symbols. Figure 25
illustrates the decoration of threads with threads properties that
are relevant in a timing-related view, i.e., it shows whether
threads are periodic, aperiodic, sporadic, timed, hybrid, or
background. In the case of periodic, sporadic, timed, and hybrid
threads the decorator indicates the period or timeout value.

Introduction of such decorators is permitted for any AADL property
or other characteristic of model objects represented by the
graphical symbol. For example, a decorator may be used to indicate
completeness of timing specification in a component, or to mark a
Wi-Fi device. The use of decorators is optional.

**Component Classifiers**

 Figure 26 illustrates the use of the graphical component category
symbols to describe the components in terms of their type,
implementation, and the content of the implementation. Component
types are shown using the basic graphical symbol for the categories.
The extension relation of a component type in terms of another
component type is shown with a solid line and an hollow triangle
arrow head. These symbols are typically used in library views that
show the content of AADL specifications or AADL packages, i.e.,
component classifiers and their extends relationships (see Annex
A.2).

In the example of Figure 26, the system type *SecureGPSSender*
extends the system type *GPSSender*. If a component type that is an
extension of another component type is shown graphically, then both
the features that are locally declared and the features inherited
from the component type being extended may be shown graphically.

 The example shows labels for component classifiers and ports. The
display of those labels is optional and may be achieved through
hovering popup mechanisms. Labels may be placed outside a symbol,
inside a symbol, or as labels with a line attachment appropriate
with the available display real estate.

|image21|

Figure − Component Types and Implementations


 Figure 26 also shows the representation of component
implementations; its graphical symbol is shown using the bold-lined
graphical symbol of the category. This bold-lining technique is
optional, but recommended if component implementations must be
visually distinguished from component types or subcomponents. The
relation to the component type is expressed by a dashed line with an
hollow triangle arrow head. Component implementation names are shown
with a dot (.) separated the component type identifier and
component implementation identifier.

|image22|

Figure − Subcomponents
  

 Subcomponents are shown using the graphical symbol of the component
category as illustrated in Figure 27. The subcomponent label shows
the subcomponent name, optionally followed by the component
classifier after a colon (:). An underline may be used for the name
as necessary to visually distinguish the name from the component
classifier label. Multiplicities of components and features can be
shown as a decorator in the upper right corner of the graphical
symbol.

It is permitted to include a text box with additional information
regarding the context of the graphical view, such as the name,
category, and property values of the AADL model object that is the
root of the graphical view. Figure 28 shows the content of a system
implementation without the enclosing component symbol and with a
text box that contains information relevant to the enclosing
component. The dashed rectangle is used to indicate the boundary of
the view panel. AADL model elements may have a label. This label
consists of the name and optionally the component classifier –
separated by a colon (:). Figure 28 illustrates the use of labels
for a subcomponent and a port.

|image23|

Figure − Component Implementation Content with Text Box
   

 Components may be parameterized with prototypes. Figure 29 shows how
such prototypes can be illustrated graphically. A paper symbol
represents the collection of prototypes and is attached to the right
corner. The content of graphical symbol can be the prototype
specification or the prototype bindings.

|image24|

Figure − Components and Prototypes
  

**Abstract Features, Ports, and Connections**

 Figure 30 shows the graphical symbols for abstract features and
ports. In this figure, the text represents the graphical symbol
legend rather than text label examples in a graphical AADL model.
The features are placed anywhere on the perimeter of the component
graphical symbol. The orientation of the graphical symbol indicates
the direction of the feature.

|image25|

Figure − Abstract Features, Ports and Connections
 

 Abstract features are represented by a solid circle, data ports by a
solid triangle, event ports by an open angle arrow, and event data
ports by a combination of both. The port direction is indicated by
the direction of the symbol tip. In the case of an abstract feature
an angle arrow is added to indicate the direction. The orientation
of the symbol must be adjusted according to the placement on the
perimeter. For example, the triangle of an incoming data port must
always be oriented such that the tip points into the component
graphical symbol.

The abstract feature and port connections are shown as a single
solid line. A legal AADL model requires connections to always be
connected to ports. In other words, a diagram showing a connection
attached to only one port is not acceptable except during the
operation of creating or modifying a connection graphically.

 Data ports may also have immediate and delayed connections. A single
solid line with a double arrow indicates an immediate connection. A
delayed connection is marked with a double line crossing the
connection line. If the direction of the connection cannot easily be
inferred from the feature symbols, then a single arrow may be added
to the connection line.

Figure − Connections & Branch Points


 A connection connects two features. Connections can be made between
components that are the same level of the system hierarchy, i.e.,
subcomponents within the same implementation, or between a component
and its enclosing component.

If a port has multiple outgoing or incoming connections, they may
either be shown as separate lines or as a single line with lines
branching out from it (see Figure 31). The branch points may be
marked by a dot to distinguish them from crossing lines.

**
Feature Groups**

 Feature groups are represented by a half circle and a solid ball
with the ball always facing to the outside of a component symbol.
Inside a component, individual ports or feature groups of
subcomponents may be connected to the half circle of the feature
group as shown in Figure 32. The half circle and solid ball
represents a collapsed view of a feature group. Feature group
connections are shown as solid line. The direction of a feature
group connection may be indicated by an arrow.

|image26|

Figure − Feature Groups & Connections
 

 Feature group types may be shown by using an expanded symbol for the
feature group and by attaching the feature group elements to the
enlarged half circle of the feature group symbol (see Figure 33).
Alternatively the half circle may be replaced by a vertical bar,
which can be expanded to accommodate a larger number of feature
group elements. The expanded symbol is useful for graphically
representing feature group type declarations and for detailing
connections to feature groups (see Figure 34).

Figure − Expanded Feature Group Type Symbol
   

 Figure 34 shows a set of feature groups in a system hierarchy.
Starting from the left, one thread supplies a data port and the
other thread furnishes a feature group. This composite feature group
is routed from a process via its enclosing system to another system.
Within that system the feature group is decomposed into one data
port and a feature group (shown using an expanded feature group
symbol). That feature group is routed to a process, which decomposes
it into its constituents, a data port and an event data port.

Figure − Feature Group Composition and Connections
  

 The expanded feature group symbol can be used to visually
distinguish between a connection that passes the whole feature group
to a subcomponent and a connection that passes a port group element
that itself is a feature group to a subcomponent by connecting the
former to the feature group half moon and the latter to the feature
group element (see Figure 34).

**
**

**Shared Access to Data and Buses**

 In AADL components may require access to other components and
provide access to components contained in them. The standard limits
such shared access to data components and buses. The same symbol is
used to represent access to bus and to data. Figure 35 illustrates
the graphical notation to represent sharing of data. Thread1
requires access to a data component and contains a data component
Data1, to which it provides access to other components. The
*provides data access* and *requires data access* features are shown
pointing in the direction of the component requiring data access.
Data access connections are shown as a single solid line. Both ends
of a data access connection must be connected. In our example the
required data access of Thread2 is resolved to the data component
Data1. Similarly, the required data access of Thread1 is resolved to
the data component Data2, which exists at the same system level as
Thread1.

Figure − Shared Data Access
   

 Figure 36 illustrates access between processors, memory, and devices
through shared access to buses. Memory, devices, processors, and
systems can have required bus access. Processors and systems can
have provided bus access. Bus access connections are shown as a
solid line. Both ends of a data access connection must be connected.
Required bus access is resolved to a bus component the same way
required data access is resolved to a data component. Also shown in
the figure is a provided bus access symbol to indicate that the bus
is made accessible outside the system.

Figure − Shared Bus Access
  

**Subprogram Calls and Subprogram Access**

 Figure 37 illustrates the graphical symbols for subprogram call
sequences and parameter passing. The sequence of calls is shown by a
set of subprogram graphical symbols with the call order indicated by
a single solid line with an open arrow head. The graphical display
of the call sequence order by use of the solid line with the open
arrow head is optional. If not shown, the call sequence can be
inferred from the left to right ordering of the subprogram symbol
placements.

Figure − Subprogram Calls and Parameter Passing
   

 Figure 37 shows a subprogram call sequence with two calls f1 and f2
to the same subprogram filter. Subprogram call labels follow the
convention of subcomponent and feature labels discussed earlier. The
call sequence ordering is determined by the placement of the
subprogram graphical symbols from left to right.

Optionally, the call sequence ordering can be explicitly shown.
Subprogram parameters are shown using a solid triangle - the same
symbol as used for data ports. A parameter connection is shown as a
solid line between two parameters or between a parameter and a data
port of a thread containing the subprogram call. The direction of
parameter connections must follow the direction of the call
sequence.

|image27|

Figure − Subprogram Access Features
   

 Figure 38 illustrates the subprogram and subprogram group access
feature symbols. The arrow direction indicates whether the feature
is provided or required and aligns with the direction of the call.
Subprogram access connections and subprogram group access
connections use a solid line.

**Modes and Mode Transitions**

 Figure 39 illustrates the graphical representation for modes.
Components and connections involved in a mode are shown in the
context of the enclosing component implementation. Individual modes
are shown as hexagons. The initial mode is shown with a bullet and a
transition to the appropriate mode. Mode transitions are shown as
simple lined arrows. The events triggering a mode transition can be
shown with dashed lines connecting the event port to the mode
transition, or by attaching the event name as a transition label
(shown in Figure 39).

Tools may show mode membership of connections and subcomponents in
the following way. Components and connections involved in a mode,
i.e., the modal configuration of a component, are shown in the
context of the enclosing component implementation. Mode1 is shown in
one color (shown in black in Figure 39) to indicate it as the
selected mode. The components, ports, and connections that are part
of the selected mode are highlighted in the same color. Other modes
and inactive components, ports, and connections are shown in a
different color (shown in gray in Figure 39). In other words, the
selection of a mode defines a subset view.

Figure 39 − Modes and Mode Transitions
  

**Modeling of Flows**

 Figure 40 shows the graphical symbols for flow source, flow sink,
and flow path specifications and their use in a component type. Flow
path symbols are connected to two ports, while flow source and flow
sink symbols are connected to one port.

|image28|

Figure − Flow Specifications


 Figure 41 illustrates how tools may graphically visualize a flow
implementation using the selection technique for mode modeling. A
flow implementation can be shown in black by selecting the flow of
interest as a flow specification in the text box. Subcomponent flows
and connections that are not part of the flow are shown in gray. An
editor can use this visualization both for displaying flows and for
defining flows. End-to-end flows can be visualized in a similar
manner. The text box has a compartment showing end-to-end flow
names. Selection of one results in showing the flow in black while
graying out the rest.

Figure − Flow Implementation Selection
  

**Packages, Property Sets, and Annexes**

 The graphical symbol for packages is a folder with the tab on the
left. The graphical symbol for a property set is a folder with the
tab on the right. The graphical symbol for annex libraries is a
folder with the tab on the left and the annex name embedded in the
annex-specific brackets from the textual AADL syntax.

The content of packages may be shown in a nested fashion. Public and
private sections of packages may be shown as appropriately labeled
compartments. Alternatively, component classifiers in a package may
be labeled using the UML convention of a plus (+) for public and
minus (-) for private classifiers.

Figure − Packages, Property Sets, and Annex Libraries
 

Implementation Suggestions
--------------------------

 This section suggests ways of graphically showing the system
structure and topology. Because of the structure of the underlying
formalisms, certain visualizations or views capture information that
is particularly useful to the user. The examples given in this
section are recommendations not requirements. A graphical toolset
may include alternative views.

**Package Library View**

 An AADL model represents a system as a set of component type and
implementation declarations. These declarations may be organized
into packages and may have *extends* relationships. This
organizational structure can be shown in a *library view* (see
Figure 43). This library view may show the content of a package as
component types and component implementations, and the *extends* and
*implements* relationship (see Figure 26).

Figure − A Component Library View
 

 The nesting of package names may be simply reflected in the package
name label, or the packages themselves may be shown as a hierarchy
based in the name nesting.

**System Instance Hierarchy**

 Actual systems have an instance hierarchy. This system instance
hierarchy is implicitly represented in the set of component types
and component implementations with one system implementation
identified as the root of the system instance. The subcomponents of
this root system implementation represent the components making up
the system, and the subcomponents contained in the component
implementations of these subcomponents recursively identify the
nested component instances.

The system instance hierarchy may be shown as a hierarchical tree or
as graphical structures. In Figure 44, the system hierarchy is shown
in tree form with a solid line with a diamond at the container end
showing the containment relationship. In Figure 45, the system
hierarchy is shown as a nesting of the graphical symbols to reflect
the containment. Both representations are permissible.

Figure − Tree-Structured Graphical Instance Hierarchy 
  

Figure − Nested Graphical Instance Hierarchy


 In many cases navigational tree views for showing hierarchical
structures are combined with a second panel showing the content of
the selected item in the navigational tree. This allows the full
instance hierarchy to be shown in the navigator and the currently
selected element in a graphical layout (see Figure 46).

Figure − Instance Navigation & Graphical Viewer
   

A. AADL Meta Model and XML Specification

Normative

This appendix is available as a separate document.

A. Unified Modeling Language (UML) Profile

Normative

The UML2 profile of AADL is defined as a subset profile of MARTE. It is
included in the OMG MARTE document.

A. Profiles and Extensions

Informative

 Profiling of the AADL standard is supported through project-specific
changes to the predeclared AADL\_Project property set and through
project-specific constraints that are checked by tools.

A project is allowed to add consistency rules to constrain the
architecture that can be processed. Such constraints must be
documented, including the motivation, rationale, implications. An
example constraint is that a subclause, such as **subcomponents**,
must be specified with **none** to indicate that no elements exist.
Another example constraint is that all communication between
processes must be through port connections. An AADL model that is
constrained through such consistency rules is still considered to be
a legal AADL model.

 The reasons for such constraints are to restrict the system to be
modeled, e.g., High integrity systems, or specific architectures
such as MILS, or to reflect inability to provide support for all
possible execution semantics in the runtime system.

 A profile may also include extensions to AADL, such as additional
properties that are defined in a property set. For example, support
for the MILS architecture may include partition-specific properties
and health monitoring specific properties.

Profiles can be turned into a standardized set of guidelines, for
example, the effort to define guidelines for modeling MILS
architectures in AADL.

A *minimal* AADL core subset is defined to support the following
restrictions:

 No modes, no user defined property sets, no runtime protection of
virtual address spaces, single processor, no virtual processor, no
virtual bus, queue depth 1, no remote subprogram calls, no sporadic,
background, hybrid, timed threads.

ARINC653
========

This annex document is available as a separate document.

Behavior Model
==============

This annex document is available as a separate document.

Code Generation
===============

Normative

This annex document is available as a separate document.

Data Modeling
=============

Normative

This annex document is available as a separate document.

Error Model
===========

Normative

This annex document is available as a separate document.

Mini Annexes
============

Normative

This annex document will contain small sublanguage annex extensions.
Candidates are: Data set support; Equivalence and Signature Matching
rules, Configuration constraint support; Mode transition and dispatch
trigger condition support.

Data Sets
---------

 This annex is to provide support for defining sets of property
associations and allowing them to be associated with a model. A data
set is a named collection of contained property associations that
all have a common root for their **applies to** paths. Such named
data sets can be defined as elements of a DataSet annex library and
can be referenced in DataSet annex subclauses.

The purpose of this annex is to provide the ability to annotate
elements of an AADL model with data that is expressed as property
associations. For example, we can define a dataset that represents
estimated execution time of threads and subprograms, a second
dataset that represents benchmark execution time of threads and
subprogram, and a third dataset that represents measured execution
time of actual code. Different sets of information can be kept in
separate datasets, e.g., security related information may be kept
separate from timing information. Different datasets can be
associated with the same model, e.g., various combinations of
timing-related datasets and security-related datasets can be
associated with the system implementation that represents the root
of a system.

NOTE: Model-related data may already exist in a user-defined format.
In that case the data does not have to be converted into an AADL
property association representation. Instead the modeler can
associate the name of a file that contains such data through a
user-defined property.

Syntax

NOTE: The Unique\_component\_implementation\_reference and
contained\_property\_association syntax rules are those defined in the
AADL core language standard.

Naming Rules

1. The defining identifier of a dataset must be unique within the same
   annex library declaration. In other words, an AADL package can
   contain only one data set annex library and each dataset annex
   library has its own name space.

2. The dataset reference must name a dataset that is defined in a
   dataset annex library. The dataset annex library is uniquely
   identified by the AADL package it is contained in.

3. The root of the **applies to** path of the contained property
   associations is the component implementation referenced in the
   **applies to** statement of the dataset definition.

Legality Rules

1. The dataset annex subclause must be declared as one of the annex
   subclauses of a component implementation extension of the
   component implementation identified as the root in the **applies
   to** of the dataset.

.. [1]
   This was the original name of the SAE AADL.

.. [2]
   VHDL is the Very-High-Speed-Integrated-Circuit Hardware Description
   Language. See IEEE VHDL Analysis and Standardization Group for
   details and status.

.. [3]
   MatLab and SimuLink are commercial tools available from The
   MathWorks.

.. |image0| image:: media/image1.jpeg
.. |image1| image:: media/image2.png
.. |image2| image:: media/image3.png
.. |image3| image:: media/image4.png
.. |image4| image:: media/image5.png
.. |image5| image:: media/image6.png
.. |image6| image:: media/image7.png
.. |image7| image:: media/image8.png
.. |image8| image:: media/image9.png
.. |image9| image:: media/image10.png
.. |image10| image:: media/image11.png
.. |image11| image:: media/image12.png
.. |image12| image:: media/image20.png
.. |image13| image:: media/image21.png
.. |image14| image:: media/image25.png
.. |image15| image:: media/image26.png
.. |image16| image:: media/image29.png
.. |image17| image:: media/image31.png
.. |image18| image:: media/image32.png
.. |image19| image:: media/image35.png
.. |image20| image:: media/image36.png
.. |image21| image:: media/image37.png
.. |image22| image:: media/image38.png
.. |image23| image:: media/image39.jpeg
.. |image24| image:: media/image40.png
.. |image25| image:: media/image41.png
.. |image26| image:: media/image43.png
.. |image27| image:: media/image48.png
.. |image28| image:: media/image50.png
