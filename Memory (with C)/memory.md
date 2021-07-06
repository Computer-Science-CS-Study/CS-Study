# 메모리

## 메모리 주소&포인터

- 16진수를 사용하는 이유 ?
  1. 컴퓨터나 휴대폰 손 메모리 위치를 표현하는데 유연하기 때문에
  2. 0~9, A~F 같이 한자리로 15까지 셀 수 있기 때문
- 8bit 가 표현할 수 있는 최대 숫자 : 11111111(2) == 255(10) == FF(16)
- 16진수와 10진수의 표현의 모호함을 구분하기 위해 16진수 앞에는 모두 "0x"를 붙인다.
- int = 4byte (1 byte==8bit) → 32bits
- 포인터 : 컴퓨터 메모리의 주소를 가리키는 것

  컴퓨터 메모리 : 0과 1인 machine code로 이루어져 있음

  - 포인터의 주소표현
    1. &
    2. printf("%p\n", 변수); //데이터 형식이 포인터

- & : 주소를 가져와라 ↔ \* : 그 주소로 가라
- 만약 변수의 자료형을 모른다면 형식 지정자를 직접 결정해야한다.

```c
int n = 50;
int *p = &n; //여기에서 int 는 포인터가 가리키는 값이 int 라는 말
```

⇒ 위의 코드에서 만약에 int p = &n; 을 하면 clang 컴파일러가 경고함

- 주소는 반드시 포인터에 저장해야한다.
- 포인터의 크기는 보통 데이터 크기의 2배이다. (일반적으로)

---

## 문자열

- 문자열의 끝은 종단 문자로 이루어짐 : "\0" → 8개의 0bit

  ex) string s = "EMMA";  
   |E|M|M|A|\0|
  |---|---|---|---|---|
  |s[0]|s[1]|s[2]|s[3]|s[4]|

- 문자열의 첫번째 글자만 가리키면 널 종단 문자를 마주칠 때까지 루프를 돌면서 찾아낸다.
- 문자열은 문자들의 나열이다. ⇒ 문자열은 문자 배열의 첫번째 바이트 주소

  ```c
  typedef char *string; //string은 어떤 char의 주소를 가지고 있는 변수
  ```

```c
char *s = "EMMA";
printf("%s\n", s); //EMMA
printf("%p\n", s); //0x100bddfqe
printf("%p\n", &s); //0x7ffeef022698
```

![그림1](https://user-images.githubusercontent.com/59546993/124593575-c2883100-de99-11eb-9c08-b355fda98188.jpeg)

⇒ s는 문자의 주소, \*s는 그 문자로 가는 거니까 E가 있음

⇒ s[1] = \*(s+1)

- 만약 두개의 같은 문자열을 입력받고 두개가 같은지 물어보면 다르다고 나온다. ⇒ 서로 차지하는 메모리의 위치가 다르기 때문

  ```c
  char *s = get_string("s : ");
  char *t = get_string("t : ");
  //여기에서 s와 t가 저장하는 것은 메모리의 첫번째 주소

  if (s == t){
  	printf("same");
  }
  else{
  	printf("differ");
  }
  ```

- malloc : C에서 메모리를 할당하는 함수 → 할당한 메모리의 첫 바이트 주소를 돌려준다.

  malloc은 요청한 크기를 할당할 뿐, 위치는 신경쓰지 않는다.

- 문자열을 복사할 때, 일반적으로 복사하면 주소가 복사된다.

  ```c
  char *s = get_string("s : ");
  char *t = s;
  ```

  ![그림2](https://user-images.githubusercontent.com/59546993/124593571-c2883100-de99-11eb-9d64-78157c1e2864.jpeg)

  ⇒ 결국 포인터가 복사되기 때문에 t를 바꿔도 s에도 영향이 감

---

## 메모리 할당 및 해제

- 메모리 할당 : 메모리 일부분을 가져와서 그곳을 가리키는 포인터를 주는 것
- 메모리를 할당했으면 반드시 다시 반환해야 한다. (더 많은 메모리 사용을 위해)
- malloc(할당) ↔ free(반환)
- valgrind : 쓸모없는 메모리(메모리 leak)를 찾아주는 디버깅 툴
- 버퍼 오버플로우 : 메모리 공간을 넘어 접근하면 일어나는 상황

  ```c
  int *x = malloc(10*sizeof(int)); // int는 4byte니까 총 40byte를 할당, 0~9까지의 정수공간을 만듦
  x[10] = 0; //10 정수 공간은 존재하지 않는 할당하려고 함 => 버퍼 오버플로우
  ```

---

## 메모리 교환, 스택, 힙

```c
#include <stdio.h>

void swap(int a, int b);

int main(void)
{
    int x = 1;
    int y = 2;

    printf("x is %i, y is %i\n", x, y);
    swap(x, y);
    printf("x is %i, y is %i\n", x, y);
}

void swap(int a, int b)
{
    int tmp = a;
    a = b;
    b = tmp;
}
```

- 다음 코드와 같이 main 함수 중간에 swap(x, y)를 넣으면 값이 반영되지 않는다.
- swap 함수 자체는 동작하지만 swap 에서 사용하는 변수 a, b 는 x, y 의 "값"을 복사한 것이기 때문에
- 메모리 구조

  ![그림3](https://user-images.githubusercontent.com/59546993/124593567-c1570400-de99-11eb-8289-27ea8b06764a.jpeg)

- 위의 문제를 해결하려면 참조에 의한 전달을 해서 swap 함수의 변수들이 main함수의 변수들을 포인팅 하게 해야한다.

  ![그림4](https://user-images.githubusercontent.com/59546993/124593551-bdc37d00-de99-11eb-98f5-bd1bc78618f2.jpeg)

  ```c
  #include <stdio.h>

  void swap(int *a, int *b);

  int main(void)
  {
      int x = 1;
      int y = 2;

      printf("x is %i, y is %i\n", x, y);
      swap(&x, &y);
      printf("x is %i, y is %i\n", x, y);
  }

  void swap(int *a, int *b)
  {
      int tmp = *a;
      *a = *b;
      *b = tmp;
  }
  ```
