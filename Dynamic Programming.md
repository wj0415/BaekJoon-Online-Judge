# Dynamic Programming 소개





큰문제를 작은 문제로 나눠서 푸는 알고리즘

다이나믹은 아무 의미가 없다.

1940년 Richard Bellman은 멋있어 보여서 사용했단다.



이 두가지를 모두 만족해야함

1. overlapping subproblem

2. optimal substructure: 문제의 정답을 작은 문제의 정답에서 찾을 수 있을 때



## Overlapping Subproblem

문제를 작은 문제들로 나눠서 푼다. 이 작은 문제들이 겹쳐야 한다.

피보나치 수열은 F0=0, F1=1, Fn = Fn-1 + Fn-2 (n>=2)

n번째 피보나치 수를 구하는 문제를 n-1번째와 n-2번째 피보나치 수를 구하는 문제로 쪼개어서 푼다.  이것이 어떻게 겹치느냐

n   은 n-1와 n-2

n-1은 n-2와 n-3

n-2은 n-3와 n-4

계속 겹침!

큰문제와 작은문제를 같은 방법으로 풀슀고 큰문제를 쪼갤수있다.



## Optimal Substructure

예시 

서울에서 부산가는 가장 빠른길: 서울->대전->대구->부산임

대전에서 부산을 가는 가장 빠른 길은 대구를 거쳐야한다.

대전 -> 대구-> 부산임. 만약 대구보다 울산을 거치는게 더 빠르다고 해버리면 서울에서 부산가는길도 대전대구가 아님 대전울산이 되어버림.

피보나치는 문제의 정답을 작은 문제의 정답을 더하는 것으로 구한다. 

그 작은문제들도 또 쪼개어서 더해서 푼다.

문제의 크기에 상관없ㅇ이 어떤 한 문제으 ㅣ정답은 일정하다.

k번째 피보나치 수를 구하면서 구한 4번째 피보나치수는 언제나 같다.



## So,,,

옵티물 서브스트럭트는 어떤 한 문제는 정답을 구할 때마다 같다. 여러번 구할 필요가 없다. 작은문제가 겹치는데 한번만 구하면 된다. 

정답을 한 번 구했으면, 정답을 어딘가에 메모해놓는다. 이것은 memoization이라고 한다. 다시 안구해도 된다! 배열에 저장하는 방식으로 코드에서 구현한다.



```c++
int fibonacci(int n){
    if (n<=1){		//예외
        return n; 
    }else{
        return fibonacci(n-1) + fibonacci(n-2);
    }
}
```

호출을 트리로 그려보면 중복이 생기는 것을 알 수있다. f(3) 이 두번 등장하는등...

중복을 제거할 수 있다. memo[n]= n번째 피보나치 수를 저장한다.

```c++
int memo[100];		//memoization 배열 생성
int fibonacci(int n){
    if (n<=1){		
        return n; 
    }else{
        if(memo[n] > 0) 	//memo가 되어있는지 확인 
            return memo[n]; //되어있다면 바로 return
        memo[n] = fibonacci(n-1) + fibonacci(n-2); //memo를 쓰는 코드
        return memo[n];
    }
}
```



## 다이나믹을 푸는 두 가지 방법

어떻게 구현하느냐의 차이이지,,, 푸는 방법이 달라지는 건 아님. 둘중에 하나라도 먼저 잘하자.

### Top Down:  보통 재귀함수를 이용해서 구현한다

1. 문제를 풀어야한다.
   - **구** : fibonacci(n)
2. 문제를 작은 문제로 나눈다.
   - **fibonacci(n-1)**과 **fibonacci(n-2)**로 문제를 나눈다.
3. 작은 문제를 푼다.
   - fibonacci(n-1)과 fibonacci(n-2)를 **호출**해 문제를 푼다.
4. 작은 문제를 풀었으니, 이제 문제를 푼다.
   - fibonacci(n-1)의 값과 fibonacci(n-2)의 값을 **더해** 문제를 푼다.

위의 피보나치 코드가 대표적인 topdown의 예시이다.

topdown의 시간복잡도를 계산하는 것이 까다롭다. 

채워야하는 칸의 수(메모배열에 몇개의 수가 채워질거냐. 피보나치는 n) * 1칸을 채우는 복잡도 (보통 함수하나의 시간복잡도임. 피보나치문제는 덧셈하나만 있다. 그래서 O(1))

피보나치는 그래서 O(N)이 됨.



### Bottom-up : 보통 for문을 이용해서 구현한다

1. 문제를 크기가 작은 문제부터 차례대로 푼다.
2. 문제의 크기를 조금씩 크게 만들면서 문제를 점점 푼다.
3. 작은 문제를 풀면서 왔기 때문에, 큰 문제는 항상 풀 수 있다.
4. 그러다보면, 언젠가 풀어야 하는 문제를 풀 수 있다.



