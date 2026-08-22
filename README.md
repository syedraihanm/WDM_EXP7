### EX7 Implementation of Link Analysis using HITS Algorithm
### DATE: 22.08.26
### AIM: To implement Link Analysis using HITS Algorithm in Python.
### Description:
<div align = "justify">
The HITS (Hyperlink-Induced Topic Search) algorithm is a link analysis algorithm used to rank web pages. It identifies authority and hub pages 
in a network of web pages based on the structure of the links between them.

### Procedure:
1. ***Initialization:***
    <p>    a) Start with an initial set of authority and hub scores for each page.
    <p>    b) Typically, initial scores are set to 1 or some random values.
  
2. ***Construction of the Adjacency Matrix:***
    <p>    a) The web graph is represented as an adjacency matrix where each row and column correspond to a web page, and the matrix elements denote the presence or absence of links between pages.
    <p>    b) If page A has a link to page B, the corresponding element in the adjacency matrix is set to 1; otherwise, it's set to 0.

3. ***Iterative Updates:***
    <p>    a) Update the authority scores based on the hub scores of pages pointing to them and update the hub scores based on the authority scores of pages they point to.
    <p>    b) Calculate authority scores as the sum of hub scores of pages pointing to the given page.
    <p>    c) Calculate hub scores as the sum of authority scores of pages that the given page points to.

4. ***Normalization:***
    <p>    a) Normalize authority and hub scores to prevent them from becoming too large or small.
    <p>    b) Normalize by dividing by their Euclidean norms (L2-norm).

5. ***Convergence Check:***
    <p>    a) Check for convergence by measuring the change in authority and hub scores between iterations.
    <p>    b) If the change falls below a predefined threshold or the maximum number of iterations is reached, the algorithm stops.

6. ***Visualization:***
    <p>    Visualize using bar chart to represent authority and hub scores.

### Program:

```python
import numpy as np
import matplotlib.pyplot as plt

def hits_algorithm(adjacency_matrix, max_iterations=100, tol=1.0e-6):
    num_nodes = len(adjacency_matrix)
    authority_scores = np.ones(num_nodes)
    hub_scores = np.ones(num_nodes)
    
    for _ in range(max_iterations):
        # Authority update: sum of incoming hubs
        new_authority = np.dot(adjacency_matrix.T, hub_scores)
        new_authority /= np.linalg.norm(new_authority, ord=2)

        # Hub update: sum of outgoing authorities
        new_hub = np.dot(adjacency_matrix, new_authority)
        new_hub /= np.linalg.norm(new_hub, ord=2)

        # Convergence check
        authority_diff = np.linalg.norm(new_authority - authority_scores, ord=2)
        hub_diff = np.linalg.norm(new_hub - hub_scores, ord=2)
        
        authority_scores, hub_scores = new_authority, new_hub
        
        if authority_diff < tol and hub_diff < tol:
            break
            
    return authority_scores, hub_scores

# 4-node directed adjacency matrix
# Row i represents directed edges from Node i -> Node j
adj_matrix = np.array([
    [0, 1, 1, 0],  
    [0, 0, 1, 1],  
    [0, 0, 0, 1],  
    [1, 0, 0, 0]   
])

# Run HITS algorithm
authority, hub = hits_algorithm(adj_matrix)

# Print initial node scores
for i in range(len(authority)):
    print(f"Node {i}: Authority Score = {authority[i]:.4f}, Hub Score = {hub[i]:.4f}")

# Print Hub rankings (descending order without mutating the score arrays)
hub_ranking = np.argsort(hub)[::-1]
print("\nRanking based on Hub Scores:")
for rank, node_idx in enumerate(hub_ranking, 1):
    print(f"Rank {rank}: Node {node_idx} (Score = {hub[node_idx]:.4f})")

# Print Authority rankings (descending order)
auth_ranking = np.argsort(authority)[::-1]
print("\nRanking based on Authority Scores:")
for rank, node_idx in enumerate(auth_ranking, 1):
    print(f"Rank {rank}: Node {node_idx} (Score = {authority[node_idx]:.4f})")

# Bar chart visualization
nodes = np.arange(len(authority))
bar_width = 0.35

plt.figure(figsize=(8, 5))
plt.bar(nodes - bar_width/2, authority, bar_width, label='Authority', color='royalblue')
plt.bar(nodes + bar_width/2, hub, bar_width, label='Hub', color='forestgreen')

plt.xlabel('Node')
plt.ylabel('Scores (L2-Normalized)')
plt.title('HITS Algorithm: Authority vs Hub Scores (4 Nodes)')
plt.xticks(nodes, [f'Node {i}' for i in nodes])
plt.legend()
plt.tight_layout()
plt.show()
```

### Output:
<img width="1306" height="499" alt="Screenshot 2026-08-22 155224" src="https://github.com/user-attachments/assets/1d4bfcc2-e6aa-4158-aaf9-6ea8b94dc9ff" />


### Result:

Thus the implement Link Analysis using HITS Algorithm in Python was successful
