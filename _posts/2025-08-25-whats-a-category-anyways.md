---
layout: post
title: What's a category anyways?
date: 2025-08-25T20:57:31.571Z
categories:
    - category theory
fmContentType: post
draft: true
slug: category
---

My [last post]({% post_url 2025-08-17-what-are-categories-of-algebras-over-an-equational-theory %}) ended with a promise that my next post - this one - would recap the interesting technical challenges in formalizing the category of algebras of an equational theory.
Unfortunately, the UniMath library has been facing technical challenges recently, and my work here (as well as the great work of several other mathematicians in the community) has not yet been merged into the main branch.
As a non-math friend recently asked me what categories are, I figure that capstone post can wait for the PR to be merged and in the meantime I can introduce categories.

There are many, many tutorials on category theory available on the internet, some of which I will link as we go, but my goal differs from many tutorials in that I hope to reach non-mathematicians as well.

## 1. Monoids revisited

Categories may be seen as a joint generalization of two seemingly unrelated concepts.
The first is of monoids.
A **monoid** is:

- a set $M$,
- a binary multiplication $(m,n) \mapsto m*n : M \to M \to M$,
- a unit element $1 \in M$, such that:
  - multiplication is _unital_, i.e. for any $m \in M$, we have $m * 1 = 1 * m = m$,
  - and multiplication is _associative_, i.e. for any $x, y, z \in M$, we have $x\*(y\*z)=(x\*y)\*z$.

So, multiplication in a monoid may be written unambiguously without the brackets.
Note that, in the language of universal algebra introduced in the last post, monoids are algebras over a signature $((1,0),(\*,2))$ with one nullary and one binary operation!

### Example 1.1. Natural numbers

The canonical monoid - in a sense, the "free-est" nontrivial monoid - is the set $\mathbb{N}$ of natural numbers with addition (and $0$!).
Unitality and associativity of addition is left as an exercise for grade schoolers.

### Example 1.2. Natural numbers, revisited

To demonstrate that the operations chosen on our set are nontrivial pieces of structure, think about $\mathbb{N}$ with _multiplication_.
This time, our multiplicative unit is $1$, and associativity and unitality of multiplication is again left as an exercise.