```c++
int d[100];
int fibonacci(int n){
    d[0] = 1;
    d[1] = 1;
    for(int i = 0; i <= n; i++){ //제일작은문제:i는 2일때, 제일큰문제는 i는 n일때
        d[i] = d[i-1] + d[i-2];
    }
    return d[n];
}
```



# 문제 풀이 전략

memo[i] = i번째 피보나치 수

d[i] 또는 dp[i]라고 표현한다. 여기에 뭐가 들어갈지 정해줘야한다. 

우리의 경우는 i번째 피보나치수가 들어간다고 정의할수있다.

이제 d[i]에 어떤 식이 들어갈지 정해줘야한다. i-1번째 피보나치수 + i-2번째 피보나치수, 즉 d[i-1]+d[i-2]

이렇게 점화식을 만들어주면, 점점 테이블을 채워나가는 방식으로 dp를 풀수 있다.



정리

- 문제에서 구하려고 하는 답을 문장으로 나타낸다. 구를 우리말로 써본다
- 예시 -> 구: 피보나치수
- n번째 피보나치 수
- 이제 그 문장에 나와있는 변수의 개수만큼 메모하는 배열을 만든다.
- TopDown인 경우에는 재귀호출의 인자의 개수
- 문제를 작은 문제로 나누고,수식을 이용해서 문제를 표현해야한다.
- 두 가지 풀이법중 더 편한 방법을 먼저 사용해본다.
- dp는 문제를 많이 풀면서 감을 잡는게 중요하다...

# 문제유형1

## [1로 만들기](https://www.acmicpc.net/problem/1463)

10 -> 5 -> 4- > 2 -> 1 : 4번

10 -> 9 -> 3 -> 1	: 3번

무조건 3으로먼저 나누는건 하지말자.

1. d[n]=n을 1로 만드는데 필요한 연산의 최소개수
2. n/3으로 만드는과정, n/3을 1로 만드는 과정으로 나눈다. 두, 세 번째 방법도 식으로 나타내준다.
3. d[n] = 1 + d[n/3]
4. d[n] = d[n/2] + 1
5. d[n] = d[n-1] + 1
6. 이렇게 총 3가지 방법중 최소값을 구해야함: min(sol1,sol2,sol3)

TopDown 방식

```c++
int go(int n){
	if (n == 1) return 0;			// 예외 처리
    if (d[n] > 0) return d[n];		 // 0보다 큰값이면 메모가 되있었던 것임 
    d[n] = go(n-1) + 1;				// n을 n-1로 만드는경우 
    if(n%2 == 0) {					// n을 n/2로 만들었을 때
        int tmp = go(n/2) + 1;
        if(d[n] > tmp) d[n] = tmp;
    }
    if(n%3 == 0 ){					//n을 n/3로 만들었을대
        int tmp = go (n/3) + 1;
        if(d[n] > tmp ) d[n] = tmp;
    }
    return d[n];
}									
```

시간복잡도: 채워야하는 칸의 개수N x 한칸의 시간복잡드(식 3개를 계산했으므로 O(1)) = O(N)



BottomUp 방식

```c++
d[1] = 0; 
for(int i=2; i<=n;i++){
    d[i] = d[i-1] + 1;					//1번째 경우
    if(i%2 == 0 && d[i] > d[i/2] + 1){	 //2번째 경우
        d[i] = d[i/2] + 1;
    } 
    if(i%3 == 0 && d[i] > d[i/3] + 1){	//3번째 경우
        d[i] = d[i/3] + 1;
    }
}// 정답은 D[n]에 있다!
```



##  *나만의 정리

구를 말로 풀어서 적어본다

식으로 적어본다.(초기값, 점화식)

d[n]=~~~

재귀함수를 쓰는경우 호출함수의 인자에 n이 들어감 for문은 n만큼 메모함. 예외처리는 초기값만 따로 처음에 할당

경우에 따라 쪼개어 주고 식을 if문으로 처리한다. 

재귀함수는 알아서 잘 반환해라 d[n]을

for문은 마지막에 d[n]을 프린트하면 되겠지?

---

## [2xn 타일링](https://www.acmicpc.net/problem/11726)

- 2×n 크기의 직사각형을 1×2, 2×1 타일로 채우는 방법의 수를 구하는 프로그램을 작성하시오. 

- 첫째 줄에 n이 주어진다. (1 ≤ n ≤ 1,000)

- 첫째 줄에 2×n 크기의 직사각형을 채우는 방법의 수를 10,007로 나눈 나머지를 출력한다.



풀이

구: 2xn 직사각형을 1x2또는 2x1로 채우는 방법의 수

