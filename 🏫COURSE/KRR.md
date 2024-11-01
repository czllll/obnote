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
* classical proposition logic --> Every proposition is either true or false and **not both**
* ∧(conjunction)  ∨(disjunction)  →  ¬(negation)
* p -> q <=> ¬p v q
### proposition Symbols and Schematic variables
* P Q -- abbreviations(缩写) for actual propositions
* α, β -- stand for arbitrary(任意的) propositional formulae
	* describe patterns of inference
### proof
* the concern of formal logic with the structure and validity of arguments, rather than the specific reasoning process.
## provability($\vdash$)
### definition
* a **property** of an argument which depends on the inference rules of a logical proof system
* eg:C can be proved from premisses {P1, . . . , Pn} in a  proof system S
```
P1, . . . , Pn ⊢S C
```
## validity($\models$)
### Definition
* An argument is valid if it is not possible  for its premisses to be true and its conclusion is false.(如果不可能出现前提为真而结论为假的情况，那么这个论证就是有效的)
* PS:论证的有效性仅仅关注逻辑结构，而不是内容的真实性
### formal semantics
* some precise specification of the ‘meanings’ of logical formulae(对逻辑公式的"含义"进行一些精确的规定)
## quantification
### universal/Existential quantification
* ∀ -- Universal Quantifier (for every)
* ∃ -- Existential Quantifier (for some)
## uses of logic（symbols）
### Semantic Entailment(语意蕴含)/logical consequence（逻辑后承）
* $\Theta \models \pi$
	* $\models$ : 表示从一组假设中可以推导出某个结论
* Read as: "Θ semantically entails π"
* definition：
	* 假设所有在集合 Θ 中的公式都为真，那么公式 π 也必然为真
* we can verify  $\Theta \models \pi$ by using a proof system S $\Theta \vdash_S \pi$
* 关注点：命题的真值和语义
> this statement is based on the **soundness**(可靠性) property of logical systems.
![[Pasted image 20241008123952.png]]
### Turnstile(推理)
* $\vdash$ vdash
*  表示语法**推导**关系，一般链接两个表达式
* 关注点：证明过程和规则
### Arrow
* ->
* 一般用于表示定理中的条件关系
* 条件语句 (φ → ψ) 被定义为"只有当 φ 为真且 ψ 为假时，整个语句才为假，**其他都为真**
## Aside -- properties of logical systems.
### Soundness(可靠性)
* the system does not allow deriving false conclusions from true premises
* provability -> validity
* $\text{If } \Theta \vdash \pi \text{ , then } \Theta \models \pi$
### Completeness（完备性）
* 如果系统中的所有语义上有效的结论都可以通过系统的推理规则从前提出发推导出来
* validity -> provability
* $\text{If } \Theta \models \pi \text{ , then } \Theta \vdash \pi$
### Consistency（一致性）
* A logical system is **consistent** if it does not allow the derivation of both a formula and its negation from the same set of premises
* For any formula ,it is impossible to have both $\Theta \vdash \pi \text{ and } \Theta \vdash \neg \pi$
#### Determining Entailment  from Inconsistency（通过不一致性来确定 ）
* A1, ..., An |= C  iff（当且仅当） the formula set {A1, ..., An, ¬C} is **inconsistent**.
#### refutation complete(反驳完备性)
* A formal system S is refutation-complete if it is able to derive false from every unsatisfiable set of formulas
# Propositional Resolution(命题消解)
## sideby：NP-Complete
* Problems in NP (Nondeterministic Polynomial time) are those for which a solution can be verified in polynomial time.
## Process of Resolution 
1. Convert all premises and the **negation** of the conclusion to **CNF**.
2. Apply the resolution rule to clauses(子句) in the CNF.
	1. This step involves finding **complementary**（互补的） literals in different clauses.
3. Repeat the application of the resolution rule until either：
	1. An empty clause (contradiction) is derived, or
	2. No new clauses can be generated.
