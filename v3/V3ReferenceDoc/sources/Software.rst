Software Components
===================

 This section defines the following categories of software
components: data, subprogram, subprogram group, thread, thread
group, and process.

 Software components may have associated source text specified using
property associations. Software source text can be processed by
source text tools to obtain a binary executable image consisting of
code and data to be loaded onto a memory component and executed by a
processor component. Source text may be written in a traditional
programming language, a very-high-level or domain-specific language,
or may be an intermediate product of processing such
representations, e.g., an object file.

Data components classifiers represent data types, while data
subcomponents represent static data in source text, and local
variables in subprograms. Data components are sharable between
threads within the same thread group or process, and across
processes and systems.

 The subprogram component models callable source text that is
executed sequentially. Subprograms are callable from within threads
and subprograms.

 Threads represent sequential sequences of instructions in loaded
binary images produced from source text. Threads model schedulable
units of control that can execute concurrently. Threads can interact
with each other through exchanges of control and data as specified
by port connections, through remote subprogram calls, and through
shared data components.

(5) A thread group is a compositional component that permits
organization of threads within processes into groups with relevant
property associations.

(6) A process represents a virtual address space. Access protection of
the virtual address space is by default enforced at runtime, but can
be disabled if specified by the property Runtime\_Protection. The
source text associated with a process forms a complete program as
defined in the applicable programming language standard. A complete
process specification must contain at least one thread declaration.
Processes may share a data component as specified by the required
subcomponent resolved to an actual subcomponent and accessed through
port connections.

1. .. rubric:: Data
  :name: data

 A data component type represents a data type in source text. The
internal structure of a source text data type, e.g., the instance
variables of a class or the fields of a record, can be represented
by data subcomponents in a data component implementation. The
provides subprogram access features of a data component type can
model the concept of methods on a class or operations on an abstract
data type. A source text data type can be modeled by a data
component type declaration with relevant properties without
providing internal details in a data component implementation.

 A data component classifier, i.e., a data component type name or a
data component type and implementation name pair (separated by a dot
.), is used as data type indicator in port declarations,
subprogram parameter declarations, and data subcomponent
declarations.

A data subcomponent represents static data in the source text.
Components can have shared access to data subcomponents, which means
that there are mutual exclusion requirements. Only those components
that explicitly declare required data access can access such
sharable data subcomponents using a concurrency control protocol as
specified by property. Data subcomponents can be shared within the
same process and, if supported by the runtime system, across
processes. When declared in a subprogram or a thread that data
subcomponent represents a local variable.

 References to data components are modeled through provides and
requires data access. Threads, processes, systems, and subprograms
may access data by reference (see Section 8.6).

NOTE: Support for shared data is not intended to be a substitute for
data flow communication through ports. It is provided to support
modeling of systems whose application logic requires them to manipulate
data concurrently in a non-deterministic order that cannot be
represented as data flow, such as database access. It is also provided
for modeling source text that does not use port-based communication.

AADL focuses on architecture modeling. When used for data modeling,
properties defined as part of the Data Modeling Annex (Annex Document B)
can be used to further characterize data components.

Legality Rules

+----------------+---------------------------------------+----------------------+
| **Category**   | **Type**  | **Implementation**   |
+----------------+---------------------------------------+----------------------+
| \ **data** | Features: | Subcomponents:   |
||   |  |
|| -  feature group  | -  data  |
||   |  |
|| -  provides subprogram access | -  subprogram|
||   |  |
|| -  requires subprogram access | -  abstract  |
||   |  |
|| -  requires subprogram group access   |  |
||   |  |
|| -  provides data access   |  |
||   |  |
|| -  feature|  |
+----------------+---------------------------------------+----------------------+

1. A data type declaration can contain provides subprogram access
   declarations as well as property associations.

1. A data type declaration must not contain a flow specification or
   modes subclause.

2. A data implementation can contain abstract, data and subprogram
   subcomponents, access connections, and data property
   associations.

3. A data implementation must not contain a flow implementation, an
   end-to-end flow specification, or a modes subclause.

Standard Properties

Data Size: Size

Code Size: Size

Type Source Name: **aadlstring**

Source Name: **aadlstring**

Source Text: **inherit list of aadlstring**

-- hardware mapping

Base Address: **aadlinteger** 0 **..** Max Base Address

Allowed Memory\_Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Memory\_Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

-- Data sharing properties

Access Right : Access Rights => read write

Concurrency Control Protocol: Supported Concurrency Control Protocols

 The value of the Type\_Source\_Name property identifies the name of
the data type declaration in the source text. The value of the
Source\_Name property identifies the name of the static or local
data variable in the source text.

Semantics

  The data component type represents a data type in the source text
 that defines a representation and interpretation for instances of
 data in the source text. This includes data transferred through
 data and event data ports, and parameter values transferred to
 subprograms. This data type (class) may have associated access
 functions (called methods in an Object-Oriented context) that are
 represented by provides subprogram access declarations in the
 **features** subclause of the data type declaration. In this case,
 the data may be accessed through the subprograms.

 A provides subprogram access declaration represents a callable
 subprogram. It represents implicitly declared subprogram
 subcomponent that is contained in the process of the subprogram
 call, i.e., a single subprogram instance exists within a process.
 Modelers may also declare subprogram subcomponents explicitly. A
 remotely callable access function of a data type is modeled by a
 provides subprogram access declaration that will be connected from
 a subprogram call or is named in the classifier reference of the
 call.

  Elements of a data component can be accessed and changed directly
 or via provides subprogram access features. This corresponds to get
 and set methods of a class. AADL does not impose visibility
 restrictions on elements of a data component.

  A data component type can have zero data component implementations.
 This allows source text data types to be modeled without having to
 represent implementation details.

(5)  A data component implementation represents the internal structure
 of a data component type. It can contain data subcomponents. This
 is used to model source language concepts such as fields in a
 record and instance variables in a class. The data subcomponent
 represents actual data values. AADL does not require the user to
 provide internal details of data representations if they are not
 relevant to the architecture model. The user may choose to reflect
 relevant data modeling information in properties, e.g., the memory
 requirements, the measurement unit used for the data, acceptable
 data types for a union type, dimensionality of an array structure,
 the super types in a type hierarchy, or the data type of a
 reference.

(6)  Data component types can be extended through component type
 extension declarations. This permits modeling of subclasses and
 type inheritance in source text. However, it is recommended to use
 the capabilities of the Data Modeling Annex to represent data model
 characteristics in AADL (see Annex Document B).

(7)  A data subcomponent represents a data instance, i.e., data in the
 source text that is potentially sharable between threads and
 persists across thread dispatches. A data subcomponent is
 considered to be static data with the exception of data
 subcomponents in subprograms, which represent local data. Each
 declared data subcomponent represents a separate instance of source
 text data.

(8)  A data subcomponent declared in a subprogram represents local data.
 This data cannot be made accessible outside the subprogram through
 a provides data access declaration.

(9)  When declaring data subcomponents, it is sufficient for the
 component classifier reference of data subcomponent declarations to
 only refer to the data component type. An implementation method can
 generate a system instance and perform memory usage analysis if a
 Data\_Size property value is specified in the data component type.

(10) Data subcomponents that are not declared in subprograms can be
 shared between threads. This is expressed by requires data access
 declarations in the component type declarations of subprograms,
 threads, thread groups, processes, and systems. The access is
 resolved to data subcomponents or provides data access
 declarations. Each required reference to shared data may have its
 own Access\_Right property value. Its value must be consistent with
 the value of the Access\_Right property of the data component or a
 provides data access. The Access\_Right property value of
 Read\_Only on a data component indicates that the component
 contains a constant value that does not change.

(11) Concurrent access to shared data is coordinated according to the
 concurrency control protocol specified by the
 Concurrency\_Control\_Protocol property value associated with the
 data component. A thread is considered to be in a critical region
 when it is accessing a shared data component. When a thread enters
 a critical region a Get\_Resource operation is performed on the
 shared data component (see *Runtime Support* in Section 5.1.1).
 Upon exit from a critical region a Release\_Resource operation is
 performed (see *Runtime Support* in Section 5.1.1). If multiple
 data components with concurrency control protocols are accessed by
 a thread, the AADL runtime system must ensure that the critical
 regions are nested, i.e., the Get\_Resource and Release\_Resource
 operations are pair-wise nested for each data component.

(12) Concurrent access to shared data may be coordinated through
 *provides subprogram (group) access*. In this case, the concurrency
 control protocol specifies how the execution of subprograms on the
 data component is coordinated.

(13) Data component classifier references are also used to specify the
 data type for data and event data ports as well as subprogram
 parameters. When ports are connected or when required data access
 and subprogram parameters are resolved, the data component
 classifier references representing the data types must be
 compatible. This means that the data type of an out port must be
 compatible with the data type of an in port, the data type of a
 provided data access declaration or a declared data component must
 be compatible with the data type of a required data component, and
 the data type of an actual parameter must be compatible with that
 of the formal parameter of a subprogram. Compatibility is
 determined by the Classifier\_Matching\_Rule property (see Section
 9.2).

(14) Data components can be declared as arrays of data subcomponents.
 Same as for other subcomponent array declarations this is a
 short-hand for declaring several subcomponents of the same type
 through separate subcomponent declarations. If the intent is to
 model a data component whose representation in the source text is
 an array data modeling properties should be used (see Appendix
 A.8).

 1. .. rubric:: Runtime Support For Shared Data Access
   :name: runtime-support-for-shared-data-access

(15) A standard set of runtime services is provided. The application
 program interface for these services is defined in the code
 generation annex of this standard (see Annex Document A). They are
 callable from within the source text.

(16) The following are subprograms that may be explicitly called by
 application source code, or they may be called by an AADL runtime
 system that is generated from an AADL model.

(17) The Get\_Resource and Release\_Resource  runtime services represent
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

Processing Requirements and Permissions

 If any source text is associated with a data component type, then a
corresponding source text data type declaration must be visible in
the outermost scope of that source text, as defined by the scope and
visibility rules of the applicable source language standard. The
name of the data component type determines the source name of the
data type. Supported mappings of the identifier to a source type
name for specific source languages are defined in the source
language annex of this standard. Such mapping can also be explicitly
specified through the Type\_Source\_Name property.

The applicable source language standard may allow a data type to be
declared using a type constructor or type modifier that references
other source text data types. A source text data type name defined
by a source type constructor may, but is not required to, be modeled
as a data component type with the referenced type features
explicitly named in a corresponding data component implementation
declaration.

 Data modeling properties allow for modeling data representations in
the source text that include stored references (pointers). A method
of implementation may disallow storing of such data references as
data values in order to assure safe execution of embedded
applications.

 A method of implementation may use a provides subprogram access
declaration to represent an implicit subprogram subcomponent. In
this case, the provides subprogram access feature does not have to
be connected to a subprogram subcomponent or a requires subprogram
access feature.

(5) A method of implementation may disallow assignments that might
result in a runtime error depending on the actual value being
assigned. If a method of implementation employs a runtime check to
determine if a specific value may be legally assigned, then any
runtime fault is associated with the thread that contains the source
of the data assignment.

(6) If two static data declarations refer to the same source text data,
then that data must be replicated in binary images. If this
replication occurs within the same virtual address space, a method
for resolving name conflicts must be in place. Alternatively the
processing method may require that each source text data be
represented by only one data component declaration per process
address space.

(7) The concurrency control protocol can be implemented through a number
of concurrency control mechanisms such as mutex, lock, semaphore, or
priority ceiling protocol. Appropriate concurrency control state is
associated with the shared data component to maintain concurrency
control. The protocol implementation must provide appropriate
implementations of the Get\_Resource and Release\_Resource
operations.

