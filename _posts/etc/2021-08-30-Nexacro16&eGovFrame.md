---
title: "Nexacro17 & 전자정부프레임워크 연동"
excerpt: "Nexacro17"
categories: 
  - Learn
tags: 
  - Learn
  - Nexacro17
toc: true
---


## 개발 환경 구성정보

|:--:|:----|
|Web|Apache-tomcat9|
|OS|Windows 10|
|JDK|1.8.0_202|
|IDE|Eclipse|
|DB|mariadb-10.4.21|

<br>
<br>

---

<br><br>

## 전자정부 프레임워크 <br>

### 설치하기

1. [전자정부프레임워크](https://www.egovframe.go.kr/home/main.do) 포털에 접속해서 개발환경을 다운로드 받는다. <br>
2. 다운로드 후 압축을 해제한다.<br>
3. ecllipse 폴더에 있는 eclipse.exe를 실행시킨다.<br>(Workspace를 전자정부의 worspace폴더로 지정한다.)<br>
4. Window > Preferences > Maven > User Settings 에서 Maven의 위치를 세팅해준다.<br>
![Maven > User Settings](https://user-images.githubusercontent.com/73421820/131315372-7e222d63-4e75-4184-bffa-d1ffe5395afb.png)<br>
#### Maven > User Settings<br><br>
![settings.xml](https://user-images.githubusercontent.com/73421820/131315464-0bfa97f8-59aa-4332-bd3c-d4a3f2b22d6b.png)<br>
#### settings.xml 의 localRepository 설정
<br><br>






<br><br>

### eGovFrame Web Project 생성하기

1. 우측 상단의 Open Perspective를 클릭한 후 eGovFrame Perspective를 열어준다.<br>
2. 상단 메뉴를 이용해 eGovFrame Web Project를 새로 생성한다.<br>
3. 프로젝트 생성 시 예제 소스와 같이 생성한다.<br>
4. Server를 생성해서 프로젝트와 연결한다.<br>
5. 서버 실행 후 예제 화면이 정상적으로 작동하는지 확인한다.<br>
<br>
<br><br>

### 샘플예제와 관련된 소스 수정

1. 샘플 예제가 정상적으로 실행되는 것을 확인 후 관련된 소스를 모두 제거한다.<br>
   - java 소스코드가 있는 egovframework.example.sample.패키지<br>
   - sql문이 있는 sqlmap > example 내 mappers 및 sample)<br>
   - WEB-INF > jsp > egovframework 폴더 전체<br>
    ![image](https://user-images.githubusercontent.com/73421820/131316937-34de8de0-8bae-49fc-8595-467a6cbe2163.png)<br><br>
2. sql-mapper-config.xml에 삭제한 샘플 VO 수정
  - 이제 사용하지 않는 VO이기 때문에 주석처리한다. 
  ![image](https://user-images.githubusercontent.com/73421820/131317098-10ed2b85-a246-4ff2-b039-79aca1ca403f.png)<br><br>
3. context-sqlMap.xml 주석 처리
  - context-sqlMap은 ibatis 설정 파일을 가지고   있다.<br> 전자정부에서 제공하는 ibatis sqlMapClientFactoryBean 클래스를 활용하고, iBatis 설정 파일과 디비 접속 정보를 프로퍼티로 전달하는 파일이다.<br> Mybatis를 이용할 예정이기 때문에 주석처리 한다.<br>
  ![image](https://user-images.githubusercontent.com/73421820/131317260-4ddae001-824d-4584-8e87-b9ffa95ba538.png)<br><br>
4. dispatcher-servlet.xml 수정
    - 컨트롤러에서 RequestMapping을 할 때 return해주는 jsp경로를 설정하는 부분이다.<br>
    현재 jsp내에 egovframework/example 폴더를 전부 다 삭제했기 때문에 수정한다.<br>
    ![image](https://user-images.githubusercontent.com/73421820/131317610-3a11824f-c1a8-41fc-b28d-79d6526ca29e.png)<br><br>
5. context-aspect.xml 수정
    - 프로젝트에서 AOP관련 설정이 지정된 XML파일이다.<br>aop:pointcut의 expression을 변경한다.<br>
    ![image](https://user-images.githubusercontent.com/73421820/131319902-13fabc8a-97fc-4535-b598-9615392ebd2c.png)<br><br>
6. dispatcher-servlet 수정
   - context:component-scan의 base-package를 변경한다.
    ![image](https://user-images.githubusercontent.com/73421820/131341140-f614e8c5-a23e-45ae-9588-59ad9c683c54.png)<br><br>

---


## 마리아 DB 연동

1. 마리아 DB에 테이블 생성
2. pom.xml에 마리아DB와 연동
    - pom.xml은 프로젝트에서 어떤 라이브러리를 사용할지 명시해놓은 장소이다.<br> pom.xml에 자기가 사용할 라이브러리의 dependency만 걸어놓으면 알아서 해당 라이브러리의 최신본은 가져와서 적용시켜주고, 라이브러리를 사용하지 않을때는 주석 처리로 없앨 수 있다.<br>
    - 기본으로 설정되어있는 *hsqldb* (프로젝트가 실행되면 그 메모리 위에 올라가는 DB)
    는 주석처리한다.<br>
    - 주석처리 된 것 중 log4jdbc,mysql,commons-dbcp의 주석을 풀어준다.
#### log4jdbc : 쿼리가 수행되면 행동과 결과를 톰캣 콘솔 log에 작성해주는 것<br>
#### mysql : maria DB설정에서 사용하는 것<br>
#### commons-dbcp : 커넥션풀을 관리하는 것 <br>
   - 마리아DB를 사용하기 위해 *mariaDB* dependency 라이브러리 소스를 추가한다.<br>
    ```xml
    <!-- 		
		<dependency>
			<groupId>org.hsqldb</groupId>
			<artifactId>hsqldb</artifactId>
			<version>2.5.0</version>
		</dependency>
     -->
        <!-- mysql이나 oracle DB 사용시 아래 설정 추가 -->
        <dependency>
            <groupId>com.googlecode.log4jdbc</groupId>
            <artifactId>log4jdbc</artifactId>
            <version>1.2</version>
            <exclusions>
                <exclusion>
                    <artifactId>slf4j-api</artifactId>
                    <groupId>org.slf4j</groupId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>commons-dbcp</groupId>
            <artifactId>commons-dbcp</artifactId>
            <version>1.4</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.31</version>
        </dependency>
        
        <dependency>
            <groupId>org.mariadb.jdbc</groupId>
            <artifactId>mariadb-java-client</artifactId>
            <version>2.4.1</version>
        </dependency>
    ```
<br>

3. context-datasource.xml 수정
    - 내 로컬 서버의 접속 데이터명과 접속 계정에 대한 정보가 담긴다.<br> 기존 hsql을 사용하던 것을 주석처리 하고 마리아디비 접속 정보를 작성한다.<br>

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:jdbc="http://www.springframework.org/schema/jdbc"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
            http://www.springframework.org/schema/jdbc  http://www.springframework.org/schema/jdbc/spring-jdbc-4.0.xsd">
    <!-- 	
        테스트 실행용 
        <jdbc:embedded-database id="dataSource" type="HSQL">
            <jdbc:script location= "classpath:/db/sampledb.sql"/>
        </jdbc:embedded-database>
        
        -->
        
        <!-- 마리아 DB  -->
        <bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
            <property name="driverClassName" value="org.mariadb.jdbc.Driver"/>
            <property name="url" value="jdbc:mariadb://127.0.0.1:3306/test"/>
            <property name="username" value="root"/>
            <property name="password" value="root"/>
        </bean>
    </beans>         
    ```

#### 127.0.0.1(=localhost)  :3308(=포트번호)  /sample(=데이터베이스명)<br><br>


---

<br><br>


## uiadapter17

### uiadapter17이란?
springframework 기반으로 구성된 개발환경에서 넥사크로플랫폼의 입/출력 데이터를 변환해주는 기능을 지원하는 모듈이다.<br>
넥사크로플랫폼17은 화면에서 transaction 함수를 통해 서버사이드(springframework기반)와 데이터 송수신 시 `기본 Request객체`를 통한 데이터 처리에는 어려움이 있기 때문에 사용한다.<br>
<br>

**uiadapter17 주요 구성요소 및 역할**

- NexacroRequestMappingHandlerAdapter<br>
    넥사크로 플랫폼의 입/출력 데이터 변환을 수행하기 위해 Spring의 RequestMappingHandlerAdapter의 확장된 형태이다.<br>
    java.util.Map의 데이터 변환을 제공한다.<br>
- NexacroMethodArgumentResolver<br>
    개발자가 작성하게 되는 Controller의 입력 파라미터 중 넥사크로플랫폼의 데이터 변환을 수행한다. (DataSet -> java bean)<br>
- NexacroHandlerMethodReturnValueHandler<br>
Controller에서 반환되는 값을 넥사크로플랫폼의 데이터로 변환한다. (java bean -> Dataset)<br>
- NexacroView<br>
    넥사크로플랫폼으로 데이터 송신 역할을 수행한다.<br>
- NexacroMappingExceptionResolver<br>
    넥사크로플랫폼에서 서비스 요청 시 예외에 대한 처리를 수행한다.<br><br>


<br><br>


### uiadaper17과 전자정부프레임워크 연동

1. pom.xml 파일 수정
    - uiadapter17관련 repository와 dependency 정보를 추가한다.
    - dependency의 uiadapter17 모듈 하위 dependency 라이브러리와 전자정부프레임워크에서 사용되는 라이브러리 중 중복되는 라이브러리는 다운로드되지 않도록 `exclusion` 처리를 한다.

    ```xml
    <!-- ############  NEXACROPLATFORM UIADAPTER17 REPOSITORY  ############ -->
		<repository>
		  <id>tobesoft</id>
		  <url>http://mangosteen.tobesoft.co.kr/nexus/repository/maven-public</url>
		  <releases>
		    <enabled>true</enabled>
		  </releases>
		  <snapshots>
		    <enabled>true</enabled>
		  </snapshots>
		</repository>
 

        <!-- ############  NEXACROPLATFORM UIADAPTER17 LIBRARY  ############ -->
        <dependency>
        <groupId>com.nexacro.uiadapter17.spring</groupId>
        <artifactId>uiadapter17-spring-core</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <exclusions>
            <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            </exclusion>
            <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            </exclusion>
        </exclusions>
        </dependency>
        <dependency>
        <groupId>com.nexacro.uiadapter17.spring</groupId>
        <artifactId>uiadapter17-spring-dataaccess</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <exclusions>
            <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            </exclusion>
            <exclusion>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            </exclusion>
            <exclusion>
            <groupId>org.apache.ibatis</groupId>
            <artifactId>ibatis-sqlmap</artifactId>
            </exclusion>
            <exclusion>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            </exclusion>
            <exclusion>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis-spring</artifactId>
            </exclusion>
            <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            </exclusion>
        </exclusions>
        </dependency>
        <dependency>
        <groupId>com.nexacro.uiadapter17.spring</groupId>
        <artifactId>uiadapter17-spring-excel</artifactId>
        <version>1.0.0-SNAPSHOT</version>
        <exclusions>
            <exclusion>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            </exclusion>
        </exclusions>
        </dependency>
    ```
    추가 후 Maven Dependencies에 uiadapter17관련 라이브러리가 추가되었는지 확인한다.<br>
    ![image](https://user-images.githubusercontent.com/73421820/131325879-cf6d4472-3e04-481a-b427-91b972c3f6fe.png)<br><br>
2. dispatcher-servlet.xml 수정
    - uiadapter17 데이터변환 처리관련 설정을 위해 수정한다.<br> uiadapter17 적용의 제일 중요하며 uiadapter17 모듈의 bean으로 등록하여 사용하기 위한 설정이다. <br>uiadapter17의 중요 기능들을 선언해서 사용할 수 있게 한다.<br>

    ```xml
    <!-- /////////////////////////////////// 넥사크로플랫폼 UIADAPTER17 설정 시작 /////////////////////////////////// -->
        <bean id="nexacroFileView"    class="com.nexacro.uiadapter17.spring.core.view.NexacroFileView" />
        <bean id="nexacroView"        class="com.nexacro.uiadapter17.spring.core.view.NexacroView">
            <property name="defaultContentType" value="PlatformXml" />
            <property name="defaultCharset" value="UTF-8" />
        </bean>
        
        <!-- 넥사크로플랫폼 RequestMappingHandlerAdapter 구현체 등록 -->
        <bean class="com.nexacro.uiadapter17.spring.core.resolve.NexacroRequestMappingHandlerAdapter" p:order="0">
            <property name="customArgumentResolvers">
                <list><bean class="com.nexacro.uiadapter17.spring.core.resolve.NexacroMethodArgumentResolver" /></list>
            </property>
            <property name="customReturnValueHandlers">
                <list>
                    <bean class="com.nexacro.uiadaptecontroresolve.NexacroHandlerMethodReturnValueHandler">
                        <property name="view"     ref="nexacroView" />
                        <property name="fileView" ref="nexacroFileView" />
                    </bean>
                </list>
            </property>
        </bean>    
        
        <!-- 넥사크로플랫폼 EXCEPTION-RESOLVER 등록 -->
        <bean id="exceptionResolver" class="com.nexacro.uiadapter17.spring.core.resolve.NexacroMappingExceptionResolver" p:order="1">
            <property name="view" ref="nexacroView" />   
            <property name="shouldLogStackTrace" value="true" />   
            <property name="shouldSendStackTrace" value="true" />
            <!-- shouldSendStackTrace 가 false 일 경우 nexacro platform으로 전송되는 에러메시지  -->
            <!-- <property name="defaultErrorMsg" value="An Error Occured. check the ErrorCode for detail of error infomation" /> -->
            <property name="defaultErrorMsg" value="fail.common.msg" />
            <property name="messageSource" ref="messageSource" />   
        </bean>    
        <!-- /////////////////////////////////// 넥사크로플랫폼 UIADAPTER17 설정 끝 /////////////////////////////////// -->
    ```
3. context-nexacro.xml 생성
    - Database 타입에 맞는 Mybatis Map의 컬럼 생성을 위해 resources/egovframework/spring경로 아래에 context-nexacro.xml 파일을 생성한다.<br>
    ![image](https://user-images.githubusercontent.com/73421820/131325719-1e647329-7897-4cfe-bd97-9fcdc945bf71.png)<br>
    - mariadb를 사용할 것이기 때문에 Database 형식을 mariadb로 셋팅한다.<br>

    ```xml
        <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:p="http://www.springframework.org/schema/p" 
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:aop="http://www.springframework.org/schema/aop"
        xsi:schemaLocation="
            http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd">
        
        <bean id="applicationContextProvider" class="com.nexacro.uiadapter17.spring.core.context.ApplicationContextProvider" lazy-init="false" />

        <bean id="hsqlDbms"   class="com.nexacro.uiadapter17.spring.dao.dbms.Hsql" />
        <bean id="oracleDbms" class="com.nexacro.uiadapter17.spring.dao.dbms.Oracle" />
        <bean id="mssqlDbms"  class="com.nexacro.uiadapter17.spring.dao.dbms.Mssql" />
        <bean id="mysqlDbms"  class="com.nexacro.uiadapter17.spring.dao.dbms.Mysql" />
        <bean id="tiberoDbms" class="com.nexacro.uiadapter17.spring.dao.dbms.Tibero" />
        
        <bean id="dbmsProvider" class="com.nexacro.uiadapter17.spring.dao.DbVendorsProvider">
            <property name="dbvendors">
                <map>
                    <entry key="MySQL"     value-ref="mysqlDbms"/>
                </map>
            </property>
        </bean>
    </beans>
    ```
4. Mybatis의 Map 사용시 interceptor plugin 셋팅
    - Mybatis의 Map사용 시 기본적으로 데이터 조회 후 결과가 없을 시 컬럼을 생성하지 않는다.<br>하지만 interceptor plugin을 셋팅하면 쿼리 resultType이 Map일 경우 조회 결과가 없어도 해당 컬럼을 생성한다.
    - Mybatis 설정 파일인 sql-mapper-config.xml에 추가한다.
    
    ```xml
    <!-- myBatis Inteceptor for get column information  -->
    <plugins>  
        <!-- NexacroMybatisMetaDataProvider :  Executor Plugin:쿼리를 실행하며, 
            List 형태의 데이터 조회 시 조회된 결과가 0건일 경우 결과 메타데이 터 정보를 조회
            resultType이 Map 일 경우 추가적으로 메타데이터 조회를 위한 쿼리를 실행한다.  -->
        <plugin interceptor="com.nexacro.uiadapter17.spring.dao.mybatis.NexacroMybatisMetaDataProvider" />
        <!-- NexacroMybatisResultSetHandler :ResultSetHandler Plugin – Executor Plugin에서 메타데이터 조회를 위해 쿼리를 실행하였을 경우에만 동작, 
            ResultSetMetaData 와 resultType에 존재하는 필드 정보를 확인하여 메타데이터 정보를 조회 한 다.  -->
        <plugin interceptor="com.nexacro.uiadapter17.spring.dao.mybatis.NexacroMybatisResultSetHandler" />
    </plugins>
    ```


<br><br>


### Nexacro17과 전자정부프레임워크 연동

1. 넥사크로 프로젝트 생성
    - 넥사크로 프로젝트를 eclipse workspace안에 생성한다.
    - 경로  : workspace\프로젝트명\src\main\넥사크로 프로젝트 명
    ![image](https://user-images.githubusercontent.com/73421820/131328227-9210ce80-3474-45b6-ab89-95bc5c3e3406.png)<br><br>
2. 프로젝트 Generate Path 생성
    - 실제 웹서버에 위치할 generate될 js 소스 경로를 eclipse workspace의 이클립스 프로젝트의 wepapp아래로 설정한다.
    - 경로 : workspace\프로젝트명\src\main\webapp\넥사크로 프로젝트 명 
    ![image](https://user-images.githubusercontent.com/73421820/131328406-f7301c6c-1550-4cc1-8382-17989a0692f5.png)<br><br>
3. 샘플 form을 디자인
    - 샘플 form을 디자인하고,<br>  web.xml  welcome-file-list에  index.html을 추가한 후<br> 샘플 화면이 정상적으로 나오는지 확인한다.
    ![image](https://user-images.githubusercontent.com/73421820/131328627-5c87f6b4-2fe3-4ff7-87c8-1ce85fa3b1cc.png)<br><br>
    ![image](https://user-images.githubusercontent.com/73421820/131328790-4275c3a9-df09-4c99-a4bb-3298deea2784.png)![image](https://user-images.githubusercontent.com/73421820/131329720-a85eec91-f26a-4310-b3d3-38b34b256169.png)<br><br>
    ![image](https://user-images.githubusercontent.com/73421820/131329544-54dbe486-b1ba-478f-be0c-6d41c48c8a08.png)<br><br>
4. 이클립스 MVC 생성
    - 자바 소스,  실제 쿼리가 정의되어있는 XML파일 생성<br>
    ![image](https://user-images.githubusercontent.com/73421820/131331507-de46fcf9-f7b0-4506-ad91-476d7d697d4b.png)<br><br>
5. context-common.xml 파일 수정
    - component-scan 관련 설정 변경 -> base-package를 본인의 프로젝트 명으로 변경해야 된다.

    ```xml
    <context:component-scan base-package="com.tobesoft.egovtest">
       <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller" />
    </context:component-scan>
    ```
6. context-mapper.xml 파일 수정
    - 생성한 쿼리의 파일 경로 및 Mapper Class scan을 위한 패키지  경로를 수정한다.
    - mapperLocations의 value를 쿼리가 있는 경로로 수정.
    - bean의 value를 mapper namespace와 ID를 연결할 interface 경로로 수정.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd">
    
    <!-- SqlSession setup for MyBatis Database Layer -->
    <bean id="sqlSession" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="configLocation" value="classpath:/egovframework/sqlmap/example/sql-mapper-config.xml" />
        <property name="mapperLocations" value="classpath:/egovframework/sqlmap/mappers/*.xml" />
    </bean>

    <!-- MapperConfigurer setup for MyBatis Database Layer with @Mapper("deptMapper") in DeptMapper Interface -->
    <bean class="egovframework.rte.psl.dataaccess.mapper.MapperConfigurer">
        <property name="basePackage" value="com.tobesoft.egovtest.mapper" />
    </bean>
    
    </beans>
    ```
#### mapperLocations : sql이 작성된 파일의 위치를 설정한다.(mappers폴더안에 .xml 으로 끝나는 파일을 모두 읽는다.)
#### basePackage : Mapper 인터페이스 파일이 위치한 패키지 경로를 설정한다.<br><br><br>
7. context-transaction.xml 파일 수정
    - Database 롤백 관련 정보를 수정한다. <br>aop:config의 expression 변경

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:tx="http://www.springframework.org/schema/tx"
        xmlns:aop="http://www.springframework.org/schema/aop"
        xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
            http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
            http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd">

        <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
            <property name="dataSource" ref="dataSource"/>
        </bean>

        <tx:advice id="txAdvice" transaction-manager="txManager">
            <tx:attributes>
                <tx:method name="*" rollback-for="Exception"/>
            </tx:attributes>
        </tx:advice>

        <aop:config>
            <aop:pointcut id="requiredTx" expression="execution(* com.tobesoft.egovtest..impl.*Impl.*(..))"/>
            <aop:advisor advice-ref="txAdvice" pointcut-ref="requiredTx" />
        </aop:config>

    </beans>
    ```
8. dispatcher-servlet.xml 파일 수정
    - annotaion-driven을 추가하고 기존 설정을 삭제한다.<br>
    컴포넌트 scan을 위한 패키지 경로를 변경한다.

    ```xml
    	<mvc:annotation-driven/>
	
    <context:component-scan base-package="com.tobesoft.egovtest" use-default-filters="false">
        <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Service"/>
        <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Repository"/>
    </context:component-scan>
    ```
9. log4j2.xml 파일 수정
    - 로그사용을 위한 패키지를 등록한다.
    - 신규 생성한 패키지와 com.nexacro 패키지를 추가로 등록해서 uiadapter17의 디버그용 로그를 확인한다.

    ```xml
    	<Logger name="com.tobesoft.egovtest" level="DEBUG" additivity="false">
        	<AppenderRef ref="colsole"/>
        </Logger>
        <Logger name="com.nexacro" level="DEBUG" additivity="false">
        	<AppenderRef ref="console"/>
        </Logger>
    ```

10.  nexacro server License 적용
    - uiadapter17에서 nexacro api를 사용하기 때문에 개발 라이센스를 resources 폴더에 복사한다.<br>
    ![image](https://user-images.githubusercontent.com/73421820/131333379-55f9a958-e963-41c1-9dc3-dafa48370aa9.png)<br><br>
11. 서버가 구동되는지 확인한다.
12. 넥사크로에서 service prefix 추가
    - Service  Edit를 눌러서  prefixId(svc)와 Type : jsp, Url은 서버와 연결되는 주소, cachelevel은 none으로 설정한다. <br>
    ![image](https://user-images.githubusercontent.com/73421820/131358487-f75f353f-e622-4c96-a7fa-1b8ee2018778.png)<br><br>
13. 생성한 컴포넌트에 데이터 바인딩, 버튼 클릭 시 실행될 스크립트 작성
14. 마리아 DB에 데이터 추가하기
15. 서버 실행 후 잘 동작하는지 확인한다.


<br><br>


---


참고 : [플레이 넥사크로](https://www.playnexacro.com/#show:learn:1385)

<br><br>