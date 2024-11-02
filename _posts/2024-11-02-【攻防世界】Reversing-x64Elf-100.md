# 【攻防世界】Reversing-x64Elf-100

#### 导入IDA

main函数

```c
#include<stdio.h>
#include <intrin.h>

long long __fastcall sub_4006FD(long long input)
{
	int i; // [rsp+14h] [rbp-24h]
	long long key[4]; // [rsp+18h] [rbp-20h]

	key[0] = (long long)"Dufhbmf";
	key[1] = (long long)"pG`imos";
	key[2] = (long long)"ewUglpt";
	for ( i = 0; i <= 11; ++i )
	{
		if ( *(char *)(key[i % 3] + 2 * (i / 3)) - *(char *)(i + input) != 1 )
			return 1LL;
	}
	return 0LL;
}

long long main(int a1, char **a2, char **a3)
{
	char input[264]; // [rsp+0h] [rbp-110h] BYREF
	unsigned long long v5; // [rsp+108h] [rbp-8h]

//	v5 = __readfsqword(0x28u);
	printf("Enter the password: ");
	if ( !fgets(input, 255, stdin) )              // //scanf("%s",&s)
		return 0LL;
	if ( (unsigned int)sub_4006FD((long long)input) )
	{
		puts("Incorrect password!");
		return 1LL;                                 // //返回1
	}
	else
	{
		puts("Nice!");
		return 0LL;
	}
}
```

思路即是求`input`​

减数=被减数-差

‍

‍

、

#### 解密

```c
#include<stdio.h>
#include <intrin.h>

long long __fastcall sub_4006FD()
{
	int i; // [rsp+14h] [rbp-24h]
	long long key[4]; // [rsp+18h] [rbp-20h]
	char input[264];
	key[0] = (long long)"Dufhbmf";
	key[1] = (long long)"pG`imos";
	key[2] = (long long)"ewUglpt";
	for ( i = 0; i <= 11; ++i )
	{
		input[i]= *(char *)(key[i % 3] + 2 * (i / 3)) -1;
		//( *(char *)(key[i % 3] + 2 * (i / 3)) - *(char *)(i + input) != 1 )
		printf("%c",input[i]);
	}

	return 0LL;
}

long long main(int a1, char **a2, char **a3)
{
	char input[264]; // [rsp+0h] [rbp-110h] BYREF
	unsigned long long v5; // [rsp+108h] [rbp-8h]

//	v5 = __readfsqword(0x28u);
//	printf("Enter the password: ");
//	if ( !fgets(input, 255, stdin) )              // //scanf("%s",&s)
//		return 0LL;
//
	sub_4006FD();
//	if ( (unsigned int)sub_4006FD((long long)input) )
//	{
//		puts("Incorrect password!");
//		return 1LL;                                 // //返回1
//	}
//	else
//	{
//		puts("Nice!");
//		return 0LL;
//	}
}
```

#### 结果：

flag=  
​`Code_Talkers`​

#### 总结：

* ida数据类型：
* ```c
  #define __int8 char
  #define __int16 short
  #define __int32 int
  #define __int64 long long
  ```
* ida传参：
* ```c
  fun((long long) arr);
  arr+i;//arr[i]
  ```

‍
