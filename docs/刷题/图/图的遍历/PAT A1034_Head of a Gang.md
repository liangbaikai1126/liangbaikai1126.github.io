**思路**：通过邻接矩阵构建帮派图，通过DFS确定不同的连通分量来确定帮派个数以及人数。

注意给出的图结构为无向图，为结点添加完权重之后，由最大权重确定帮派牢大。

整个帮派的阈值由边权确定，即每条边只使用一次，使用后将边权置0，防止重复使用边。

注意题目给出最大的n为1000，但考虑到通话是双方的，因此maxn应该大于2000。
```cpp
#include<iostream>
#include<cstdio>
#include<string>
#include<map>
using namespace std;

const int maxn = 2010;

map<string, int> Str2Int;
map<int, string> Int2Str;
map<string, int> Gang;
bool visit[maxn];
int G[maxn][maxn], weight[maxn];
int N, K, num;

int transform(string s) {
    if(Str2Int.find(s) != Str2Int.end())
        return Str2Int[s];
    Str2Int[s] = ++num;
    Int2Str[num] = s;
    return num;
} 

void DFS(int nowVisit, int& head, int& numMember, int& totalWeight) {
    numMember++;
    visit[nowVisit] = true;
    if(weight[nowVisit] > weight[head])
        head = nowVisit;
    for(int i = 1; i <= num; i++)
        if(G[nowVisit][i] > 0) {
            totalWeight += G[nowVisit][i];
            G[nowVisit][i] = G[i][nowVisit] = 0;
            if(!visit[i]) DFS(i, head, numMember, totalWeight);
        }
}

int main() {
    cin >> N >> K;
    string s1, s2;
    int w;
    for(int i = 0; i < N; i++) {
        cin >> s1 >> s2 >> w;
        int id1 = transform(s1);
        int id2 = transform(s2);
        G[id1][id2] += w;
        G[id2][id1] += w;
        weight[id1] += w;
        weight[id2] += w;
    }
    int head, numMember, totalWeight;
    for(int i = 1; i <= num; i++) 
        if(!visit[i]) {
            head = i, numMember = 0, totalWeight = 0; 
            DFS(i, head, numMember, totalWeight);
            if(numMember > 2 && totalWeight > K)
                Gang[Int2Str[head]] = numMember;
        }
    cout << Gang.size() << endl;
    for(auto &res : Gang) 
        cout << res.first << " " << res.second << endl;
    return 0;
}
```