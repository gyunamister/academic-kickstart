*(Last updated: 17. April. 2020)*

This blog post is a supplement for Data Mining instruction at *Business Process Intelligence, RWTH-Aachen*.

### Concept
Association rule aims at discovering interesting relations between variables (mostly sets of variables) in large databases. A typical example is that customers who purchase beer are likely also to buy diapers. Importantly, we need to distinguish frequent itemset and association rules. In essence, frequent itemset is a joint probability, e.g., $P(beer,diaper)$, while association rule is a conditional probability, e.g., $P(diaper|beer)$. Thus, we can say that association rule more likely reflects the _relation_ aspect.

In fact, frequent itemsets are part of the calculation of association rules. Frequent itemsets are informally defined as itemsets having high _support_. $support(X)= \frac{N_{X }}{N}$, where $N$ is the number of instances and $N_X$ is the number of instances covering $X$. (You may understand it as the joint probability of elements in $X$).

Association rules are informally defined as relations between two sets having high _confidence_. $confidence(X \Rightarrow Y)= \frac{N_{X \cup Y}}{N_X}=\frac{support_{X \cup Y}}{support_X}$, where $N_X$ is the number of instances covering $X$ and $N_{X \cup Y}$ is the number of instances covering $X$ and $Y$. (You may understand it as conditional probability of two sets $X,Y$).

An association rule is evaluated as "good" if it has higher _support_, _confidence_ closer to 1, and _lift_ higher than 1.

(To deal with _lift_)

### Association Rule Exercise
Given the example below, let's evaluate the association rule, $Tea \Rightarrow Coffee$.

![IMAGE](resources/09E116AED17AB3B544DD8D47E242991B.jpg =718x211)

It has _support_ of $0.15$, _confidence_ of $0.75$, and _lift_ of $0.83$. Since _support_ is low and _lift_ is less than $1$, we can say that this rule is not desired.