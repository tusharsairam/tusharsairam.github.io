---
title: Quines (Coding)
---

>[!definition] Quine
>A quine is a program that self-replicates/prints its own source code

## How are they relevant?
- Quines are a consequence of the **fixed-point theorem**, which itself comes from **Cantor's ubiquitous diagonal argument**
- Quines are related to **Gödel's incompleteness theorem**

As long a programming language is Turing complete, quines in that language are always possible (and infinite such quines) according to the FP theorem

>[!warning] What isn't a quine?
>Opening the source file and viewing it. That's cheating and it's not really a program that generates its own source code anyways

* It is impossible for a program to directly manipulate itself
* A quine is separated into two components --> *data* and *code*
	* The *data* is the code itself represented in text form and is algorithmically derived
	* The *code* uses the data to print the code and then it uses the data to print the data

Interestingly, Douglas Hofstadter coined the phrase *"to quine"*, that means ==*to write a sentence part first and follow it with the same part in quotation marks*==. The connection seems a little clearer now. For example, quining "say" -> "say 'say'" and so on. There's also a neat analogy to cellular biology -- ==the *code is the cell* and the *data is the cell's DNA*==. The cell replicates itself using its DNA and it involves replicating the DNA itself

