---
title: buu-re-简单注册器
tags:
  - CTF
  - RE
  - buu
abbrlink: 55aa4ca9
date: 2021-03-10 20:59:15
---

拿到题目后，是一个APK文件，使用jdx反编译后在mainActivity里面发现了一段核心代码

```
                if (flag == 1) {
                    char[] x = "dd2940c04462b4dd7c450528835cca15".toCharArray();
                    x[2] = (char) ((x[2] + x[3]) - 50);
                    x[4] = (char) ((x[2] + x[5]) - 48);
                    x[30] = (char) ((x[31] + x[9]) - 48);
                    x[14] = (char) ((x[27] + x[28]) - 97);
                    for (int i = 0; i < 16; i++) {
                        char a = x[31 - i];
                        x[31 - i] = x[i];
                        x[i] = a;
                    }
                    textview.setText("flag{" + String.valueOf(x) + "}");
                    return;
                }

```

然后将此段用C程序写出来即可得到答案

```c
#include<iostream>

using namespace std;

int main()
{
	char x[] = "dd2940c04462b4dd7c450528835cca15";
                    x[2] = (char) ((x[2] + x[3]) - 50);
                    x[4] = (char) ((x[2] + x[5]) - 48);
                    x[30] = (char) ((x[31] + x[9]) - 48);
                    x[14] = (char) ((x[27] + x[28]) - 97);
                    for (int i = 0; i < 16; i++) {
                        char a = x[31 - i];
                        x[31 - i] = x[i];
                        x[i] = a;
                    }
    cout<<x<<endl;
    return 0;
}
```

求得flag：`59acc538825054c7de4b26440c0999dd`