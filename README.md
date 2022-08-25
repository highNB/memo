파이썬에서 C++ 로 작성된 DLL 사용 정리
===================================
작업환경
------------------------------
+ OS: 윈도우10, 64bit
+ dll제작: Visual Studio 2022
+ python: Visual Studio Code 

설치해야 하는것
------------------------------
1. Visual Studio 2022
+ Visual Studio 2022 (ver 17.3.2)
+ "C++를 사용한 데스크톱 개발" 체크하고 설치
 
2. Visual Studio Code
+ Visual Studio Code (ver 1.70.2)
+ Python extension (ver 2022.10.1)
+ (선택) 우측 확장(Ctrl+X)에서 한국어 설치
 
3. python 
+ python (ver 3.10.6 64bit)

순서
------------------------------
# 1. DLL제작 (Visual Studio)
+ 새 프로젝트 -> DLL(동적 연결 라이브러리) -> 생성, 이 때 클래스 이름으로 dll 생성됨 
+ 'DLL(유니버셜 Windows)' 아님
+ 새 헤더파일 추가(헤더.h) 
	아래와 같이 작성
	<pre>	<code>
	extern "C" __declspec(dllexport) int myCppFunc(int a, int b);
	</code>	</pre>	
+ 새 소스파일 추가 (소스.cpp)
	아래와 같이 작성
	<pre>	<code>
	#include "pch.h"
	#include "헤더.h"
	int myCppFunc(int a, int b) { return a * b; }
	</code>	</pre>
+ 빌드
	* 해당 프로젝트 위치에 [클래스이름.dll] 생성됨

 
+ DLL관련 참고 예제
	* https://docs.microsoft.com/en-us/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp?view=msvc-170



# 2. 파이썬 에서 사용
+ 파이썬에서 사용시 DLL파일만 있으면 됨
+ 아래 코드 Visual Studio Code에 복사
+ 최초 실행 시 우측 하단 인터프리터 선택 -> 위에서 설치한 파이썬(ver.3.10.6) 연결

<pre>
<code>
import ctypes 
#ctypes를 이용하여 파이썬에서 C/C++ 타입의 문법을 사용
#다음과 같이 줄여서 사용 가능 (import ctypes as c)

mydll = ctypes.WinDLL("DLL경로명")
# 절대경로 ("C:\\Desktop\\DLL파일명.dll")
# 상대경로 (".\\dll\\DLL파일명.dll")   .(dot) => 파이썬 파일 위치 부터

myPyFunc = mydll['myCppFunc']
# myPyFunc -> 파이썬에서 사용할 함수명
# myCppFunc -> dll헤더에 있는 함수명

myPyFunc.argtypes = (ctypes.c_int,ctypes.c_int)
# 파라미터 타입

myPyFunc.restype = ctypes.c_int
# 리턴 타입

print(myPyFunc(19,10000))
# 사용예시
</code>
</pre>
