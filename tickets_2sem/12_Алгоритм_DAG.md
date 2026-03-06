## Алгоритм DAG

### Идея

В DAG (_Directly Acyclic Graph_) можно найти кратчайшие пути от стартовой вершины до остальных, выполнив топологическую сортировку, а затем в ее порядке выполнять релаксацию ребер.


### Релаксация

**Релаксация** - атомарная операция в алгоритмах кратчайших путей. 
Релаксацией ребра `(u, v)` с весом `w` называют операцию `dist[v] = min(dist[v], dist[u] + w)`. 
При уменьшении `dist[v]` обновляют `parent[v] = u` для корректного восстановления пути в дальнейшем.


### Принцип работы

1. Проверяем, что граф - ацикличный
2. Получаем топологический порядок вершин
3. В топологическом порядке делаем релаксацию, обновляя `dist[v]` и `parent[v]`

### Структуры данных

- Список смежности: `vector<vector<pair<int, long long>>>`
- Массив расстояний `dist`: `vector<int>`
- Массив предков `parent`: `vector<int>`


### Сложность

- Временная: `O(V + E)`: топ сорт `O(V + E)` и релаксация `O(E)`
- Пространственная: `O(V)`

### Уточнение

Алгоритм корректен и для отрицательных весов.

### Реализация на C++

```cpp
const long long INF = std::numeric_limits<long long>::max();

bool dag_alg(int n
            , const std::vector<std::vector<std::pair<int, long long>>>& adj
            , int s
            , std::vector<long long>& dist
            , std::vector<int>& parent) {
    
    std::vector<int> top_s;
    bool is_cyclic = top_sort(n, adj, top_s);
    if (!is_cyclic) return false;

    dist.assign(n, INF);
    parent.assign(n, -1);

    dist[s] = 0;

    for (int v : top_s) {
        if (dist[v] == INF) continue;
        for (const auto& e : adj[v]) {
            int u = e.first;
            long long w = e.second;
            if (dist[v] > dist[u] + w) {
                dist[v] = dist[u] + w;
                parent[v] = u;
            }
        }
    }
    return true;
}
```