(8) A method of implementation may choose to support only locking of one
resource at a time, or locking of multiple resources simultaneously.
In the former case it is the responsibility of the caller of
Get\_Resource in such an order that deadlock and starvation is
avoided. In the latter case, the Get\_Resource implementation must
assure absence of deadlock and starvation.

(9) A method of implementation may choose to generate the Get\_Resource
and Release\_Resource calls as part of the AADL runtime system
generation, or it may choose to require the application programmer
to place those calls into the application code. In the latter case,
implementation methods may validate the sequencing of those calls to
assure compliance with the AADL specification.

Examples

**package** personnel

**public**

**with** Base\_Types, retep::relief;

**data** Person

**end** Person;

**data** Personnel\_record

-- Methods are not required, but when provided act as access methods

**features**

-- a method on the data type

-- subprogram type for signature

update\_address: **provides subprogram access** update\_address;

**end** Personnel\_record;

**data implementation** Personnel\_record.others

**subcomponents**

-- here we declare the internal structure of the data type

-- One field is defined in terms of another type;

-- the type name is sufficient, it defaults to others.

-- the package Base\_Types is defined in the Data Model Annex document.

-- It provides data component classifiers for common data types.

Name : **data** Base\_Types::String;

Home\_address : **data** retep::relief::Address;

**end** Personnel\_record.others;

**data** Personnel\_database

**end** Personnel\_database;

**data implementation** Personnel\_database.oracle

**end** Personnel\_database.oracle;

**subprogram** update\_address

**features**

person: **in out parameter** Personnel\_record;

street :**in parameter** Base\_Types::String;

city: **in parameter** Base\_Types::String;

**end** update\_address;

-- use of a data type as port type.

**thread** SEI\_Personnel\_addition

**features**

new\_person: **in event data port** Personnel\_record;

SEI\_personnel: **requires data access** Personnel\_database.oracle;

**properties**

Dispatch\_Protocol => aperiodic;

**end** SEI\_Personnel\_addition;

**end** personnel;

**package** retep::relief

**public**

**data** Address

**features**

-- a subprogram access feature without parameter detail

getStreet : **provides subprogram access**;

getCity : **provides subprogram access**;

**end** Address;

**data implementation** Address.others

**properties**

Data\_Size => 512 Bytes;

**end** Address.others;

**end** retep::relief;

-- The implementation is shown as a private declaration

-- The public and the private part of a package are separate AADL
specifications

**package** retep::relief

**private**

**with** Base\_Types;

**data implementation** Address.others

**subcomponents**

street : **data** Base\_Types::String;

streetnumber: **data** Base\_Types::Integer;

city: **data** Base\_Types::String;

zipcode: **data** Base\_Types::Integer;

**end** Address.others;

**end** retep::relief;

Subprograms and Subprogram Calls
--------------------------------

 A subprogram component represents sequentially executed source text
that is called with parameters. A subprogram may not have any state
that persists beyond the call (static data). Subprograms can have
local variables that are represented by data subcomponents in the
subprogram implementation. All parameters and required access to
persistent data must be explicitly declared as part of the
subprogram type declaration. In addition, any events raised within a
subprogram must be specified as part of its type declaration.

 A subprogram call sequence is declared in a thread implementation or
in a subprogram implementation. Subprogram call sequences may be
mode-specific. Subprograms can be called from threads and from other
subprograms that execute within a thread. These calls can be local
calls, i.e., performed in the context of the caller thread, or they
can be remote calls to subprograms that are executed in the context
of another thread.

Subprogram instances may be modeled explicitly through subprogram
subcomponent declarations. In this case, components can be modeled
as requiring and providing access to subprogram instances.

 Subprogram instances may be implied by a subprogram call reference
to a subprogram type or implementation. In this case, a subprogram
is implicitly instantiated within the containing process.

Syntax

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

Naming Rules

1. The defining identifier of a subprogram call sequence declaration
   must be unique within the local namespace of the component
   implementation that contains the subprogram call sequence.

1. The defining identifier of a subprogram call declaration must be
   unique within the local namespace of the component implementation
   that contains the subprogram call.

2. If the called subprogram name is a subprogram classifier reference,
   its component type identifier or component implementation name must
   exist in the package namespace.

3. The subprogram classifier reference of a subprogram call may be a
   subprogram type reference.

4. If the called subprogram name is a subprogram subcomponent reference,
   the subprogram subcomponent must exist in the component
   implementation containing the subprogram call declaration.

5. If the called subprogram name is a requires subprogram access
   reference, the requires subprogram access must exist in the component
   type of the component implementation containing the subprogram call
   declaration.

Legality Rules

+------------------+---------------------------------------+----------------------+
| **Category** | **Type**  | **Implementation**   |
+------------------+---------------------------------------+----------------------+
| **subprogram**   | Features: | Subcomponents:   |
|  |   |  |
|  | -  out event port | -  data  |
|  |   |  |
|  | -  out event data port| -  abstract  |
|  |   |  |
|  | -  feature group  | -  subprogram|
|  |   |  |
|  | -  requires data access   |  |
|  |   |  |
|  | -  requires subprogram access |  |
|  |   |  |
|  | -  requires subprogram group access   |  |
|  |   |  |
|  | -  parameter  |  |
|  |   |  |
|  | -  feature|  |
+------------------+---------------------------------------+----------------------+

1. A subprogram type declaration can contain parameter, out event port,
   out event data port, and feature group declarations as well as
   requires data, subprogram, and subprogram group access
   declarations. It can also contain a flow specification subclause,
   a modes subclause, and property associations.

1. A subprogram implementation can contain abstract, subprogram, and
   data subcomponents, a subprogram calls subclause, a connections
   subclause, a flows subclause, a modes subclause, and property
   associations.

2. Only one subprogram call sequence can apply to a given mode.

Consistency Rules

1. The reference to a provides subprogram access of a processor in a
   subprogram call (**processor** .
   *provides\_subprogram\_access\_*\ identifier) must identify a
   provides subprogram access feature of the processor that the thread
   executing the call is bound to.

2. A subprogram call may reference a subprogram classifier. A project
   may enforce a consistency rule that this reference be to a subprogram
   subcomponent declaration or requires subprogram access declaration.
   This ensures that a modeler consistently models subprogram calss the
   same way.

Standard Properties

-- Properties related to source text

Source\_Name: aadlstring

Source\_Text: **inherit list of aadlstring**

Source\_Language: **inherit list of** Supported\_Source\_Languages

Type\_Source\_Name: **aadlstring**

-- Properties specifying memory requirements of subprograms

Code\_Size: Size

Data\_Size: Size

Stack\_Size: Size

Heap\_Size: Size

Allowed\_Memory\_Binding\_Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed\_Memory\_Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual\_Memory\_Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

-- execution related properties

Compute\_Execution\_Time: Time\_Range

Compute\_Deadline: Time

Client\_Subprogram\_Execution\_Time: Time\_Range

Reference\_Processor: **inherit classifier** ( processor )

-- remote subprogram call related properties

Urgency: **aadlinteger** 0 **..** Max\_Urgency

Actual\_Subprogram\_Call: **reference** (subprogram)

Allowed\_Subprogram\_Call: **list of** **reference** (subprogram)

**Actual\_Subprogram\_Call\_Binding: list of reference (bus, processor,
memory, device)**

Allowed\_Subprogram\_Call\_Binding:

**inherit** **list** **of** **reference** (bus, processor, device)

Subprogram\_Call\_Type: **enumeration** (Synchronous, SemiSynchronous)

=> Synchronous

Semantics

  A subprogram component represents sequentially executable source
 text that is called with parameters. The results of a subprogram
 call must be available to the caller at the time those results are
 used. This allows for synchronous and semi-synchronous calls.

 A subprogram type declaration specifies all interactions of the
 subprogram with other parts of the application source text.
 Subprogram parameters are specified as features of a subprogram
 type (see Section 8.4). This includes **in** and **in out**
 parameters passed into a subprogram and **out** and **in out**
 parameters returned from a subprogram on a call, events being
 raised from within the subprogram through its **out event port**
 and **out event data port**, required access to static data by the
 subprogram are specified as part of the features subclause of a
 subprogram type declaration, and required access to subprograms
 that are contained in another component and are called by this
 subprogram.

  A subprogram implementation represents implementation details that
 are relevant to architecture modeling. It specifies calls to other
 subprograms and the mode in which the call sequence occurs. It also
 specifies any local data of the subprogram, i.e., data that does
 not persist beyond the call.

  All access to data that persists beyond the life of the subprogram
 execution, i.e., any state that is maintained by a subprogram, must
 be modeled through requires data access. If requires data access is
 declared for a subprogram type, access to the data subcomponent may
 be performed in a critical region to assure concurrency control for
 calls from different threads (for more on concurrency control see
 Sections 5.1 and 5.4).

(5)  Subprogram source text can contain Send\_Output service calls to
 cause the transmission of events and event data through its **out
 event** ports (see Section 8.3). The fact that events may emit from
 a subprogram call is documented by the declaration of **out event
 ports** and **out event data ports** as features of the subprogram.

(6)  Subprogram implementations and thread implementations can contain
 subprogram calls. A thread or subprogram can contain multiple calls
 to the same subprogram - with the same parameter values or with
 different parameter values.

(7)  Subprogram call sequences can be declared to apply to specific
 modes. In this case a call sequence is only executed if one of the
 specified modes is the current mode.

(8)  Modeling of subprograms is not required and the level of detail is
 not prescribed by the standard. Instead it is determined by the
 level of detail necessary for performing architecture analyses or
 code generation.

(9)  In an object-oriented application methods are called on an object
 instance and the object instance is available within the method by
 the name *this*. In AADL a subprogram call can identify the
 subprogram being called by the provides subprogram access feature
 of a data component. In AADL, the data component must be explicitly
 passed into a subprogram as parameter (by value) or as requires
 data access (by reference). Requires data access may require
 concurrency control to ensure mutual exclusion.

(10) Ordering of subprogram calls is by default determined by the order
 of the subprogram call declarations. Annex-specific notations,
 e.g., the Behavior Annex, can be introduced to allow for other call
 order specifications, such as conditional calls and iterations.

(11) The flow of parameter values between subprogram calls as well as to
 and from ports of enclosing threads is specified through parameter
 connection declarations (see Section 9.3).

(12) Subprogram instances may be modeled explicitly through subprogram
 subcomponent declarations, or they may be implied from the call
 references to subprogram classifiers. A subprogram instance means
 that the subprogram executable binaries exist in the load image of
 the containing process. For subprograms, whose source text
 implementation is reentrant, it is assumed that a single instance
 of the subprogram binaries exist in the process address space if
 the instances are not declared explicitly as subcomponents. In the
 case of remote subprogram calls a proxy may be loaded for the
 calling thread and the actual subprogram is part of the load image
 of the process with the thread servicing the remote subprogram
 call.

(13) A subprogram subcomponent declaration explicitly represents a
 subprogram instance that resides in the protected address space of
 the containing process. Subprogram calls refer to the subprogram
 subcomponent or to requires subprogram access declarations. In case
 of a requires subprogram access the call is local to a subprogram
 instance in the containing process, or is remote to a subprogram
 instance in another process. Subprogram access connection
 declarations identify the subprogram instance to be called.

(14) The standard permits modeling of subprograms and subprogram calls
 without requiring the declaration of subprogram instances. In this
 case, subprogram calls may refer to subprogram classifiers and the
 source language processing system will determine the subprogram
 instance to be called. In the case of remote subprogram calls the
 target subprogram is identified by subprogram call properties. An
 Allowed\_Subprogram\_Call property, if present, identifies the
 remote subprogram(s) that are allowed to be used in a call binding.
 An Actual\_Subprogram\_Call property records the actual binding to
 a subprogram or provides subprogram access feature. Constraints on
 the buses and processors over which such calls can be routed can be
 specified with the Allowed\_Subprogram\_Call\_Bindings property.

