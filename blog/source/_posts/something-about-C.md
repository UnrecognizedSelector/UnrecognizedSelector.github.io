---
title: something about C
date: 2017-03-02 15:21:08
tags:
---

###UVa 272 - TEX Quotes

```
#include <iostream>
using namespace std;
int main() {
    string line;
    bool isleft = true;
    while(getline(cin, line)){
        for(int i = 0; i < line.length(); i++){
            if(line[i] == '"'){
                if(isleft)
                    cout << "``";
                else
                    cout << "''";
                isleft = !isleft;
            }
            else
                cout << line[i];
        }
        cout << endl;
    }
    return 0;
}
```
水题。字符替换。

###UVa 10082 - WERTYU

```
#include <iostream>
#include <string>
using namespace std;

int main() {
    const string letter = {"`1234567890-=QWERTYUIOP[]\ASDFGHJKL;'ZXCVBNM,./"};
    string line;
    int i , j;
    while(getline(cin, line)){
        for(i = 0; i < line.length(); i++){
            for(j = 0; j < letter.length(); j++){
                if(line[i] == letter[j]){
                    cout << letter[j - 1];
                    break;
                }
            }
            if(j >= letter.length()){
                cout << line[i];
            }
        }
        cout << endl;
    }
    return 0;
}
```
第一次提交WA，对着书检查发现是字符数组存在问题。用数组的方式来解决这类非常庞大的键盘键位替换问题真是一种好方法……

###UVa 401 - Palindromes

```
#include <iostream>
#include <string>
#include <cctype>
using namespace std;

const string letter = {"A   3  HIL JM O   2TUVWXY51SE Z  8 "};
const string msg[] = {  " -- is not a palindrome.",
                        " -- is a regular palindrome.",
                        " -- is a mirrored string.",
                        " -- is a mirrored palindrome." };

char tomir(char c) {
    if(isalpha(c))
        return letter[c - 'A'];
    return letter[c - '0' + 25];
}

int main() {
    string line;
    int i , j;
    while(getline(cin, line)){
        int len = line.length();
        int p = 1, m = 1;
        for(i = 0; i < (len + 1) / 2; i++){
            if(line[i] != line[len - i - 1])
                p = 0;
            if(tomir(line[i]) != line[len - i - 1])
                m = 0;
        }
        cout << line << msg[p + m * 2] << endl;
        cout << endl;
    }
    return 0;
}
```
这道题目要总结的东西太多了，感觉都可以拿出来单独写……首先思路上我是完全参考书本，看完题目之后想过许多方案，但是范例的解答实在太让人佩服。把握整体之后自己码了一遍。
字符串`letter`存储的即是26个字母和9个数字的镜像，没有以空格替代；
字符串数组`msg`存储的是题目要求输出的四句话，最后以非常巧妙的方式简化了输出；
值得注意的是以上均为全局变量；
函数`tomir`将字符转化为对应的镜像字符；
*cctype*下包含的多个函数需要了解。

###UVa 1586 - Molar mass

```
#include <iostream>
#include <cstdio>
#include <string>
#include <cctype>
#include <cmath>
using namespace std;

double mass(char c){
    if(c == 'C') return 12.01;
    else if(c == 'H') return 1.008;
    else if(c == 'N') return 14.01;
    else if(c == 'O') return 16.0;
    else return 0;
}
int toInt(char c){
    return c - '0';
}

int main() {
    int t, i;
    cin >> t;
    getchar();
    while(t--){
        double total = 0, tmp = 0;
        int cnt = 0;

        string line;
        getline(cin, line);

        for(i = 0; i < line.length(); ++i){
            if(isalpha(line[i])){
                if(cnt != 0){
                    total += cnt * tmp;
                    cnt = 0;
                    tmp = 0;
                }
                if(isdigit(line[i + 1]))
                    tmp += mass(line[i]);
                else
                    total += mass(line[i]);
                //cout << cnt << " " << tmp << " " << total << endl;
            }
            else if(isdigit(line[i])){
                if(isdigit(line[i - 1])){ continue; }
                else if(isdigit(line[i + 1])){
                    cnt = toInt(line[i]) * 10 + toInt(line[i + 1]);
                }
                else{
                    cnt = toInt(line[i]);
                }
                //cout << cnt << " " << tmp << " " << total << endl;
            }
        }
        if(cnt == 0)
            total += tmp;
        else
            total += cnt * tmp;


        printf("%.3f\n",total);
    }
    return 0;
}
```
求化学式的原子量。值得注意的是在有限量输入后的`getline`之前，要把数组组数后面的换行符吃掉，使用`getchar()`需包含*cstdio*。