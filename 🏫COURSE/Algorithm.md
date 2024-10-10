# Fundamentals 
## Logic
### Propositional logic(命题逻辑)
#### position
* **definition**
	* a declarative sentence 
	* either true or false
* notation
	* propositional variables:  $p, q, r, s, p1, p2$
#### Compound proposition
* definition:
	* many statements are formed by **combining** one or more propositions
	* using **connectives**: not, and, or, if...then, if and only if
#### implication/conditional statement 推导箭头
* ![[Pasted image 20241008152019.png]]
	* **注意**：只有当条件为真，结论为假时，推导才为假
#### Tautology and contradiction
* Tautology (重言式)：无论其组成命题的真值如何，整个公式总是为真。
* Contradiction（矛盾式）：无论其组成命题的真值如何，整个公式总是为假
#### Logical equivalence(逻辑等价性)
##### Definition
* If p ↔ q is a tautology, formulas *p* and *q* are called **logically equivalent**
*  *p ≡ q* denotes that p and q are logically equivalent.(⇐⇒ sometimes used instead of ≡)
#### Theorem
##### Fundamental equivalences of proposition logic
* Idempotent laws幂等率
	* 重复应用同一操作不会改变结果
	* (p ∧ p) ≡ p, (p ∨ p) ≡ p
* Commutative laws交换律
* Associative laws 结合律
* Absorption laws吸收率
	* *p ∧ (p ∨ q) ≡ p*
	- *p ∨ (p ∧ q) ≡ p*
- Distributive laws分配律
	- *p ∧ (q ∨ r) ≡ (p ∧ q) ∨ (p ∧ r)* 
	- *p ∨ (q ∧ r) ≡ (p ∨ q) ∧ (p ∨ r)*
- Double negation
- De Morgan's laws
	- *¬(p ∧ q) ≡ (¬p ∨ ¬q) *
	- *¬(p ∨ q) ≡ (¬p ∧ ¬q)*
- Tertium non datur排中率
	- 对于任何命题要么真要么假,没有中间状态
	- *(p ∧ ¬p) ≡ F*
	- *(p ∨ ¬p) ≡ T*
- Identity laws 同一律
	- *(p ∧ T) ≡ p* 合取同一律 
	- *(p ∨ F) ≡ p* 析取同一律
- Domination laws 支配律
	- *(p ∨ T) ≡ T*
	- *(p ∧ F) ≡ F* 
##### Equivalences using conditional statements(使用条件语句的等价)
1. $p → q ≡ ¬p ∨ q$  *Disjunctive form of conditional(条件句的析取形式)*
2.  $p → q ≡ ¬q → ¬p$ *contraposition逆否命题*
3. $p ∨ q ≡ ¬p → q$  *Conditional form of disjunction(析取的条件形式)*
4. $p ∧ q ≡ ¬(p → ¬q)$ *Negated Conditional Form of Conjunction(合取的否定条件形式)*
5. $¬(p → q) ≡ p ∧ ¬q$
6. $(p → q) ∧ (p → r) ≡ p → (q ∧ r)$
7. $(p → r) ∧ (q → r) ≡ (p ∨ q) → r$
8. $(p → q) ∨ (p → r) ≡ p → (q ∨ r)$
9. $(p → r) ∨ (q → r) ≡ (p ∧ q) → r$
##### Equivalences using bi-conditional statements
1. $p ↔ q ≡ (p → q) ∧ (q → p)$
2.  $p ↔ q ≡ ¬p ↔ ¬q$  
3.  $p ↔ q ≡ (p ∧ q) ∨ (¬p ∧ ¬q)$  
4.  $¬(p ↔ q) ≡ p ↔ ¬q$
#### 余下的概念英文列举
* negation 否定
* conjunction 和
* disjunction 或
* converse 否命题
* contrapositive 逆否命题
* Bi-conditional/bi-implications statement 双条件语句
	* ↔ 当且仅当