(15) The following control flow semantics apply to subprogram calls,
 when the call refers to:

-  Subprogram classifier: execution by the calling thread

-  Provides subprogram access of data type: execution by the calling
   thread

-  Subprogram subcomponent in calling thread: execution by the calling
   thread

-  Provides subprogram access feature of a data component: execution by
   calling thread

-  Subprogram access to subprogram component in enclosing thread group,
   process, or system: execution by calling thread

-  Subprogram access to subprogram component in another thread group,
   process, or system: execution by calling thread

-  Provides subprogram access of another thread: execution by called
   thread

-  Provides subprogram access feature of a device: execution inside
   device

-  Subprogram access of a processor: execution inside the processor
   (operating system)

-  Subprogram classifier and the call has a subprogram call binding
   property that refers to provides subprogram access in other thread:
   execution by called thread

 The results of a subprogram call must be available to the caller at
the time those results are used. In the case of a local call the
results are available when the call returns, i.e., the call is
performed as a synchronous call. In the case of remote call, the
caller thread is by default suspended until the execution of the
subprogram completes (synchronous call). The caller thread may issue
multiple concurrently executing subprogram calls and wait for their
result when needed (semi-synchronous call). The
Subprogram\_Call\_Type property indicates whether synchronous or
semi-synchronous calls are desired.

In the case of a remote call, the thread servicing the subprogram
call assures that only one call at a time is serviced. In other
words, it acts as a critical region for all calls to provides
subprogram access features of a thread.

 Provides subprogram access features may be declared for processors
or devices. In the case of processors they represent operating
system services provided by the processor. In the case of a device,
they represent services on the device that can be invoked by the
application software.

Processing Requirements and Permissions

 The subprogram call order defines a default execution order for the
subprogram calls. Alternate call orders can be modeled in an annex
subclause introduced for that purpose.

The legality rules require that call declarations either refer only
to subprogram classifiers or to subprogram instances (subcomponents
and provides/requires subprogram access). This rule can be relaxed
to allow a mix of both if this is appropriate for the development
process.

 An implementation method may support synchronous calls only or also
semi-synchronous calls.

Examples

**data** matrix

**end** matrix;

**data** weather\_forecast

**end** weather\_forecast;

**data** date

**end** date;

**subprogram** Matrix\_delta

**features**

A: **in parameter** matrix;

B: **in parameter** matrix;

result: **out parameter** matrix;

**end** Matrix\_delta;

**subprogram** Interpret\_result

**features**

A: **in parameter** matrix;

result: **out parameter** weather\_forecast;

**end** Interpret\_result;

**data** weather\_DB

**features**

getCurrent: **provides subprogram access** getCurrent;

getFuture: **provides subprogram access** getFuture;

**end** weather\_DB;

**subprogram** getCurrent

**features**

result: **out parameter** Matrix;

**end** getCurrent;

**subprogram** getFuture

-- a subprogram whose source text sends an event

-- the subprogram also has access to shared data

**features**

date: **in parameter** date;

result: **out parameter** Matrix;

bad\_date: **out event port**;

wdb: **requires data access** weather\_DB;

**end** getFuture;

**thread** Predict\_Weather

**features**

target\_date: **in event data port** date;

prediction: **out event data port** weather\_forecast;

past\_date: **out event port**;

weather\_database: **requires data access** weather\_DB;

**end** Predict\_Weather;

**thread implementation** Predict\_Weather.others

**calls** main : {

-- subprogram call on a data component provides subprogram access
feature

-- out parameter is not resolved, but will be identified by user of
value

current: **subprogram** weather\_DB.getCurrent;

-- subprogram call on a data component provides subprogram access
feature with port value

-- as additional parameter. Event is mapped to thread event

future: **subprogram** weather\_DB.getFuture;

-- in parameter actuals are out parameter values of previous calls

-- they are identified by the call name and the out parameter name

diff: **subprogram** Matrix\_delta;

-- call with out parameter value resolved to be passed on through a port

interpret: **subprogram** Interpret\_result;

};

**connections**

fdconn: **parameter** target\_date -> future.date;

pdconn: **port** future.bad\_date -> past\_date;

daconn: **parameter** current.result -> diff.A;

dbconn: **parameter** future.result -> diff.B;

iaconn: **parameter** diff.result -> interpret.A;

pconn: **parameter** interpret.result -> prediction;

fwconn: **data access** weather\_database <-> future.wdb;

**end** Predict\_Weather.others;

Subprogram Groups and Subprogram Group Types
--------------------------------------------

 Subprogram groups represent subprogram libraries. Subprogram groups
can be made accessible to other components through subprogram group
access features (see Section 8.4) and subprogram group access
connections (see Section 9.4). This grouping concept allows the
number of connection declarations to be reduced, especially at
higher levels of a system when a number of provided subprograms from
one subcomponent and its contained subcomponents must be connected
to requires subprogram access in another subcomponent and its
contained subcomponents. The content of a subprogram group is
declared through a subprogram group type declaration. This
declaration is then referenced when subprogram groups are declared
as subcomponents.

Naming Rules

1. The defining identifier of a subprogram group type must be unique
   within the package namespace of the package where the subprogram
   group type is declared.

1. Each subprogram group provides a local namespace. The defining
   subprogram identifiers of subprogram declarations in a subprogram
   group type must be unique within the namespace of the subprogram
   group type.

2. The local namespace of a subprogram group type extension includes the
   defining identifiers in the local namespace of the subprogram group
   type being extended. This means, the defining identifiers of
   subprogram or subprogram group declarations in a subprogram group
   type extension must not exist in the local namespace of the
   subprogram group type being extended. The defining identifiers of
   subprogram or subprogram group refinements in a subprogram group type
   extension must refer to a subprogram or subprogram group in the local
   namespace of an ancestor subprogram group type.

3. The defining subprogram identifiers of subprogram access feature
   declarations in feature group refinements must not exist in the local
   namespace of any subprogram group being extended. The defining
   subprogram identifier of subprogram\_refinement declarations in
   subprogram group refinements must exist in the local namespace of any
   feature group being extended.

4. The package name of the unique subprogram group type reference must
   refer to a package name in the global namespace. The subprogram group
   type identifier of the unique subprogram group type reference must
   refer to a subprogram group type identifier in the named package.

Legality Rules

+------------------------+---------------------------------------+-----------------------+
| **Category**   | **Type**  | **Implementation**|
+------------------------+---------------------------------------+-----------------------+
| **subprogram group**   | Features: | Subcomponents:|
||   |   |
|| -  feature group  | -  subprogram |
||   |   |
|| -  provides subprogram access | -  subprogram group   |
||   |   |
|| -  requires subprogram access | -  data   |
||   |   |
|| -  requires subprogram group access   | -  abstract   |
||   |   |
|| -  provides subprogram group access   |   |
||   |   |
|| -  feature|   |
+------------------------+---------------------------------------+-----------------------+

1. A subprogram group type can contain provides and requires subprogram
   access, and provides and requires subprogram group access.

1. A subprogram group implementation can contain abstract, data,
   subprogram group, and subprogram subcomponents as well as data
   and subprogram access connections.

2. A subprogram group type or implementation may contain zero or more
   subcomponent declarations. If it contains zero elements, then the
   subprogram group type or implementation is considered to be
   incompletely specified.

Standard Properties

-- Port properties defined to be **inherit**, thus can be associated
with a

-- feature group to apply to all contained ports.

Source Text: inherit list of **aadlstring**

-- properties related to execution time

Reference Processor: **inherit classifier** ( processor )

-- Properties specifying memory requirements of subprograms

Code Size: Size

Data Size: Size

Stack Size: Size

Heap Size: Size

Allowed Memory Binding Class:

**inherit** **list** **of** classifier **(memory, system, processor,**
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Semantics

 A subprogram group declaration represents groups of component
subprograms, i.e., subprogram libraries. Subprograms in a subprogram
group may require access to other subprograms or subprogram groups.

Requires subprogram group access is resolved to provides subprogram
group access or a subprogram group subcomponent.

 The subprograms of a subprogram group or a subprogram group access
feature can be connected to or referenced in a subprogram call.

Processing Requirements and Permissions

 Subprogram groups represent subprogram libraries. These can be
application libraries or system libraries. Libraries may be shared
across multiple applications, i.e., across multiple processes.

Methods of implementation may optionally allow a provides subprogram
access declaration of a subprogram group to not be connected to a
subprogram instantiation, i.e., subprogram subcomponent. It may
assume these subprograms to be implicitly declared and instantiated
as part of a subprogram group instantiation.

Examples

**subprogram group** mathlib

**features**

matrixMultiply: **provides subprogram access** ;

matrixAdd: **provides subprogram access** ;

vectorAdd: **requires subprogram access** ;

**end** mathlib;

Threads
-------

 A thread models a concurrent task or an active object, i.e., a
schedulable unit that can execute concurrently with other threads.
Each thread represents a sequential flow of control that executes
instructions within a binary image produced from source text. One or
more AADL threads may be implemented in a single operating system
thread. A thread always executes within the virtual address space of
a process, i.e., the binary images making up the virtual address
space must be loaded before any thread can execute in that virtual
address space. Threads are dispatched, i.e., their execution is
initiated periodically by the clock or by the arrival of data or
events on ports, or by arrival of subprogram calls from other
threads.

AADL supports an input-compute-output model of communication and
execution for threads and port-based communication. The inputs
received from other components are frozen at a specified point, by
default the dispatch of a thread. As a result the computation
performed by a thread is not affected by the arrival of new input
until an explicit request for input, by default the next dispatch.
Similarly, the output is made available to other components at a
specified point in time, for data ports by default at completion
time or thread deadline. In other words, AADL is able to support
both synchronous execution and communication behavior, e.g., in the
form of deterministic sampling of a control system data stream, as
well as asynchronous concurrent processing.

 Systems modeled in AADL can have operational modes (see Section 12).
A thread can be active in a particular mode and inactive in another
mode. As a result, a thread may transition between an active and
inactive state as part of a mode switch. Only active threads can be
dispatched and scheduled for execution. Threads can be dispatched
periodically or as the result of explicitly modeled events that
arrive at event ports, event data ports. Completion of the normal
execution including error recovery will result in an event being
delivered through the reserved Complete event out port. Completion
under unrecoverable error conditions will result in an event being
delivered through the reserved Abort and Stop ports.

If the thread execution results in a fault that is detected, the
source text may handle the error. If the error is not handled in the
source text, the thread is requested to recover and prepare for the
next dispatch. If an error is considered thread unrecoverable, its
occurrence is reported through the reserved Error out event data
port.

Legality Rules

+----------------+---------------------------------------+-----------------------+
| **Category**   | **Type**  | **Implementation**|
+----------------+---------------------------------------+-----------------------+
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
+----------------+---------------------------------------+-----------------------+

1. A thread type declaration can contain port, feature group, requires
   and provides data access declarations, as well as requires and
   provides subprogram access declarations. It can also contain flow
   specifications, a modes subclause, and property associations.

1. A thread component implementation can contain abstract, data,
   subprogram, and subprogram group subcomponent declarations, a
   calls subclause, a flows subclause, a modes subclause, and thread
   property associations.

2. The Complete **out** event port, Error **out** event data port, Abort
   **out** event port, and Stop **out** event port are reserved,
   i.e., they must be declared of the specified port type. They must
   be explicitly defined as features in order to be referenced,
   e.g., in connections, flows and mode transitions.

Consistency Rules

1. At least one of the Compute\_Entrypoint,
   Compute\_Entrypoint\_Source\_Text or
   Compute\_Entrypoint\_Call\_Sequence property must have a value that
   indicates the source code to execute after a thread has been
   dispatched when an implementation is to be generated or consistency
   with source code is to be checked. Other entrypoint properties are
   optional, i.e., if a property value is not defined, then the
   entrypoint is not called.

2. The Period property must have a value if the Dispatch\_Protocol
   property value is periodic, sporadic, timed, or hybrid.

Standard Properties

-- Properties related to source text

Source Text: **inherit list of aadlstring**

Source Language: **inherit list of** Supported\_Source\_Languages

-- Properties specifying memory requirements of threads

Code Size: Size

Data Size: Size

Stack Size: Size

Heap Size: Size

-- Properties specifying thread dispatch properties

Dispatch Protocol: Supported\_Dispatch\_Protocols

Dispatch Trigger: **list of** **reference** (port)

Dispatch Able: **aadlboolean **

Dispatch Offset: **inherit** Time

First Dispatch Time **: inherit** Time

Period: **inherit Time**

-- the default value of the deadline is that of the period

Deadline: inherit Time => Period

-- Scheduling properties

Priority: **inherit** **aadlinteger**

POSIX Scheduling Policy : **enumeration** (SCHED FIFO, SCHED RR, SCHED
OTHERS)

Criticality: **aadlinteger**

Time Slot: **list of aadlinteger **

-- Properties specifying execution entrypoints and timing constraints

Initialize Execution Time: Time Range

Initialize Deadline: Time

Initialize Entrypoint: **classifier** ( subprogram classifier )

Initialize Entrypoint Call\_Sequence: **reference** ( subprogram call
sequence )

Initialize Entrypoint Source Text: **aadlstring**

Compute Execution Time: Time Range

Compute Deadline: Time

Compute Entrypoint: **classifier** ( subprogram classifier )

Compute Entrypoint Call Sequence: **reference** ( subprogram call
sequence )

Compute Entrypoint Source Text: **aadlstring**

Activate Execution Time: Time Range

Activate Deadline: Time

Activate Entrypoint: **classifier** ( subprogram classifier )

Activate Entrypoint Call Sequence: **reference** ( subprogram call
sequence )

Activate Entrypoint Source Text: **aadlstring**

Deactivate Execution Time: Time Range

Deactivate Deadline: Time

Deactivate Entrypoint: **classifier** ( subprogram classifier )

Deactivate Entrypoint Call\_Sequence: **reference** ( subprogram call
sequence )

Deactivate Entrypoint Source Text: **aadlstring**

Recover Execution Time: Time Range

Recover Deadline: Time

Recover Entrypoint: **classifier** ( subprogram classifier )

Recover Entrypoint Call Sequence: **reference** ( subprogram call
sequence )

Recover Entrypoint Source Text: **aadlstring**

Finalize Execution Time: Time Range

Finalize Deadline: Time

Finalize Entrypoint: **classifier** ( subprogram classifier )

Finalize Entrypoint Call Sequence: **reference** ( subprogram call
sequence )

Finalize Entrypoint Source Text: **aadlstring**

Reference Processor: **inherit classifier** ( processor )

-- mode to enter as result of activation

Resumption Policy: **enumeration** ( restart, resume )

-- Properties specifying constraints for processor and memory binding

Allowed Processor Binding Class:

**inherit** **list** **of** **classifier** (processor, virtual
processor, device, system)

Allowed Processor Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, device, system)

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Connection Binding Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

