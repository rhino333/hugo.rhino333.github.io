---
title: "이베이 코딩 테스트 후기"
date: 2018-06-05T23:51:54+09:00
draft: true
---
2018.06.02(토) 오후 2시 코딩 테스트를 쳤다.  
<a href='https://www.hackerrank.com'>https://www.hackerrank.com</a> 에서 지원서에 썼던 이메일로 로그인하여 시험을 진행했다. 시험 40분 전 쯤 메일이 와서 테스트 문제를 3개 풀어보고 2시 부터 5시 사이에 내가 원하는 시간에 75분간 시험을 치를 수 있었다. 나는 시험을 계속 기다리고 있었기 때문에 2시가 되자마자 시험을 진행했다.  

# 1번 문제

C++에서 class간의 상속을 구현하는 간단한 문제였다. 하지만 class를 쓴지 오래되다보니 기억이 잘 나지 않아 문제만 읽어보고 나중에 풀 요량으로 처음엔 넘겼다. 다시 문제를 풀러 돌아왔을 때는 시간이 충분하지 않아 마음이 급했다. 당장 virtual 상속도 기억나지 않았고, 어떻게 상속받은 부모 class의 생성자를 초기화 하는지도 몰라 급하게 검색을 하며 꾸역꾸역 문제를 풀었다. 다행히 출력 결과는 답과 맞았지만 구현된 코드는 전혀 보기 좋지 않았다. C++에서 class와 상속에 대해 복습하고 괜찮은 예제를 찾아 연습을 더 해야겠음을 느꼈다.  

{% gist rhino333/4dc53847527b64f20bd265961d7cb35b %}
```C++
#include <string>
#include <iostream>
using namespace std;

class Animal {
protected:
    bool isMammal;
    bool isCarnivorous;
    
public:
    Animal(bool isMammal, bool isCarnivorous) {
        this->isMammal = isMammal;
        this->isCarnivorous = isCarnivorous;
    }
    
    bool getIsMammal() {
        return this->isMammal;
    }
    
    bool getIsCarnivorous() {
        return this->isCarnivorous;
    }
    
    virtual string getGreeting() = 0;
    
    void printAnimal(string name) {
        cout << "A " << name << " says '" << this->getGreeting()
        <<"', is " << (this->getIsCarnivorous() ? "" : "not ")
        << "carnivorous, and is " << (this->getIsMammal() ? "" : "not ")
        << "a mammal." << endl;
    }
};

// Write your code here. Do not use a 'public' access modifier in your class declaration.

class Dog: public Animal {
public:
    
    string getGreeting() {
        return "ruff";
    }
    
    Dog():Animal(true, true) {
        this->isMammal = true;
        this->isCarnivorous = true;
    }
    
};

class Cow: public Animal {
public:
    string getGreeting() {
        return "moo";
    }
    
    Cow():Animal(true, false) {
        this->isMammal = true;
        this->isCarnivorous = false;
    }
};

class Duck: public Animal {
public:
    string getGreeting() {
        return "quack";
    }
    
    Duck():Animal(false, false) {
        this->isMammal = false;
        this->isCarnivorous = false;
    }
};

int main()
{
    Dog dog;
    Animal *pointer = &dog;
    dog.printAnimal("dog");
    
    Cow cow;
    pointer = &cow;
    cow.printAnimal("cow");
    
    Duck duck;
    pointer = &duck;
    duck.printAnimal("duck");
}
```

# 2번 문제
두 문자열(공백 포함)을 입력받는다. 두번째로 입력받은 문자열의 단어에 속하지 않은 단어를 첫번째 문자열에서 골라 입력받은 단어 순서로 출력하는 것이었다. C++에서 문자열 입출력 관련 문제를 많이 풀어보지 않아 조금 당황했다. 바로 구글링하여 꽤 괜찮은 소스를 발견했다.  

### C++에서 공백 기준으로 문자열 자르기
```C++
#include <iostream>
#include <sstream>
#include <string>

using namespace std;

int main()
{
    string s("string to split");
    istringstream iss(s);

    do
    {
        string sub;
        iss >> sub;
        cout << "Substring: " << sub << endl;

    } while (iss);

}
```
출처 : <a href='https://hashcode.co.kr/questions/87/스트링-공백으로-split-하기'>https://hashcode.co.kr/questions/87/스트링-공백으로-split-하기</a>  

찾은 코드로 문자열을 공백 기준으로 자른 뒤 각 단어를 unordered_map에 key: string, value: int로 하여 카운트했다. 이때 카운트 값이 1이면 두번째 문자열에 포함된 단어가 아니라는 뜻이므로 이를 출력하게했다. 그런데 테스트 케이스 2개 중 첫번째가 계속 오답으로 나와 끝내 해결하지 못하고 제출했다. 코드를 Xcode에 옮겨 다시 실행 해봤을 때는 별 문제가 없었다. 아마 시험 환경의 컴파일러 문제라던가 내가 찾지 못한 다른 원인이 있을 것 같지만 현재로썬 알 길이 없다. 이 문제 덕분에 C++에서 문자열 자르는 방법에 대해 알게되었으니 만족해야지. 다만, 같은 문제를 python으로 구현하니 코드가 굉장히 짧아 놀랐다. 앞으로는 C++과 Python 두 언어를 주력으로 사용할 수 있게끔 연습해두어야 겠다는 생각이 들었다.  