d[n]= d[n-1]+d[n-2] // n-1은 12를 쓰면 되고 n-2는 21을 2개 넣으면 되니깐

초기값 d[1]= 1, d[2]=2

BottomUp

```c++
int d[1000];
int ans(int n){
	d[1]=1;
	d[2]=2;
	for(int i=3; i<=n;i++){
    	d[i] = d[i-1] + d[i-2];
	}
	return d[n]/10007;
}
```

틀렸어ㅠㅠㅠ



1로만들기 문제는 n을 어떻게 변형할것인가 였다

이 문제는 **n번째열의 타일을 어떻게 놓을것인가**를 결정해야한다.

첫번째 방법은 세로로 놓는다. 앞부분은 넓이는 2xn-1

두번째 방법은 가로로 놓는다. 앞부분의 넓이는 2xn-2

초기값은 2*0: 채우는 방법은 없다

2x1은 2*0놓은거에 1놓기

2x2는 21에 하나 놓는거+ 20에 두개놓는거로 생각할수있다!

이 때 조심해야할것은 20인경우를 채우는 방법은 없지만 초기값을 1로 설정해줘야 22를 구할때 1이 카운트 될수 있다. 위에 내가 만든 식을 이욯하려면... ㅠㅠ 철학적인 문제였어. 예를 들면 공집합도 집합하나로 보는것처럼, null인 문자열도 문자열하나로 보는것도 이런 느낌이래.

하지만 내가 틀린이유는... ㅠㅠ

```c++
int d[1000];
int ans(int n){
	d[0]=1;						//초기값도 철학적으로 바꿔주었다
	d[1]=1;
	for(int i=2; i<=n;i++){
    	d[i] = d[i-1] + d[i-2];
        d[i] %= 10007;			//나머지연산을 여기에서 해줘야됨...
	}
	return d[n];
}
```

결국 구의 문제였다. 문제/입력/출력 중에 문제도 보지만 출력도 포함되어야돼. 당연한것이 dp 문제는 언제나 값이 똑같아야 하는데 마지막 구하는 값만 딱 나머지 연산하면 웃기자너.

---

## [2xn 타일링 2](https://www.acmicpc.net/problem/11727)

위와 똑같은 문제이지만 12 21 뿐만아니라 22로도 채우는 문제이다.

```c++
#include <cstdio>
int main(){
    int d[1001];
    int n;
    scanf("%d",&n);
    d[0] = 1;
    d[1] = 1;
    for(int i=2; i <= n; i++){
        d[i] = d[i-1] + 2*d[i-2];
        d[i] %= 10007;
    }
    printf("%d\n",d[n]);
}
```



## [1, 2, 3 더하기](https://www.acmicpc.net/problem/9095)

정수 n을 1,2,3의 조합으로 나타내는 방법의 수를 구하라

d[n]= 문제 구랑 같음

d[1]=1 d[2]=2 d[3]=4 or

d[1]=1 d[2]=2 d[0]=1

d[n]=d[n-1]+d[n-2]+d[n-3]

시간복잡도는 오앤이 된다. 마찬가지임.



```c++
#include <cstdio>
int main(){
    int T;
    scanf("%d",&T);
    while(T--){
    	int d[11];
    	int n;
    	scanf("%d",&n);
    	d[0]=1;
    	d[1]=1;
    	d[2]=2;
    	
    	for(int i=3;i<=n;i++){
    		d[i]=d[i-1]+d[i-2]+d[i-3];
		}
    	printf("%d\n",d[n]);
	}
}
```



---

## [붕어빵 판매하기](https://www.acmicpc.net/problem/11052)

붕어빵 N개를 가지고 있다.

붕어빵 i개를 팔아서 얻을 수 있는 수익이 P[i]일 때, N개를 모두 판매해서 얻을 수 있는 최대 수익을 구하시오.

온라인저지에서 사라진 문제인듯 

N 을 입력되고 i(1부터 n)개를 팔아서 얻을 수있는 수익이 주어진다. 짬뽕해서 얻을 수있는 최대수익 구하기 문제이다.

d[n]: n개를 팔았을때 최대수익

~~d[n]= p[1]*d[n-1] + p[2]*d[n-2] + ... + p[n-1]d[1] + p[n]d[0]~~

허걱 위처럼 곱해서 더하는게 아니라 더한것들을 비교를 해서 최대를 구하는거 아닐까. 도대체 왜 저렇게 생각을 했니 ㄷㄷㄷㄷ

max(p[i] + d[n-i]) i는 최소값이 1 최대값이 n

그래서 총 채워야하는 칸은 n 개, d[n]한칸을 채울때 n번 검사를 해야돼 오앤제곱됨

```c++
for(int i=1; i<=n;i++){
    for(int j=1;j<=i;j++){
        d[i]= max(d[i], d[j-i]+p[j]); // assume initial value is 0
    }
}
```