Allowed Connection Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, bus, virtual bus, device, memory, system)

Not Collocated: **record** (

Targets: **list** **of** **reference** (data, thread, process, system,
connection);

Location: **classifier** ( processor, memory, bus, system ); )

Collocated: **record** (

Targets: **list** **of** **reference** (data, thread, process, system,
connection);

Location: **classifier** ( processor, memory, bus, system ); )

-- Binding value filled in by binding tool

Actual Processor Binding: **inherit** **list of** **reference**
(processor, virtual processor, device, system)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

Actual Connection\_Binding: **inherit list of** **reference**
(processor, virtual processor, bus, virtual bus, device, system, memory)

-- property indicating whether the thread affects the hyperperiod

-- for mode switching

Synchronized Component: **inherit** **aadlboolean** => **true**

-- property specifying the action for executing thread at actual mode
switch

Active Thread Handling Protocol:

**inherit** Supported\_Active\_Thread\_Handling\_Protocols => abort

Active\_Thread\_Queue\_Handling\_Protocol:

**inherit enumeration** (flush, hold) => flush

NOTE: Entrypoints for thread execution can be specified in three ways:
by identifying source text name, by identifying a subprogram classifier
representing the source text, or by a call sequence.

Compute\_Entrypoint => **classifier** ( ControlAlgorithm.basic );

Compute\_Entrypoint\_Call\_Sequence => **reference** ( callseq1 );

Compute\_Entrypoint\_Source\_Text => MyControlAlgorithm;

Semantics

  Thread semantics are described in terms of thread states, thread
 dispatching, thread scheduling and execution, and fault handling.
 Thread execution semantics apply once the appropriate binary images
 have been loaded into the respective virtual address space (see
 Section 5.6).

 Threads are dispatched periodically or by the arrival of data and
 events, or by arrival of subprogram calls from other threads.
 Subprogram calls always trigger dispatches. Subsets of ports can be
 specified to trigger dispatches. By default, any one of the
 incoming **event ports** and **event data ports** triggers a
 dispatch. The Dispatch\_Trigger property can specify different
 subsets of ports, including **data ports** (see Section 5.4.2) as a
 disjunction or the Behavior Annex (see Section Annex Document D)
 can be used to specify additional logical conditions on thread
 dispatch triggering.

  Port input is frozen at dispatch time or a specified time during
 thread execution and made available to the thread for access in the
 form of a port variable (see Section 8.3). From that point on its
 content is not affected by new arrival of data and event for the
 remainder of the current execution. This assures a
 input-compute-output model of execution. By default, input of ports
 is frozen for all ports that are not candidates for thread dispatch
 triggering; for dispatch trigger candidates, only those port(s)
 actually triggering a specific dispatch is frozen. Whether input of
 specific ports is frozen at a dispatch and the time at which it is
 frozen can be explicitly specified (see Section 8.3.2).

  Threads may be part of modes of containing components. In that case
 a thread is active, i.e., eligible for dispatch and scheduling,
 only if the thread is part of the current mode (see Sections 5.4.1
 and 13.6).

(5)  Threads can contain mode subclauses that define thread-internal
 operational modes. Threads can have property values that are
 different for different thread-internal modes (see Section 5.4.5).

(6)  Every thread has a predeclared **out event port** named Complete.
 If this port is connected, i.e., named as the source in a
 connection declaration, then an event is raised implicitly on this
 port when nominal execution including recovery of a thread dispatch
 completes.

(7)  Every thread has a predeclared **out event data port** named Error.
 If this port is connected, i.e., named as the source in a
 connection declaration, then an event is raised implicitly on this
 port when a thread unrecoverable error is detected (see Section
 5.4.4 for more detail). This supports the propagation of thread
 unrecoverable errors as event data for fault handling by a thread.

(8)  Threads may contain subprogram subcomponents that can be called
 from within the thread, and also by other threads if it is made
 accessible through a provides subprogram access declaration.
 Similarly, a thread can contain a subprogram group declaration,
 which represents an instance of a subprogram library dedicated to
 the thread. The subprograms within the subprogram library can be
 called from within the thread or by other threads if it is made
 accessible through a provides subprogram group access declaration.
 For further details about calling subprograms see Section 5.2.
 Finally, a thread can contain data subcomponents. They represent
 static data owned by the thread, i.e., state that is preserved
 between thread dispatches. The thread has exclusive access to this
 data component unless it specifies it to be accessible through a
 provides data access declaration.

 1. .. rubric:: Thread States and Actions
   :name: thread-states-and-actions

(9)  A thread executes a code sequence in the associated source text
 when dispatched and scheduled to execute. This code sequence is
 part of a binary image accessible in the virtual address space of
 the containing process. It is assumed that the process is bound to
 the memory that contains the binary image (see Section 5.6).

(10) A thread goes through several states. Thread state transitions
 under normal operation are described here and illustrated in Figure
 5. Thread state transitions under fault conditions are described in
 Section 5.4.4

(11) The initial state is *thread halted*. When the loading of the
 virtual address space as declared by the enclosing process
 completes (see Section 5.6), a thread is *initialized* by
 performing an initialization code sequence in the source text. Once
 initialization is completed the thread enters the *suspended
 awaiting dispatch* state if the thread is part of the initial mode,
 otherwise it enters the *suspended awaiting mode* state. When a
 thread is in the *suspended awaiting mode* state it cannot be
 dispatched for execution.

(12) A thread may be declared to have modes. In this case, each mode
 represents a behavioral state within the execution of the thread.
 When a thread is dispatched it is assumed to execute in a specific
 mode. It may resume execution in the behavioral state (mode) in
 which it completed its previous dispatch execution, e.g., state
 reflected in a static data component, or it may execute in a
 specific mode based on the input received at dispatch time. A modal
 thread can have mode-specific property values. For example, a
 thread can have different worst-case execution times for different
 modes, each representing a different execution path through the
 source code. The result is a model that more accurately reflects
 the actual system behavior. The Behavior Model Annex Document D
 allows for a refined specification of thread behavior, e.g., it may
 explicitly specify the conditions under which the thread executes
 in one mode or another mode and it can represent intermediate
 behavioral states.

(13) When a mode transition is initiated, a thread that is part of the
 old mode and not part of the new mode *exits* the mode by
 transitioning to the *suspended awaiting mode* state after
 performing *thread deactivation* during the *mode change in
 progress* system state (see Figure 23). If the thread is periodic
 and its Synchronized\_Component property is true, then its period
 is taken into consideration to determine the actual mode transition
 time (see Sections 12 and 13.6 for detailed timing semantics of a
 mode transition). If an aperiodic or a sporadic thread is executing
 a dispatch when the mode transition is initiated, its execution is
 handled according to the Active\_Thread\_Handling\_Protocol
 property. The execution of a background thread is suspended through
 deactivation while the thread is not part of the new mode. A thread
 that is not part of the old mode and part of the new mode *enters*
 the *mode* by transitioning to the *suspended awaiting dispatch*
 state after performing *thread activation*.

(14) When in the *suspended awaiting dispatch* state, a thread is
 awaiting a dispatch request for performing the execution of a
 compute source text code sequence as specified by the
 Compute\_Entrypoint property on the thread or on the event or event
 data port that triggers the dispatch. When a dispatch request is
 received for a thread, data, event information, and event data is
 made available to the thread through its port variables (see
 Sections 8.2 and 9.1). The thread is then handed to the scheduler
 to perform the computation. Upon successful completion of the
 computation, the thread returns to the *suspended* *awaiting
 dispatch* state. If a dispatch request is received for a thread
 while the thread is in the compute state, this dispatch request is
 handled according to the specified Overflow\_Handling\_Protocol for
 the event or event data port of the thread.

(15) A thread may enter the *thread halted* state, i.e., will not be
 available for future dispatches and will not be included in future
 mode switches. If re-initialization is requested for a thread in
 the *thread halted* state (see Section 5.6), then its virtual
 address space is reloaded, the processor to which the thread is
 bound is restarted, or the system instance is restarted.

