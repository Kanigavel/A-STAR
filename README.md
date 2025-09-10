<h1>ExpNo 4 : Implement A* search algorithm for a Graph</h1> 
<h3>Name: Kanigavel M      </h3>
<h3>Register Number: 212224240070          </h3>
<H3>Aim:</H3>
<p>To ImplementA * Search algorithm for a Graph using Python 3.</p>
<H3>Algorithm:</H3>

``````
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
          successor.g = q.g + distance between 
                              successor and q
          successor.h = distance from goal to 
          successor (This can be done using many 
          ways, we will discuss three heuristics- 
          Manhattan, Diagonal and Euclidean 
          Heuristics)
          
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

``````

<hr>
<h2>Sample Graph I</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/b1377c3f-011a-4c0f-a843-516842ae056a)

<hr>

<h2> Program : <h2>
    


    
    import heapq
    class Node:
        def __init__(self, name, g=0, h=0, parent=None):
            self.name = name
            self.g = g  
            self.h = h  
            self.f = g + h
            self.parent = parent

        def __lt__(self, other):
            return self.f < other.f

    def astar_search(graph, heuristics, start, goal):
        open_list = []
        closed_list = set()

        start_node = Node(start, g=0, h=heuristics[start])
        heapq.heappush(open_list, start_node)

        while open_list:
            current = heapq.heappop(open_list)

            if current.name == goal:
                path = []
                while current:
                    path.append(current.name)
                    current = current.parent
                return path[::-1]   # reverse

            closed_list.add(current.name)

            for (neighbor, cost) in graph[current.name]:
                if neighbor in closed_list:
                    continue

                g = current.g + cost
                h = heuristics.get(neighbor, 0)  
                neighbor_node = Node(neighbor, g, h, current)

               
                if any(open_node.name == neighbor and open_node.f <= neighbor_node.f for open_node in open_list):
                    continue

                heapq.heappush(open_list, neighbor_node)

        return None



    if __name__ == "__main__":
        v, e = map(int, input().split())
        graph = {}

        for _ in range(e):
            u, vtx, cost = input().split()
            cost = int(cost)
            if u not in graph:
                graph[u] = []
            if vtx not in graph:
                graph[vtx] = []
            graph[u].append((vtx, cost))
            graph[vtx].append((u, cost))   

        heuristics = {}
        for _ in range(v):
            try:
                node, h = input().split()
                heuristics[node] = int(h)
            except ValueError:
                
                break

        
        for node in graph:
            heuristics.setdefault(node, 0)

        
        start = list(graph.keys())[0]
        goal_candidates = [k for k, v in heuristics.items() if v == 0]
        goal = goal_candidates[0] if goal_candidates else list(graph.keys())[-1]

        result = astar_search(graph, heuristics, start, goal)

        if result:
            print("Path found:", result)
        else:
            print("No path found")


<h2>Sample Input</h2>
<hr>
10 14 <br>
A B 6 <br>
A F 3 <br>
B D 2 <br>
B C 3 <br>
C D 1 <br>
C E 5 <br>
D E 8 <br>
E I 5 <br>
E J 5 <br>
F G 1 <br>
G I 3 <br>
I J 3 <br>
F H 7 <br>
I H 2 <br>
A 10 <br>
B 8 <br>
C 5 <br>
D 7 <br>
E 3 <br>
F 6 <br>
G 5 <br>
H 3 <br>
I 1 <br>
J 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'F', 'G', 'I', 'J']


<hr>
<h2>Sample Graph II</h2>
<hr>

![image](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/acbb09cb-ed39-48e5-a59b-2f8d61b978a3)


<hr>
<h2>Sample Input</h2>
<hr>
6 6 <br>
A B 2 <br>
B C 1 <br>
A E 3 <br>
B G 9 <br>
E D 6 <br>
D G 1 <br>
A 11 <br>
B 6 <br>
C 99 <br>
E 7 <br>
D 1 <br>
G 0 <br>
<hr>
<h2>Sample Output</h2>
<hr>
Path found: ['A', 'E', 'D', 'G']
<H3>Output :</H3>
<img width="711" height="628" alt="image" src="https://github.com/user-attachments/assets/ce39e776-98fe-41e1-801b-baf9d5baa599" />
<img width="475" height="362" alt="image" src="https://github.com/user-attachments/assets/b5b35f6a-b29b-4aed-95e4-5146f56310a9" />



<h3>Result :</h3>
The A* Search Algorithm was successfully implemented in Python.
