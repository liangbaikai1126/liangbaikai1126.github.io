**思路**：直接进行字符串模拟，暴力求解答案。
```cpp
#include<iostream>
#include<string>
using namespace std;

int main() {
    string A, B, C;
    cin >> A >> B;
    int G_A = 0, S_A = 0, K_A = 0;
    int G_B = 0, S_B = 0, K_B = 0;
    int G_C, S_C, K_C;
    int cnt = 0;
    for(int i = 0; i < A.size(); i++) {
        if(A[i] == '.') cnt++;
        else if(cnt == 0) G_A = 10 * G_A + A[i] - '0';
        else if(cnt == 1) S_A = 10 * S_A + A[i] - '0';
        else if(cnt == 2) K_A = 10 * K_A + A[i] - '0';
    }
    cnt = 0;
    for(int i = 0; i < B.size(); i++) {
        if(B[i] == '.') cnt++;
        else if(cnt == 0) G_B = 10 * G_B + B[i] - '0';
        else if(cnt == 1) S_B = 10 * S_B + B[i] - '0';
        else if(cnt == 2) K_B = 10 * K_B + B[i] - '0';
    }
    G_C = G_A + G_B, S_C = S_A + S_B, K_C = K_A + K_B;
    S_C += K_C / 29, K_C %= 29;
    G_C += S_C / 17, S_C %= 17;
    C = to_string(G_C) + "." + to_string(S_C) + "." + to_string(K_C);
    cout << C;
    return 0;
}
```