(16) A thread may be requested to enter its *thread halted* state
 through a *stop* request after completing the execution of a
 dispatch or while not part of the active mode. In this case, the
 thread may execute a *finalize* entrypoint before entering the
 *thread halted* state. A thread may also enter the *thread halted*
 state immediately through an *abort* request. Any resources locked
 by Get\_Resource are released (see Figure 5).

(17) Figure 5 presents the top-level hybrid automaton (using the
 notation defined in Section 1.6) to describe the dynamic semantics
 of a thread from the perspective of a scheduler. The hybrid
 automaton states complement the application modes declared for
 threads. Figure 7 elaborates the performing *thread computation*
 state of Figure 5. Figure 6 elaborates the executing *nominally*
 substate of Figure 7. The bold faced edge labels in Figure 5
 indicate that the transitions marked by the label are coordinated
 across multiple hybrid automata. The scope of the labels is
 indicated in parentheses, i.e., interaction with the process hybrid
 automaton (Figure 8), with the system hybrid automaton (Figure 22)
 and with system wide mode switching (see Figure 23). Thread
 initialization is only started when the process containing the
 thread has been loaded as indicated by the label
 **loaded(process)**. The label **started(system)** is coordinated
 with other threads and the system hybrid automaton to transition to
 *System operational* only after threads have been initialized. In
 some systems it is desirable to initialize all threads, while in
 other system it is acceptable for threads to be created and
 initialized more dynamically, possibly even at activation and
 deactivation.

(18) The hybrid automata contain assertions. In a time-partitioned
 system these assertions will be satisfied. In other systems they
 will be treated as anomalous behavior.

(19) For each of the states representing a *performing thread* action
 such as *initialize*, *compute*, *recover*, *activate*,
 *deactivate*, and *finalize*, an execution entrypoint to a code
 sequence in the source text can be specified. Each entrypoint may
 refer to a different source text code sequence which contains the
 entrypoint, or all entrypoints of a thread may be contained in the
 same source text code sequence. In the latter case, the source text
 code sequence can determine the context of the execution through a
 Dispatch\_Status runtime service call (see Section 5.4.8). The
 execution semantics for these entrypoints is described in Section
 5.4.3.

(20) An *Initialize\_Entrypoint* (enter the state performing *thread*
 *initialization* in Figure 5) is executed during system
 initialization and allows threads to perform application specific
 initialization, such as ensuring the correct initial value of its
 **out** and **in out** ports. A thread that has halted may be
 re-initialized.

(21) The *Activate\_Entrypoint* (enter the state performing *thread*
 *activation* in Figure 5) and *Deactivate\_Entrypoint* (enter the
 state performing *thread* *activation* in Figure 5) are executed
 during mode transitions and allow threads to take user-specified
 actions to save and restore application state for continued
 execution between mode switches. These entrypoints may be used to
 reinitialize application state due to a mode transition. Activate
 entrypoints can also ensure that **out** and **in out** ports
 contain correct values for operation in the new mode.

(22) The *Compute\_Entrypoint* (enter state performing *thread*
 *computation* in Figure 5) represents the code sequence to be
 executed on every thread dispatch. Each provides subprogram access
 feature represents a separate compute entrypoint of the thread.
 Remote subprogram calls are thread dispatches to the respective
 entrypoint. Event ports and event data ports can have port specific
 compute entrypoints to be executed when the corresponding event or
 event data dispatches a thread.

(23) A *Recover\_Entrypoint* (enter the state executing *recovery* in
 Figure 7) is executed when a fault in the execution of a thread
 requires recovery activity to continue execution. This entrypoint
 allows the thread to perform fault recovery actions (for a detailed
 description see Section 5.4.4).

(24) A *Finalize\_Entrypoint* (enter the state performing *thread*
 *finalize* in Figure 5) is executed when a thread is asked to
 terminate as part of a process unload or process stop.

(25) If no value is specified for any of the entrypoints, then there is
 no invocation at all.

Figure − Thread States and Actions
  

** from ports**
When an aperiodic, sporadic, timed, or hybrid thread
declares multiple in event and event data ports in its type that can
be dispatch triggers and more than one of these queues are nonempty,
the port with the higher Urgency property value gets serviced first.
If several ports with the same Urgency are non-empty, then the
Queue\_Processing\_Protocol is applied across these ports and must
be the same for them. In the case of FIFO the oldest event will be
serviced (global FIFO). It is permitted to define and use other
algorithms for picking among multiple non-empty queues. Disciplines
other than FIFO may be used for managing each individual queue.



Thread Dispatching
~~~~~~~~~~~~~~~~~~

 Threads are dispatched periodically determined by a clock or by the
arrival of events, event data, of calls to provides subprogram
access. By default any event port or event data port can trigger a
dispatch. In that case, only the input of the port triggering the
dispatch and any data port is available to the application program.

A thread may have a Dispatch\_Trigger property to specify a subset
of event, data, or event data ports that can trigger a thread
dispatch. In this case, arrival of events or event data on any of
the listed ports can trigger the dispatch.

 The default disjunction of ports triggering a dispatch can be
overwritten by a logical condition on the ports expressed by a annex
subclauses of the Behavior Annex notation (see Annex Document D).

 For periodic threads arrival of events or event data will not result
in a dispatch. Events and event data are queued in their incoming
port and are accessible to the application code of the thread.
Periodic thread dispatches are solely determined by the clock
according to the time interval specified through the Period property
value.

(5) The Dispatch\_Protocol property of a thread determines the
characteristics of dispatch requests to the thread. This is modeled
in the hybrid automaton in Figure 5 by the Enabled(t) function as
the Wait\_For\_Dispatch invariant. The Enabled function determines
when a transition from Wait\_For\_Dispatch to performing thread
computation will occur. The Wait\_For\_Dispatch invariant captures
the condition under which the Enabled function is evaluated. The
consequence of a dispatch is the execution of the entrypoint source
text code sequence at its *current execution* position. This
position is set to the first step in the code sequence and reset
upon completion (see Section 5.4.3).

(6) For a thread whose dispatch protocol is periodic, a dispatch request
is issued at time intervals of the specified Period property value.
The Enabled function is t = Period. The Wait\_For\_Dispatch
invariant is t ≤ Period ∧ δt = 1. The dispatch occurs at t = Period.
The Compute\_Entrypoint of the thread is called.

(7) Periodic threads can have a Dispatch\_Offset property value. In this
case the dispatch time is offset from the period by the specified
amount. This allows two periodic threads with the same period to be
aligned, where the first thread has a pre-period deadline, and the
second thread has a dispatch offset greater than the deadline of the
first thread. This is a static alignment of thread execution order
within a frame, while the immediate data connection achieves the
same by dynamically aligning completion time of the first thread and
the start of execution of the second thread (see Section 9.2.5).

(8) For threads whose dispatch protocol is aperiodic, sporadic, timed,
or hybrid, a dispatch request is the result of an event or event
data arriving at an event or event data port of the thread, or a
remote subprogram call arriving at a provides subprogram access
feature of the thread. This *dispatch trigger condition* is
determined as follows:

-  Arrival of an event or event data on any incoming event, or event
   data port, or arrival of any subprogram call request on a provides
   subprogram access feature. In other words, it is a disjunction of all
   incoming features.

-  By arrival on a subset of incoming features (port, subprogram
   access). This subset can be specified through the Dispatch\_Trigger
   value of the thread.

-  By a user-defined logical condition on the incoming features that can
   trigger the dispatch expressed through an annex subclause expressed
   in the Behavior Annex sublanguage notation (see Annex Document D).

 For a thread whose dispatch protocol is aperiodic, a dispatch
request is the result of an event or event data arriving at an event
or event data port of the thread, or a remote subprogram call
arriving at a provides subprogram access feature of the thread.
There is no constraint on the inter-arrival time of events, event
data or remote subprogram calls. The dispatch actually occurs
immediately when a dispatch request arrives in the form of an event
at an event port with an empty queue, or if an event is already
queued when a dispatch execution completes, or a remote subprogram
call arrives. The Enabled function by default has the value true if
there exists a port or provides subprogram access (p) in the set of
features that can trigger a dispatch (E) with a non-empty queue,
i.e., ∃ p in E: p ≠ ∅. This evaluation function may be redefined by
the Behavior Annex (see Annex Document D). The Wait\_For\_Dispatch
invariant is that no event, event data, or subprogram call is
queued, i.e., ∀ p in E: p = ∅. The Compute\_Entrypoint of the port
triggering the dispatch, or if not present that of the thread, is
called.

If multiple ports are involved in triggering the dispatch the
Compute\_Entrypoint of the thread is called. The list of ports
actually satisfying the dispatch trigger condition that results in
the dispatch is available to the source text as output parameter of
the Await\_Dispatch service call (see Section 5.4.8).

 For a thread whose dispatch protocol is sporadic, a dispatch request
is the result of an event or event data arriving at an event or
event data port of the thread, or a remote subprogram call arriving
at a provides subprogram access feature of the thread. The time
interval between successive dispatch requests will never be less
than the associated Period property value. The Enabled function is t
≥ Period∧∃ p in E: p ≠ ∅. The Wait\_For\_Dispatch invariant is t <
Period ∨ (t > Period ∧ ∀ p in E: p = ∅). The dispatch actually
occurs when the time condition on the dispatch transition is true
and a dispatch request arrives in the form of an event at an event
port with an empty queue, or an event is already queued when the
time condition becomes true, or a remote subprogram call arrives
when the time condition is true. The Compute\_Entrypoint of the port
triggering the dispatch, or if not present that of the thread, is
called.

 For a thread whose dispatch protocol is timed, a dispatch request is
the result of an event, event data, or remote subprogram arrival, or
it occurs by an amount of time specified by the Period property
since the last dispatch.. In other words, the Period represents a
time-out value that ensure a dispatch occurs after a given amount of
time if no events, event data, or remote subprogram calls have
arrived or are queued. The Enabled function by default has the value
true if there exists a port or provides subprogram access (p) in the
set of features that can trigger a dispatch (E) with an event, event
data, or call in its queue, or time equal to the Period has expired
since the last dispatch, i.e., ∃ p in E: p ≠∅ ∨ t = Period. t is
reset to zero at each dispatch. This evaluation function may be
redefined by the Behavior Annex (see Annex Document D). The
Wait\_For\_Dispatch invariant is that no event, event data, or call
is queued, i.e., ∀ p in E: p = ∅ ∧ t < Period. The
Compute\_Entrypoint of the port triggering the dispatch, or if not
present that of the thread, is called. If a timeout occurs, i.e.,
the dispatch is triggered at the end of the period, the
Recover\_Entrypoint is called.

(5) A thread whose dispatch protocol is hybrid, combines both aperiodic
and periodic dispatch behavior in the same thread. A dispatch
request is the result of an event, event data, or remote subprogram
call arrival, as well as periodic dispatch requests at a time
interval specified by the Period property value. The Enabled
function is t = Period ∨ ∃ p in E: p ≠ ∅. t is reset to zero at each
periodic dispatch. The evaluation function for events, event data,
or subprogram calls may be redefined by the Behavior Annex. The
Wait\_For\_Dispatch invariant is that no event, event data, call, or
periodic dispatch is queued and the period has not expired, i.e., ∀
p in E: p = ∅ ∧ t ≤ Period. The Compute\_Entrypoint of the port
triggering the dispatch, or if not present that of the thread, is
called.

(6) If several events or event data occur logically simultaneously and
are routed to the same port of an aperiodic, sporadic, timed, or
hybrid thread, the order of arrival for the purpose of event
handling according the above rules is implementation-dependent. If
several events or event data occur logically simultaneously and are
routed to the different ports of the same aperiodic, sporadic,
timed, or hybrid thread, the order of event handling is determined
by the Urgency property associated with the ports.

