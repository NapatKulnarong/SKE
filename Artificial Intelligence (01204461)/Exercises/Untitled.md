## Agglomerative Hierarchical Clustering (Single Linkage)

Points: A(1,1), B(1,2), C(1,5), D(6,1), E(6,3)

**Euclidean distance**
$$d(p,q)=\sqrt{(x_p-x_q)^2+(y_p-y_q)^2}$$

Examples: $d(A,B)=\sqrt{0^2+1^2}=1$, $d(D,E)=\sqrt{0^2+2^2}=2$, $d(B,C)=3$, $d(A,D)=5$, $d(B,D)=\sqrt{5^2+1^2}\approx5.10$, $d(A,E)=\sqrt{5^2+2^2}\approx5.39$

**Single linkage** (distance between clusters = closest pair)
$$d(C_i,C_j)=\min_{a\in C_i,\;b\in C_j} d(a,b)$$

### (a) Merges

**Iter 1:** smallest entry is $d(A,B)=1.00$, so merge $\{A,B\}$, $h_1=1.00$
- $d(AB,C)=\min(4.00,\,3.00)=3.00$
- $d(AB,D)=\min(5.00,\,5.10)=5.00$
- $d(AB,E)=\min(5.39,\,5.10)=5.10$

**Iter 2:** smallest is $d(D,E)=2.00$, so merge $\{D,E\}$, $h_2=2.00$
- $d(AB,DE)=\min(5.00,\,5.10)=5.00$
- $d(C,DE)=\min(6.40,\,5.39)=5.39$

**Iter 3:** $d(AB,C)=3.00 < d(AB,DE)=5.00 < d(C,DE)=5.39$, so merge $\{A,B,C\}$, $h_3=3.00$
- $d(ABC,DE)=\min(5.00,\,5.39)=5.00$

**Iter 4:** merge $\{A,B,C\}$ and $\{D,E\}$, $h_4=5.00$ (closest pair is $A$–$D$)

### (b) Cut at $h=2.50$
Merges with $h\le2.5$: $h_1,h_2$. Clusters $=3$: $\{A,B\},\{C\},\{D,E\}$

### (c) Cut at $h=4.00$
Merges with $h\le4$: $h_1,h_2,h_3$. Clusters $=2$: $\{A,B,C\},\{D,E\}$

### (d) Dendrogram (heights)
```
h=5  ┌───────┴───────┐
h=3  ├───┐           │
h=2  │   │       ┌───┤
h=1  ├─┐ │       │   │
     A B C       D   E
```
Bars: $AB$ at 1, $DE$ at 2, $(AB)C$ at 3, $(ABC)(DE)$ at 5.