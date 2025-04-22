---
title: c++ 에서 병렬처리 정렬 알고리즘의 성능
date: 2025-04-22T15:04:36+09:00
categories:
  - c++
tags:
  - c++
  - jenkins
---

서버 내 코드를 개선할게 없느라 가끔씩 지나가며 보는데 작은 알고리즘부터 수정 중이다.

병렬프로그래밍을 찾다보니 정렬알고리즘을 찾게 되었고 최적화할 수 있음을 알게되서 테스트해보았다.

기존의 코드는 std::sort 를 사용하고 있었다. 이 정렬 알고리즘은 concurrency::parallel_sort 로 바꿀 수 있었는데 PPL 의 특성상 비표준인데다가 윈도우즈에서만 작동가능한 코드였다. chatgpt 는 concurrency::parallel_sort 대신에 std::sort 와 std::execution::par 를 사용하는 것을 추천해주었다. (c++17 이상에서만 유효하다.) 이 알고리즘에 대한 설명은 chatgpt 를 참고하자.

### 테스트코드
기존의 std::sort 를 사용한 정렬과 std::sort + std::execution::par 를 사용한 코드를 비교해보았다.

```c++
#define WIN32_LEAN_AND_MEAN
#include <Windows.h>
#include <vector>
#include <string>
#include <random>
#include <chrono>
#include <execution>
#include <iostream>
#include <locale>

int main( void )
{
	std::vector<int> vec;

	for ( auto i = 0; i < 1000000; ++i )
		vec.push_back( i );

	std::random_device rd;
	std::mt19937 engine( rd() );
	std::shuffle( vec.begin(), vec.end(), engine );

	std::vector<int> vec1 = vec;
	std::vector<int> vec2 = vec;

	{
		// 시간 측정 시작
		auto start = std::chrono::high_resolution_clock::now();

		std::sort( vec1.begin(), vec1.end(), []( auto lhs, auto rhs )
		{
			return lhs < rhs;
		} );

		// 시간 측정 끝
		auto end = std::chrono::high_resolution_clock::now();

		// 경과 시간 계산 (단위: 마이크로초)
		auto duration = std::chrono::duration_cast<std::chrono::microseconds>( end - start );

		std::cout << "실행 시간: " << duration.count() << " 마이크로초" << std::endl;
	}

	{
		// 시간 측정 시작
		auto start = std::chrono::high_resolution_clock::now();

		std::sort( std::execution::par, vec1.begin(), vec1.end(), []( auto lhs, auto rhs )
		{
			return lhs < rhs;
		} );

		// 시간 측정 끝
		auto end = std::chrono::high_resolution_clock::now();

		// 경과 시간 계산 (단위: 마이크로초)
		auto duration = std::chrono::duration_cast<std::chrono::microseconds>( end - start );

		std::cout << "실행 시간: " << duration.count() << " 마이크로초" << std::endl;
	}

	return 0;
}
```

배열에 int 값을 여러개넣고 섞은 후 다시 정렬하는데에 걸리는 시간을 측정하는 코드이다.

### 테스트결과
이 코드를 실행했을 때 다음과 같은 결과를 얻을 수 있었다. 단위는 '마이크로초'이다.

|원소갯수|std::sort|std::sort+std::execution::par|
|100|20|61|
|300|70|98|
|500|111|86|
|1000|258|128|
|10000|3302|461|
|100000|41760|4022|
|1000000|509795|48412|
|10000000|5024498|564703|
|100000000|68011136|5948772|

사용된 컴퓨터는 Intel Core i5-12400F 2.5GHz CPU, 64GB 메모리의 컴퓨터이며 Visual Studio 2019 x64 환경에서 실행하였다.

표를 본다면 원소갯수가 300개까지는 정렬전략을 고르지 않고 기본전략대로 사용하는 것이 더 좋을 것으로 보인다. 대략 원소갯수 500개 정도부터는 std::execution::par 를 설정하는 것이 검색시간을 줄이는 효과가 나타나는 것을 알 수 있었다. 1000개는 대략 1/2 로 감소시키는 효과가 있었고 10000개부터는 거의 1/10 으로 감소시킬 수 있었다.

### 결론
원소갯수 300개 미만은 std::sort 를 사용하면 된다.
원소갯수 300개 이상은 std::sort + std::execution::par 를 사용하는 것이 좋다.