(7) For a thread whose dispatch protocol is background, the thread is
dispatched upon completion of its initialization entrypoint
execution the first time it is active in a mode. The Enabled
function is true. The Wait\_For\_Dispatch invariant is t = 0. The
dispatch occurs immediately. If the Dispatch\_Trigger property is
set, then its execution is initiated through the arrival of an event
or event data on one of those ports. In that case, the Enabled
function is true for any Dispatch port p ∈ Dispatch\_Trigger : p ≠
∅.

(8) The different dispatch protocols can be summarized as follows:

+----+----+
+----+----+
+----+----+
+----+----+
+----+----+
+----+----+

 Note that background threads do not have their current execution
position reset on a mode switch. In other words, the background
thread will resume execution from where it was previously suspended
due to a mode switch.

Note that background threads are suspended when not active in the
current mode. If they have access to shared data components, they
may have locked the resource at the time of suspension and
potentially cause deadlock if active threads also share access to
the same data component.

 A background thread is scheduled to execute such that all other
threads’ timing requirements are met. If more than one background
thread is dispatched, the processor’s scheduling protocol determines
how such background threads are scheduled. For example, a FIFO
protocol for background threads means that one background thread at
a time is executed, while fair share means that all background
threads will make progress in their execution.

 The Overflow\_Handling\_Protocol property for event or event data
ports specifies the action to take when events arrive too
frequently. These events are ignored, queued, or are treated as an
error. The error treatment causes the currently active dispatch to
be aborted, allowing it to clean up through the Recover\_Entrypoint
and then be redispatched. For more details on port queuing see
section 8.3.3.

Examples

**thread** Prime\_Reporter

**features**

  Received\_Prime : **in event data port** Base\_Types::Integer;

**properties**

  Dispatch\_Protocol => Timed;

| **end** Prime\_Reporter;
| **thread** Prime\_Reporter\_One **extends** Prime\_Reporter

**features**

  Received\_Prime : **refined to in event data port**
Base\_Types::Integer

    {Compute\_Entrypoint\_Source\_Text =>
"Primes.On\_Received\_Prime\_One";};

-- function called on message-based dispatch

**properties**

  Period => 9 Sec; -- timeout period

  Priority => 45;

  Compute\_Entrypoint\_Source\_Text => "Primes.Report\_One";  

-- function called in case of timeout

**end** Prime\_Reporter\_One;

Thread Scheduling and Execution
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 When a thread action is *performing thread computation* (see Figure
5), the execution of the thread’s entrypoint source text code
sequence is managed by a scheduler. This scheduler coordinates all
thread executions on one processor as well as concurrent access to
shared resources. While performing the execution of an entrypoint
the thread can be *executing nominally* or *executing recovery* (see
Figure 7). While executing an entrypoint a thread can be in one of
five substates: ready, running, awaiting resource, awaiting return,
and awaiting resume (see Figure 6).

A thread initially enters the *ready* state. A scheduler selects one
thread from the set of threads in the ready state to run on one
processor according to a specified scheduling protocol. It ensures
that only one thread is in the *running* state on a particular
processor. If no thread is in the ready state, the processor is idle
until a thread enters the ready state. A thread will remain in the
running state until it completes execution of the dispatch, until a
thread entering the ready state preempts it if the specified
scheduling protocol prescribes preemption, until it blocks on a
shared resource, or until an error occurs. In the case of
completion, the thread transitions to the suspended *awaiting
dispatch* state, ready to service another dispatch request. In the
case of preemption, the thread returns to the ready state. In the
case of resource blocking, it transitions to the *awaiting resource*
state.

 Shared data is accessed in a critical region. Resource blocking can
occur when a thread attempts enter a critical region while another
thread is already in this critical region. In this case the thread
enters the *Awaiting resource* state. A
Concurrency\_Control\_Protocol property value associated with the
shared data component determines the particular concurrency control
mechanism to be used (see Section 5.1). The Get\_Resource and
Release\_Resource service calls are provided to indicate the entry
and exit of critical regions (see Section 5.1.1). When a thread
completes execution it is assumed that all critical regions have
been exited, i.e., access control to shared data has been released.
Otherwise, the execution of the thread is considered erroneous.

 Subprogram calls to remote subprograms are synchronous or
semi-synchronous. In the synchronous case, a thread in the running
state enters the *awaiting return* state when performing a call to a
subprogram whose service is performed by a subprogram in another
thread. The service request for the execution of the subprogram is
transferred to the remote subprogram request queue of a thread as
specified by the Actual\_Subprogram\_Call property that specifies
the binding of the subprogram call to a subprogram in another
thread. When the thread executing the corresponding remote
subprogram completes and the result is available to the caller, the
thread with the calling subprogram transitions to the ready state.
In the semi-synchronous case, the calling thread continues to
execute concurrently until it awaits the result of the call (see
service call Await\_Result in Section 5.4.8).

(5) A background thread may be temporarily suspended by a mode switch in
which the thread is not part of the new mode, as indicated by the
**exit(Mode)** in Figure 6. In this case, the thread transitions to
the *awaiting mode\_entry* state. If the thread was in a critical
region, it will be suspended once it releases all resources on exit
of the critical region. A background thread resumes execution when
it becomes part of the current mode again in a later mode switch. It
then transitions from the *awaiting\_mode\_entry* state into the
*ready* state.

(6) Execution of any of these entrypoints is characterized by actual
execution time (*c*) and by elapsed time (*t*). Actual execution
time is the time accumulating while a thread actually runs on a
processor. Elapsed time is the time accumulating as real time since
the arrival of the dispatch request. Accumulation of time for *c*
and *t* is indicated by their first derivatives δ\ *c* and δ\ *t*. A
derivative value of 1 indicates time accumulation and a value of 0
indicates no accumulation. Figure 6 shows the derivative values for
each of the scheduling states. A thread accumulates actual execution
time only while it is in the running state. The processor time, if
any, required to switch a thread between the running state and any
of the other states, which is specified in the
Thread\_Swap\_Execution\_Time property of the processor, is not
accounted for in the Compute\_Execution\_Time property, but must be
accounted for by an analysis tool.

(7) The execution time and elapsed time for each of the entrypoints are
constrained by the entrypoint-specific <entrypoint>\_Execution\_Time
and entrypoint-specific <entrypoint>\_Deadline properties specified
for the thread. If no entrypoint specific execution time or deadline
is specified, those of the containing thread apply. There are three
timing constraints:

-  Actual execution time, *c*, will not exceed the maximum
   entrypoint-specific execution time.

-  Upon execution completion the actual execution time, *c*, will have
   reached at least the minimum entrypoint-specific execution time.

-  Elapsed time, *t*, will not exceed the entrypoint-specific deadline.

Figure − Thread Scheduling and Execution States
   

 Execution of a thread is considered anomalous when the timing
constraints are violated. Each timing constraint may be enforced and
reported as an error at the time, or it may be detected after the
violation has occurred and reported at that time. The implementor of
a runtime system must document how it handles timing constraints.

1. .. rubric:: Execution Fault Handling
  :name: execution-fault-handling

A fault is defined to be an anomalous undesired change in thread
execution behavior, possibly resulting from an anomalous undesired
change in data being accessed by that thread or from violation of a
compute time or deadline constraint. An error is a fault that is
detected during the execution of a thread. Detectable errors are
classified as *thread recoverable* errors, or *thread unrecoverable*
errors.

 A *thread recoverable* error may be handled as part of normal
execution by that thread, e.g., by exception handlers programmed in
the source text of the application. The exception handler may
propagate the error to an external handler by sending an event or
event data through a port.

 If the thread recoverable error is not handled by the application,
the thread affected by the error is given a chance to recover
through the invocation of the thread’s recover entrypoint. The
recover entrypoint source text sequence has the opportunity to
update the thread’s application state. The recover entrypoint is
assumed to have access to an error code through a runtime service
call Get\_Error\_Code. The recover entrypoint may report the fact
that it performed recovery through a user–defined port. Upon
completion of the recover entrypoint execution, the performance of
the thread’s dispatch is considered complete. In the case of
performing thread computation, this means that the thread
transitions to the suspended await dispatch state (see Figure 5),
ready to perform additional dispatches. Concurrency control on any
shared resources must be released. If the recover entrypoint is
unable to recover the error becomes a *thread unrecoverable* error.
This thread-level fault handling in terms of thread scheduling
states is illustrated in Figure 7.

(5) A thread recoverable error may occur during the execution of a
remote subprogram call. In this case, the thread servicing the
remote call is given a chance to recover as well as the thread that
made the call.

(6) In the presence of a thread recoverable error, the maximum interval
of time between the dispatch of a thread and its returning to the
suspended awaiting dispatch state is the sum of the thread’s compute
deadline and its recover deadline. The maximum execution time
consumed is the sum of the compute execution time and the recover
execution time. In the case when an error is encountered during
recovery, the same numbers apply.

(7) *Thread unrecoverable* errors are reported as event data through the
Error port of the thread, where they can be communicated to a
separate error handling thread for further analysis and recovery
actions.

(8) A thread unrecoverable error causes the execution of a thread to be
terminated prematurely without undergoing recovery. The thread
unrecoverable error is reported as an error event through the
predeclared Error event data port, if that port is connected. If
this implicit error port is not connected, the error is not
propagated and other parts of the system will have to recognize the
fault through their own observations. In the case of a thread
unrecoverable error, the maximum interval between the dispatch of
the thread and its returning to the suspended awaiting dispatch
state is the compute deadline, and the maximum execution time
consumed is the compute execution time.

(9) For errors detected by the runtime system, error details are
recorded in the data portion of the event as specified by the
implementation. For errors detected by the source text, the
application can choose its encoding of error detail and can raise an
event in the source text. If the propagated error will be used to
directly dispatch another thread or trigger a mode change, only an
event needs to be raised. If the recovery action requires
interpretation external to the raising thread, then an event with
data must be raised. The receiving thread that is triggered by the
event with data can interpret the error data portion and raise
events that trigger the intended mode transition.

Figure − Performing Thread Execution with Recovery
  

  A timing fault during initialize, compute, activation, and
 deactivation entrypoint executions is considered to be a thread
 recoverable error. A timing fault during recover entrypoint
 execution is considered to be a thread unrecoverable error.

 If any error is encountered while a thread is executing a recover
 entrypoint, it is treated as a thread unrecoverable error. In other
 words, an error during recovery must not cause the thread to
 recursively re-enter the executing recovery state.

  If a fault is encountered by the application itself, it may
 explicitly raise an error through a Raise\_Error service call on
 the Error port with the error class as parameter. This service call
 may be performed in the source text of any entrypoint. In the case
 of recover entrypoints, the error class must be *thread
 unrecoverable*.

  Faults may also occur in the execution platform. They may be
 detected by the execution platform components themselves and
 reported through an event or event data port, as defined by the
 execution platform component. They may go undetected until an
 application component such as a health monitoring thread detects
 missing health pulse events, or a periodic thread detects missing
 input. Once detected, such errors can be handled locally or
 reported as event data.

 1. .. rubric:: Thread Internal Modes and Mode Transitions
   :name: thread-internal-modes-and-mode-transitions

(5)  A thread can have modes declared inside its type or implementation.
 They represent thread-internal execution paths and allow
 mode-specific property values to be associated with the thread. For
 example, the thread can have different execution times under
 different modes. Application source text (programming code)
 actually executes branch and merge points in various places of its
 code sequence and branches based on state values or based on input.
 In terms of the mode abstraction this means that the (mode) state
 at time of dispatch may affect the branch condition, the input to
 the thread execution may affect the branch condition, or a
 combination, or a change in the state value is the result of
 computation based on the previous value and/or input.

