RATIONALE
=========

This Architecture Analysis & Design Language (AADL) standard document
was prepared by the SAE AS-2C Architecture Description Language
Subcommittee, Embedded Computing Systems Committee, Aerospace Avionics
Systems Division.

The language was originally published as SAE AS5506 in 2004. The
language has been refined and extended based on industrial experience as
AADL V2 and published as AS5506A in 2009. The improvements focused on
better support for architecture templates and modeling of layered and
partitioned architectures. AADL V2.1, a revision that addresses a number
of errata that have been reported and agreed upon by the committee, was
published as AS5506B in 2012. This document AS5506C documents AADL V2.2,
a second revision that addresses a number of additional errata and minor
improvements to the language since AS5506B. These errata and changes
have been approved by the committee.

The committee has started work on a major revision AADL V3 based on
industrial experience, using AADL V2.2 as baseline. That revision will
introduce new concepts and has a with a publication target date of
2018/19.


Foreword
========

The AADL standard was prepared by the SAE Avionics Systems Division
(ASD) Embedded Computing Systems Committee (AS-2) Architecture
Description Language (AS-2C) subcommittee.

This standard addresses the requirements defined in SAE ARD5296,
Requirements for the Avionics Architecture Description
Language [1]_.

The AADL standard consists of a core language standard that is
defined in this document and a collection of standardized property
sets and/or sublanguages that are defined in annex documents. The
core language standard provides full support for modeling the
application task and communication architecture, the hardware
platform, and the physical environment of embedded
software-intensive systems, including standardized predeclared
properties to characterize task execution and communication timing,
as well as deployment of the application on the hardware platform.
The standardized extensions allow core AADL models to be annotated
with information that is not represented by the core language to
meet specific embedded system analysis needs such as security
analysis, dependability analysis, and behavioral analysis, and
support for automated generation and integration of systems.

The starting point for the AADL standard development was MetaH, an
architecture description language and supporting toolset, developed
at Honeywell Technology Laboratories under the Defense Advanced
Research Research Projects Agency (DARPA) and Army Aviation and
Missile Command (AMCOM) sponsorship.

The AADL standard has been designed to be compatible with real-time
operating system standards such as POSIX and ARINC 653.

The AADL standard is aligned with Object Management Group (OMG)
Unified Modeling Language (UML) and Modeling and Analysis of
Real-Time Embedded systems (MARTE) through a standardized profile
for AADL.

The AADL standard includes a specification of an AADL-specific XML
interchange format.

The AADL standard provides guidelines for users to transition
between AADL models and program source text written in Ada (ISO/IEC
8652/2007 (E) Ed.3) and C (ISO/IEC 9899:1999).

Introduction
============

The SAE Architecture Analysis & Design Language (referred to in this
document as AADL) is a textual and graphical language used to design
and analyze the software and hardware architecture of
performance-critical real-time systems. These are systems whose
operation strongly depends on meeting non-functional system
requirements such as reliability, availability, timing,
responsiveness, throughput, safety, and security. AADL is used to
describe the structure of such systems as an assembly of software
components mapped onto an execution platform. It can be used to
describe functional interfaces to components (such as data inputs
and outputs) and performance-critical aspects of components (such as
timing). AADL can also be used to describe how components interact,
such as how data inputs and outputs are connected or how application
software components are allocated to execution platform components.
The language can also be used to describe the dynamic behavior of
the runtime architecture by providing support to model operational
modes and mode transitions. The language is designed to be
extensible to accommodate analyses of the runtime architectures that
the core language does not completely support. Extensions can take
the form of new properties and analysis specific notations that can
be associated with components and are standardized themselves.

AADL was developed to meet the special needs of performance-critical
real-time systems, including embedded real-time systems such as
avionics, automotive electronics, or robotics systems. The language
can describe important performance-critical aspects such as timing
requirements, fault and error behaviors, time and space
partitioning, and safety and certification properties. Such a
description allows a system designer to perform analyses of the
composed components and systems such as system schedulability,
sizing analysis, and safety analysis. From these analyses, the
designer can evaluate architectural tradeoffs and changes.

Since AADL supports multiple and extensible analysis approaches, it
provides the ability to analyze the cross cutting impacts of change
in the architecture in one specification using a variety of analysis
tools. AADL is designed to be used with analysis tools that support
the automatic generation of the source code needed to integrate the
system components and build a system executive. Since the models and
the architecture specification drive the design and implementation,
they can be maintained to permit model driven architecture based
changes throughout the system lifecycle.

Information and Feedback
========================

 The website at http://www.aadl.info is an information source
regarding the SAE AADL standard. It makes available papers on AADL,
its benefits, and its use. Also available are papers on MetaH, the
technology that demonstrated the practicality of a model-based
system engineering approach based on architecture description
languages for embedded real-time systems.

 The website provides links to three SAE AADL related discussion