4. Interpret the results
	1. If an empty clause is derived（空子句）, the original proposition is proved (by contradiction).
	2. If no empty clause is derived and no new clauses can be generated, the proof is inconclusive.
## concept
### disjunction（析取式）
* A disjunction is a compound proposition formed by connecting two or more propositions using the logical "or" operator.
### Conjunctive Normal Form（CNF）合取范式
#### Definition
* CNF is a formula consisting of a series of disjunctive clauses connected by conjunction (AND).
#### Form  
* (A ∨ B) ∧ (¬C ∨ D) ∧ (E ∨ F ∨ G)  
    Here, each parenthesis contains a disjunctive clause, and the clauses are connected by conjunction.
#### Representation
* {{p}, {¬q}, {r, s}, {t, ¬u, ¬v}} -> represents
* p ∧ ¬q ∧ (r ∨ s) ∧ (t ∨ ¬u ∨ ¬v)
#### rules
* Rewrite → :
	* (φ → ψ) ⇒ (¬φ ∨ ψ)
* Move negations inwards:  
	* ¬¬φ ⇒ φ (Double Negation Elimination)  
	* ¬(φ ∨ ψ) ⇒ (¬φ ∧ ¬ψ) (De Morgan)  
	* ¬(φ ∧ ψ) ⇒ (¬φ ∨ ¬ψ) (De Morgan)
* Distribute ∨ over ∧ :  
	* φ ∨ (α ∧ β) ⇒ (φ ∨ α) ∧ (φ ∨ β)
### Disjunctive Normal Form （DNF） 析取范式
#### Definition
* DNF is a formula consisting of a series of conjunction clauses connected by disjunctive (OR).
#### Form  
* (A ∧ B) ∨ (¬C ∧ D) ∨ (E ∧ F ∧ G)
    - DNF是"OR of ANDs"(析取的合取)
# Pover9
## 谓词
* 用于描述个体的属性或关系
* `Fast(x)`: 表示 x 是快的（属性）。
- `Loves(x, y)`: 表示 x 爱 y（关系）。
# Aside
## reflexive（自反性）
### definition
* A relation R on set A is reflexive if ∀x ∈ A, xRx holds
### form
* ∀x (xRx)
### eg
* =
* >=
* ⊆

# Classical Logic
## Formal Proofs in Sequent Calculus(序列演算构建逻辑证明)
### Sequents(序列)
#### Definition
* ![[Pasted image 20241017184750.png]]
* 在逻辑推理中用于表示推理规则的结构，上述截图表示
	* If all of the αs are true then at least one of the βs is true.
	* same as
	* ![[Pasted image 20241017184856.png]]
#### logical theorem（逻辑定理）
* If the simple sequent $⇒ β$ is valid, then $β$ is called a  logical theorem.
*  $α_1​,…,α_m​⇒$ 
	* a sequent with an empty right-hand side  asserts that the set of premisses {$α_1​,…,α_m$} is inconsistent. 
	* The sequent means that there is no possible way for all premises to be true simultaneously, because they lead to a logical contradiction.
	* Explain: If it's impossible for all of them to be true at the same time, than  {$α_1​,…,α_m$} is considered as **inconsistent**.
