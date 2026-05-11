## Association Rule Learning

Association Rule Learning is a technique in Machine Learning and Data Mining used to discover interesting relationships, patterns, or associations among items in large datasets.

It answers questions like:

- Which products are bought together?
- What events tend to occur together?
- If X happens, how likely is Y?

The most famous application is market basket analysis.

Example:

> Customers who buy bread and butter often buy milk.

**Core Idea**

Association rule learning tries to find rules of the form:

$$
X→Y
$$

Meaning:

> If X occurs, Y is likely to occur too.

Where:

- $X$ = antecedent (left-hand side)
- $Y$ = consequent (right-hand side)

Example:
$$
{Bread,Butter}→{Milk}
$$
Interpretation: People buying Bread and Butter also tend to buy Milk.

## Apriori Algorithm

The **Apriori algorithm** is a classic algorithm in **data mining** and **machine learning** used for **association rule learning**. It helps discover patterns like:

> “Customers who buy bread and butter also tend to buy milk.”

It is widely used in:

* Market basket analysis
* Recommendation systems
* Inventory planning
* Web usage mining
* Fraud detection

The algorithm was introduced by the Data Mining community to efficiently find **frequent itemsets** and generate **association rules**.

**Advantages**

1. Simple and Easy: Very intuitive algorithm.
2. Good for Sparse Data: Works well when transactions are not huge.
3. Strong Theoretical Foundation: Association rule mining became popular because of Apriori.

**Disadvantages of Apriori**

1. Multiple Database Scans: Needs repeated scanning of dataset.
2. Candidate Explosion: Too many candidates for large datasets.
3. Slow on Big Data: Not ideal for massive databases.

**Improvements Over Apriori**

Several advanced algorithms improve Apriori:

| Algorithm | Improvement                  |
| --------- | ---------------------------- |
| FP-Growth | Avoids candidate generation  |
| ECLAT     | Uses vertical dataset format |
| AIS       | Early association mining     |
| DHP       | Hash-based pruning           |

Apriori is a foundational algorithm for:

* Frequent itemset mining
* Association rule learning

Main workflow:

1. Find frequent itemsets
2. Prune infrequent ones
3. Generate larger candidates
4. Produce association rules

Key principle:

> “All subsets of a frequent itemset must also be frequent.”

**Why Apriori Works Efficiently**

Without pruning, checking all combinations is exponential.

If there are n items:

$$
2^n -1
$$

possible itemsets exist.

Apriori reduces search space using:

* Minimum support threshold
* Apriori property
* Candidate pruning

## Core Idea of Apriori