## C++ 코드
```C++
#include <map>
#include <iostream>
#include <vector>
#include <string>
#include <sstream>
#include <algorithm>
#include <unordered_map>

using namespace std;

int main() {
    string s, t;
    getline(cin,s);
    getline(cin,t);
    unordered_map<string,int> mp;
    istringstream iss(s);
    do
    {
        string sub;
        iss >> sub;
        cout << sub << ' ';
        mp[sub]++;
    } while (iss);
    puts("");
    istringstream ist(t);
    do
    {
        string sub;
        ist >> sub;
        cout << sub << ' ';
        mp[sub]++;
    } while (ist);
    puts("");
    for(auto& m : mp) {
        if(m.second == 1) cout << m.first << '\n';
    }
}
```
### Python 코드
```Python
s = input()
t = input()
s = s.split(' ')
t = t.split(' ')

for x in s:
    if x not in t:
        print(x)
```

# 3번 문제
3번 문제는 10진수 숫자를 입력 받으면 이를 적절한 로마자로 변형하여 출력하는 것이었다. 2번 문제에서 한참 해매다가 시간을 얼마 안 남기고 본 터라 제대로 구현을 생각도 못해봤다.  
구글링을 해보니 괜찮은 소스가 있다. 파이썬으로 구현할 때는 type에 관해 신경쓰지 않고 빠르게 알고리즘을 작성할 수 있어서 정말 좋았다. 그동안 C++로만 알고리즘을 연습해와서 익숙하다고 생각했는데, 그 익숙함을 넘어서는 편의를 파이썬이 제공해 주었다. 

### Python 코드
```python
def int2roman(num):
    def to(n, d):
        if n == 9 : return d[0]+d[2]
        if n >= 5 : return d[1]+to(n - 5, d)
        if n == 4 : return d[0]+d[1]
        if n >= 1 : return d[0]+to(n - 1, d)
        return ''
 
    def i2rd(n, d):
        if n == 0: return ''
        return i2rd(n / 10, d[2:])+to(n % 10, d)
 
    digit = 'IVXLCDMF'
    return i2rd(num, digit)

print(int2roman(1238))
```
출처: <a href='http://bab2min.tistory.com/224'>http://bab2min.tistory.com/224</a>

### C++ 코드
```C++
#include <iostream>
using namespace std;

string digit = "IVXLCDMF";

string int2roman(int n, string d) {
    if(n == 9){
        return d[0]+d[2]+"";
    }
    if(n >= 5) {
        return d[1]+int2roman(n-5, d);
    }
    if(n==4) {
        return d[0]+d[1]+"";
    }
    if(n>=1) {
        return d[0] + int2roman(n-1, d);
    }
    return "";
}

string i2d(int n, string d) {
    if(n == 0) return "";
    return i2d(n/10,d.substr(2,d.size()-2))+int2roman(n%10, d);
}

int main() {
    int num;
    cin >> num;
    cout << i2d(num,digit);
}
```
# 4번 문제
BST를 구현하는 것이었나? 문제가 굉장히 길고 시간도 없어서 문제를 끝까지 못읽었다. 아마 BST를 구현해서 숫자 배열을 입력 받고 하나의 숫자를 입력 받은 후 구현한 자료구조를 사용해 해당 숫자가 배열 내에 있는지 확인하는 것이었던 것 같다.  

# 5번 문제
좌표 관련된 것이었는데 문제조차 제대로 읽지 못했다.

시험을 치르면서 익숙하지 않은 온라인 환경에서의 코딩과 불편한 디버깅 때문에 상당히 애를 먹었다. 그 덕분에 원활한 디버깅이 되지 않았고 2번 문제에 많은 시간을 소모하게되었다. 또한, 모든 문제가 영어로 출제되었는데 4번, 5번 문제 같은 경우 지문이 길어 시간 안에 문제를 이해하기조차 버거웠다.  

##### 참으로 아쉬운 시험이었다.  
내가 class와 상속에 익숙했더라면, 디버깅을 잘 지원해줬더라면, 혹은 코드를 복붙해서 바로 Xcode로 실행만 시킬 수 있엇다면...  
지금 와서 후회는 의미 없겠지만 내가 부족한 부분들을 알게됐다는 것에 의의를 두어야겠다. 기업에서 요구하는 코딩 능력과 내가 그간 연습해온 알고리즘 문제 풀이 능력 사이에 괴리가 있었구나도 느꼈다.  

C++로만 알고리즘 문제풀이를 연습했었는데, 이번 Python으로 문제를 풀어보면서 참 좋은 언어라는 것을 새삼 느꼈다. 알고리즘의 구현은 빠르게 Python으로 하고 성능이 필요한 부분에서 C++을 같이 쓴다면 효과적일 것 같단 생각이 들었다. 앞으로는 Python으로도 알고이즘 문제풀이 연습을 병행해야겠다.
다음 시험에선 아쉽지 않기 위해.