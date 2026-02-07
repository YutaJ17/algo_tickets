## 1. Обход в глубину (DFS)

### Идея

**DFS(Depth-First-Search)** - это алгоритм обхода/поиска по графу, исследующий его как можно глубже, прежде чем вернуться назад.

### Принцип работы
1. Начинаем с исходной вершины
2. Посещаем ее и помещаем как посещенную
3. Рекурсивно посещаем все непосещенные смежные вершины
4. Когда тупик - возвращаемся назад

### Структура данных
- _Рекурсивный подход_ использует стек вызовов.
- _Итеративный подход_ явно использует стек.

### Сложность
- Временная: `O(V + E)`, где `V` - количество вершин, `E` - количество ребер
- Пространственная: `O(V)` для хранения посещенных вершин

### Применение
- Поиск пути между двумя вершинами
- Проверка связности графа
- Поиск циклов в графе
- Топологическая сортировка
- Поиск компонентов связности
- Решение головоломок (лабиринты, судоку)
- Обход деревьев (preorder, inorder, postorder)

### Реализация на C++

#### Рекурсивный DFS для графа (список смежности)

```cpp
#include <iostream>
#include <vector>
#include <list>

class Graph {
    int V; // количество вершин
    std::vector<std::list<int>> adj; // список смежности
    
    // вспомогательная рекурсивная функция
    void DFSUtil(int v, std::vector<bool>& visited) {
        // помечаем вершину как посещенную
        visited[v] = true;
        std::cout << v << " ";

        // вызываем для смежных вершин
        for (int neighbor : adj[v]) {
            if (!visited[neighbor]) {
                DFSUtil(neighbor, visited);
            }
        }
    } 

public:
    Graph(int V): V(V) {
        adj.resize(V);
    }

    // функция добавления ребра
    void addEdge(int v, int w) {
        adj[v].push_back(w);
        adj[w].push_back(v);  // unordered graph
    }

    // DFS обход, начиная с вершины v
    void DFS(int start) {
        std::vector<bool> visited(V, false);
        DFSUtil(start, visited);
    }

    // DFS для несвязного графа (обход всех компонент)
    void DFSComplete() {
        std::vector<bool> visited(V, false);

        for (int v = 0; v < V; v++) {
            if (!visited[v]) {
                DFSUtil(v, visited);
            }
        }
    }
};

int main() {
    Graph g(6);
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 3);
    g.addEdge(1, 4);
    g.addEdge(2, 4);
    g.addEdge(3, 5);

    std::cout << "рекурсивный DFS обход, начиная с вершины 0: ";
    g.DFS(0);

    std::cout << std::endl;
}
```

#### Итеративный DFS для графа (список смежности + стек)

```cpp
#include <iostream>
#include <vector>
#include <list>
#include <stack>

class Graph {
    int V;
    std::vector<std::list<int>> adj;

public:
    Graph(int V): V(V) {
        adj.resize(V);
    }

    void addEdge(int v, int w) {
        adj[v].push_back(w);
    }

    void DFS_iterative(int start) {
        std::vector<bool> visited(V, false);
        std::stack<int> s;

        s.push(start);

        while (!s.empty()) {
            int v = s.top();
            s.pop();

            if (!visited[v]) {
                std::cout << v << " ";
                visited[v] = true;
            }

            for (int neighbor : adj[v]) {
                if (!visited[neighbor]) {
                    s.push(neighbor);
                }
            }
        }
    }
};

int main() {
    Graph g(6);
    g.addEdge(0, 1);
    g.addEdge(0, 2);
    g.addEdge(1, 3);
    g.addEdge(1, 4);
    g.addEdge(2, 4);
    g.addEdge(3, 5);

    std::cout << "итеративный DFS, начиная с вершины 0: ";
    g.DFS_iterative(0);
}
```