(6)  A mode transition of a thread-internal mode may be implicit in that
 it is determined by the application source code of the thread. This
 source code may follow and execution sequence based on the content
 of thread input or by the value of a static data variable. In the
 former case, the current mode is determined at the time the thread
 input is determined, by default thread dispatch time. In the latter
 case, the value of the static data determines the current mode at
 the next dispatch. The effect is a possible change in mode-specific
 property values to reflect a change in source text internal
 execution behavior, e.g., a change in worst-case execution time,
 and in the entrypoint or call sequence to be executed at the next
 dispatch.

(7)  Change of a thread-internal mode may be explicitly modeled by
 declaring a mode transition that names an incoming thread port, an
 event raised within a thread (declared as **self**.eventname), or
 names a subprogram call with an outgoing event. Change of a
 thread-internal mode may also be modeled through the Behavior Model
 Annex Document D. In this case, mode transitions are tracked by the
 runtime system to determine the most recent current mode for the
 next dispatch of a thread.

(8)  If a higher fidelity behavioral model is desired, the Behavior
 Annex (Annex Document D), which uses the AADL modes as the initial
 set of states, can be used for more complex behavioral
 specifications. The final authority of the actual behavior of the
 source text is the program code itself.

 1. .. rubric:: System Synchronization Requirements
   :name: system-synchronization-requirements

(9)  An application system may consist of multiple threads. Each thread
 has its own hybrid automaton state with its own *c* and *t*
 variables. This results in a set of concurrent hybrid automata. In
 the concurrent hybrid automata model for the complete system, ST is
 a single real-valued variable shared by all threads that is never
 reset and whose rate is 1 in all states. ST is called the
 *reference timeline*.

(10) A set of periodic threads are said to be logically dispatched
 simultaneously at global real time *ST* if the order in which all
 exchanges of control and data at that dispatch event are identical
 to the order that would occur if those dispatches were exactly
 dispatched simultaneously in true and perfect real time. The
 *hyperperiod h* of a set of periodic threads is the next time
 *ST+h* at which they are logically dispatched simultaneously. The
 hyperperiod is the least common multiple of the periodic thread
 periods.

(11) An application system is said to be synchronized if the dispatch of
 all periodic threads contained in that application system occurs
 logically simultaneously at intervals of their hyperperiod. In a
 globally synchronous system ST is a global *reference time*, i.e.,
 a single real-valued variable representing a global clock. It
 represents a single global *synchronization domain*.

(12) Within a synchronization domain, perfect synchronization may not
 occur in a actual system.  There may always be clock error bounds
 in a distributed clock, and jitter in exactly when events (like a
 dispatch) would occur even with perfect clock interrupts due to
 things like non-preemptive blocking times (during which clock
 interrupts might be masked briefly).  Within a synchronization
 domain, it is the responsibility of each physical implementation to
 take these imperfections into account when providing the
 synchronization domain for programmers (e.g., make sure the message
 transmission schedule includes enough margin for the message to
 arrive at the destination by the time it is needed, taking into
 account these various effects in the particular implementation).

 1. .. rubric:: Asynchronous Systems
   :name: asynchronous-systems

(13) In a globally asynchronous system there are multiple *reference
 times*, i.e., multiple variables ST\ :sub:`j`. They represent
 different *synchronization domains*. Any time related coordination
 and communication between threads, processors, and devices across
 different synchronization domains must take into account
 differences in the *reference time* of each of those
 synchronization domains.

(14) Reference times in the form of a clock or time domain can be
 represented by instances of a user-defined abstract, processor, or
 device type. Characteristics of this reference time component, such
 as clock drift rate and maximum clock drift can be specified as
 properties of these instances. Processors, devices, buses, memory,
 virtual buses, virtual processors, and systems can be assigned
 different reference times through the Reference\_Time property.
 Similarly, application components can be assigned reference times
 to represent the fact that they may read the time, e.g., to
 timestamp data.The reference time for thread execution is
 determined by the reference time of the processor on which the
 thread executes. The reference time of communication between
 threads, devices, and processors is determined by the reference
 time of the source and destination, and the reference time of any
 execution platform component involved in the communication if it is
 time-driven.

(15) The reference time for thread execution is determined by the
 reference time of the processor on which the thread executes. The
 reference time of communication between threads, devices, and
 processors is determined by the reference time of the source and
 destination, and the reference time of any execution platform
 component involved in the communication if it is time-driven.

(16) Message-passing semantics of communication and thread execution is
 represented by aperiodic threads whose dispatch is triggered by
 arrival of messages and message may be queued in the event data
 port. This communication paradigm is insensitive to time, thus, not
 affected by multiple synchronization domains.

(17) Data-stream semantics of communication and thread execution are
 represented by periodic threads and data ports. In this case the
 sampling of the input is sensitive to a common reference time
 between the source and the destination thread if the connections
 are immediate and delayed to ensure deterministic communication.
 Deterministic communication minimizes latency jitter, while
 non-deterministic communication can result in latency jitter in
 units of the sampling rate, the latter often leading to instability
 of latency sensitive applications such as control systems. In the
 case of sampling data port connections the non-deterministic nature
 of sampling accommodates different reference times. Similarly, a
 periodic thread may non-deterministically sample event ports and
 event data ports, e.g., a health monitor sampling an alarm queue.

(18) The Allowed\_Connection\_Type property of a bus specifies the types
 of connections supported by a bus. Buses that connect processors
 with different reference times may exclude immediate and delayed
 connections from their support if determinism cannot be guaranteed
 through a protocol.

(19) Mode switching requires time-sensitive coordination of deactivation
 and activation of threads and connections. There is the time
 ordering of events that request mode switching, and the
 coordination of switching modes in multiple modal subsystems as
 part of a single mode switch. Timed coordination can be guaranteed
 within one synchronization domain and may be feasible across
 synchronization domains with bounded time drift through appropriate
 protocols.

(20) Solutions have been devised to address this issue.

-  ARINC653: Thread execution and communication within a partition is
   assumed to be within the same synchronization domain, cross-partition
   communication is assumed to be message-based or (phase-)delayed for
   sampling ports. This assures that placement of partitions on
   different processors or at different parts of the timeline within one
   processor does not affect the timing. However, this delayed
   communication places a synchronicity requirement on those partitions
   that communicate with each other.

-  Globally Asynchronous Locally Synchronous (GALS): This model reflects
   the fact that some systems cannot be globally synchronized, e.g.,
   integrated modular avionics (IMA) system may consist of a collection
   of ARINC653 subsystems and interact via an ARINC664 network. In this
   case the burden is placed on the application system to deal with
   synchronicity within subsystems and asynchronicity across subsystems.
   This can be reflected in AADL by multiple synchronization domains and
   the requirement that data port connections across synchronization
   domains are sampled connections.

-  Time Triggered Architecture (TTA): In this model a central
   communication medium provides a statically allocated time-division
   protocol and acts as a global reference time. Either part of the
   protocol provides reference time ticks to subsystems. Execution of
   subsystems can be aligned with the arrival of data at assigned time
   slots in the communication protocol to assure deterministic
   communication of data streams.

-  Physically Asynchronous Logically Synchronous (PALS): In this model a
   logical protocol or application layer provides coordination of
   time-sensitive events across asynchronous subsystems. For example,
   the system may periodically re-synchronize clocks, thus, bound clock
   drift. This clock drift bound may be accommodated by appropriate time
   slack the same way jitter in a synchronous system is accommodated.
   Similarly, hand-shaking protocols may be used to coordinate less
   frequently occurring synchronization events, e.g., globally
   synchronous mode switching if required.

   1. .. rubric:: Runtime Support For Threads
 :name: runtime-support-for-threads

 A standard set of runtime services are provided. The application
program interface for these services is defined in the applicable
source language annex of this standard. They are callable from
within the source text. The following subprograms may be explicitly
called by application source code, or they may be called by an AADL
runtime system that is generated from an AADL model.

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

Processing Requirements and Permissions

 Multiple models of implementation are permitted for the dispatching
of threads.

-  One such model is that a runtime executive contains the logic
   reflected in Figure 5 and calls on the different entrypoints
   associated with a thread. This model naturally supports source text
   in a higher level domain language.

-  An alternative model is that the code in the source text includes a
   code pattern that reflects the logic of Figure 5 through explicitly
   programmed calls to the standard Await\_Dispatch runtime service,
   including a repeated call (while loop) to reflect repeated dispatch
   of the compute entrypoint code sequence.

 Multiple models of implementation are permitted for the
implementation of thread entrypoints.

-  One such model is that each entrypoint is a possibly separate
   function in the source text that is called by the runtime executive.
   In this case, the logic to determine the context of an error is
   included in the runtime system.

-  A second model of implementation is that a single function in the
   source text is called for all entrypoints. This function then invokes
   an implementer-provided Dispatch\_Status runtime service call to
   identify the context of the call and to branch to the appropriate
   code sequence. This alternative is typically used in conjunction with
   the source text implementation of the dispatch loop for the compute
   entrypoint execution.

  A method of implementing a system is permitted to choose how
 executing threads will be scheduled. A method of implementation is
 required to verify to the required level of assurance that the
 resulting schedule satisfies the period and deadline properties.
 That is, a method of implementing a system should schedule all
 threads so that the specified timing constraints are always
 satisfied.

 The use of the term preempt to name a scheduling state transition
 in Figure 6 does not imply that preemptive scheduling disciplines
 must be used; non-preemptive disciplines are permitted.

  Execution times associated with transitions between thread
 scheduling states, for example context swap times (specified as
 properties of the hosting processor), are not billed to the
 thread’s actual execution time, i.e., are not reflected in the
 Compute Execution Time property value. However, these times must be
 included in a detailed schedulability model for a system. These
 times must either be apportioned to individual threads, or to
 anonymous threads that are introduced into the schedulability model
 to account for these overheads. A method of processing
 specifications is permitted to use larger compute time values than
 those specified for a thread in order to account for these
 overheads when constructing or analyzing a system.

  A method of implementing a system must support the periodic
 dispatch protocol. A method of implementation may support only a
 subset of the other standard dispatch protocols. A method of
 implementation may support additional dispatch protocols not
 defined in this standard.

(5)  A method of implementing a system may perform loading and
 initialization activities prior to the start of system operation.
 For example, binary images of processes and initial data values may
 be loaded by permanently storing them in programmable memory prior
 to system operation.

(6)  A method of implementing a system must specify the set of errors
 that may be detected at runtime. This set must be exhaustively and
 exclusively divided into those errors that are thread recoverable
 or thread unrecoverable, and those that are exceptions to be
 handled by language constructs defined in the applicable
 programming language standard. The set of errors classified as
 source language exceptions may be a subset of the exceptions
 defined in the applicable source language standard. That is, a
 method of implementation may dictate that a language-defined
 exceptional condition should not cause a runtime source language
 exception but instead immediately result in an error. For each
 error that is treated as a source language exception, if the source
 text associated with that thread fails to properly catch and handle
 that exception, a method of implementation must specify whether
 such unhandled exceptions are thread recoverable or thread
 unrecoverable errors.

(7)  A consequence of the above permissions is that a method of
 implementing a system may classify all errors as thread
 unrecoverable, and may not provide an executing recovery scheduling
 state and transitions to and from it.

(8)  A method of implementing a system may enforce, at runtime, a
 minimum time interval between dispatches of sporadic threads. A
 method of implementing a system may enforce, at runtime, the
 minimum and maximum specified execution times. A method of
 implementing a system may detect at runtime timing violations.

