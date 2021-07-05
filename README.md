# Memory (with C)
> cs 50 강의를 들으며 정리한 자료입니다

**:book: Contents**
1. [Address](#address)
2. [Pointer](#pointer)
3. [String](#string)
4. [Heap and Stack](#heap-and-stack)

## Address
* 컴퓨터가 숫자를 10진수나 2진수가 아닌 16진수(0x..)로 표현할 때가 많다
* 주소를 알고싶다면 변수 앞에 &를 붙이고, 출력포맷은 %p이다
<br>  => 그 값을 가리키는 포인터 값(주소를 가리키는 것)을 돌려받는 것이다
```c
int n = 50;
printf("%p\n", &n);
```
* 변수 앞에 *를 붙이면 "그 주소로 가줘"라는 의미로, 그 주소 안에 어떤 값이 들어있는지를 알려준다
* &를 통해 주소를 받고, *를 통해 그 주소의 값으로 간다
```c
int n = 50;
printf("%i\n", *&n);
```
<br>

## Pointer
* 포인터는 주소(address)를 가지는 변수이다
```c
int n = 50;
int *p = &n;          // 포인터라는 의미로 변수명이 p이고, int형 공간을 가리키는 포인터이다
printf("%p\n", p);    // 주소값 출력
printf("%i\n", *p);   // 주소에 들어있는 값 출력
```
* 큰 네모는 변수 p를 나타내고, 주소를 저장한다
* 또 다른 변수는 n이고, 숫자 50을 저장한다
<br>[그림]<br>
<img src="https://user-images.githubusercontent.com/69183944/124456935-3c4eea80-ddc6-11eb-987e-cfbf53804b28.png" width="400" height="180">
<br>
:bulb: 메모리에서 다른 곳을 가리키고 또 가리키면 아주 정교한 자료형을 만들 수 있다 ex) 가계도나 배열 등 <br>
:bulb: 포인터의 크기가 꼭 두배여야하는 것은 아니지만 대부분 그렇게 동작한다
<br>

## String
#### 문자열
* 문자열 끝에는 종단 문자 \0이 있어 문자열의 끝을 구별해준다
<br>  => null 종단 문자는 8개의 0비트로 되어 있다
* 문자열 s는 첫 글자를 가리키는 포인터다
* null 종답 문자를 통해 문자열의 끝을 구별한다
<br>[그림]<br>
<img src="https://user-images.githubusercontent.com/69183944/124459097-c13b0380-ddc8-11eb-94dc-f80e68ca7e35.png" width="400" height="180">
* string은 char* 이다 <br>

```c
char *s = "EMMA";
printf("%p\n", s); //0x42aa02
printf("%s\n", s); //EMMA
printf("%p\n", &s[0]); //0x42aa02
printf("%p\n", &s[1]); //0x42aa03
printf("%p\n", &s[2]); //0x42aa04
printf("%p\n", &s[3]); //0x42aa05
```
```c
char *s = "EMMA";
printf("%c\n", *s) //E
printf("%c\n", *(s+1)) //M
printf("%c\n", *(s+2)) //M
printf("%c\n", *(s+3)) //A
```
:bulb: printf에서 %s 를 사용해서 출력을 요청하면 그 주소로 가서 첫 문자만 출력하는 것이 아니라 다음 문자를 계속 출력하다가 null 종단 문자가 나오면 출력을 멈추기 때문에 s를 출력했을 때는 한 문자가 아니라 문자열을 출력한다
<br>

#### 문자열 비교
```c
char s[10] = "EMMA";
char *t = "EMMA";

if(s == t)
    printf("Same\n");
else
    printf("Different\n");

printf("%p\n", s); //0xed76a0
printf("%p\n", t); //0xed76e0
```
* s와 t는 메모리 공간의 첫 바이트 주소를 얻는다. 두 메모리는 다른 곳에 저장되어 있으므로 s="EMMA", t="EMMA" 라고 하더라도 "Different" 를 출력한다
<br>  => 따라서 문자열을 직접 비교할 수 없다 <br>
<img src="https://user-images.githubusercontent.com/69183944/124463415-d1a1ad00-ddcd-11eb-91b7-016bae8f3b15.png" width="400" height="180">

#### 문자열 복사
* s의 주소를 t도 가지고 있기 때문에 t를 수정했을 때 둘 다 수정된다
* 즉, 문자열을 복사하는 것이 아니라 주소를 복사한다
```c
char s[] = "emma";
char *t= s;

t[0] = toupper(t[0]);
printf("%p\n", s); //0x7ffe2e892cd3
printf("%s\n", s); //Emma
printf("%p\n", s); //0x7ffe2e892cd3
printf("%s\n", t); //Emma
```
```c
char s[] = "emma";
char *t = malloc(strlen(s) + 1); //널 종단문자 때문에 +1

//널 종단문자도 복사해야하므로 +1
for(int i=0, n=strlen(s); i < n+1; i++){
	t[i] = s[i];
}

t[0] = toupper(t[0]);

printf("%p\n", s); //0x7ffc115c1bd3
printf("%s\n", s); //emma
printf("%p\n", s); //0x7ffe2e892cd3
printf("%s\n", t); //Emma
```
💡 마지막 종단 문자를 복사하지 않으면 그 뒤의 쓰레기값도 출력될 수 

## Heap and Stack 