백준님은 붕어빵이 i 개 있을때 마지막사람에게 j개를 팔았을 경우로 딱 해석하였다. 나는 대충 해석함 ㅠㅠ



지금까지 푼 문제정리

붕어빵: 마지막사람에게 몇개를 팔건지

1,2,3더하기문제: 마지막에 올수있는 수가 1인지 2인지 3인지

타일링문제들: n번째 열에 어떤 타일이 올수있는지

접근방식유사함.

---

# 문제유형2

## [이친수](https://www.acmicpc.net/problem/2193)

- 문제: 이진수에 속하는 이친수는 0으로 시작하지 않고 1이 두번 연속으로 나타나지 않는다. 즉, 11을 부분 문자열로 갖지않는다. N은 1이상 90이하. N자리 이친수의 개수를 구해야한다.
- 입력: N
- 출력: N자리 이친수의 개수

풀이

d[n]: n자리 이친수의개수 

d[n] = d[n-2] + d[n-1]

이친수는 0,1로 이루어져 있어서 2가지의 경우로 나눠 볼수있다.

1. n번째 자리에 0이 오는경우: n-1은 0과 1동시 가능,즉 0붙이면 됨 :d[n-1]
2. n번째 자리에 1이 오는경우: n-1은 반드시 0 이여야함. n-2는 0과 1

너의 해결법! n자리에 오는 경우를 생각해보면 된다. 너는 걍 n-1가 뭐일때 n인경우를 생각했자나. 그렇게 말고 n자리에 오는 수를 고정하고 n-1이나 그보다 더 앞자리를 점차 보면됨.



풀이2

d[N] [L]: N자리 이친수의 개수중에 마지막자리가 L로 끝나는 것의 개수

0으로 끝나는 경우는 D[n] [0] = d[n-1] [0] + d[n-1] [1]

1으로 끝나는 경우는 d[n] [1] = d[n-1] [0]

정답은 dn0+dn1



이문제는... n이 90일때 d[90]이 int값을 넘어선다.... ㅠㅠㅠ 그러므로 long long을 써야한다.ㅠ



## [쉬운 계단 수](https://www.acmicpc.net/problem/10844) 

dnl: n자리 계단수 마지막수 L 

dnl = dn-1l-1 + dn-1l+1 (1<=l<=8)

dn0 = dn-11

dn9=dn-18

```c++
#include <cstdio>
#define mod 1000000000
int main(){
    
    int n;
    scanf("%d",&n);
	long long d[101][11];

    for(int i=1;i<=9;i++) d[1][i]=1;

	for(int i=2;i<=n;i++){
    	for(int j=0;j<=9;j++){
        	d[i][j] = 0;
       		if(j >= 1) d[i][j] += d[i-1][j-1];	
        	if(j <= 8) d[i][j] += d[i-1][j+1];	
        	d[i][j] %= mod;					  // type 범위를 초과해서 원치않는 값을 배열에 저장하는 경우를 방지 ex) 98 입력하면 -값나옴
    	}
	}
	
	long long ans=0;				//long long!! 
	for(int i=0;i<=9;i++)
		ans += d[n][i];
	printf("%lld\n", ans%=mod);
	
	return 0;
}
```

---

## [오르막 수](https://www.acmicpc.net/problem/11057)

자리가 오름차순을 이루는 수 . 이때, 인접한 수가 같아도 오름차순으로 친다. 수와 길이가 N이 주어졌을 때, 오르막 수의 개수를 구하는 프로그램을 작성하시오. 수는 0으로 시작할 수있다.

N은 1이상 1000이하

첫째 줄에 길이가 N인 오르막 수의 개수를 10007로 나눈 나머지를 출력한다.

```c++
#include <cstdio>
long long d[1001][11];				//전역변수로 선언해야 초기화가 됨. 지역변수로 선언하면 틀림

int main(){
    int n;
    scanf("%d", &n);
    
    for(int i=0;i<=9;i++) d[1][i] = 1;
    for(int i=2;i<=n;i++){
        for(int j=0;j<=9;j++){
            d[i][j] += (10 - j);            
        }
    }
    long long ans=0;
    for(int i=0;i<=9;i++){
        ans += d[n][i];
    }
    
    printf("%lld",ans%=10007);
    return 0;
}
```

---

## [스티커](https://www.acmicpc.net/problem/9465)

스티커 2n개가 2xn모양으로 배치되어 있다.

스티커 한장을 떼면 변을 공유하는 스티커는 모두 찢어져서 사용할 수 없다.

점수의 합을 최대로 만드는 문제



풀이

n열에 다음 3가지 경우가 올수 있다. 엑스가 안뜯는거

0	1	2

x	0	x

x	x	0