(9)  A method of implementing a system may support handling of errors
 that are detected while a thread is in the suspended, ready, or
 blocked state. For example, a method of implementation may detect
 event arrivals for a sporadic thread that violate the specified
 period. Such errors are to be kept pending until the thread enters
 the executing state, at which instant the errors are raised for
 that thread and cause it to immediately enter the recover state.

(10) If alternative thread scheduling semantics are used, a thread
 unrecoverable error that occurs in the perform thread
 initialization state may result in a transition to the perform
 thread recovery state and thence to the suspended awaiting mode
 state, rather than to the thread halted state. The deadline for
 this sequence is the sum of the initialization deadline and the
 recovery deadline.

(11) If alternative thread scheduling semantics are used, a method of
 implementation may prematurely terminate threads when a system mode
 change occurs that does not contain them, instead of entering
 suspended awaiting mode. Any blocking resources acquired by the
 thread must be released.

(12) If alternative thread scheduling semantics are used, the load
 deadline and initialize deadline may be greater than the period for
 a thread. In this case, dispatches of periodic threads shall not
 occur at any dispatch time prior to the initialization deadline for
 that periodic thread.

(13) This standard does not specify which thread or threads perform
 virtual address space loading. This may be a thread in the runtime
 system or one of the application threads.

NOTE: The deadline of a calling thread will impose an end-to-end
deadline on all activities performed by or on behalf of that thread,
including the time required to perform any remote subprogram calls made
by that thread. The deadline property of a remotely called subprogram
may be useful for scheduling methods that assign intermediate deadlines
in the course of producing an overall end-to-end system schedule.

Thread Groups
-------------

 A thread group represents an organizational component to logically
group threads contained in processes. The type of a thread group
component specifies the features and required subcomponent access
through which threads contained in a thread group interact with
components outside the thread group. Thread group implementations
represent the contained threads and their connectivity. Thread
groups can have multiple modes, each representing a possibly
different configuration of subcomponents, their connections, and
mode-specific property associations. Thread groups can be
hierarchically nested.

 A thread group does not represent a virtual address space nor does
it represent a unit of execution. Therefore, a thread group must be
directly or indirectly contained within a process.

Legality Rules

+--------------------+---------------------------------------+-----------------------+
| **Category**   | **Type**  | **Implementation**|
+--------------------+---------------------------------------+-----------------------+
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
+--------------------+---------------------------------------+-----------------------+

1. A thread group component type can contain provides and requires data
   access, as well as port, feature group, provides and requires
   subprogram access declarations, and provides and requires
   subprogram group access declarations. It can also contain flow
   specifications, modes subclauses, and property associations.

1. A thread group component implementation can contain abstract, data,
   subprogram, subprogram group, thread, and thread group
   subcomponent declarations.

2. A thread group implementation can contain a connections subclause, a
   flows subclause, a modes subclause, and properties subclause.

3. A thread group must not contain a subprogram calls subclause.

Standard Properties

-- Properties related to source text

Source\_Text: **inherit list of aadlstring**

-- Inheritable thread properties

Synchronized Component: **inherit** **aadlboolean** => **true**

Active Thread Handling Protocol:

**inherit** Supported Active Thread Handling Protocols => abort

Period: **inherit** Time

Deadline: **inherit** Time => Period

Dispatch Offset: **inherit** Time

First Dispatch Time **: inherit** Time

-- Scheduling properties

Priority: **inherit** **aadlinteger**

Time Slot: **list of aadlinteger **

Criticality: **aadlinteger**

-- execution time related properties

Reference Processor: **inherit classifier** ( processor )

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

-- startup properties

Startup Deadline: Time

Startup Execution Time: Time Range

-- Properties specifying constraints for processor and memory binding

Allowed Processor Binding Class:

**inherit** **list** **of** **classifier** (processor, virtual
processor, device, system)

Allowed Processor Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, device, system)

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Processor Binding: **inherit** **list of** **reference**
(processor, virtual processor, device, system)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

Allowed Connection Binding Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

Allowed Connection Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, bus, virtual bus, device, memory, system)

Actual Connection Binding: **inherit list of** **reference** (processor,
virtual processor, bus, virtual bus, device, system, memory)

NOTE: Property associations of thread groups are inheritable (see
Section 11.3) by contained subcomponents. This means if a contained
thread does not have a property value defined for a particular property,
then the corresponding property value for the thread group is used.

Semantics

 A thread group allows threads contained in processes to be logically
organized into a hierarchy. A thread group type declares the
features and required subcomponent access through which threads
contained in a thread group can interact with components declared
outside the thread group.

Thread groups may contain subprogram subcomponents and subprogram
groups The code of such subprograms and subprogram groups resides in
the address space of the containing process. The subprograms may be
called by threads contained in the thread group. The subprograms may
also be called from outside the thread group if made accessible
through a provides subprogram access declaration or subprogram group
access declaration.

 Thread groups may contain data components. They represent state that
may be shared between threads inside the thread group through access
connections to the requires data access features of those threads,
and shared outside the thread group through provides data access
features of the thread group.

 A thread group implementation contains threads and thread groups.
Thread group nesting permits threads to be organized hierarchically.
A thread group implementation also contains connections to specify
the interactions between the contained subcomponents and modes to
represent different configurations of subsets of subcomponents and
connections as well as mode-specific property associations.

1. .. rubric:: Processes
  :name: processes

 A process represents a virtual address space, i.e., it represents a
space partition unit whose boundaries are enforced at runtime. The
Runtime\_Protection process property indicates whether runtime
protection is disabled. The virtual address space contains the
program formed by the source text associated with the process and
its subcomponents. Threads of a process must be explicitly declared.

Legality Rules

+----------------+---------------------------------------+-----------------------+
| **Category**   | **Type**  | **Implementation**|
+----------------+---------------------------------------+-----------------------+
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
+----------------+---------------------------------------+-----------------------+

1. A process component type can contain port, feature group, provides
   and requires data access, provides and requires subprogram access
   declarations, and provides and requires subprogram group access
   declarations. It can also contain flow specifications, modes
   subclause, and property associations.

1. A process component implementation can contain abstract, data,
   subprogram, subprogram group, thread, and thread group
   subcomponent declarations.

2. A process implementation can contain a connections subclause, a flows
   subclause, a modes subclause, and a properties subclause.

3. A thread group must not contain a subprogram calls subclause.

Consistency Rules

1. The complete source text associated with a process component must
   form a complete and legal program as defined in the applicable source
   language standard. This source text shall include the source text
   that corresponds to the complete set of subcomponents in the
   process’s containment hierarchy along with the data and subprograms
   that are referenced by required subcomponent declarations.

Standard Properties

-- Runtime enforcement of virtual address space boundary

Runtime Protection : **inherit** **aadlboolean**

-- Properties related to source text

Source Text: **inherit list of aadlstring**

Source Language: **inherit list of** Supported\_Source\_Languages

-- Properties related to virtual address space loading

Load Time: Time\_Range

Load Deadline: Time

-- Inheritable thread properties

Synchronized\_Component: **inherit** **aadlboolean** => **true**

Active Thread Handling Protocol:

**inherit** Supported Active Thread Handling Protocols => abort

Period: **inherit** Time

Deadline: **inherit** Time => Period

Dispatch Offset: **inherit** Time

-- execution time related properties

Reference Processor: **inherit classifier** ( processor )

-- Scheduling related properties

Priority: **inherit** **aadlinteger**

-- mode related properties

Resumption Policy: **enumeration** ( restart, resume )

Deactivation Policy: **enumeration** (inactive, unload) => inactive

-- process initialization

Startup Deadline: Time

Startup Execution Time: Time Range

-- Properties specifying constraints memory binding

Allowed Processor Binding\_Class:

**inherit** **list** **of** **classifier** (processor, virtual
processor, device, system)

Allowed Processor Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, device, system)

Actual Processor Binding: **inherit** **list of** **reference**
(processor, virtual processor, device, system)

Allowed Connection Binding Class:

**inherit** **list** **of** **classifier**\ (processor, virtual
processor, bus, virtual bus, device, memory, system)

Allowed Connection Binding: **inherit** **list** **of** **reference**
(processor, virtual processor, bus, virtual bus, device, memory, system)

Actual Connection Binding: **inherit list of** **reference** (processor,
virtual processor, bus, virtual bus, device, system, memory)

Allowed Memory Binding Class:

**inherit** **list** **of** **classifier** (memory, system, processor,
virtual processor)

Allowed Memory Binding: **inherit list** **of** **reference** (memory,
system, processor, virtual processor)

Actual Memory Binding: **inherit** **list of** **reference** (memory,
system, processor, virtual processor)

Semantics

 Every process has its own virtual address space. This address space
provides access to source code and data associated with the process
and all its contained components. This address space boundary is by
default enforced at runtime, but can be disabled through the
Runtime\_Protection property.

Threads contained in a process execute within the virtual address
space of the process.

 Processes may contain subprogram subcomponents. The code of such
subprograms resides in the address space of the process. The calling
semantics to such subprograms are defined in Section 5.2.

 A process may contain mode declarations. In this case, each mode can
represent a different configuration of contained threads, their
connections, and mode-specific property associations. The transition
between modes is determined by the mode transition declarations and
is triggered by the arrival of *mode transition trigger events* (see
Sections 12 and 13.6).

(5) The associated source text for each process is compiled and linked
to form binary images in accordance with the applicable source
language standard. These binary images must be loaded into memory
before any thread contained in a process can execute, i.e., enter
its *perform thread initialization* state.

(6) The time to load binary images into the virtual address space of a
process is bounded by the Load\_Deadline and Load\_Time properties.
The failure to meet these timing requirements is considered an
error.

(7) The process states, transitions, and actions are illustrated in
Figure 8. Once a processor of an execution platform is started,
binary images making up the virtual address space of processes bound
to the processor are ready to be loaded, which is indicated by
**started(processor)**. If the process is bound to a virtual
processor of a processor, then process loading begins when the
virtual processor is started, which is indicated by
**started(vprocessor)**. Loading may take zero time for binary
images that have been preloaded in ROM, PROM, or EPROM. Completion
of loading, which is indicated by **loaded(process)**, triggers
threads to be initialized (see Figure 5).

(8) A process, i.e., its contained threads, can be stopped (also known
as a deferred abort), which is indicated by **stop(process)** , or
by stopping the virtual processor or processor to which the process
is bound. A process is considered stopped when all threads of the
process are halted, are awaiting a dispatch, or are not part of the
current mode and have executed their finalize entrypoint.

(9) A process, i.e., its contained threads, can be aborted, which is
indicated by **abort(process)**. In this case, all contained threads
terminate their execution immediately and release any resources (see
Figure 5, Figure 6, and Figure 7).

Figure − Process States and Actions
   

Processing Requirements and Permissions

 A method of implementation must link all associated source text for
the complete set of subcomponents of a process, including the
process component itself and all actual subcomponents specified for
required subcomponents. This set of source compilation units must
form a single complete and legal program as defined by the
applicable source language standard. Linking of this set of source
compilation units is performed in accordance with the applicable
source language standard for the process.

If the applicable source language standard used to implement a
component permits a mixture of source languages, then subcomponents
may have different source language property values.

 This standard permits dynamic virtual memory management or dynamic
library linking after process loading has completed and thread
execution has started. However, a method for implementing a system
must assure that all deadline properties will be satisfied to the
required level of assurance for each thread.

NOTE: An AADL process represents only a virtual address space and
requires at least one explicitly declared thread subcomponent in order
to be executable. The explicitly declared thread in AADL allows for
unambiguous specification of port connections to threads. In contrast, a
POSIX process represents both a protected address space and an implicit
thread.