### Predicate logic（谓词逻辑）
#### example
* x = y + 3
* Q(x, y), where x and y are *variables* and Q is the *predicate*
* When values are *assigned* to the variables x and y, the statement Q(x, y) has a *truth value*.
解释：
* 谓词可以用符号表示，如例子中的 Q。
- 谓词与变量结合（如 Q(x, y)）形成命题函数。
- 当给变量赋值后，命题函数就变成了一个具体的、可以判断真假的命题。
#### N-ary predicates
* $P (x_1, x_2, . . . , x_n)$  is also called n-ary predicate
* the form above is the **value** of the propositional function P at the n-tuple $(x_1, x_2, . . . , x_n)$
#### quantification
##### Universal quantification (全称量词)
* ∀x 
* negation
	* $¬∀x P (x) ≡ ∃x ¬P (x)$
##### Existential quantification（存在量词）
* ∃x
* negation
	* $¬∃x P (x) ≡ ∀x ¬P (x)$
##### De Morgan’s Laws for quantifiers
* $¬∃x P (x)≡ ∀x ¬P (x)$
* $¬∀x P (x)≡∃x ¬P (x)$
## Sets And Relations
### Sets
* $x ∈ A$
* $x \notin A$
#### Definition
##### Roster method (列举法)/Extensional set notation（外延表示法）
* By enumerating the elements
* $A := \{ 0, 1, 2, 3, 4, 5 \} = \{0, 1, 2, · · · , 5 \}$
##### Set builder notation（集合构建器表示法）/Intensional set notation(内涵表示法)
* By characteristic properties of the elements
* $B := \{x | x ∈ A \ and \ x \ is \ even \}$
* ***:*** instead of *|*  and ***a comma ,*** instead of *and*
	* $B = \{x : x ∈ A, x even \}$
##### Russell's Paradox(罗素悖论)
* $R = \{A \ | \ A \ is\  a \ set, A ∉ A \}$
	* question is :Does the set RRR contain itself?
	* Suppose $R∈R$
		* by the definition of R, if R is a member of R, then R must satisfy the condition $R \notin R$
		*  lead to a contradiction
	* Suppose $R \notin R$
		* by the definition of R, if R is not a member of R, then R should satisfy the condition to be included in R.
#### Properties
* all elements in a set are **distinct**
* not in a fixed ordering
* the same set can be described in different ways
* sets may contain different kinds of elements
#### Notation for standard sets of numbers
* ℕ := {0, 1, 2, 3, ...} set of natural numbers
* ℤ := {0, 1, -1, 2, -2, 3, -3, ...} set of integers
* ℤ>0 := {1, 2, 3, ...} set of positive natural numbers
* ℚ := {a/b | a, b ∈ ℤ, b ≠ 0} set of rational numbers(有理数)
* ℝ := the set of real numbers
* ℝ>0 := the set of positive real numbers
#### Empty set
* ∅, or by {}

#### Cardinality（基数）
##### Definition
* 是一个描述集合大小或元素数量的概念
$$
|M| := \begin{cases}
\text{the number of elements in } M, & \text{if } M \text{ is finite} \\
\infty, & \text{otherwise.}
\end{cases}
$$
#### Subsets and supersets
* subset
	* A ⊆ B
* proper subset
	* A ⊂ B 或 A ⊊ B
* superset
	* A ⊇ B
* proper super set
	* B ⊃ A 或 B ⊋ A
#### Power set(幂集)
#####  Definition
* The power set of a set S is the set of **all subsets** of S
##### Notation
* P(s) also denotes by Pow(s) or by $2^S$
#### Set operations
* Intersection 交集
* Union 并集
* Difference 差集
* Symmetric difference 对称差
	* $A ⊕ B := (A − B) ∪ (B − A)$ 
	* also denoted by A △ B
* Complements 补集
	* A̅
#### Laws for Sets
* Absorption laws(吸收率)
	* $A ∩ (A ∪ B) = A$
	- $A ∪ (A ∩ B) = A$
* Distributive laws(分配律)
	* $A ∩ (B ∪ C) = (A ∩ B) ∪ (A ∩ C)$
	- $A ∪ (B ∩ C) = (A ∪ B) ∩ (A ∪ C)$
- De Morgan's laws(得摩根律)
	- $\overline{A \cap B} = \overline{A} \cup \overline{B}$
	- $\overline{A \cup B} = \overline{A} \cap \overline{B}$
* Identity laws(同一律)
	* A ∩ U = A 
	* A ∪ ∅ = A
#### 
### Pairs and tuples
* they ordered
### Relations
