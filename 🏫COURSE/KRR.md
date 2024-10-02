[[2024-10-01]]
# Introduction
## course element 
* overview on knowledge representation
* motivation and philosophical issues
* fundamental principles of logical analysis
* several important representational formalisms -> motivation and capabilities
* examples with potential practicality of KR methods
## book
* Russell S. and Norvig P. Artificial Intelligence, A Modern  Approach, 3rd Edition (especially chapters 7–12). 
* Brachman RJ and Levesque HJ, Knowledge Representation and  Reasoning, Morgan Kaufmann 2004.
* article 

## course topic
* Classical Logic and Proof Systems
* Automated Reasoning
* Programming in **Prolog**.
* Representing and reasoning about time and change.
* Space and physical objects.
* Specialised AI representations: 
	* situation calculus, 情景演算
	* non-monotonic logic, 非单调逻辑：处理不完整或变化的信息
	* description logic, 描述逻辑：推理概念知识
	* fuzzy logic.模糊逻辑：处理不精确或模糊的信息
* Ontology and AI Knowledge Bases.
	* Ontology: It provides a structured representation of concepts, relationships, and axioms within a specific domain 
## assessment and ddl
![[Pasted image 20241001113027.png]]

![[Pasted image 20241001111730.png]]
* Assignment 2 => 4th Dec
## KRR's Relationships
### AI and KR paradigm
* GOFAI - "good old-fashioned ai"
	* GOFAI is based on the idea that intelligence can be achieved by manipulating symbols and applying logic rules
### KRR vs ML
* ML --> concerned with **classification** an regression...
	* automatically creates modes **from** data
	* contain knowledge in an implicit form (**black box nature**)
* KRR --> concerned with **inference**
	* typically uses hand-crafted models
	* store knowledge in  an explicit way.(The knowledge is  structured using formal languages or logical representations.)
## KRR's dimensions
* time and change
* spatial information
## Concepts of KRR
### Uncertainty
* classical logic can handle some aspects of uncertainty without resorting to full probabilistic reasoning.
### Ontology
* a structured way to represent knowledge about a specific field or area
* **defining** its key **elements** and how they **relate** to each other in a logically consistent manner.

# classical logic: concepts and uses of logic
## Propositional logic(命题逻辑)
* A proposition is an expression of a fact.
* classical proposition logic --> Every proposition is either true or false and not both
* ∧(conjunction)  ∨(disjunction)  →  ¬(negation)
### proposition Symbols and Schematic variables
* P Q -- abbreviations(缩写) for actual propositions
* α, β -- stand for arbitrary(任意的) propositional formulae
	* describe patterns of inference
### proof
* the concern of formal logic with the structure and validity of arguments, rather than the specific reasoning process.
## provability
### definition
* a **property** of an argument which depends on the inference rules of a logical proof system
* eg:C can be proved from premisses {P1, . . . , Pn} in a  proof system S
```
P1, . . . , Pn ⊢S C
```
## validity
### Definition
* An argument is valid if it is not possible  for its premisses to be true and its conclusion is false.(如果不可能出现前提为真而结论为假的情况，那么这个论证就是有效的)
* PS:论证的有效性仅仅关注逻辑结构，而不是内容的真实性
### formal semantics
* some precise specification of the ‘meanings’ of logical formulae(对逻辑公式的"含义"进行一些精确的规定)
## quantification
### universal/Existential quantification
* ∀ -- Universal Quantifier (for every)
* ∃ -- Existential Quantifier (for some)
## uses of logic
### Semantic Entailment(语意蕴含)/logical consequence（逻辑后承）
* $\Theta \models \pi$
	* $\models$ : 表示从一组假设中可以推导出某个结论
* Read as: "Θ semantically entails π"
* definition：
	* 假设所有在集合 Θ 中的公式都为真，那么公式 π 也必然为真
* we can verify  $\Theta \models \pi$ by using a proof system S $\Theta \vdash_S \pi$
> this statement is based on the **soundness**(可靠性) property of logical systems.
## Addition -- properties of logical systems.
### Soundness(可靠性)
* the system does not allow deriving false conclusions from true premises
* $\text{If } \Theta \vdash \pi \text{ , then } \Theta \models \pi$
### Completeness（完备性）
* 如果系统中的所有语义上有效的结论都可以通过系统的推理规则从前提出发推导出来
* $\text{If } \Theta \models \pi \text{ , then } \Theta \vdash \pi$
### Consistency（一致性）
* A logical system is **consistent** if it does not allow the derivation of both a formula and its negation from the same set of premises
* For any formula ,it is impossible to have both $\Theta \vdash \pi \text{ and } \Theta \vdash \neg \pi$

# Propositional Resolution(命题消解)
## NP-Complete
* Problems in NP (Nondeterministic Polynomial time) are those for which a solution can be verified in polynomial time.




