# [A Star Algorithm](https://ko.wikipedia.org/wiki/A*_알고리즘)

최단경로 알고리즘

[데이크스트라](./Dijkstra.md) 알고리즘과 유사하지만<br>
각 꼭짓점을 통과하는 최상의 경로를 추정값(```휴리스틱 추정값 h(x)```)을 매기는 방법을 이용한다.

```h(x)```를 구하는 방식<br>
 - 휴스턴 : 상,하,좌,우 4방향 ```int h = abs(dx) + abs(dy);``` <br>
 - 유클리드 : 대각선 포함 8방향 ```int h = (int)floor(sqrt(dx*dx + dy*dy));```


```f(x) = g(x) + h(x)```<br>
```g(n)``` : 출발 지점부터 꼭짓점 n 까지의 경로 가중치<br>
```h(n)``` : 꼭짓점 n 으로부터 목표까지의 추정 경로 가중치

## 변경 Log

>  탐색을 통해 도달 할 수 있는지 판단 확인했음<br>
> 
>  하지만이동경로를 찾을 수 없음<br>
>  from 테이블을 만들어서 해결<br>
> 
>  최단거리가 기록되지 않음<br>
>  F() 테이블을 만들어 cost를 비교해 짧은 거리를 from에 저장해서 해결

```cpp
#define COST_UNIT 10            // g값과 단위를 맞추기 위한 수
#define STRAIGHT_COST 10        // 상하좌우 이동시 g에 추가하는 코스트
#define DIAGONAL_COST 14        // 대각선 이동시 g에 추가하는 코스트
#define MOVEABLE_DIRECTION 8    // 모든 방향의 총 갯수
#define MAX_MAP_SIZE 1000       // 맵 사이즈

struct Node {
    Node(int px, int py, int g, int h) : px(px), py(py), g(g), h(h) {}

    int px;
    int py;
    int g;
    int h;

    int F() const { return g + h; };
    void Print(){ 
        cout << "/ x " << px << "/ y " << py << "/ g " << g << "/ h " << h <<"/ f" << F() << endl;
    }
};
struct NodeCompare
{
    bool operator()(const Node& n1, const Node& n2) const {
        return n1.F() > n2.F();
    }
};
int GetH(const int& cx, const int& cy, const int& gx, const int& gy) {
    int dx = cx - gx;
    int dy = cy - gy;
    return (int)floor(COST_UNIT * sqrt(dx * dx + dy * dy));
}
struct SFrom
{
    SFrom(int x, int y) : x(x), y(y) {}
    SFrom() {};
    int x = -1;
    int y = -1;
};

int AStar(const vector<vector<bool>>& m, const int& startx, const int& starty, const int& goalx, const int& goaly)
{
    int step = 0;
    priority_queue<Node, vector<Node>, NodeCompare> q;
    vector<vector<bool>> visit(MAX_MAP_SIZE, vector<bool>(MAX_MAP_SIZE, false));
    vector<vector<SFrom>> from(MAX_MAP_SIZE, vector<SFrom>(MAX_MAP_SIZE));
    vector<vector<int>> fTable(MAX_MAP_SIZE, vector<int>(MAX_MAP_SIZE, INT_MAX));
    q.push(Node(startx, starty, 0, GetH(startx, starty, goalx, goaly)));

    //8방향 이동
    //0포함 짝수 -> 십자, 홀수->대각
    int dirx[8] = {  0, 1, 1, 1, 0,-1,-1,-1 };
    int diry[8] = { -1,-1, 0, 1, 1, 1, 0,-1 };

    while (!q.empty()) {
        Node n = q.top();
        visit[n.py][n.px] = true;
        n.Print();
        q.pop();
        if (n.px == goalx && n.py == goaly)
        {
            break;
        }
        for (int i = 0; i < MOVEABLE_DIRECTION; i++)
        {
            int nx = n.px + dirx[i];
            int ny = n.py + diry[i];
            if (nx < 0 || ny < 0 || nx >= MAX_MAP_SIZE || ny >= MAX_MAP_SIZE || m[ny][nx] == false || visit[ny][nx] == true)
                continue;

            int gCost = i % 2 == 0 ? STRAIGHT_COST : DIAGONAL_COST;
            q.push(Node(nx, ny, n.g + gCost, GetH(nx, ny, goalx, goaly)));

            if(fTable[ny][nx] > q.top().F())
                from[ny][nx] = SFrom(n.px, n.py);
        }
    }

    int cx = goalx;
    int cy = goaly;
    cout << "x:" << cx << " y:" << cy << endl;
    while (true) {

        SFrom f = from[cy][cx];
        cx = f.x;
        cy = f.y;
        cout <<" y:" << cy << "x:" << cx << endl;
        step++;
        if(cx == startx && cy == starty)
            return step;
    }
}

```
