General AADL Concepts
=====================

*Naming Rules*


1. AADL is a case sensitive language.

2. Identifiers in AADL consist of upper case and lower case letters, digits, and the underscore character. The first identifier character must be a letter.

3. Identifiers in AADL must not be one of the reserved words.

4. AADL supports nested name spaces. AADL package, component classifier, and type introduce a name space that is nested inside the name space of containing model element. 

5. Defining identifiers must be unique within a name space.

6. Model elements are referenced by a *dot* separated sequence of identifiers as *name path*. This name path can be *relative*, i.e., start within the name space of the model element that contains the reference. 
This name path can also be *fully qualified*, i.e., start with a top-level package identifier. 
   
7. Property references consist of an optional name path and the property reference prefixed with **#**. 
 

