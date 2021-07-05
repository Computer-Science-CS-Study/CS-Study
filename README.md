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
<br>

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
💡 마지막 종단 문자를 복사하지 않으면 그 뒤의 쓰레기값도 출력될 수 있다
<br>

## Heap and Stack 
#### 메모리 할당
* malloc 은 할당된 메모리의 첫 바이트 주소를 돌려준다
* 메모리 할당이란 메모리 일부분을 가져와서 그 곳을 가리키는 포인터를 준다
* free 는 할당되었던 메모리를 반환한다
```c
char s[] = "emma";
char *t = malloc(strlen(s) + 1); //널 종단문자 때문에 +1

//널 종단문자도 복사해야하므로 +1
for(int i=0, n=strlen(s); i < n+1; i++){
	t[i] = s[i];
}

t[0] = toupper(t[0]);

free(t);
```
```
void f(void){
	// 40byte 의 메모리를 요청
	int *x = malloc(10 * sizeof(int)); //메모리의 주소를 x라는 포인터에 저장
	x[10] = 0; //버퍼 오버플로우 => 배열의 경계(0~9)를 넘어 10에 접근하므로 잘못됨
	free(x); //사용한 후 메모리 해제해 줌
}

int main(void){
	f();

	return 0;
}
```
<br>

#### 메모리 구조
* 0과 1로 컴파일된 코드는 메모리 위쪽의 machine code 에 저장된다
* 프로그램이 전역 변수나 정보를 쓴다면 globals 공간에 놓이게 된다
* malloc 이 메모리를 할당하는 곳은 heap이라는 공간이다
* 함수가 호출될 때 지역변수는 stack 이라는 공간에 쌓이게 된다<br>
<img src="https://user-images.githubusercontent.com/69183944/124470261-601a2c80-ddd6-11eb-8b96-42d2bd0d277a.png" width="250" height="400"> <br>
<br>

#### 메모리 교환 (잘못된 코드)
* 함수에 인자를 전달할 때 그 값을 복사해서 전달한다
* 따라서 인자로 전달하지만 함수는 x와 y 자체가 아니라 x와 y의 복사본을 전달받는다
```c
#include <stdio.h>

void wrong_swap(int a, int b);

int main(void){
	int x = 1;
	int y = 2;
	
	printf("x is %i, y is %i\n", x, y); // 1 2
	wrong_swap(x, y);
	printf("x is %i, y is %i\n", x, y); // 1 2
}

void wrong_swap(int a, int b){
	int tmp = a;
	a = b;
	b = tmp;
}
```
<br>

#### Swap 코드의 메모리 구조
* c 프로그램의 기본 작동 방식으로 스택 프레임이라는 공간이 주어진다. argv나 argc 그리고 x와 y 같은 지역 변수를 저장하는 공간이다 main 함수 안에 있는 변수는 모두 이 main 프레임 영역에 저장된다 <br>
<img src="https://user-images.githubusercontent.com/69183944/124470967-42999280-ddd7-11eb-9683-d10c5f45d4ac.png" width="250" height="400"> <br>

* main 함수가 swap 함수를 호출하면 해당 함수를 위한 메모리 영역이 main 위에 쌓인다 <br>
<img src="https://user-images.githubusercontent.com/69183944/124471154-7c6a9900-ddd7-11eb-9851-715b5925b415.png" width="250" height="400"> <br>

* x와 y가 바닥에 놓이고 swap을 호출하면 a, b, tmp 이렇게 세 개의 변수가 swap 프레임 메모리에 존재한다
<img src="https://user-images.githubusercontent.com/69183944/124471284-a754ed00-ddd7-11eb-9fd4-d882a349b8c5.png" width="250" height="400"> <br>

* 함수의 마지막 줄까지 수행이 완료되면, 스택에서 swap 프레임은 사라진다. 하지만 메모리가 사라지는 것은 아니다. 그냥 이 프로그램을 위해 더 이상 사용하지 않는 것이다 <br>
<img src="https://user-images.githubusercontent.com/69183944/124471542-f1d66980-ddd7-11eb-924b-2b08219a5a1a.png" width="250" height="400"> <br>
<br>

#### 메모리 교환 (올바른 코드)
* swap 함수를 통해 x와 y의 값을 교환하려면, 참조를 사용한다. main에서 x와 y의 값을 swap에게 전달하지 않고 x와 y의 주소를 알려줘서 swap 함수가 그 주소로 가서 값을 바꾸게 하는 것이다<br>
<img src="https://user-images.githubusercontent.com/69183944/124471829-4f6ab600-ddd8-11eb-8cdf-e9927a0392fa.png" width="250" height="400"> <br>
```c
#include <stdio.h>

void swap(int *a, int *b);

int main(void){
	int x = 1;
	int y = 2;
	
	printf("x is %i, y is %i\n", x, y); // 1 2
	swap(&x, &y);
	printf("x is %i, y is %i\n", x, y); // 1 2
}

// x와 y의 값 자체를 교환
void swap(int *a, int *b){
	int tmp = *a; // tmp에 a가 가리키는 주소의 값(x 값)을 tmp에 대입
	*a = *b; // b가 가리키는 주소의 값(y 값)을 a가 가리키는 주소의 값(y 값)에 대입
	*b = tmp; // a가 가리키는 주소의 값(y 값)에 tmp 값을 대입
}
```