forums:

-  The SAE AADL User Forum to ask questions and share experiences about
   modeling with SAE AADL,

-  The AADL Toolset User Forum to ask questions and share experiences
   with the Open Source AADL Tool Environment, (OSATE) and

-  The SAE Standard Document Corrections & Improvements Forum that
   records errata, corrections, and improvements to the current release
   of the SAE AADL standard.

The website provides information and a download site for the Open
Source AADL Tool Environment. It also provides links to other
resources regarding the AADL standard and its use.

Questions and inquiries regarding working versions of annexes and
future versions of the standard can be addressed to info@aadl.info.

Informal comments on this standard may be sent via e-mail to
errata@aadl.info. If appropriate, the defect correction procedure
will be initiated. Comments should use the following format:

!topic Title summarizing comment

!reference AADL-ss.ss(pp)

!from Author Name yy-mm-dd

!keywords keywords related to topic

!discussion

text of discussion

 where ss.ss is the section, clause or subclause number, pp is the
paragraph or line number where applicable, and yy-mm-dd is the date
the comment was sent. The date is optional, as is the !keywords
line.

Multiple comments per e-mail message are acceptable. Please use a
descriptive Subject in your e-mail message.

 When correcting typographical errors or making minor wording
suggestions, please put the correction directly as the topic of the
comment; use square brackets [ ] to indicate text to be omitted and
curly braces { } to indicate text to be added, and provide enough
context to make the nature of the suggestion self-evident or put
additional information in the body of the comment, for example:

!topic [c]{C}haracter

