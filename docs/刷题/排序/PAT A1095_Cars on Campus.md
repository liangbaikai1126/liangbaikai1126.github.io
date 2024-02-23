**思路**：通过把时间转化为秒的形式，先记录所有输入的记录，再根据 id 以及 time 进行排序。对于同一个 id 的记录进行相邻的 in 和 out 的查找，并将查找结果存入 valid 数组中，同时增加这一对记录的停留时间到哈希表中，更新最大停留时间。剩下的 K 次查询，用 now 处理至当前查询时间，numCar 作为车辆记录数。由于每次查询都是根据时间进行的，即 now 和 numCar 是连续变化的。

```cpp
#include<iostream>
#include<algorithm>
#include<string>
#include<map>
#include<cstdio>
using namespace std;

const int MAXN = 10005;
const int MAXK = 80005;

int N, K;

typedef struct {
    string id, status;
    int time;
} Record;

Record all[MAXN], valid[MAXN];
map<string, int> parkTime;

int TimeToInt(int hh, int mm, int ss) {
    return hh * 3600 + mm * 60 + ss;
}

bool cmpIdTime(Record a, Record b) {
    if(a.id != b.id) return a.id < b.id;
    return a.time < b.time;
}

bool cmpTime(Record a, Record b) {
    return a.time < b.time;
}

int main() {
    cin >> N >> K;
    int hh, mm, ss;
    for(int i = 0; i < N; i++) {
        cin >> all[i].id;
        scanf("%d:%d:%d", &hh, &mm, &ss);
        cin >> all[i].status;
        all[i].time = TimeToInt(hh, mm, ss);
    }
    sort(all, all + N, cmpIdTime);
    int maxTime = -1, validNum = 0;
    for(int i = 0; i < N - 1; i++) {
        if(all[i].id == all[i + 1].id &&
           all[i].status == "in" &&
           all[i + 1].status == "out") {
            valid[validNum++] = all[i];
            valid[validNum++] = all[i + 1];
            int inTime = all[i + 1].time - all[i].time;
            if(parkTime.count(all[i].id) == 0) {
                parkTime[all[i].id] = 0;
            }
            parkTime[all[i].id] += inTime;
            maxTime = max(maxTime, parkTime[all[i].id]);
       }
    }
    sort(valid, valid + validNum, cmpTime);
    int now = 0, numCar = 0;
    for(int i = 0; i < K; i++) {
        scanf("%d:%d:%d", &hh, &mm, &ss);
        int time = TimeToInt(hh, mm, ss);
        while(now < validNum && valid[now].time <= time) {
            if(valid[now].status == "in") numCar++;
            else numCar--;
            now++;
        }
        cout << numCar << endl;
    }
    for(auto &pt : parkTime) {
        if(pt.second == maxTime) 
            cout << pt.first << " ";
    }
    printf("%02d:%02d:%02d", maxTime / 3600, maxTime % 3600 / 60, maxTime % 60);
    return 0;
}
```