### Inference rules of Sequent Calculus
#### conjunction introduction/elimination rule(合取规则)
* $\frac{\Gamma \Rightarrow \alpha, \Delta \ and \ \Gamma \Rightarrow \beta, \Delta}{\Gamma \Rightarrow (\alpha \land \beta), \Delta} \ [\Rightarrow \land]$ and on the right
* $\frac{\alpha, \beta, \Gamma \Rightarrow \Delta}{(\alpha \land \beta), \Gamma \Rightarrow \Delta} \ [\land \Rightarrow]$
#### Disjunction Rules(析取规则)
* $\frac{\alpha, \Gamma \Rightarrow \Delta \quad \beta, \Gamma \Rightarrow \Delta}{(\alpha \lor \beta), \Gamma \Rightarrow \Delta} [\lor\Rightarrow]$
* $\frac{\Gamma \Rightarrow \alpha, \beta, \Delta}{\Gamma \Rightarrow (\alpha \lor \beta), \Delta} [\Rightarrow\lor]$
#### Negation Rules(否定规则)
* $\frac{\Gamma \Rightarrow \alpha, \Delta}{\lnot\alpha, \Gamma \Rightarrow \Delta} [\lnot\Rightarrow]$
* $\frac{\Gamma, \alpha \Rightarrow \Delta}{\Gamma \Rightarrow \lnot\alpha, \Delta} [\Rightarrow\lnot]$
#### Implication rule
* $\frac{\Gamma, A \Rightarrow B, \Delta}{\Gamma \Rightarrow A \rightarrow B, \Delta} \; [\Rightarrow \rightarrow]$
* $\frac{\Gamma \Rightarrow A, \Delta \quad B, \Gamma \Rightarrow \Delta}{(A \rightarrow B), \Gamma \Rightarrow \Delta} \; [\rightarrow \Rightarrow]$
#### Universal quantifier(∀⇒)
* $\frac{\Gamma \Rightarrow A(t), \Delta}{\Gamma \Rightarrow \forall x A(x), \Delta} \; [\Rightarrow \forall]$
* $\frac{A(t), \Gamma \Rightarrow \Delta}{\forall x A(x), \Gamma \Rightarrow \Delta} \; [\forall \Rightarrow]$
	* which is not **reversible**, because the top part makes a much weaker claim than the bottom one.Here is a reversible one:
	* $\frac{\varphi(\kappa), \forall \upsilon [\varphi(\upsilon)], \Gamma \Rightarrow \Delta}{\forall \upsilon [\varphi(\upsilon)], \Gamma \Rightarrow \Delta} \; [\forall \Rightarrow]$
* 
### Sequent Calculus Proof Systems(序列演算证明系统)
* we use $\vdash sc$, to assert that a sequent is provable in a sequent calculus system.SC
### Decision Procedures(决策过程)
* A decision procedure for some class of problems is an algorithm  which can solve any problem in that class **in a finite time**
#### Decidability（可判定性）
* 谓词逻辑（一阶逻辑）是不可判定的
* Semi-decidablity
	* we can verify every positive case in finite time, but cannot refute every negative case in finite time.
##  Propositional Semantics(命题语义)
### Model in True-Functional Semantics
* Essentially, a model assigns **truth values** (true or false) to each **atomic proposition**.
### Soundness and Completeness
![[Pasted image 20241101191914.png]]
if a proof system S to be both sound and complete. we have above.
$\models_{TF}$ means that 真值功能语义（Truth-Functional Semantics）,限定在该语义下；与其同级的还有
* 模态语义（Modal Semantics） 用于模态逻辑
* 一阶语义（First-Order Semantics）用于谓词逻辑
## Representation in First-Order Logic/Predicate Logic
### Terminology
* property (1 argument)
* relation (at least 2 arguments)
* predicate 
### Formulae
- **Atomic Formula**: An expression of the form:
	- ρ(α₁, ..., αₙ)  
	- (α₁ = α₂)
  Where:
  - ρ: A relation symbol of arity **n**.
  - αᵢ: Either a **constant** or a **variable**.
- **First-Order Logic Formula**:
	- Either an **atomic formula** or a (finite) expression of one of the forms:
	    - ¬α (Negation)
	    - (α κ β) (Binary connective)
	    - ∀x[α] (Universal quantifier)
	    - ∃x[α] (Existential quantifier)
  Where:
  - α and β: First-order formulae.
  - κ: Any of the binary connectives:
    - ∧ (AND)
    - ∨ (OR)
    - → (IMPLIES)
    - ↔ (IF AND ONLY IF)