**d[n] [s] = n열의 상태가 s인 경우, n열까지의 총 점수.**

a[i] [j] 를 각 스티커의 점수라고 하자. 열행

d[n] [0] n번열에 온상태가 안뜯고 안뜯은 상태 = max(d[n-1] [0,  d[n-1] [1], d[n-1] [2]])

d[n] [1] =max(d[n-1] [0] , d[n-1] [2] + a[n] [1] )

d[n] [2] =max(d[n-1] [0] , d[n-1] [1] + a[n] [2] ) 

정답은 max(d[n] [0],  d[n] [1], d[n] [2])

```c++
#include <cstdio>

int a[2][100001];						//sticker점수
int d[100001][3];

int max(int a,int b, int c){
    return (a<b)? (b<c?c:b) : (a<c?c:a);
}

int main(){
   
   	int t;
   	scanf("%d",&t);
   	while(t--){
        int n;
   		scanf("%d",&n);
   		for(int i=0;i<2;i++){
   			for(int j=1;j<=n;j++){
   				scanf("%d", &a[i][j]);
			}
		}
        d[0][0]=d[0][1]=d[0][2]=0;
        for(int i=1;i<=n;i++){
       		d[i][0] = max(d[i-1][0], d[i-1][1], d[i-1][2]);
       		d[i][1] = max(d[i-1][0] + a[1][i], 0, d[i-1][2] + a[1][i]);
       		d[i][2] = max(d[i-1][0] + a[0][i], 0, d[i-1][1] + a[0][i]);
//       		printf("d[%d][0] = %d, d[%d][1] = %d, d[%d][2] = %d\n",i,d[i][0],i,d[i][1],i,d[i][2]);
        }
        printf("%d\n",max(d[n][0],d[n][1],d[n][2]));
	}
   	
   	
    return 0;
}
```

