# 메모리 퀴즈

## 문제 1)

1. 문제 

    다음 코드는 컴파일 시, 에러가 납니다. 
    여기에서 "EMMA"를 두 번 입력했을 때, "same"과 "different"라는 결과를 도출하기 위해서는 어떻게 코드를 각각 바꿔야 합니까?

    ```c
    #include <stdio.h>

    int main(void)    {    

        char a[10];    
        char b[10];
        
        printf("type string1 : ");
        scanf("%s", a);
        
        printf("type string2 : ");    
        scanf("%s", b);
        
        if (a == b)    {
            printf("same");
        }    
        else{
            printf("different");
        }
    }
    ```

2. 정답 

    ```c
    #include <stdio.h>

    int main(void)
    {
        char a[10];
        char b[10];

        printf("type string1 : ");
        scanf("%s", a);

        printf("type string2 : ");
        scanf("%s", b);

        if (*a == *b) //결과 same
        {
            printf("same");
        }
        else
        {
            printf("different");
        }

        if (&a == &b) //결과 different
        {
            printf("same");
        }
        else
        {
            printf("different");
        }
    }
    ```

---

## 문제 2)

1. 문제

    buffer overflow가 무엇인지 설명하고, malloc을 이용해서 메모리를 할당하고 buffer overflow가 일어나는 코드를 작성하여라. (include 작성할 필요없음)

2. 정답

    ```c
    int *x = malloc(10*sizeof(int));
    x[10]=0;
    ```

---

## 문제 3)

1. 문제 

    다음 코드에서의 문제점을 메모리 구조를 그려서 설명하고 해결책을 메모리 구조와 함께 코드로 작성해라.

    ```c
    #include <stdio.h>

    void swap1(int a, int b);
    void swap2(int *a, int *b);

    int main(void)
    {
        int x = 1;
        int y = 2;

        printf("x is %i, y is %i\n", x, y);
        swap1(x, y);
        printf("x is %i, y is %i\n", x, y);
        swap2(&x, &y);
        printf("x is %i, y is %i\n", x, y);
    }

    void swap1(int a, int b)
    {
        int tmp = a;
        a = b;
        b = tmp;
    }
    ```

2. 정답

    ```c
    #include <stdio.h>

    void swap2(int *a, int *b);

    int main(void)
    {
        int x = 1;
        int y = 2;

        printf("x is %i, y is %i\n", x, y);
        swap2(&x, &y);
        printf("x is %i, y is %i\n", x, y);
    }

    void swap2(int *a, int *b)
    {
        int tmp = *a;
        *a = *b;
        *b = tmp;
    }
    ```