Note how much more complex this example is!
Notate [$\mathbb{N}$ with addition](#example-11-natural-numbers) as $(\mathbb{N},+)$ and [$\mathbb{N}$ with multiplication](#example-12-natural-numbers-revisited) as $(\mathbb{N},\*)$.
In $(\mathbb{N},+)$, new numbers may only really be created by adding $1$ over and over again, but in $(\mathbb{N},\*)$, multiplication by any of the infinitely many prime numbers sort of branches off the numbers you can see in a whole new direction[^1].

### Example 1.3. Endomorphisms of spaces

This will be perhaps the largest motivating example to get where we're going.
Let $X$ be a space, in whatever way that means to your heart, and think about all of the functions $X \to X$.
Functions may of course be _composed_: for $f, g : X \to X$, we have a function $f \circ g = x \mapsto g(f(x)) : X \to X$, and there is also a natural _identity map_ $id = x \mapsto x : X \to X$.
Composition turns out to be associative and unital, and so we can consider the **endomorphism monoid** $End(X)$ of $X$.

We learn a great deal about $X$ - in a sense, everything space-like we could want to know about it[^2] - by understanding its endomorphism monoid.
The only real unfortunate thing about $End(X)$ is that it doesn't tell us how $X$ talks with other spaces - say, there's a $Y$ out in the universe, and what does $X \to Y$ behave like?
We yearn to know...

## 2. Posets??

What if we don't care _how_ things are related but merely if they are?
Orderings are a solid tool in the box here.

### Definition 2.1. Propositions

A proposition is, informally, a thing that's true or false.
Thinking of a definition of a set $X$ as defining some idea, and elements of $X$ as proofs of that idea, we may say $X$ is a **proposition** when it has at most one element.

The idea here is that propositions should be those things which don't care about _how_ they're proved, only if they are true or false.
For example, the set of all divisors of a number $n$ can have many different numbers, but the set asking only if $m$, say, divides $n$ is a proposition.

Now let $Prop$ notate the class of all propositions.

### Definition 2.2. Preorders

A **preorder** on $X$ is a binary relation $\leq : X \to X \to Prop$ that is:

- reflexive: for all $x \in X$, we have $x \leq x$, and
- transitive: for all $x, y, z \in X$, if $x \leq y$ and $y \leq z$, then $x \leq z$.

Perhaps start noticing that reflexivity looks somewhat like the unit element in a monoid, transitivity looks obliquely like a multiplication of relations between elements, and the proposition-valued-ness of our relation suggests that that transitivity-operation is associative in a sense.
Hmm...

Preorders are a somewhat unnatural concept, in my opinion, as one can have funny instances where both $x \leq y$ and $y \leq x$ but not conclude that $x = y$, as seen in the following:

#### Example 2.2.1. Computation behavior

Think about how long it takes to compute some function, say, $f : \mathbb{N} \to \mathbb{N}$, in whatever form computation means to you.
How long does it take to perform the computation, perhaps how much paper do you need if you were to compute it by hand?
Say a computable function $f$ is _bounded_ by a function $g$ - that $f = O(g)$ - if computing $f(n)$, for large enough $n \in \mathbb{N}$, eventually takes no more than a constant multiple of $g(n)$-many steps.
The relation defined by $f \leq g \iff f = O(g)$ is a preorder on the set of computable functions.

Look at how lossy this is!
Very different functions can be judged to have the same computational behavior by this relation - $n \mapsto n+m$ is always $O(1)$ regardless of the $m$.

In a sense, less important from this [time-complexity](https://en.wikipedia.org/wiki/Computational_complexity_of_mathematical_operations) point of view is the functions themselves than their "complexity classes".
This motivates the concept of a partial order:

### Definition 2.3. Partial orders

A preorder $(X , \leq)$ is a **partial order** when it is _antisymmetric_, i.e. when we may indeed conclude from $x \leq y, y \leq x$ that $x = y$.
A set equipped with a partial order is called a **poset**.

I regard posets as the more natural objects, in that, when we can't tell the difference between $x,y$ via our available order-theoretic machinery, then $x=y$ genuinely.
In that way, posets are preorders that agree with equality.

#### Example 2.3.1. Computation behavior, revisited

The set of **complexity classes** of computable functions, with the natural ordering relation, is a poset.
This is true for abstract nonsense reasons, but concretely, the quotient we take by forgetting function definitions and only remembering their complexity classes is compatible with their ordering.

#### Example 2.3.2. Natural numbers, revisited

There are two natural poset structures on $\mathbb{N}$:

- $m \leq n$ in the usual sense from grade school, and
- $m \| n$, where "less than" really means "divides".

This divisibility example is one where we see that, in a poset, not every pair of elements needs to be related!
Here, $2$ does not divide $3$, and neither does $3$ divide $2$.

#### Example 2.3.3. Social hierarchies

This example is from [Eugenia Cheng's][cheng] keynote talk at 2025's Joint Mathematics Meeting in Seattle.

Let $S$ denote the set of social classes in a society, and say $A \leq B$ if social class $B$ has institutional power over social class $A$.
Classes always have some degree of authority over themselves; this is the reflexivity relation.
Transitivity here means that, if $C$ has power over $B$ who has power over $A$, then $C$ also has power over $A$.

A question Cheng asks her students: is $(S,\leq)$ a partial order or a preorder?
That is, if both $A$ and $B$ have power over each other, do $A$ and $B$ represent the _same_ social class?
It's always an interesting class discussion.

## 3. Categories, finally

We're ready.

### Definition 3.1. Categories

A **category** is:

- a space $C$ of objects,
- for pairs $x, y \in C$, a set $hom(x,y)$ of _morphisms_ from $x$ to $y$,
- for each $x \in C$, a designated _identity_ morphism $id \in hom(x,x)$,
- for triples $x,y,x \in C$, a _composition_ map $\circ : hom(y,z)\to hom(x,y)\to hom(x,z)$, such that:
  - composition by $id$ is unital, i.e. for any $f \in hom(x,y)$, and notating $id_x, id_y$ for the identities at $x, y$ resp., we have $f \circ id_x = id_y \circ f = f$, and
  - composition is associative, i.e. for tuples $x,y,z,w \in C$ and $f \in hom(x,y), g\in hom(y,z), h\in hom(z,w)$, we have $h \circ (g \circ f) = (h \circ g) \circ f \in hom(x,w)$.

### Example 3.2. Monoids

Every monoid $(M,\*,1)$ defines a category $BM$ with merely[^3] one object $\star$.
We have $hom(\star,\star)=M$, $\circ = \*$ i.e. composition is multiplication in the monoid, and $id = 1$.
That $BM$ is a category is immediate from the monoid axioms.

### Example 3.3. Posets

Every poset $(P, \leq)$ defines a category $BP$.
The objects of $BP$ are the elements of $P$, and $hom(x,y) = (x \leq y)$; here, we see the utility of thinking of propositions as special sets.
Reflexivity implies the existence of identity morphisms, transitivity gives us our composition, and associativity and unitality follow from proposition-valued-ness of $\leq$.

This should make clear how categories generalize both monoids and posets!
Categories are like monoids in that we can "multiply" morphisms in an associative and unital way, but generalized in that not every pair of morphisms can be multiplied.
They are like posets in that not every pair of elements needs to be "related", but the "relation" is still transitive (associative) and reflexive (unital), but generalized in that elements can be "related" to each other in more than one way.

Categories are both a very abstract concept with a huge number of examples, and a highly unifying one with a wealth of tools.
We'll explore some of these tools in the future!

[^1]: This may be seen as a version of the [Fundamental Theorem of Arithmetic!][fta]

[^2]: This philosophy is motivated by the [Yoneda lemma][yoneda].

[^3]: Eventually you may hope to think of $BM$ as really having a _connected groupoid_ of objects arising from invertible elements in $M$, but it's good enough to think of it as only having one object. This is also another reason why we care about posets rather than preorders; "invertible elements" in a preorder $P$, i.e. pairs $x \leq y, y \leq x$, should really be thought of as the same.

[fta]: https://en.wikipedia.org/wiki/Fundamental_theorem_of_arithmetic
[yoneda]: https://bartoszmilewski.com/2015/09/01/the-yoneda-lemma/
[cheng]: https://eugeniacheng.com/