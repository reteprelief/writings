Annex Subclauses and Annex Libraries
====================================

 Annex subclauses allow annotations expressed in a sublanguage to be
attached to component types, component implementations, feature
group types, and property sets. Examples of standardized
sublanguages are defined in the Error Model Annex Document C and in
the Behavior Model Annex Document D.

 Annex libraries contain reusable declarations expressed in a
sublanguage and are declared in packages. Those reusable
declarations can be referenced by annex subclauses.

A major use of these annex declarations is to accommodate new
analysis methods through analysis specific notations or
sublanguages.

 The AADL standard consists of a core language document and a set of
standardized annex documents, some of which introduce sublanguage
notations. Individual projects can introduce project-specific
sublanguage notations to support project-specific analysis needs.
Use of such sublanguage notations results in AADL models that are
compliant with the core language standard. Standard compliant AADL
tools are required to accept such AADL specifications (see
*Processing Requirements and Permissions*).

 Examples of annex libraries are error states machines that can be
associated with components in annex subclauses (see Annex Document
E).

Syntax

annex\_subclause ::=

**annex** *annex*\ \_identifier (

( **{\*\*** annex\_specific\_language\_constructs **\*\*}** ) \|
**none** )

[ in\_modes ] ;

annex\_library ::=

**annex** *annex*\ \_identifier

(( **{\*\*** annex\_specific\_reusable\_constructs **\*\*}** ) \|
**none** ) ;

Naming Rules

1. The annex identifier must be the name of an approved annex or a
   project-specific identifier different from the approved annex
   identifiers.

2. The mode identifiers in the in\_modes statement must refer to modes
   in the component type or component implementation for which the annex
   subclause is declared.

Legality Rules

1. Annex subclauses can only be declared in component types, component
   implementations, and feature group types.

1. A component type, component implementation, or feature group type
   declaration may contain at most one annex subclause for each
   annex. If the annex subclause has an in\_modes statement, then
   there must be at most one annex subclause per mode for each
   annex.

2. Annex libraries must be declared in packages.

3. A package declaration may contain at most one annex library
   declaration for each annex.

Semantics

 An annex subclause provides additional specification information
about a component to be interpreted by analysis methods. Annex
subclauses apply to component types and component implementations.
Such annex subclauses can introduce analysis specific notations such
as constraints and assertions expressed in predicate logic or
behavioral descriptions expressed in temporal logic. Such notation
can refer to subcomponents, connections, modes, and transitions as
well as features and subcomponent access.

Annex subclauses can be declared to be applicable to specific modes
by specifying them with an in\_modes statement. An annex subclause
without an in\_modes statement contains annex statements that are
applicable in all modes. This capability allows users to attach mode
specific annex annotations to core AADL models.

 An annex library provides reusable specifications expressed in an
annex specific notation. Users can place multiple reusable annex
specific constructs inside an annex library declaration. An example
of a reusable annex specification is a fault state machine as
defined by the Error Model Annex Document C.

Processing Requirements and Permissions

 Processing methods compliant with the core AADL standard must accept
AADL specifications with approved and project-specific annex
subclauses and specifications, but are not required to process the
content of annex subclauses and annex library declarations. An AADL
analysis tool must provide the option to report the use of
project-specific annex sublanguages. Processing methods compliant
with a given annex sublanguage must process specifications as
defined in that annex sublanguage.

Annex specific sublanguages can use any vocabulary word except for
the symbol **\*\*}** representing the end of the annex subclause or
specification.

 Annex specific sublanguages may introduce reserved words that may be
the same or different from those in the core language or other annex
sublanguages. If the annex sublanguage uses a reserved word that is
a legal identifier in the AADL core language, then it must support
the ability to refer to this named element in the core model.

 Annex specific sublanguages can utilize the core language property
mechanism, i.e., properties can be defined in property sets that
apply to elements in the sublanguage annex. For example, a property
occurrence can be defined to apply to an error event in an error
model.

(5) Annex sublanguages may choose not to support inheritance of
sublanguage declarations contained in annex libraries of ancestor
component type or component implementation declarations by their
extensions.

Examples

**package** AnnexExample

**public**

**with** Sampling;

**thread** Collect\_Samples

**features**

Input\_Sample : **in data port **\ Sampling::Sample;

Output\_Average : **out data port **\ Sampling::Sample;

**annex** Error\_Model **{\*\***

Model => Transient\_Fault\_Model;

Occurrence => poisson 10e-4 **applies to** Transient\_Fault;

**\*\*}**;

**end** Collect\_Samples;

**end** AnnexExample;