### how to define 
#### Exactly
To state that a property holds for exactly n objects, we need to  assert that it holds for at least n objects, but deny that it holds for  at least n + 1 objects:
$$
\forall t \Big[ \text{Triangle}(t) \to \\
    \quad \big( \exists x \exists y \exists z [ \text{SideOf}(x, t) \land \text{SideOf}(y, t) \land \text{SideOf}(z, t) \\
    \quad \land \neg (x = y) \land \neg (y = z) \land \neg (x = z) ] \big) \\
    \quad \land \neg \exists x \exists y \exists z \exists w [ \text{SideOf}(x, t) \land \text{SideOf}(y, t) \land \text{SideOf}(z, t) \land \text{SideOf}(w, t) \\
    \quad \land \neg (x = y) \land \neg (y = z) \land \neg (x = z) \land \neg (w = x) \land \neg (w = y) \land \neg (w = z) ] \Big]

$$
we can see that in pure first-order logic gets increasingly complex as the numbers increase.we can use **syntactic sugar**
- Shorthand Quantifier Notations:
	  - **∃>₂ x [Φ(x)]**: There are **at least 3** things with property **Φ**.
	  - **∃<₄ x [Φ(x)]**: There are **at most 3** things with property **Φ**.
	  - **∃₃ x [Φ(x)]**: There are **exactly 3** different things with property **Φ**.
	  - **∃! x [Φ(x)]**: There is **exactly one** thing with property **Φ**.
- Usage:
	  - These notations are useful as **shorthand** (or "syntactic sugar") to simplify the representation of **quantifiers** in first-order logic.
	  - They can be translated into equivalent first-order formulae, allowing the use of standard first-order inference rules after translation.


## semantics of 1st-Order Logic
### Model
### Predicate Logic Model Cheat Sheet

#### What is a Model in Predicate Logic?
- A **model** $\mathcal{M} = \langle D, \delta \rangle$ is a mathematical structure used to evaluate the truth of predicate logic formulas.
#### Components of a Model:
1. **Domain (D)**:
   - A non-empty set that represents all possible objects the variables can refer to.
   - Example: $D = \{a, b, c, \ldots\}$

2. **Interpretation Function ($ \delta $)**:
   - A function that assigns:
     - **Constants** to specific objects in $D$.
     - **Predicate symbols** to relations over $D$.
     - **Function symbols** to functions mapping $D^n$ to $D$ (where $n$ is the arity of the function).

#### How Models Interpret Formulas:
- **Atomic Formulas**:
  - $\rho(\alpha_1, \ldots, \alpha_n)$ is true in $\mathcal{M}$ iff $\langle \delta(\sigma_1), \ldots, \delta(\sigma_n) \rangle \in \delta(\rho)$.
  - $(\alpha = \beta)$ is true in $\mathcal{M}$ iff $\delta(\alpha) = \delta(\beta)$.

- **Logical Connectives**:
  - **Negation** ($\neg\phi$): true in $\mathcal{M}$ iff $\phi$ is not true in $\mathcal{M}$.
  - **Conjunction** ($\phi \land \psi$): true in $\mathcal{M}$ iff both $\phi$ and $\psi$ are true in $\mathcal{M}$.
  - **Disjunction** ($\phi \lor \psi$): true in $\mathcal{M}$ iff at least one of $\phi$ or $\psi$ is true in $\mathcal{M}$.
  - **Implication** ($\phi \rightarrow \psi$): true in $\mathcal{M}$ iff $\phi$ is false or $\psi$ is true.

- **Quantifiers**:
  - **Universal Quantifier** ($\forall x[\phi(x)]$): true in $\mathcal{M}$ iff $\phi(x)$ is true for every $x$ in $D$.
  - **Existential Quantifier** ($\exists x[\phi(x)]$): true in $\mathcal{M}$ iff $\phi(x)$ is true for at least one $x$ in $D$.