이문제는 다른사람들이 나보다 훨씬 잘품 ㅠㅠㅠ 막 8ms 무엇... 나는 140ms 나옴 ㅠ [다른사람들 풀이](https://www.acmicpc.net/problem/status/9465)도 연구해보도록!!!!!

---

## [포도주 시식](https://www.acmicpc.net/problem/2156)

일렬로 된 포도주 시식. 잔을 선택하면 원샷하고 원래위치로 내려놓기& 연속으로 3잔 모두 마시기 x

입력: 첫째 줄에 포도주 잔의 개수 n이 주어진다. (1≤n≤10,000) 둘째 줄부터 n+1번째 줄까지 포도주 잔에 들어있는 포도주의 양이 순서대로 주어진다. 포도주의 양은 1,000 이하의 음이 아닌 정수이다.

출력: 최대로 마실 수 있는 포도주의 양

```c++
#include <cstdio>

int grape[10001];					//포도주 각잔의 포도주양
int d[10001][2];					//i번째 포도주를 마셨는지 안마셨는지에 대해서 그때까지 마신 최댓값
									//0이면 i번째 안마신거 1이면 마신거
int max(int a, int b){
    return (a<b) ? b : a;
}

int main(){
    
    int n;
    scanf("%d",&n);
    for(int i=1;i<=n;i++){
        scanf("%d",&grape[i]);
    }
    d[1][0]=0;
    d[1][1]=grape[1];
    for(int i=2;i<=n;i++){
        d[i][0]= max(d[i-1][0],d[i-1][1]);	//앞에 뭐가 와도 상관없음
        d[i][1]= max(d[i-2][0],max(d[i-2][0]+grape[i-1],d[i-2][1]))+ grape[i]; //xxo xoo oxo 만
    }
    printf("%d\n",max(d[n][0],d[n][1]));
    
    return 0;
}
```

```c++
//백준 풀이 여기서 ?는 dont care x:안마심 o:마심
/*
sol1
d[i][j]=a[1]~a[i] 포도주의 i번째 잔이 연속으로 j번 마신거다.
d[n][0]: 0번 연속 마셨다. ??x  max(d[n-1][0],d[n-1][1],d[n-1][2]) ;
d[n][1]: 1번 연속 마셨다. ?xo	 d[n-1][0] + a[n]
d[n][2]: 2번 연속 마셨다.	x00  d[n-1][1] + a[n]
*/
/*
sol2
d[n]: n까지 마신 포도주양
0번 연속 ??x: d[n-1]
1번 연속 ?x0: d[n-2] + a[n]
2번 연속 x00: d[n-3] + a[n-1] + a[n]
이렇게 i-3까지 들어가는 경우에는 예외처리 골치아픔. 근데 정답2개(d[1]와 d[2]) 미리 구하고 3이상인 경우만 dp처리하면 예외처리 안 해줘도 됨.
*/
```



정리

이친수: n번째 자리에 무엇이 올것인가.

쉬운계단수: n번째 자리에 오는 수가 무엇이가 앞에 뭐가 오는지

오르막수: n-1번째 수에 어떤게 오는지

스티커: n번째  어떤 상태의 스티커 오는지 앞에는 어떤게 올수는지 살펴봄.

포도주: n번째가 몇번째 연속인지 1차원 이나 2차원 풀이 2개가 있음.

---

# 문제유형 3 (식의 마지막이 ~여야)

## [가장 긴 증가하는 부분 수열](https://www.acmicpc.net/problem/11053) - 다시 풀자

수열 A가 주어졌을 때, 가장 긴 증가하는 부분 수열을 구하는 프로그램을 작성하시오.

예를 들어, 수열 A = {10, 20, 10, 30, 20, 50} 인 경우에 가장 긴 증가하는 부분 수열은 A = {**10**, **20**, 10, **30**, 20, **50**} 이고, 길이는 4이다.

입력:

첫째 줄에 수열 A의 크기 N (1 ≤ N ≤ 1,000)

둘째 줄에는 수열 A를 이루고 있는 Ai (1 ≤ Ai ≤ 1,000)

출력: 첫째 줄에 수열 A의 가장 긴 증가하는 부분 수열의 길이



내 실패한 풀이의 이유: 10 20 30 5 6 7 8 9 10 여기에서 벌써 성립못함... ㅠㅠ

dp[i]를 보통 구하는 답으로 해놓는것이 유리해 보인다.

```c++
#include <cstdio>

int d[1001]; //i번째 수와 그전 수들 중 가장 큰 값중 비교해서 더 큰 값
int cnt;

int main(){
    
    int n;
    scanf("%d\n",&n);
    for(int i=1; i<=n; i++){
        scanf("%d", &d[i]);
    }
    for(int i=1; i<=n; i++){
        if(d[i] < d[i-1]){
        	d[i] = d[i-1];
        }
        else cnt++;
        for(int j=1;j<=i;j++){
            printf("d[%d] = %d ", j , d[j]);
        }printf("\n");
//        printf("cnt: %d\n",d[i]);
    }
    printf("%d\n",cnt);
    return 0;
}
```

```c++
//백준
// d[i] = a[i]를 마지막으로 하는 가장 긴 증가하는 부분 수열의 길이
// d[i] 는 a[i]이 반드시 포함되어야 한다.
// 가장 긴 부분 수열이 a[?], a[??], ..., a[j], a[i]라고 했을 때, 겹치는 부분 문제를 찾아보자.
// d[i] = max(d[j])+1 (j < i, a[j] < a[i])
// a[j] a[i]

#include <cstdio>
int a[1001];
int d[1001];
int main(){
	int n;
    scanf("%d\n",&n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    for(int i=1;i<=n;i++){
        d[i] = 1;
        for(int j=1; j < i;j++){
            if(a[j] < a[i] && d[i] < d[j] + 1){ //증가하는&&가장 긴
                d[i] = d[j] + 1;
            }
        }
    }
    int ans=d[1];
    for(int i=1;i<=n;i++){
  	  	if(ans < d[i]){
    		ans = d[i];
    	}
    }
    printf("%d\n",ans);
}
```

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
int main() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (int i=0; i<n; i++) {
        cin >> a[i];
    }
    vector<int> d(n);
    for (int i=0; i<n; i++) {
        d[i] = a[i];
        for (int j=0; j<i; j++) {
            if (a[j] < a[i] && d[j]+a[i] > d[i]) {
                d[i] = d[j]+a[i];
            }
        }
    }
    int ans = *max_element(d.begin(),d.end());
    cout << ans << '\n';
    return 0;
}
```



## [가장 큰 증가 부분 수열](https://www.acmicpc.net/problem/11055)

수열의 증가 부분 수열 중에 합이 가장 큰 것을 구하는 프로그램을 작성하시오.

```c++
#include <cstdio>
int a[1001];
int d[1001];
int main(){
    
    int n;
    scanf("%d",&n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    //d[n]: a[n]을 수열의 마지막으로 하는 부분수열의 합중 가장 큰 값
    for(int i=1;i<=n;i++){
        d[i] = a[i];
        for(int j=1;j<i;j++){
            if(a[j] < a[i] && d[i] < d[j] + a[i])
                d[i] = d[j] + a[i];
        }
    }
    int ans=d[1];
    for(int i=2; i <= n; i++){
        if(ans<d[i])
            ans=d[i];
    }
    printf("%d\n",ans);
    return 0;
}
```



## [가장 긴 감소하는 부분 수열](https://www.acmicpc.net/problem/11722)

```c++
#include <cstdio>
int a[1001];
int d[1001];
int main(){
	int n;
    scanf("%d\n",&n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    for(int i=1;i<=n;i++){
        d[i] = 1;
        for(int j=1; j < i;j++){
            if(a[j] > a[i] && d[i] < d[j] + 1){ //감소하는 && 가장 긴
                d[i] = d[j] + 1;
            }
        }
    }
    int ans=d[1];
    for(int i=1;i<=n;i++){
  	  	if(ans < d[i]){
    		ans = d[i];
    	}
    }
    printf("%d\n",ans);
}
```

또는 수열 a를 뒤집어서 LIS로 풀어도 된다.

또는 뒤에서 부터 구할 수도 있음. 앞에 더 작은게 있는지 확인.

```c++
#include <cstdio>
int a[1001];
int d[1001];
int main(){
	int n;
    scanf("%d\n",&n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    for(int i=n;i>=1;i++){
        d[i] = 1;
        for(int j=i+1; j <= n;j++){
            if(a[i] > a[j] && d[i] < d[j] + 1){ //감소하는 && 가장 긴
                d[i] = d[j] + 1;
            }
        }
    }
    int ans=d[1];
    for(int i=2;i<=n;i++){
  	  	if(ans < d[i]){
    		ans = d[i];
    	}
    }
    printf("%d\n",ans);
    return 0;
}
```



## [가장 긴 바이토닉 부분 수열](https://www.acmicpc.net/problem/11054) - 다시하기

수열이 증가만 하거나 감소만 하거나 증가하다가 감소해야됨.

```c++
#include <cstdio>
int a[1001];
int d[1001][2];
int main(){
	int n;
    scanf("%d\n",&n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    for(int i=1; i<=n; i++){
        d[i][0] = 1;
       
        for(int j = 1; j < i; j++){
            if(a[j] < a[i] && d[i][0] < d[j][0] + 1){ 
                d[i][0] = d[j][0] + 1;
            }
        }  
    }
    for(int i=n;i>=1;i--){
    	d[i][1] = 1;
    	for(int j = i+1; j <= n; j++){
            if(a[j] < a[i] && d[i][1] < d[j][1] + 1){
                d[i][1] = d[j][1] + 1;
            }
        }
    }
    int ans = d[1][0] + d[1][1];
    for(int i=2;i<=n;i++){
  	  	if(ans < d[i][0] + d[i][1]-1){
    		ans = d[i][0] + d[i][1]-1;
    	}
    }
    printf("%d\n",ans);
}
```



## [연속합](https://www.acmicpc.net/problem/1912)

```c++
#include <cstdio>
#include <algorithm>
using namespace std;
int a[100001];
int d[100001]; 							//a[n]를 마지막으로 하는 부분수열의 연속합.

int main(){
	int n;
    scanf("%d\n", &n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    
    d[1]=a[1];							//초기시작을 이렇게 해줘야해 음수일수 있어. 
    for(int i=2;i<=n;i++){ 			 	 //얘도 2로 시작하고 
        d[i] = max(a[i], d[i-1] + a[i]);
    }
    
    int ans = d[1];						//ans =0 으로 하면 안되지!입력에 음수도 있음.
    for(int i=2;i<=n;i++){
        if(ans<d[i]) ans=d[i];
    }
    printf("%d\n",ans);
    
    return 0;
}
```



## [계단 오르기](https://www.acmicpc.net/problem/2579)

```c++
#include <cstdio>
#include <algorithm>
using namespace std;
int a[301];
int d[301]; //n계단 을 밟았을때 얻을 수 있는 총점수의 최댓값

int main(){
	int n;
    scanf("%d", &n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    
	d[1] = a[1];
	d[2] = a[1] + a[2];
    for(int i=3;i<=n;i++){
    	d[i]=max(d[i-2]+a[i], d[i-3]+a[i-1]+a[i]);
    }
    
    printf("%d\n", d[n]);
    
    return 0;
}
```

```c++
//2차원 풀이
#include <cstdio>
#include <algorithm>
using namespace std;
int a[301];
int d[301][3]; //i 번째 계단에 올라갔을 때 최대 점수 i번째 계단 j개가 연속해서 올라온 계단

int main(){
	int n;
    scanf("%d", &n);
    for(int i=1; i<=n; i++){
        scanf("%d", &a[i]);
    }
    
	d[1][1] = a[1];
    for(int i=2;i<=n;i++){
    	d[i][1] = max(d[i-2][1], d[i-2][2] + a[i]);
        d[i][2] = d[i-1][1] + a[i];
    }
    printf("%d\n", max(d[n][1], d[n][2]));
    
    return 0;
}
```



---

# 문제유형4



## [제곱수의 합](https://www.acmicpc.net/problem/1699)

O + O + O + i^2 = N

min(d[N-i^2] + 1)

time complextiy: nrootn 

```c++
#include <cstdio>
#include <algorithm>
using namespace std;

int d[100001]; //i를 제곱수들의 합으로 표현할때 그 항의 최소 개수

int main(){
	int n;
    scanf("%d", &n);
    
    d[1]=1;
    for(int i=1;i<=n;i++){
        d[i]=i;
        for(int j=1; j*j <= i; j++){
            if(d[i] > d[i - j*j] + 1){
                d[i] = d[i - j*j] + 1;
            }
        }
    }
    
    return 0;
}
```



## [타일 채우기](https://www.acmicpc.net/problem/2133)



```c++
#include <cstdio>
#include <algorithm>
using namespace std;

int d[31]; //3*벽을 채우는 경우의 수

int main(){
	int n;
    scanf("%d", &n);
    
    d[0] = 1;
    for(int i=2;i<=n;i++){
        //d[n]= 3*d[n-2] +2*d[n-4]+2*d[n-6]+...
        d[i]=3*d[i-2];
        for(int j=4;j < n;j+=2){
            d[i] += 2*d[i-j];
        }
    }
    printf("%d\n", &n);
    
    return 0;
}
```



## [파도반 수열](https://www.acmicpc.net/problem/9461)

```c++
#include <cstdio>

long long d[101]; //

int main(){
	int t;
    scanf("%d", &t);
    
    d[1]=d[2]=d[3] = 1;
    d[4]=d[5]=2;
    
    for(int i=6;i<=100;i++){
       d[i]=d[i-1]+d[i-5]; //d[i-2] + d[i-3] 도 됨. 변만 보면 아!
    }
    
    while(t--){
        int n;
        scanf("%d", &n);
        printf("%lld\n",d[n]);
    }
    
    
    
    
    return 0;
}
```





## [합분해♨](https://www.acmicpc.net/problem/2225)

```c++
#include <cstdio>

long long d[201][201] ;
// d[i][j] : 0부터 i까지의 정수 j개를 더해서 
// 그합이 i가 되는 경우의수 

long long mod =  1000000000; 
//출력값을 위한 정수 

int main(){
	
	int n,k;
	scanf("%d %d",&n,&k);
	
	d[0][0]=1LL;
	for(int i=0; i <= n; i++){
		for(int j=1; j <= k; j++){
			for(int l = 0; l <= i; i++){
				d[i][j] += d[i-l][j-1];
				d[i][j] %=  mod;
			}
		}
	}
	printf("%d\n",d[n][k]);
    return 0;
}
```

```c++
#include <cstdio>

long long d[2][201];
long long mod = 1000000000;


int main(){
    								   //1차원적 풀이
    d[0][0]=1LL; 					    //d[0]=1;
	for(int i=1; i<=k; i++){
    	for(int j=0; j<=n; j++){ 	     //j=n; j>=0; j--;
        	for(int l=0; l <= j; l++){ 	 //l=1
            	d[i][j] += d[i-1][j-l]; 		  		//i->i%2 , i-1-> 1-i%2
            	d[i][j] %= mod;						   //메모리 줄이는 풀이
        	}
    	}
	}
    return 0;
}
```



## 암호합♨

```c++
#include <iostream>
#include <string>
using namespace std;

long long d[5001]; 		// i번째 수까지의 해석수
long long mod = 1000000;
int main(){
	
    string s;
    cin >> s;
    
    int len = s.size();
//    printf("len = %d\n",len);
    
    if(s[0] == '0') d[0]=0; // 0xxx 같은 수를 위해 ㅠㅠ
    else d[0] = 1;			// 이런 예외 꼭 생각해야됨
    
    for(int i = 1; i < len; i++){
    	
    	int x = s[i-1] - '0';
    	int y = s[i] - '0' ;
//    	printf("i = %d, x = %d, y = %d\n", i , x, y);
    	
    	if(1 <= y && y <= 9){
    		d[i] += d[i-1];
			d[i] %= mod; 
		}
		
		if(i == 1){
			if(x == 1 ||(x == 2 && ( y >= 0 && y <= 6 ))){
				d[1] += 1;
			}
		}
    	
    	if(x == 0) continue; 
    	
        //10~19
    	if(x == 1) d[i] += d[i-2];
    	//20~26
    	if(x == 2 && (y >= 0 && y <= 6)) d[i] += d[i-2] ;
    
    	d[i]%= mod;
	
//        printf("i = %d, d[%d] = %d\n",i, i, d[i]);
    }
    
    cout << d[len-1] << endl;
    
    return 0;
}
```

백준님 풀이

[c](https://gist.github.com/Baekjoon/33126f8bcbfa55ebc9b1) [c++](https://gist.github.com/Baekjoon/dc071a89a81cb88fb887#file-2011-cc-L26) string 입력을 어떻게 하는지 잘 나와있음

