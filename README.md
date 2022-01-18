# 블로그 프로젝트
<자바 웹을 다루는 기술>을 끝마치고 더 높은 역량을 갖추기 위해 블로그 프로젝트를 시작한다.<br>
책을 보며 했던 클론코딩이 아닌, 스스로 찾아보고 고민하며 만들어보고자 한다.

## 기술 스택
여러가지 기술스택들을 비교해 프로젝트의 기능 및 목적에 맞게 선정하는 것이 맞으나 이 프로젝트의 목적은
공부했던 내용을 복습하는 것이기 때문에 각 기술 스택들의 정의와 기능을 정리하려고 한다.<br>
<br>

<h3>1) Spring Framework</h3> <br>
<hr/>

<h3>2) Oracle DB</h3>

###### DB란 무엇일까? <br>
- 통합된 데이터(Intergrated data) : 자료를 하나로 통합하여 수정, 생성 작업이 발생하더라도 중복되지 않습니다.
- 공유된 데이터(Shared data) : 여러 응용 시스템에서 공동으로 접근가능하고 소유할 수 있는 데이터입니다.
- 운영 데이터(Operational data) : 운영 가능하고 업무에 반드시 필요한 데이터입니다.
- 저장된 데이터(Stored data) : 컴퓨터에서 접근 가능한 저장된 데이터입니다.

RDBMS(관계형 데이터베이스 관리 시스템)이고 하나의 데이터베이스를 만든 후 관련되는 응용 프로그램들을 변경하지 않고도
새로운 데이터 항목을 데이터 베이스에 추가 할 수 있다는 장점이 있다.<br>
그 외의 RDBMS에는 사이베이스, 인포믹스, Mysql, SQL Server가 있다.
<hr/>

<h3>3) HTML5</h3>
월드와이드웹을 통해 제공되는 정보를 나타낼 목적으로 사용되는 마크업 언어이며 차세대 웹표준으로 확정된 HTML의 5번째 버전을 의미한다.

###### HTML5의 디자인 원칙
 - 호환성
 - 실용성
 - 상호 운영성
 - 보편적 접근성
<hr/>

<h3>4) CSS</h3>

<hr/>
<h3>5) Javascript</h3>

<hr/>
<h3>6) Jquery</h3>

<hr/>

<h3>7) Ajax</h3>

<hr/>

<h3>8) Mybatis</h3>
<h4>필요한 설정파일</h4>
1. configuration xml 파일
typeAliases 태그 : VO(DTO) 객체를 매퍼.xml에서 간편하게 사용할 수 있도록 alias(별명)를 지정하는 태그
<br><br>
2. mapper xml 파일

* select 태그<br>
 ex)
 ```
 <select id = "login" resultType = "memberVO" parameterType = "java.util.Map">
     <![CDATA[
      select * from members where id = #{id} and password = #{password}
     ]]>
 </select>
 ```
 
 |속성|설명|
 |-------|----------------|
 |id|구문을 찾기 위한 유일한 구분자|
 |parameterType|구문에 전달되는 파라미터의 alias나 풀 클래스명|
 |resultType|구문의 결과를 받을 alias나 풀 클래스명|
 |resultMap|resultType과 다르게 xml 내 선언해서 사용하는 커스텀 맵|<br><br>
 
 
3. mybatis context xml 파일

<h4>Database Connection Pool(DBCP)</h4>

```
<bean id="dataSource" class="org.apache.ibatis.datasource.pooled.PooledDataSource">
	<property name="driver" value="${jdbc.driverClassName}" />
	<property name="url" value="${jdbc.url}" />
	<property name="username" value="${jdbc.username}" />
	<property name="password" value="${jdbc.password}" />
</bean>
```

 JDBC를 통해 DB를 연결할 경우, 매번 드라이버를 로드하고 Connection 객체를 가져와야하는데 매우 비효율적이다.<br>
 이를 해소하기 위해 웹 컨테이너가 실행되면서 DB와 연결된 Connection 객체를 미리 Pool에 생성해두고 필요할 때에 가져다쓰고 반환한다.<br>
 DB의 부하를 줄이고 유동적으로 연결을 관리할 수 있다.

 
<h4>SqlSessionFactory</h4>

```
<bean id ="sqlSessionFactory" class= "org.mybatis.spring.SqlSessionFactoryBean" >
	    <property name="dataSource" ref="dataSource" />
	  <property name="configLocation" value="classpath:mybatis/models/modelConfig.xml"  />
	  <property name="mapperLocations" value="classpath*:mybatis/mappers/*.xml" />
	</bean >
```
데이터베이스와의 연결과 SQL의 실행에 대한 모든 것을 가진 가장 중요한 객체이다.<br>
이 객체가 DataSource를 참조하여 Mybatis와 DB서버를 연동시켜준다.

<h4>SessionFactory</h4>
: 단일 데이터 저장소, 안정적인 스레드를 위한 목적으로 사용된다.<br>
SessionFactory를 사용함으로서 다수의 스레드가 DB세션에 동시에 접근가능하며 특정 데이터베이스에 매핑된 캐시가 변하지 않는다.<br>
SessionFactory는 프로그램이 실행될 때만 생성되고 프로그램 내의 소스코드가 이에 접근할 수 있도록 Singleton 형태를 지닌다.

<h4>Config</h4>
Mybatis는 SQL Mapping 프레임워크로 별도의 설정 파일을 가질 수 있다.
* mybatis-config.xml 파일을 이용
* sqlSessionFactory에 다음과 같이 configLocation 속성을 추가

--> 별도의 설정파일이 필요한 다른 이유는 마이바티스 XML파일이 매퍼 클래스와 동일한 classpath에 있지 않은 경우다.<br>
Spring + Mybatis를 사용하면, DataSource와 SqlSessionFactory를 정의해서 빈으로 주입시켜줘야한다.
* configLocation : mybatis 설정파일이 위치한 경로를 지정(주로 mybatis-config.xml 위치)
* mapperLocations : Mapper를 스캔하기 위한 XML 파일 경로 지정(*mapper.xml 파일들이 모여있는 위치)

<h4>SqlSessionTemplate</h4>
SessionFactory에 DatabaseSource를 주입하여 DB와의 연결통로를 만들고, 각 DAO에서 DB와의 연결을 사용하기 위해 SessionTemplate를 주입한다.

```
<bean id ="sqlSession" class= "org.mybatis.spring.SqlSessionTemplate" >
    <constructor-arg ref= "sqlSessionFactory"></constructor-arg >
</bean >
```

<h4>MapperScannerConfigurer</h4>

```
<bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="mapper.AAA" />
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactoryAAA" />
</bean>
```
Mapper 인터페이스의 수가 많아지면 MapperScannerConfigurer를 이용하여 Mapper 인터페이스의 객체를 한 번에 등록하는 것이 편리함.<br>
이를 이용하면 지정한 패키지 아래 모든 인터페이스가 Mapper 인터페이스로 간주되어 Mapper 인터페이스의 객체가 DI컨테이너에 등록되는 것이다.<br>
basePackage 속성에서 지정하는 것은 Mapper 인터페이스를 검색할 대상이 되는 Package


출처 : https://hyeounstory.tistory.com/127?category=885376

<hr/>

## 참조
[마크다운 사용법](https://gist.github.com/ihoneymon/652be052a0727ad59601)
