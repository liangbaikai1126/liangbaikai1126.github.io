**思路**：直接进行简单的字符串模拟替换即可。
```cpp
#include<iostream>
#include<string>
#include<vector>
using namespace std;

typedef struct {
    string id, passwd;
} Info;

vector<Info> ans;

void proc_passwd(Info a) {
    bool isChanged = false;
    for(int i = 0; i < a.passwd.size(); i++) {
        if(a.passwd[i] == '1') { 
            a.passwd[i] = '@';
            isChanged = true;
        }
        else if(a.passwd[i] == '0') { 
            a.passwd[i] = '%';
            isChanged = true;
        }
        else if(a.passwd[i] == 'l') {
            a.passwd[i] = 'L';
            isChanged = true;
        }
        else if(a.passwd[i] == 'O') {
            a.passwd[i] = 'o';
            isChanged = true;
        }
    }
    if(isChanged) ans.push_back(a);
}

int main() {
    int N;
    cin >> N;
    Info temp;
    for(int i = 0; i < N; i++) {
        cin >> temp.id >> temp.passwd;
        proc_passwd(temp);
    }
    if(ans.size() == 0) {
        if(N == 1) cout << "There is 1 account and no account is modified";
        else cout << "There are " << N << " accounts and no account is modified";
    } else {
        cout << ans.size() << endl;
        for(auto &a : ans) {
            cout << a.id << " " << a.passwd << endl;
        }
    }
    return 0;
}
```