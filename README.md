<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 

<h3>Name: Kavinraj S </h3>
<h3>Register Number: 212223100019 </h3>
<h3>Date: 6-08-2026 </h3>

<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

// A* Search Algorithm
1.  Initialize the open list
2.  Initialize the closed list
    put the starting node on the open 
    list (you can leave its f at zero)

3.  while the open list is not empty
    a) find the node with the least f on 
       the open list, call it "q"

    b) pop q off the open list
  
    c) generate q's 8 successors and set their 
       parents to q
   
    d) for each successor

        
    i) if successor is the goal, stop search
        
    ii) else, compute both g and h for successor
    successor.g = q.g + distance between successor and q
    successor.h = distance from goal to 
    successor (This can be done using many 
    ways, we will discuss three heuristics- 
    Manhattan, Diagonal and Euclidean Heuristics)
    successor.f = successor.g + successor.h

    iii) if a node with the same position as 
    successor is in the OPEN list which has a 
    lower f than successor, skip this successor

    iV) if a node with the same position as 
    successor  is in the CLOSED list which has
    a lower f than successor, skip this successor
    otherwise, add  the node to the open list
    end (for loop)
  
    e) push q on the closed list
    end (while loop)
<hr>
<h2>Sample Graph I</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/b1377c3f-011a-4c0f-a843-516842ae056a)

**Program**
```
# A* Algorithm
from collections import defaultdict
import networkx as nx
import matplotlib.pyplot as plt

def a_star(graph, heuristic, start, goal):
    open_set = {start}
    closed_set = set()
    g = {start: 0}
    parent = {start: None}
    while open_set:
        current = min(open_set, key=lambda node: g[node] + heuristic[node])
        if current == goal:
            path = []
            while current is not None:
                path.append(current)
                current = parent[current]
            path.reverse()
            print("\nShortest Path :", " -> ".join(path))
            print("Total Cost :", g[goal])
            return path
        open_set.remove(current)
        closed_set.add(current)
        for neighbour, cost in graph[current]:
            if neighbour in closed_set:
                continue
            new_cost = g[current] + cost
            if neighbour not in open_set:
                open_set.add(neighbour)
            elif new_cost >= g.get(neighbour, float('inf')):
                   continue
            g[neighbour] = new_cost
            parent[neighbour] = current
    print("Path does not exist!")
    return None

graph = defaultdict(list)
G = nx.Graph()
n, e = map(int, input("Enter number of nodes and edges: ").split())
print("\nEnter the edges (u v cost):")
for i in range(e):
    u, v, cost = input(f"Edge {i+1}: ").split()
    cost = int(cost)
    graph[u].append((v, cost))
    graph[v].append((u, cost))
    G.add_edge(u, v, weight=cost)

print("\nAdjacency List")
for node in graph:
    print(node, "->", graph[node])

heuristic = {}
print("\nEnter Heuristic Values")
for i in range(n):
    node, h = input(f"{i+1}. Node Heuristic : ").split()
    heuristic[node] = int(h)
print("\nHeuristic Values")
print(heuristic)

plt.figure(figsize=(8,6))                                # Draw Original Graph

pos = nx.spring_layout(G, seed=20)
nx.draw_networkx_nodes(G, pos, node_color="skyblue", node_size=1800)
nx.draw_networkx_labels(G, pos, font_size=12,font_weight="bold")
nx.draw_networkx_edges(G, pos, width=2)
edge_labels = nx.get_edge_attributes(G, 'weight')
nx.draw_networkx_edge_labels(G, pos,  edge_labels=edge_labels)
plt.title("Original Graph")
plt.axis("off")
plt.show()

start = input("\nEnter Start Node: ")
goal = input("Enter Goal Node: ")
if start not in graph:
    print("Invalid Start Node!")
elif goal not in graph:
    print("Invalid Goal Node!")
else:
    shortest_path = a_star(graph, heuristic, start, goal)
    if shortest_path:                                              
        path_edges = []
        for i in range(len(shortest_path)-1):
            path_edges.append((shortest_path[i], shortest_path[i+1]))
        plt.figure(figsize=(8,6))

        nx.draw_networkx_nodes(G, pos, node_color="skyblue",  node_size=1800)
        nx.draw_networkx_labels(G, pos, font_size=12,   font_weight="bold")

        # Draw all edges
        nx.draw_networkx_edges(G , pos, edge_color="gray", width=2)



        # Highlight shortest path
        nx.draw_networkx_edges(G, pos,  edgelist=path_edges, edge_color="red", width=4)
        nx.draw_networkx_edge_labels(G, pos, edge_labels=edge_labels)
        plt.title("Shortest Path Highlighted (Red)")
        plt.axis("off")
        plt.show()
```


<hr>
<h2>Input</h2>
<hr>
Enter number of nodes and edges: 6 7

Enter the edges (u v cost):

Edge 1: A B 4

Edge 2: A C 3

Edge 3: B D 5

Edge 4: C D 2

Edge 5: C E 6

Edge 6: D G 4

Edge 7: E G 2

Adjacency List

A -> [('B', 4), ('C', 3)]

B -> [('A', 4), ('D', 5)]

C -> [('A', 3), ('D', 2), ('E', 6)]

D -> [('B', 5), ('C', 2), ('G', 4)]

E -> [('C', 6), ('G', 2)]

G -> [('D', 4), ('E', 2)]

Enter Heuristic Values

 Node Heuristic : A 7

 Node Heuristic : B 6
 
 Node Heuristic : C 4
 
 Node Heuristic : D 2
 
 Node Heuristic : E 1
 
 Node Heuristic : G 0

<hr>
<h2>Output</h2>
<hr>
<img width="1917" height="1077" alt="image" src="https://github.com/user-attachments/assets/920b89e2-08f3-4b84-9bd5-d930ee8563c8" />

**Result:**

The Python program to Implement A * Search algorithm for a Graph has been executed successfully.
