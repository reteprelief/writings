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


#. If the component interface contains requires\_mode declarations then it
must not contain any mode or mode transition declarations. 

#. In case of component interface extensions, only one interface being extended can contain mode related definitions. 
If the extensions adds mode related definitions, then they must be requires\_mode declaration if the inherited declarations are requires\_mode, 
or they must be mode and transition definitions if the inherited definitions are modes and transitions.

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

