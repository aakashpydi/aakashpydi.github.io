---
published: false
---
## Decision Tree Implementation

I implemented three variants of a decision tree from scratch in this project. They are (a) a binary decision tree with no pruning using the ID3 algorithm, (b) a binary decision tree with a given maximum depth, and (c) a binary decision tree with post-pruning using reduced error pruning. [(Link to Github Repo of Source Code)](https://github.com/aakashpydi/DecisionTreeImplementation). 

Here is some analysis I carried out to understand and compare the different implementations. 

### Vanilla Model
A binary decision tree with no pruning using the ID3 algorithm.

![](/images/decision_tree_images/vanilla_node_count.png)

![](/images/decision_tree_images/vanilla_accuracies.png)

---

### Depth Limited Model
A binary decision tree with a given maximum depth.

![](/images/decision_tree_images/depth_node_count.png)

![](/images/decision_tree_images/depth_accuracies.png)

![](/images/decision_tree_images/depth_optimal_depth.png)

---

### Decision Tree with Post Pruning
A binary decision tree with post pruning.

![](/images/decision_tree_images/prune_node_count.png)

![](/images/decision_tree_images/prune_accuracies.png)

---
