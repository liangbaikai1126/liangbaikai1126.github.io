**思路**：用栈来模拟给定出栈顺序的过程即可
```cpp
#include<cstdio>
#include<stack>
using namespace std;

const int maxn = 1005;
int m, n, k;
int seq[maxn];

int main() {
    scanf("%d %d %d", &m, &n, &k);
    stack<int> st;
    while(k--) {
        while(!st.empty()) st.pop();
        for(int i = 0; i < n; i++) {
            scanf("%d", &seq[i]);
        }
        int idx = 0;
        bool flag = true;
        for(int i = 1; i <= n; i++) {
            st.push(i);
            if(st.size() > m) {
                flag = false;
                break;
            }
            while(!st.empty() && st.top() == seq[idx]) {
                st.pop();
                idx++;
            }
        }
        if(st.empty() && flag == true) 
            printf("YES\n");
        else printf("NO\n");
    }
    return 0;
}
```