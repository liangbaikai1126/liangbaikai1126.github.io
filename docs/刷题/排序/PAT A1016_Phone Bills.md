**思路**：将输入的信息封装成结构，根据时间顺序进行排序。针对每个用户先寻找是否存在 on 在前，off 在后的情况，存在则一定需要输出。
找到之后寻找所有有效记录，并以分钟为单位进行 money 和 time 的计算，注意题目给出的是美分/每分钟，最后计算是用美元做单位，进制为 100。

```cpp
#include<iostream>
#include<string>
#include<cstdio>
#include<algorithm>
using namespace std;

const int MAXN = 1005;

typedef struct {
    string name;
    int dd, hh, mm, month;
    bool isOnline;
} Record;

Record rec[MAXN], temp;

int toll[30], N;

bool cmp(Record a, Record b) {
    if(a.name != b.name) return a.name < b.name;
    else if(a.month != b.month) return a.month < b.month;
    else if(a.dd != b.dd) return a.dd < b.dd;
    else if(a.hh != b.hh) return a.hh < b.hh;
    else return a.mm < b.mm;
}

void proc_valid(int on, int off, int& time, int& money) {
    temp = rec[on];
    while(temp.dd < rec[off].dd || temp.hh < rec[off].hh || temp.mm < rec[off].mm) {
        time++;
        money += toll[temp.hh];
        temp.mm++;
        if(temp.mm >= 60) {
            temp.mm = 0;
            temp.hh++;
        }
        if(temp.hh >= 24) {
            temp.hh = 0;
            temp.dd++;
        }
    }
}

int main() {
    for(int i = 0; i < 24; i++)
        cin >> toll[i];
    cin >> N;
    string status;
    for(int i = 0; i < N; i++) {
        cin >> rec[i].name;
        scanf("%d:%d:%d:%d", &rec[i].month, &rec[i].dd, &rec[i].hh, &rec[i].mm);
        cin >> status;
        if(status == "on-line") 
            rec[i].isOnline = true;
        else 
            rec[i].isOnline = false;
    }
    sort(rec, rec + N, cmp);
    // next 是下一个用户
    int on = 0, off, next, needPrint;
    while(on < N) {
        // needPrint 为是否需要输出
        needPrint = 0;
        next = on;
        while(next < N && rec[next].name == rec[on].name) {
            if(needPrint == 0 && rec[next].isOnline) {
                needPrint = 1;
            } else if(needPrint == 1 && !rec[next].isOnline) {
                needPrint = 2;
            }
            next++;
        }
        if(needPrint != 2) {
            on = next;
            continue;
        }
        cout << rec[on].name << " ";
        printf("%02d\n", rec[on].month);
        // 输出所有配对
        int totalMoney = 0;
        while(on < next) {
            while(on < next - 1 && !(rec[on].isOnline && !rec[on + 1].isOnline)) {
                on++;
            }
            off = on + 1;
            if(off == next) {
                on = next;
                break;
            }
            printf("%02d:%02d:%02d ", rec[on].dd, rec[on].hh, rec[on].mm);
            printf("%02d:%02d:%02d ", rec[off].dd, rec[off].hh, rec[off].mm);
            int money = 0, time = 0;
            // 注意得到的 money 为 cent 而非 dollar
            proc_valid(on, off, time, money);
            totalMoney += money;
            printf("%d $%.2f\n", time, money / 100.0);
            on = off + 1;
        }
        printf("Total amount: $%.2f\n", totalMoney / 100.0);
    }
    return 0;
}
```