[Video Explanation](https://www.youtube.com/watch?v=o_hnNBM0jtk)

Apriori works on the principle:

> **If an itemset is frequent, then all of its subsets must also be frequent.**

This is called the **Apriori Property**.

For example:

If `{Bread, Milk, Butter}` appears frequently in transactions, then:

* `{Bread, Milk}`
* `{Bread, Butter}`
* `{Milk, Butter}`
* `{Bread}`
* `{Milk}`
* `{Butter}`

must also appear frequently.

This property allows the algorithm to **prune** many impossible combinations early, making it efficient.

## Important Terminology

### Itemset

A set of items.

* `{Bread}`
* `{Milk, Bread}`
* `{Beer, Diaper, Milk}`

### Transaction

A collection of items bought together.

| Transaction ID | Items                     |
| -------------- | ------------------------- |
| T1             | Bread, Milk               |
| T2             | Bread, Diaper, Beer, Eggs |
| T3             | Milk, Diaper, Beer, Coke  |
| T4             | Bread, Milk, Diaper, Beer |
| T5             | Bread, Milk, Diaper, Coke |

### Support

Support measures how often an itemset appears.

$$
Support(A)=\frac{\text{Transactions containing A}}{\text{Total Transactions}}
$$

If Bread appears in 4 out of 5 transactions:

$$
Support(Bread)=\frac{4}{5}=0.8
$$

### Confidence

Confidence measures the reliability of a rule.

For rule:

$$
A \rightarrow B
$$

confidence is:

$$
Confidence(A \rightarrow B)=\frac{Support(A \cup B)}{Support(A)}
$$

If 3 people buy both Bread and Milk, and 4 buy Bread:

$$
Confidence(Bread \rightarrow Milk)=\frac{3}{4}=0.75
$$

Meaning:
75% of people who buy Bread also buy Milk.

### Lift

Lift tells whether two items are actually related.

$$
Lift(A \rightarrow B)=\frac{Confidence(A \rightarrow B)}{Support(B)}
$$
Interpretation:

* Lift > 1 → positive association
* Lift = 1 → independent
* Lift < 1 → negative 

## Working of Apriori Algorithm

The algorithm works level by level.

### Find Frequent 1-itemsets

Count occurrences of each individual item.

Suppose minimum support = 60%.

**Count Individual Items**

| Item   | Count | Support |
| ------ | ----- | ------- |
| Bread  | 4     | 80%     |
| Milk   | 4     | 80%     |
| Diaper | 4     | 80%     |
| Beer   | 3     | 60%     |
| Coke   | 2     | 40%     |
| Eggs   | 1     | 20%     |

So frequent 1-itemsets are:

$$
L_1 = {Bread, Milk, Diaper, Beer}
$$

Coke and Eggs are removed.

### Generate Candidate 2-itemsets

Create combinations from frequent 1-itemsets.

Candidates:

* {Bread, Milk}
* {Bread, Diaper}
* {Bread, Beer}
* {Milk, Diaper}
* {Milk, Beer}
* {Diaper, Beer}

**Count Supports**

| Itemset       | Count | Support |
| ------------- | ----- | ------- |
| Bread, Milk   | 3     | 60%     |
| Bread, Diaper | 3     | 60%     |
| Bread, Beer   | 2     | 40%     |
| Milk, Diaper  | 3     | 60%     |
| Milk, Beer    | 2     | 40%     |
| Diaper, Beer  | 3     | 60%     |

Frequent 2-itemsets:

$$
L_2 = { \{Bread, Milk\}, \{Bread, Diaper\}, \{Milk, Diaper\}, \{Diaper, Beer\}}
$$

### Generate Candidate 3-itemsets

> To generate a 3-itemset, Apriori joins two frequent 2-itemsets that share one common item.

Combine frequent 2-itemsets.

**Possible** candidate:

$$
{Bread, Milk, Diaper}
$$

Check support:

Appears in T4 and T5 only.

Support:

$$
\frac{2}{5}=40%
$$

Below minimum support. So no frequent 3-itemsets. Algorithm stops.

## Generating Association Rules

Now generate rules from frequent itemsets.

Take:

$$
{Bread, Milk}
$$

**Possible** rules:

1. Bread → Milk
2. Milk → Bread

### Rule 1: Bread → Milk

$$
Confidence=\frac{Support(Bread,Milk)}{Support(Bread)}
$$

$$
=\frac{3/5}{4/5}
=\frac{3}{4}
=75%
$$

Interpretation:

75% of Bread buyers also buy Milk.

### Rule 2: Milk → Bread

$$
Confidence=\frac{3/5}{4/5}=75%
$$

Same in this dataset.

## Apriori Algorithm Pseudocode

```text
L1 = frequent 1-itemsets

for (k = 2; Lk-1 not empty; k++):

    Generate candidate itemsets Ck from Lk-1

    Scan database and count supports

    Lk = candidates in Ck with support >= minimum support

Return all frequent itemsets
```

## Small Real-Life Example

Suppose a coffee shop observes:

| Customer | Purchased              |
| -------- | ---------------------- |
| C1       | Coffee, Sandwich       |
| C2       | Coffee, Cake           |
| C3       | Coffee, Sandwich, Cake |
| C4       | Sandwich, Juice        |
| C5       | Coffee, Sandwich       |

Minimum support = 40%

Frequent itemsets:

* Coffee
* Sandwich
* Cake
* {Coffee, Sandwich}

Possible rule:

$$
Coffee \rightarrow Sandwich
$$

This helps the shop:

* Bundle products
* Place items nearby
* Create combo offers

## Complexity

Apriori has exponential worst-case complexity because of combinational itemset generation.

Time complexity depends on:

* Number of transactions
* Number of unique items
* Minimum support threshold

Lower support threshold → more candidates → slower execution.

## Question
<details>
<summary>Question 1</summary
                     
Given the transaction database:

| TID | Items   |
| --- | ------- |
| T1  | A, B, C |
| T2  | A, C    |
| T3  | A, B    |
| T4  | B, C    |
| T5  | A, B, C |

Minimum Support = 60%

Tasks:

1. Find all frequent 1-itemsets.
2. Find all frequent 2-itemsets.
3. Determine whether any 3-itemset is frequent.

</details>
<details>
<summary>Question 2</summary>

Transactions:

| TID | Items               |
| --- | ------------------- |
| T1  | Milk, Bread         |
| T2  | Bread, Butter       |
| T3  | Milk, Bread, Butter |
| T4  | Milk                |
| T5  | Bread, Milk         |

Tasks:

1. Calculate:

   * Support(Milk)
   * Support(Bread)
   * Support(Milk, Bread)

2. Calculate confidence for:
   $$
   Milk \Rightarrow Bread
   $$

3. Determine whether the rule is strong if:

   * Minimum Confidence = 70%

</details>

<details>
<summary>Question 3</summary>

Transactions:

| TID | Items   |
| --- | ------- |
| T1  | A, B    |
| T2  | A, C    |
| T3  | A, B, C |
| T4  | B, C    |
| T5  | A, B, C |
| T6  | A, B    |

Minimum Support = 50%

Tasks:

1. Generate candidate 1-itemsets.
2. Find frequent 1-itemsets.
3. Generate candidate 2-itemsets.
4. Find frequent 2-itemsets.
5. Generate candidate 3-itemsets.
6. Apply pruning.
7. Find final frequent itemsets.

</details>

<details>
<summary>Question 4</summary>

Transactions:

| TID | Items               |
| --- | ------------------- |
| T1  | Bread, Butter, Milk |
| T2  | Bread, Milk         |
| T3  | Butter, Milk        |
| T4  | Bread, Butter       |
| T5  | Bread, Butter, Milk |

Tasks:

1. Find support count of all itemsets.
2. Calculate:
   $$
   Confidence(Bread \Rightarrow Butter)
   $$
3. Calculate:
   $$
   Confidence(Butter \Rightarrow Milk)
   $$
4. Find lift for:
   $$
   Bread \Rightarrow Milk
   $$

</details>

<details>
<summary>Question 5</summary>
    
Transactions:

| TID | Items   |
| --- | ------- |
| T1  | A, B, D |
| T2  | B, C    |
| T3  | A, B, C |
| T4  | A, C    |
| T5  | B, C, D |

Minimum Support = 40%

Tasks:

1. Find:

   * Frequent 1-itemsets
   * Frequent 2-itemsets
   * Frequent 3-itemsets

2. Generate all possible association rules.

3. Compute confidence for each rule.

</details>

<details>
<summary>Question 6</summary>
    
Transactions:

| TID | Items                 |
| --- | --------------------- |
| T1  | Pen, Notebook         |
| T2  | Pen, Pencil           |
| T3  | Notebook, Pencil      |
| T4  | Pen, Notebook, Pencil |
| T5  | Pen, Notebook         |

Minimum Support = 40%
Minimum Confidence = 60%

Tasks:

1. Find all frequent itemsets.
2. Generate all valid association rules.
3. Determine which rules satisfy minimum confidence.

</details>

<details>
<summary>Question 7</summary>
    
Transactions:

| TID | Items   |
| --- | ------- |
| T1  | A, B, C |
| T2  | A, B    |
| T3  | B, C    |
| T4  | A, C    |
| T5  | A, B, C |
| T6  | A, B, C |

Minimum Support = 50%

Tasks:

1. Compute support for:

   * A
   * B
   * C
   * AB
   * AC
   * BC
   * ABC

2. Which itemsets are frequent?

3. Generate rules from:
   $$
   {A,B,C}
   $$

</details>

<details>
<summary>Question 8</summary
                       
Transactions:

| TID | Items               |
| --- | ------------------- |
| T1  | Tea, Sugar          |
| T2  | Tea, Biscuit        |
| T3  | Tea, Sugar, Biscuit |
| T4  | Sugar, Biscuit      |
| T5  | Tea, Sugar          |

Tasks:

1. Find support count for all 1-itemsets.

2. Find support count for all 2-itemsets.

3. Determine frequent itemsets for:

   * Minimum Support = 60%

4. Generate rules with:

   * Minimum Confidence = 70%

</details>

<details>
<summary>Question 9</summary>
    
Transactions:

| TID | Items   |
| --- | ------- |
| T1  | A, B    |
| T2  | A, C    |
| T3  | B, C    |
| T4  | A, B, C |
| T5  | A, B    |

Tasks:

1. Calculate:
   $$
   Support(A \cup B)
   $$
2. Calculate:
   $$
   Confidence(A \Rightarrow B)
   $$
3. Calculate:
   $$
   Lift(A \Rightarrow B)
   $$

</details>
<details>
<summary>Question 10</summary>
    
Transactions:

| TID | Items               |
| --- | ------------------- |
| T1  | Milk, Bread, Eggs   |
| T2  | Bread, Butter       |
| T3  | Milk, Bread         |
| T4  | Eggs, Butter        |
| T5  | Milk, Bread, Butter |

Minimum Support = 40%

Tasks:

1. Perform Apriori step-by-step.

2. Show:

   * Candidate itemsets
   * Pruned itemsets
   * Frequent itemsets

3. Generate association rules from the final frequent itemsets.
</details>
