### VerifAPS — Library for the Verification of Automated Producton System Software
VerifAPS (**Verif**iaction for **A**utomated **P**roducton **S**ystem)
is a library for supporting the verification of software for
automation manufacturing domain.

Allows the parsing and analyzation of Structured Text, Sequential
Function Chart, Instruction List and Function Block Diagrams. Ladder
Diagrams are incoming. This also includes reading from PCLOpenXML and
Object-oriented extension (IEC 61131 2013). The library offers
symbolic execution expressible in SMV or SMT files. Controller for
nuXmv/nuSMV is including, e.g., parsing of counter example.

[Website](https://formal.iti.kit.edu/weigl/verifaps-lib) [Github](https://github.com/VerifAPS/verifaps-lib)

Upon VerifAPS lives *geteta* and *stvs*, which offers functionalities for **Generalized Test Tables*

> Generalised test tables extend the concept of test tables, which are
> already frequently used in quality management of reactive
> systems. The main idea is to allow more general table entries, thus
> enabling a table to capture not just a single test case but a family
> of similar behavioural cases. We show how generalised test tables
> can be encoded into verification conditions for state-of-the-art
> model checkers. — from the
> [paper](https://formal.iti.kit.edu/weigl/biblio/?lang=de&key=BeckertChaEA2017)

*stvs* provides a graphical user interface for the verification of
Structured Text source code against generalized test tables. It is a
graphical frontend for the geteta backend, providing a useful and
beautiful user interface, e.g., a visualization of specification
violations. stvs is open source, provided under GNU Public License v3.

Features includes

* Verification of Structured Text against one or more specified generalized test tables
* Transform a generalized test table into a concrete test table
* Source Code Editor for Structured Text

[The Backend _geteta_](https://formal.iti.kit.edu/weigl/geteta) ∣ [The Frontend _stvs_](https://formal.iti.kit.edu/weigl/stvs)
