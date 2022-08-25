#파이썬에서 다른 언어(C++)로 작성된 DLL 사용 빠르게 정리
===================================
작업환경
------------------------------
+ OS: 윈도우10, 64bit
+ dll제작: Visual Studio 2022
+ python: Visual Studio Code 

필수설치
------------------------------
1. Visual Studio (또는 DLL 제작 가능한 IDE)
+ 2022버전으로 테스트 함
 
2. Visual Studio Code 및 Python 확장 설치
+ 설치 후 경로설정 하고 재부팅
+ Python 설치 (vsc 확장 에서 설치함)(ver 2022.10.1)
+ (선택) 확장에서 한국어 확장팩 설치
 
절차
------------------------------
#1. DLL제작 (Visual Studio)
+ Visual Studio 설치할때 "C++를 사용한 데스크톱 개발" 체크 해야 DLL라이브러리 프로젝트 선택 가능 
+ 새 프로젝트 -> DLL(동적 연결 라이브러리) -> 생성 -> 코드 작성 후 빌드하면 [클래스이름.dll] 생성됨 
+ 새 헤더파일 추가(헤더.h) 
	아래와 같이 작성
	<pre>
	<code>
	extern "C" __declspec(dllexport) int myCppFunc(int a, int b);
	</code>
	</pre>
+ 새 소스파일 추가 (소스.cpp)
	아래와 같이 작성
	<pre>
	<code>
	#include "pch.h"
	#include "헤더.h"
	int myCppFunc(int a, int b) { return a * b; }
	</code>
	</pre>
+ 빌드
##해당 프로젝트 위치에 [클래스이름.dll] 생성됨

 
+여기까지 안되면 참고(dll제작 및 사용)
## https://docs.microsoft.com/en-us/cpp/build/walkthrough-creating-and-using-a-dynamic-link-library-cpp?view=msvc-170



#2. 파이썬 에서 사용
아래 코드 참고
<pre>
<code>
import ctypes 
#ctypes를 이용하여 파이썬에서 C/C++ 타입의 문법을 사용
#다음과 같이 줄여서 사용 가능 (import ctypes as c)

mydll = ctypes.WinDLL("[DLL경로명]")
# 절대경로 ("C:\\Desktop\\[DLL파일명].dll")
# 상대경로 (".\\dll\\[DLL파일명].dll")   .(dot) => 파이썬 파일 위치 부터

myPyFunc = mydll['[myCppFunc]']
# myPyFunc -> 파이썬에서 사용할 함수명
# myCppFunc -> dll헤더에 있는 함수명

myPyFunc.argtypes = (ctypes.c_int,ctypes.c_int)
# 파라미터 타입

myPyFunc.restype = ctypes.c_int
# 리턴 타입

print(myPyFunc(10220,200))
# 사용예시
</code>
</pre>















안되면 확인할것
------------------------------

+1. python 설치 (윈도우) (3.10.6 64bit)
+1. VSC C/C++ 확장팩 설치
+1. https://www.msys2.org/ 설치