!topic it[']s meaning is not defined

Scope
=====

 This standard defines a language for describing both the software
architecture and the execution platform architectures of
performance-critical, embedded, real-time systems; the language is
known as the SAE AADL. An AADL model describes a system as a
hierarchy of components with their interfaces and their
interconnections. Properties are associated to these constructions.
AADL components fall into two major categories: those that represent
the physical hardware and those representing the application
software. The former is typified by processors, buses, memory, and
devices, the latter by application software functions, data,
threads, and processes. The model describes how these components
interact and are integrated to form complete systems. It describes
both functional interfaces and aspects critical for performance of
individual components and assemblies of components. The changes to
the runtime architecture are modeled as operational modes and mode
transitions.

 The language is applicable to systems that are:

-  real-time,

-  resource-constrained,

-  safety-critical systems,

-  and those that may include specialized device hardware.

This standard defines the core AADL that is designed to be
extensible. While the core language provides a number of modeling
concepts with precise semantics including the mapping to execution
platforms and the specification of execution time behavior, it is
not possible to foresee all possible architecture analyses.
Extensions to accommodate new analyses and unique hardware
attributes take the form of new properties and analysis specific
notations that can be associated with components. Users or tool
vendors may define these extensions. Extensions may be proposed as
annex documents for inclusion in the AADL standard.

This standard does not specify how the detailed design or
implementation details of software and hardware components are to be
specified. Those details can be specified by a variety of software
programming and hardware description languages. The standard
specifies relevant characteristics of the detailed design and
implementation descriptions, such as source text written in a
programming language or hardware description language, from an
external (black box) perspective. These relevant characteristics are
specified as AADL component properties, and as rules of conformance
between the properties and the described components.

This standard does not prescribe any particular system integration
technologies, such as operating system or middleware application
program interfaces or bus technologies or topologies. However,
specific system architecture topologies, such as the ARINC 653
executives, can be modeled through software and execution platform
components. AADL can be used to describe a variety of hardware
architectures and software infrastructures. Integration technologies
can be used to implement a specified system. The standard specifies
rules of conformance between AADL system architecture specifications
and actual system implementations.

The standard was not designed around a particular set of tools. It
is anticipated that systems and software tools will be provided to
support the use of AADL.

Purpose/Extent
--------------
 
The purpose of AADL is to provide a standard and sufficiently
precise (machine-processable) way of modeling the architecture of an
embedded, real-time system, such as an avionics system or automotive
control system, to permit analysis of its properties, and to support
the predictable integration of its implementation. Defining a
standard way to describe system components, interfaces, and
assemblies of components facilitates the exchange of engineering
data between the multiple organizations and technical disciplines
that are invariably involved in an embedded real-time system
development effort. A precise and machine-processable way to
describe conceptual and runtime architectures provides a framework
for system modeling and analysis; facilitates the automation of code
generation, system build, and other development activities; and
significantly reduces design and implementation defects.

AADL describes application software and execution platform
components of a system, and the way in which components are
assembled to form a complete system or subsystem. The language
addresses the needs of system developers in that it can describe
common functional (control and data flow) interfacing idioms as well
as performance-critical aspects relating to timing, resource
allocation, fault-tolerance, safety and certification.

AADL describes functional interfaces and non-functional properties
of application software and execution platform components. The
language is not suited for detailed design or implementation of
components. AADL may be used in conjunction with existing standard
languages in these areas. AADL describes interfaces and properties
of execution platform components including processor, memory,
communication channels, and devices interfacing with the external
environment. Detailed designs for such hardware components may be
specified by associating source text written in a hardware
description language such as VHDL [2]_. AADL can describe interfaces
and properties of application software components implemented in
source text, such as threads, processes, and runtime configurations.
Detailed designs and implementations of algorithms for such
components may be specified by associating source text written in a
software programming language such as Ada or C, or domain-specific
modeling languages such as MatLab\ :sup:`®`/Simulink:sup:`®`\  [3]_.

AADL describes how components are composed together and how they
interact to form complete system architectures. Runtime semantics of
these components are specified in this standard. Various mechanisms
are available to exchange control and data between components,
including message passing, event passing, synchronized access to
shared components, and remote procedure calls. Thread scheduling
protocols and timing requirements may be specified. Dynamic
reconfiguration of the runtime architecture may be specified through
operational modes and mode transitions. The language does not
require the use of any specific hardware architecture or any
specific runtime software infrastructure.

Rules of conformance are specified between specifications written in
AADL, source text and physical components described by those
specifications, and physical systems constructed from those
specifications. The AADL is not intended to describe all possible
aspects of any possible component or system; selected syntactic and
semantic requirements are imposed on components and systems. Many of
the attributes of an AADL component are represented in an AADL model
as properties of that component. The conformance rules of the
language include the characteristics described by these properties
as well as the syntactic and semantic requirements imposed on
components and systems. Compliance between AADL specifications and
items described by specifications is determined through analysis,
e.g., by tools for source text processing and system integration.

AADL can be used for multiple activities in multiple development
phases, beginning with preliminary system design. The language can
be used by multiple tools to automate various levels of modeling,
analysis, implementation, integration, verification and
certification.

 
Field of Application
--------------------
 
AADL was developed to model embedded systems that have challenging
resource (size, weight, power) constraints and strict real-time
response requirements. Such systems should tolerate faults and may
utilize specialized hardware such as I/O devices. These systems are
often certified to high levels of assurance. Intended fields of
application include avionics systems, automotive systems, flight
management systems, engine and power train control systems, medical
devices, industrial process control equipment, robotics, and space
applications. AADL may be extended to support other applications as
the need arises.

Structure of Document
---------------------

   1. .. rubric:: A Reader's Guide
 :name: a-readers-guide

As necessary, the term AADL V2 will be used to refer to the revised
version of AADL defined in this document.

The AADL standard consists of this core language document and a set
of annex documents of standardized extensions. This core language
document contains a number of sections and appendices. The sections
define the core AADL. The appendices provide additional information,
both normative and informative about the core language. Annex
documents introduce additional standardized properties and possibly
language extensions in the form of specialized notations.

AADL concepts are introduced in Section 3, Architecture Analysis &
Design Language Summary. They are defined with full syntactic and
semantic descriptions as well as naming and legality rules in
succeeding sections. The vocabulary and symbols of AADL are defined
in Section 15. Appendix B , Glossary, provides informative
definitions of terms used in this document. Other appendices include
a syntax Summary and Predeclared Property Sets. The remainder of
this section introduces notations used in this document and
discusses standard conformance.

This core AADL document consists of the following:

-  Section 2, References, provides normative and applicable references
   as well as terms and definitions.

-  Section 3, Architecture Analysis & Design Language Summary,
   introduces and defines the concepts of the language.

-  Section 4, Components, Packages, and Annexes, defines the common
   aspects of components, which are the design elements of AADL, as well
   as component template parameterization. It also introduces the
   package, which allows organization of the design elements in the
   design space. This section closes with a description of annex
   subclauses and libraries as annex-specific notational extensions to
   the core AADL.

The next sections introduce the language elements for modeling
application and execution platform components in modeled systems or
systems of systems.

-  Section 5, Software Components, defines those modeling elements of
   AADL that represent application system software components, i.e.,
   data, subprogram, subprogram group, thread, thread group, and
   process.

-  Section 6, Execution Platform Components, defines those modeling
   elements of AADL that model execution platform components, i.e.,
   processor, virtual processor, memory, bus, virtual bus, and device.

-  Section 7, System Composition, defines system as a compositional
   modeling element that combines execution platform and application
   system software components.

-  Section 8, Features and Shared Access, defines the features of
   components that are connection points with other components. These
   consist of ports, subprogram parameters, provided and required access
   to data, subprograms, and buses, as well as grouping of features into
   feature groups.

-  Section 9, Connections , defines the constructs to express
   interaction between components in terms of connections between
   component features.

-  Section 10, Flows, defines the constructs to express flows through a
   sequence of components, and connections.

-  Section 11, Properties, defines the AADL concept of properties
   including property sets, property value association, property type,
   and property declaration. Property associations and property
   expressions are used to specify values. Property set, property type,
   and property name declarations are used to extend AADL with new
   properties.

-  Section 12, Modes, defines modes and mode transitions to support
   modeling of operational modes with mode-specific system
   configurations and property values.

-  Section 13, Operational System, defines the concepts of system
   instance and binding of application software to execution platforms.
   This section defines the execution semantics of the operational
   system including the semantics of system-wide mode switches.

-  Section 14, Layered System Architectures, defines support for
   modeling layered architectures.

Section 15, Lexical Elements, defines the basic vocabulary of the
language. As defined in this section, identifiers in AADL are case
insensitive. Identifiers differing only in the use of corresponding
upper and lower case letters are considered as the same. Similarly,
reserved words in AADL are case insensitive.

The following Appendix sections complete the definition of the core
AADL.

-  Appendix A Predeclared Property Sets, contains the standard AADL set
   of predeclared properties, property types, and property constants.

-  Appendix B Glossary, contains a glossary of terms.

-  Appendix C Syntax Summary, contains a summary of the syntax as
   defined in the sections of this document.

-  Appendix D Graphical Aadl Notation, defines a graphical
   representation of AADL.

-  Appendix E AADL Meta Model And Xml Specification, defines an
   XML-based interchange format in form of an XMI meta model and an XML
   schema.

-  Appendix F Unified Modeling Language (UML) Profile, defines a profile
   for UML that extends and tailors UML to support modeling in terms of
   AADL concepts. This profile is defined in the context of the Object
   Management Group (OMG) Modeling and Analysis of Real-Time Embedded
   systems (MARTE).

-  Appendix G Profiles And Extensions, contains profiles and extensions
   that have been approved by the standards body.

The annex documents introduce additions and extensions to the core
AADL.

-  Annex Document A Code Generation, provides guidance for automatic
   generation and integration of runtime systems and application code in
   different implementation languages. It defines a standardized set of
   properties for recording mappings from the AADL model to source text
   and for automatic code generation.

-  Annex Document B Data Modeling, provides guidance on data modeling
   and how to map relevant data modeling information into an AADL model
   if desirable. It defines a standardized set of properties and basic
   data types in support of data modeling.

-  Annex Document C Error Model, defines a standardized core language
   extension in the form of a sublanguage notation and properties the
   component to support annotating AADL models with safety-criticality
   and dependability related information of a system.

-  Annex Document D Behavior Model, defines a standardized core language
   extension in the form of a sublanguage notation to specify the
   behavior of AADL components as AADL model annotations.

The core language and the annex documents are *normative*, except
that the material in each of the items listed below is informative:

-  Text under a NOTE or Examples heading.

-  Each clause or subclause whose title starts with the word Example''
   or Examples''.

All implementations shall conform to the core language. In addition,
an implementation may conform separately to one or more Annexes that
represent extensions to the core language.

The following appendices and annexes are informative and do not form
a part of the formal specification of AADL:

-  Appendix B Glossary

-  Appendix C Syntax Summary

-  Appendix G Profiles And Extensions

   1. .. rubric:: Structure of Clauses and Subclauses
 :name: structure-of-clauses-and-subclauses

 Each section of the core standard is divided into clauses and
subclauses that have a common structure. Each section, clause, and
subclause first introduces its subject and then presents the
remaining text in the following format. Not all headings are
required in a particular clause or subclause. Headings will be
centered and formatted as shown below.

All paragraphs are numbered with numbering restarting with each
section. Naming rules, legality rules, and consistency rules have
their own paragraph numbering also restarting with each section.
They can be identified by section number and paragraph number.

Syntax

 Syntax rules, concerned with the organization of the symbols in the
AADL expressions, are given in a variant of Backus-Naur-Form (BNF)
that is described in detail in Section 1.5.

Naming Rules

 *Naming rules* define rules for names that represent defining
identifiers and references to previously defined identifiers. Each
rule is labeled by (N#), where # is a natural number restarting with
1 for each section.

Legality Rules

 *Legality rules* define semantic restrictions on AADL
specifications. Legality rules must be validated by AADL processing
tools when a model is loaded into the tool. Each rule is labeled by
(L#), where # is a natural number restarting with 1 for each
section.

Consistency Rules

 *Consistency rules* define consistency restrictions on system
instances. A consistency rule must be validated by AADL processing
tools upon a user request or when an analysis method that relies on
the rule is invoked. Each rule is labeled by (C#), where # is a
natural number restarting with 1 for each section.

Standard Properties

 *Standard properties* define the properties that are defined within
this standard for various categories of components. The listed
properties are fully described in Appendix A .

Semantics

 *Semantics* describes the static and dynamic meanings of different
AADL constructs with respect to the system they model. The semantics
are concerned with the effects of the execution of the constructs,
not how they would be specifically executed in a computational tool.

Runtime Support

 AADL concepts may require runtime support through an operating
system or other runtime system on a processor. Such service calls
may be programmed explicitly in the application source code, or may
be part of a runtime system generated from an AADL specification.
The Code Generation Annex provides guidance on the use of these
runtime services by application code or the runtime system.

Processing Requirements and Permissions

 AADL specifications may be processed manually or by tools for
analysis and generation. This section documents additional
requirements and permissions for determining compliance. Providers
of processing method implementations must document a list of those
capabilities they support and those they do not support.

NOTE: Notes emphasize consequences of the rules described in the
(sub)clause or elsewhere. This material is informative.

Examples

 Examples illustrate the possible forms of the constructs described.
This material is informative\ *. *

1. .. rubric:: Error, Exception, Anomaly and Compliance
  :name: error-exception-anomaly-and-compliance

 AADL can be used to specify dependable systems. A system can be
compliant with its specification and this standard even when that
system contains failed components that no longer satisfy their
specifications. This section defines the terms fault, error,
exception, anomaly and noncompliance [ISO/IEC/IEEE 24765 2010]; and
defines how those terms apply to AADL specifications, physical
components (implementations), models of components, and tools that
accept AADL specifications as inputs.

  A *fault* is defined to be an anomalous undesired change in
 execution behavior, possibly resulting from an anomalous undesired
 change in data being accessed by a thread or from violation of a
 compute time or deadline constraint. A fault in a physical
 component is a root cause that may eventually lead to a component
 error or failure. A fault is often a specific event such as a
 transistor burning out or a programmer making a coding mistake.

 An *error* in a physical component occurs when an existing fault
 causes the internal state of the component to deviate from its
 nominal or desired operation. For example, a component error may
 occur when an add instruction produces an incorrect result because
 a transistor in the adding circuitry is faulty.

  A *failure* in a physical component occurs when an error manifests
 itself at the component interface. A component fails when it does
 not perform its nominal function for the other parts of the system
 that depend on that component for their nominal operation.

  A component failure may be a fault within a system that contains
 that component. Thus, the sequence of fault, error, failure may
 repeat itself within a hierarchically structured system. *Error
 propagation* occurs when a failed component causes the containing
 system or another dependent component to become erroneous.

(5)  A component may persist in a faulty state for some period of time
 before an error occurs. This is called *fault latency*. A component
 may persist in an erroneous state for some period of time before a
 failure occurs. This is called *error latency*.

(6)  An *exception* represents a kind of exceptional situation; it may
 occur for an erroneous or failed component when that error or
 failure is detected, either by the component itself or another
 component with which it interfaces. For example, a fault in a
 software component that eventually results in a divide-by-zero may
 be detected by the processor component on which it depends. An
 exception is always associated with a specific component. This
 document defines a standard model for exceptions for certain kinds
 of components (e.g., defines standard recovery sequences and
 standard exception events).

(7)  An *anomaly* occurs when a component is in an erroneous or failed
 state that does not result in a standard exception. Undetected
 errors may occur in systems. A detected error may be handled using
 mechanisms other than the standard exception mechanisms. For
 example, an error may propagate to multiple components before it is
 detected and mitigated. This standard defines nominal and
 exceptional behaviors for components. Anomalies are any other
 undefined erroneous component behaviors, which are nevertheless
 considered compliant with this standard.

(8)  An AADL specification is *compliant* with the AADL core language
 standard if it satisfies all the syntactic and legality rules
 defined in Sections 4 - 15. An AADL specification is compliant with
 an AADL Annex standard if it satisfies all the syntactic and
 legality rules defined in the respective normative Annex.

(9)  A component or system is *compliant* with an AADL specification of
 that component or system if the nominal and exceptional behaviors
 of that component or system satisfy the applicable semantics of the
 AADL specification, as defined by the semantic rules in this
 standard. A component or system may be a physical implementation
 (e.g., a piece of hardware), or may be a model (e.g., a simulation
 or analytic model). A model component or system may exhibit only
 partial semantics (e.g., a schedulability model only exhibits
 temporal semantics). Physical components and systems must exhibit
 all specified semantics, except as permitted by this standard.

(10) *Noncompliance* of a component with its specification is a kind of
 design fault. This may be handled by run-time fault-tolerance in an
 implemented actual system. A developer is permitted to classify
 such components as anomalous rather than noncompliant.

(11) A tool that operates on AADL specifications is *compliant* with the
 core language standard if the tool checks for compliance of input
 specifications with the syntactic and legality rules defined
 herein, except where explicit permission is given to omit a check;
 and if all physical or model components or systems generated by the
 tool are compliant with the specifications used to generate those
 components or systems. The AADL standard allows profiles of
 language subsets to be defined and requires a minimum subset of the
 language to be supported (see Appendix G ). A tool must clearly
 specify any portion of the language not supported and warn the user
 if a specification contains unsupported language constructs, when
 appropriate. A tool is compliant with the XMI interchange format if
 it supports saving and reading of AADL model in the XMI interchange
 format. A tool is compliant with a language extension annex if the
 tool checks for compliance of input specifications with the
 syntactic and legality rules defined in the respective annex
 document.

(12) Compliance of an AADL specification with the syntactic and legality
 rules can be automatically checked, with the exception of a few
 legality rules that are not in general tractably checkable for all
 specifications. Compliance of a component or system with its
 specification, and compliance of a tool with this standard, cannot
 in general be fully automatically checked. A verification process
 that assures compliance to the degree required for a particular
 purpose must be used to perform the latter two kinds of compliance
 checking.

 1. .. rubric:: Method of Description and Syntax Notation
   :name: method-of-description-and-syntax-notation

 The language is described by means of a context-free syntax together
with context-dependent requirements expressed by narrative rules.
The meaning of a construct in the language is defined by means of
narrative rules.

 The context-free syntax of the language is described using the
variant Backus-Naur Form (BNF) [BNF 1960] as defined herein.

-  Lower case words in courier new font, some containing embedded
   underlines, are used to denote syntactic categories. A syntactic
   category is a nonterminal in the grammar. For example:

component\_feature\_list

-  Boldface words are used to denote reserved words, for example:

**implementation**

-  A vertical line separates alternative items.

software\_category ::= **thread** \| **process**

-  Square brackets enclose optional items. Thus the two following rules
   are equivalent.

property\_association ::= property\_name **=>** [ **constant** ]
expression

property\_association ::=

property\_name **=>** expression

\| property\_name **=>** **constant** expression

-  Curly brackets with a \* symbol enclose a repeated item. The item may
   appear zero or more times; the repetitions occur from left to right
   as with an equivalent left-recursive rule. Thus the two following
   rules are equivalent.

declaration\_list ::= declaration { declaration }\ :sup:`\*`

declaration\_list ::= declaration

\| declaration declaration\_list

-  Curly brackets with a + symbol specify a repeated item with one or
   more occurrences. Thus the two following rules are equivalent.

declaration\_list ::= { declaration }\ :sup:`+`

declaration\_list ::= declaration { declaration }\ :sup:`\*`

-  Parentheses (round brackets) enclose several items to group terms.
   This capability reduces the number of extra rules to be introduced.
   Thus, the first rule is equivalent with the latter two.

property\_association ::= **identifier** ( **=>**\ \| **+=>**)
property\_expression

property\_association ::= **identifier** assign property\_expression

assign ::= **=>** \| **+=>**

-  Square brackets, curly brackets, and parentheses may appear as
   delimiters in the language as well as meta-characters in the grammar.
   Square, curly, and parentheses that are delimiters in the language
   will be written in bold face in grammar rules, for example:

property\_association\_list ::=

**{** property\_association { **;** property\_association }\ :sup:`\*`
**}**

-  The syntax rules may preface the name of a nonterminal with an
   italicized name to add semantic information. These italicized
   prefaces are to be treated as comments and not a part of the grammar
   definition. Thus the two following rules are equivalent.

component ::= identifier : component\_classifier ;

component ::= *component*\ \_identifier : component\_classifier ;

-  A construct is a piece of text (explicit or implicit) that is an
   instance of a syntactic category, for example:

My\_GPS: **thread** GPS.dualmode ;

 The syntax description has been developed with an emphasis on an
abstract syntax representation to provide clarity to the reader.

1. .. rubric:: Method of Description for Discrete and Temporal
  Semantics
  :name: method-of-description-for-discrete-and-temporal-semantics

 Discrete and temporal semantics of the language are defined in
sections that define AADL concepts using a concurrent hierarchical
hybrid automata notation, together with additional narrative rules
about those diagrams. This notation consists of a hierarchical
finite state machine notation, augmented with real-valued variables
to denote time and time-varying values, and with edge guard and
state invariant predicates over those variables to define temporal
constraints on when discrete state transitions may occur.

 A semantic diagram defines the nominal scheduling and
reconfiguration behavior for a modeled system as well as scheduling
and reconfiguration behavior when failures are detected. A physical
realization of a specification may violate this definition, for
example due to runtime errors. A violation of the defined semantics
is called an anomalous behavior. Certain kinds of anomalous
behaviors are permitted by this standard. Legal anomalous behaviors
are defined in the narrative rules.

Semantics for individual components are defined using a sequential
hierarchical hybrid automaton. System semantics are defined as the
concurrent composition of the hybrid automata of the system
components.

 Ovals labeled with lower case phrases are used to denote discrete
states. A component may remain in one of its discrete states for an
interval of time whose duration may be zero or greater. Every
semantic automaton for a component has a unique initial discrete
state, indicated by a heavy border. For example,

|image1|

 Directed edges labeled with one or more comma-separated, lower case
phrases are used to denote possible transitions between the discrete
states of a component. Transitions over an edge are logically
instantaneous, i.e., the time interval in which a transition from a
discrete state (called the source discrete state) to a discrete
state (called the destination discrete state) has duration 0. For
example,

|image2|

 Permissions that allow a runtime implementation of a transition to
occur over an interval of time are expressed as narrative rules.
However, all implemented transitions must be atomic with respect to
each other, all observable serializations must be admitted by the
logical semantics, and all temporal predicates as defined in
subsequent paragraphs must be satisfied.

Hybrid automaton can have hierarchical states. Oblong boxes labeled
with lower case phrases denote abstract discrete states, for which
another hybrid semantics diagram with an identically labeled oblong
box for which another hybrid semantics diagram with an identically
labeled oblong box specifies the discrete states and edges that make
up that abstract discrete state. For example,

|image3|

 Abstract discrete states are reusable, i.e., a hybrid semantics
diagram can contain several oblong boxes with the same label An
abstract state label or an edge label may include italicized letters
that are not a part of the formal name but are used to distinguish
multiple instances. For example, both abstract discrete states below
will be defined by a single diagram labeled executing.

|image4|

 If there is an external edge that enters or exits an abstract
discrete state in the defining diagram for, and there are no edges
within that definition that connect any internal discrete state with
that external edge, then there implicitly exist edges from every
contained discrete state in the defining diagram to or from that
external edge. In that case, a transition into or out of an abstract
discrete state represents transitions into or out any of its
internal states. For example, in the following diagram there is an
implicitly defined halt edge out of both the ready and the running
discrete states.

|image5|

 Real-valued variables whose values are time-varying may appear in
expressions that annotate discrete states and edges of hybrid
semantic diagrams. Specific forms of annotation are defined in
subsequent paragraphs. The set of real-valued variables associated
with a semantic diagram are those that appear in any expression in
that diagram, or in any of the defining diagrams for abstract
discrete states that appear in that diagram. Real-valued
time-varying variables will be named using an italicized front. The
initial values for the real-valued time-varying variables of a
hybrid semantic diagram are undefined whenever they are not
explicitly defined in narrative rules.

In addition to standard rational literals and arithmetic operators,
expressions may also contain functions of discrete variables. The
names of functions and discrete variables will begin with upper case
letters. The semantics for function symbols and discrete variables
will be defined using narrative rules. For example, the
subexpression Max(Compute Execution Time) may appear in a semantic
diagram, together with a narrative rule stating that the value is
the maximum value of a range-valued component property named Compute
Execution Time.

 Edges may be annotated with assignments of values to variables
associated with the semantic diagram. When a transition occurs over
an edge, the values of the variables are set to the assigned values.
For example, in the following diagram, the values of the variables
*c* and *t* are set to 0 when the component transitions into the
ready discrete state.

|image6|

 Discrete states may be annotated with expressions that define the
possible rates of change for real-valued variables during the
duration of time a component is in that discrete state. The rate of
a variable is denoted using the symbol δ, for example δ\ *x*\ =[0,1]
(the rate of the variable *x* may be any real value in the range of
0 to 1). If, rates of change are not explicitly shown within a
discrete state for a time-varying variable, then the rate of change
of that variable in that state is defined to be 1. For example, in
the following diagram the rate of change for the variable *c* is 1
while the component is in the discrete state running, but its value
remains fixed while the component is in the ready state, equal to
the value that existed when the component transitioned into the
ready state.

|image7|

 A discrete state may be annotated with Boolean-valued expressions
called invariants of that discrete state. In this standard, all
semantic diagrams are defined so that the values of the variables
will always satisfy the invariants of a discrete state for every
possible transition into that discrete state. A transition must
occur out of a discrete state before the values of any time-varying
variables cause any invariant of that discrete state to become
false. Invariants are used to define bounds on the duration of time
that a component can remain in a discrete state. For example, in the
following diagram the component must transition out of the running
state before the value of the variable *c* exceeds 10.

|image8|

 An edge may be annotated with Boolean-valued expressions called
guards of that edge. A transition may occur from a source discrete
state to a destination discrete state only when the values of the
variables satisfy all guards for an edge between those discrete
states. A guard on an edge is evaluated before any assignments on
that edge are performed. For example, in the following diagram the
component may only complete when the value of the variable *c* is 5
or greater (but must complete before *c* exceeds 10 because of the
invariant).

|image9|

 A sequential semantic automaton defines semantics for a single
component. A system may contain multiple components. The semantics
of a system are defined to be the concurrent composition of the
sequential semantic automata for each component. Except as described
below, every component is represented by a copy of its defined
semantic automaton. All discrete states and labels, all edges and
labels, and all variables, are local to a component. The set of
discrete states of the system is the cross-product of the sets of
discrete states for each of its cross product components. The set of
transitions that may occur for a system at any point in time is the
union of the transitions that may occur at that instant for any of
its components.

If an edge label appears in boldface, then a transition may occur
over that edge only when a transition occurs over all edges having
that same boldface label within the synchronization scope for that
label. The synchronization scope for a boldface label is indicated
in parentheses. For example, if a transition occurs over an edge
having a boldface label with a synchronization scope of process,
then every thread contained in that process in which that boldface
label appears anywhere in its hybrid semantic diagram must
transition over some edge having that label. That is, transitions
over edges with boldface labels occur synchronously with all
similarly labeled edge transitions in all components associated with
the component with the specified synchronization scope as described
in the narrative. Furthermore, every component in that
synchronization scope that might participate in such a transition in
any of its discrete states must be in one of those discrete states
and participate in that transition. For example, when the
synchronization scope for the edge label **s** is the same for all
three of the following concurrent semantic automata, a transition
over the edge labeled **s** may only occur when all three components
are in their discrete states labeled a, and all three components
simultaneously transition to their discrete states labeled c.

|image10|

 If a variable appears in boldface, then there is a single instance
of that variable that is shared by all components in the
synchronization scope of the variable. The synchronization scope for
a boldface variable will be defined in narrative rules.

1. .. rubric:: References
  :name: references

   1. .. rubric:: Normative References
 :name: normative-references

 The following normative documents contain provisions that, through
reference in this text, constitute provisions of this standard.

 ISO/IEC/IEEE 24765 Systems and Software Engineering – Vocabulary,
Dec. 2010.

ISO/IEC 9945-1:1996 [IEEE/ANSI Std 1003.1, 1996 Edition],
Information Technology – Portable Operating System Interface (POSIX)
– Part 1: System Application Program Interface (API) [C Language].

 ISO/IEC 14519:1999 [IEEE/ANSI Std 1003.5b-1999], Information
Technology – POSIX Ada Language Interfaces – Binding for System
Application Program Interface (API) – Real-time Extensions.

 ISO/IEC 8652:2007 Ed.3, Information Technology – Programming
Languages – Ada Reference Manual.

(5) ISO/IEC 9899:1999, Information Technology – Programming Languages –
C.

(6) Unified Modeling Language Specification [UML 2007, version 2.1.1],
August 2007, version 2.1.1.

(7) SAE AS-5506:2004, Architecture Analysis & Design Language (AADL),
November 2004.

(8) SAE AS-5506/1:2006, Architecture Analysis & Design Language (AADL)
Annex Volume 1, June 2006.

1. .. rubric:: Informative References
  :name: informative-references

 The following informative references contain background information
about the items with the citation.

 [BNF 1960] NAUR, Peter (ed.), "Revised Report on the Algorithmic
Language ALGOL 60," *Communications of the ACM*, Vol. 3 No. 5, pp.
299-314, May 1960.

[IFIP WG10.4-1992] IFIP WG10.4 on Dependable Computing and Fault
Tolerance, 1992, J.-C. Laprie, editor, Dependability: Basic
Concepts and Terminology, *Dependable Computing and Fault
Tolerance*, volume 5, Springer-Verlag, Wien, New York, 1992.

 [Henz 96] Theory of Hybrid Automata, Thomas A. Henzinger,
Electrical Engineering and Computer Science, University of
California at Berkley, *Proceedings of the 11th Annual Symposium on
Logic in Computer Science* (LICS), IEEE Computer Society Press,
1996, pp. 278-292

1. .. rubric:: Terms and Definitions
  :name: terms-and-definitions

 Terms are introduced throughout this standard, indicated by *italic*
type. Informational definitions of terms are given in Appendix B ,
Glossary. Definitions of terms used from other standards, such as
ISO/IEC/IEEE 24765 Systems and Software Engineering – Vocabulary,
Dec. 2010, ISO/IEC 9945-1:1996 [IEEE/ANSI Std 1003.1, 1996 Edition],
*Information Technology – Portable Operating System Interface
(POSIX)*, or IFIP WG10.4 *Dependability: Basic Concepts and
Terminology* [IFIP WG10.4-1992], are so marked. Terms not defined in
this standard are to be interpreted according to the Webster's Third
New International Dictionary of the English Language. Terms
explicitly defined in this standard are not to be presumed to refer
implicitly to similar terms defined elsewhere. A full description of
the syntax and semantics of the concept represented by the terms is
found in the respective document sections, clauses, and subclauses.

