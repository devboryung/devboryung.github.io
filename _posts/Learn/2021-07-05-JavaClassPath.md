---
title: "[JAVA] 패키지와 클래스패스"
excerpt: "JAVA"
categories: 
  - Learn
tags: 
  - Learn
toc: true
---


## 패키지란?

패키지는 서로 관련된 클래스를 묶어 놓은 것이다.<br>
*왜? Java8 기준으로 약 4,000개의 클래스가 있어서 관리하기 편할 수 있도록 작은 단위로 나눠 관리하기 위해 관련된 것들 끼리 묶음.*<br><br>
소스 파일을 컴파일 하면 클래스 파일(*.class), 패키지는 폴더 안에 클래스 파일들을 넣어 둔 것이다(패키지 안에 또 다른 패키지 폴더가 있을 수 있다.).<br>

모든 클래스는 패키지 안에 들어있어야 하며, 클래스의 실제 이름은(full name)은 패키지를 포함한다(*java.lang.String*).<br>
rt(runtime).jar 파일은 자바 프로그램이 실행하는데 필요한 클래스들을 압축한 파일이다.<br>
**Java9부터 rt.jar파일 없어짐. 왜? rt.jar파일은 너무 커서 작은 module로 나눴다.**<br>
`jar`파일은 클래스 파일을 묶어둔 것이다(jar파일은 jar.exe/다른 압축 푸는 프로그램을 통해서 압축을 풀 수 있다.).<br>

<br>
<br>


## 패키지의 선언

패키지는 소스파일의 첫 번째 문장으로 단 한번 선언한다.<br>
같은 소스 파일의 클래스들은 모두 같은 패키지에 속하게 된다(*아래 코드의 PackageTest와 PackageTest2 클래스 모두 com.codechobo.book 패키지에 속한다.*).<br>
패키지 선언이 없으면 이름없는(unnamed/이클립스에선 default package) 패키지에 속하게 된다.<br>

```java
package com.codechobo.book;

public class PackageTest{
    public static void main(String[] args){
        System.out.println("Hello, world!");
    }

    class PackageTest2{}
}
```
<br>

## 클래스패스 란?

cmd창에서 바로 클래스 파일을 실행할 수 없고, 일단 해당 패키지의 상위 폴더로 이동한 다음에 실행해야 제대로 작동된다.<br>
매번 상위 폴더로 이동한 다음에 실행하려면 불편하다.<br>
이 불편함을 없애줄 수 있는 것이 클래스패스이다.<br>
클래스패스에 패키지 폴더가 들어있는 경로를 저장해 놓으면 바로 클래스 파일을 실행시킬 수 있다.<br>
<br>

클래스 패스는 클래스 파일(*.class)의 위치를 알려주는 경로이다.<br>
classPath라는 환경변수로 관리하며, 경로간의 구분자';'를 이용해서 경로를 여러개 추가할 수 있다.<br>
classPath라는 환경변수를 만들고(새 시스템 변수), 패키지의 루트를 등록해주어야 한다.<br>
패키지 루트를 등록 후 커멘드창을 다시 열어서 클래스를 실행하면 바로 실행이 된다.<br>









<br><br>





[참고 영상] <https://www.youtube.com/watch?v=hcHJgmX8VlA>