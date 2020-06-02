# [Java] JaCoCo with PowerMock code coverage problem
> JaCoCo PowerMock no coverage report
> JaCoCo + PowerMock doesn’t work
> 
> [ant:jacocoReport] Classes in bundle '' do no match with execution data. For report generation the same class files must be used as at runtime.
> [ant:jacocoReport] Execution data for class does not match.

**TOC**
- [TLTR; Solution](#tltr-solution)
- [coverage report](#coverage-report)
- [Code coverage with JaCoCo](#code-coverage-with-jacoco)
    - [How to JaCoCo calculate coverage](#how-to-jacoco-calculate-coverage)
    - [Problem situation](#problem-situation)
    - [Problem  example](#problem-example)
    - [JaCoCo Offline Instrumentation](#jacoco-offline-instrumentation)
    - [build.gradle](#buildgradle)
- [Conclusion](#conclusion)
- [References](#references)

### TLTR; Solution
JaCoCo의 [Offline Instrumentation](https://github.com/powermock/powermock/wiki/Code-coverage-with-JaCoCo#offline-instrumentation) 방식을 사용하면 제대로 된 코드 커버리지를 측정할 수 있다. (예제 프로젝트 https://github.com/SangHakLee/jacoco-offline-gradle)


## coverage report
> [JaCoCo](https://www.jacoco.org/jacoco/trunk/doc)와 [PowerMock](https://github.com/powermock/powermock)을 함께 쓰면 커버리지를 제대로 측정할 수 없다.
> 테스트 코드를 수행하지 않은 것이 아니라, 테스트 코드는 수행하는데 테스트 코드가 얼마나 수행된지 나타내는 커버리지를 제대로 측정할 수 없다.
> 
> [이것은 잘 알려진 문제점이다.](https://github.com/jacoco/jacoco/issues/676)


### Code coverage with JaCoCo
PowerMock에서는 이미 이 이슈를 알고 있고 원인과 해결책을 제공한다.
https://github.com/powermock/powermock/wiki/Code-coverage-with-JaCoCo


#### How to JaCoCo calculate coverage
JaCoCo가 코드 커버리지를 측정할 수 있는 이유는 런타임 환경에서 코드 커버리지를 측정할 수 있게 바이트 코드(.class)를 조작하였기 때문이다.
> JVM이 동적으로 바이트 코드를 읽는 과정에서 바이트 코드를 조작해서 커러비지를 측정할 수 있는 카운터 코드들을 동적으로 추가한다.
> [자바 바이트코드](https://iamsang.com/blog/2012/08/19/introduction-to-java-bytecode)

이러한 바이트 코드 조작에 [Java Agent](https://www.jacoco.org/jacoco/trunk/doc/agent.html)를 이용한다. 
> JaCoCo는 기본적으로  Java Agent를 이용해서 [On-the-fly](https://www.jacoco.org/jacoco/trunk/doc/implementation.html) 방식으로 바이트 코드를 조작한다.
> 
> `On-the-fly`는 즉석에서라는 뜻이다.
> `On-the-fly`는 코드 커버리지 측정 시 런타임 환경(JRE)의 [클래스 로더](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%ED%81%B4%EB%9E%98%EC%8A%A4%EB%A1%9C%EB%8D%94)가 `.class` 바이트 코드를 읽고 런타임에 실행하는 순간에 코드를 조작한다. (**런타임**의 [클래스 로더](https://www.javatpoint.com/classloader-in-java)가 바이트 코드를 로드하는 시점)
> 
> 따라서, 클래스 파일을 보면 바이트 코드 자체는 조작된 것이 없다. (정적인 바이트 코드는 그대로 두고 로드하면서 메모리에서 조작)
> 
> 클래스 파일 자체를 조작하지 않고 **클래스 로더가 원본 바이트 코드를 메모리에 로드할 때 조작**하여 JVM 실행 엔진이 인터프리터 방식으로 해당 라인을 수행할 때 **조작된 바이트 코드를 이용해 커버리지를 측정한다.**
> 
> 이러한 방식으로 동작하기 때문에 `On-the-fly` **즉석에서**라는 이름이 붙여진 거 같다.


#### Problem situation

**하지만**, `On-the-fly` 방식과 PowerMock을 사용하면 [Javaasist](https://www.javassist.org) 때문에 문제가 발생한다.
> The main issue is that Javassist reads classes from disk and all JaCoCo changes are disappeared. As result zero code coverage for classes witch are loaded by PowerMock class loader.
> https://github.com/powermock/powermock/wiki/Code-coverage-with-JaCoCo#on-the-fly-instrumentation

설명했듯, JaCoCo의 `On-the-fly`는 디스크에 저장된 클래스 파일을 그대로 사용하지 않고 런타임에서 조작하여 커버리지를 측정한다. 
 **그러나, PowerMock의 `Javaasist`는 미리 클래스 파일을 조작하고 디스크에 저장한 뒤 이 클래스 파일을 런타임에서 사용한다.**
 
 즉, PowerMock이 읽어 온 클래스 파일(바이트 코드)과 JaCoCo가 읽은 클래스 파일(바이트 코드)이 다르게 되는 것이다.
 - **JaCoCo**: JVM  메모리에 올라간 바이트 코드 이용
 - **PowerMock**: Javaassist가 디스크(로컬)에 저장된 클래스 파일을 이용

##### Problem  example
```bash
$ ./gradlew test
...
> Task :jacocoTestReport
[ant:jacocoReport] Classes in bundle 'api' do no match with execution data. For report generation the same class files must be used as at runtime.
[ant:jacocoReport] Execution data for class com/example/api/BaseApi does not match.
[ant:jacocoReport] Execution data for class com/example/api/user/UserApi does not match.
...
```
- 에러를 읽어 보면, **리포트를 만들기 위해선 런타임에서 같은 클래스 파일을 사용해야한다고 한다.**

이러한 문제는 예전부터 알려졌으며, 이러한 문제를 해결하기 위해 PowerMock 팀에선 [Javassist -> ByteBuddy](https://github.com/powermock/powermock/issues/727)로 변경을 계획 중이이라고 한다.
**하지만, 이 변경은 아직도 이뤄지지 않았다.(2020)**

#### JaCoCo Offline Instrumentation

따라서 PowerMock에선 `Offline` 방식으로 이 문제를 해결할 수 있다고 안내한다. https://github.com/powermock/powermock/wiki/Code-coverage-with-JaCoCo#offline-instrumentation
> `Offline`은 `On-the-fly`와 다르게 미리 클래스 파일 자체를 조작하여 디스크(로컬)에 저장 후 이것을 이용한다.
> 
> 따라서, 빌드된 클래스 파일들을 보면 코드에 이상한 내용들이 들어간 것을 확인할 수 있다.
> ```java
> // Api.class
> public class Api {
>       public Api() {
>           boolean[] var1 = $jacocoInit();
>           super();
>           var1[0] = true;
>       }
> }
> ```
> 위의 예시와 같이 미리 바이트 코드를 조작해서 이 클래스 파일을 런타임에서 사용한다. (Java Agent가 개입하여 바이트 코드 로딩 시 개입하지 않는다.)
> 
> 따라서, PowerMock과 JaCoCos는 디스크의 같은 클래스 파일을 사용하기 때문에 제대로 된 리포트를 받아볼 수 있다.

JaCoCo 사용 시 PowerMock을 사용하지 않는 대부분의 경우에선 기본 instrumentation인 `On-the-fly`를 그대로 사용하면 된다.

`Offline` 방식은 빌드 스크립트 수정해야 하기 때문에 `On-the-fly`를 [사용할 수 없는 경우에만](https://www.eclemma.org/jacoco/trunk/doc/offline.html) 사용하고 나머진 `On-the-fly` 방법을 그대로 사용하는 것을 추천한다.

---

![JaCoCo   PowerMock](https://user-images.githubusercontent.com/9030565/83384563-7d860580-a422-11ea-8a13-6dbba074e5cb.jpg)
- 기본적으로 컴파일 시 그림 좌상단의 바이트 코드와 같이 컴파일된 클래스 파일이 생성된다.
- JaCoCo On-the-fly의 경우 이 클래스 파일을 조작하지 않고 그대로 로딩하면서 바이트 코드 조작이 필요하면 이 때 조작한다.
- PowerMock, JaCoCo Offline의 경우 클래스 파일을 미리 조작하여 바이트 코드 자체가 변형된다.
	- PowerMock의 경우 기존 메소드나 클래스를 대신할 바이트 코드가 들어갈 것이고
	- JaCoCo의 경우 코드 커버리지 측정을 위한 바이트 코드들이 들어간다.
- 그림을 보면 JaCoCo On-the-fly와 PowerMock의 바이트 코드와 런타임 실행 데이터가 다르다.
- 그림을 보면 JaCoCo Offline과 PowerMock의 바이트 코드와 런타일 실행 데이터가 일치한다.


---

#### build.gradle
Gradle에서 `Offline` 방식은  간단하게 옵션 변경 등으로 설정할 수 없고 빌드 스크립트를 수정해야 한다.

Maven의 경우 비교적 쉬운 방법으로 `Offline Instrumentation` 사용할 수 있다.
- https://github.com/powermock/powermock-examples-maven/tree/master/jacoco-offline
- https://www.eclemma.org/jacoco/trunk/doc/examples/build/pom-offline.xml

하지만, Gradle의 경우 build 스크립트에 tasks 들을 추가시켜야 한다.

아래에 예시는 절대적이지 않으며, 각 프로젝트 환경에 따라서 task 내용 수정이 필요할 수 있다.

```groovy
// build.gradle

plugins {
    id 'java'
    id 'jacoco'
}

configurations {
    // for jacoco-powermock
    jacocoAnt
    jacocoRuntime
}

repositories {
    jcenter()
}

dependencies {
    testImplementation 'junit:junit:4.12'

    // for jacoco-powermock
    jacocoAnt group: 'org.jacoco', name: 'org.jacoco.ant', version: '0.8.5', classifier: 'nodeps'
    jacocoRuntime group: 'org.jacoco', name: 'org.jacoco.agent', version: '0.8.5', classifier: 'runtime'
}

// jacoco Offline Instrumentation https://github.com/powermock/powermock/wiki/Code-coverage-with-JaCoCo
task instrument(dependsOn: ['classes']) {
    ext.outputDir = buildDir.path + '/classes-instrumented'
    doLast {
        ant.taskdef(
            name: 'instrument',
            classname: 'org.jacoco.ant.InstrumentTask',
            classpath: configurations.jacocoAnt.asPath
        )
        ant.instrument(destdir: outputDir) {
            sourceSets.main.output.classesDirs.each { fileset(dir: it) }
        }
    }
}
gradle.taskGraph.whenReady { graph ->
  if (graph.hasTask(instrument)) {
    tasks.withType(Test) {
      doFirst {
        systemProperty 'jacoco-agent.destfile', buildDir.path + '/jacoco/tests.exec'
        classpath = files(instrument.outputDir) + classpath + configurations.jacocoRuntime
      }
    }
  }
}
task report(dependsOn: ['instrument', 'test']) {
    doLast {
        ant.taskdef(
            name: 'report',
            classname: 'org.jacoco.ant.ReportTask',
            classpath: configurations.jacocoAnt.asPath
        )
        ant.report() {
            executiondata {
                ant.file(file: buildDir.path + '/jacoco/tests.exec')
            }
            structure(name: 'Example') {
                classfiles {
                    sourceSets.main.output.classesDirs.each { fileset(dir: it) }
                }
                sourcefiles {
                    fileset(dir: 'src/main/java')
                }
            }
            html(destdir: buildDir.path + '/reports/jacoco')
        }
    }
}
test.dependsOn instrument
```
- **task instrument**
  - `classes task`(Assembles main classes) 에 의존되기 때문에 `.java` -> `.class` 생성 (`/build/classes`)
  - `ant.instrument` task를 통해 `/build/classes-instrumented` 경로에 바이트 코드가 조작된 클래스 파일 생성
  - `/build/classes`, `/build/classes-instrumented`를 보면 같은 클래스 파일인데 코드 내용이 조금씩 다름
    - `$jacocoInit();` 이런 코드들이 조작되어 들어감
- **test.dependsOn instrument**
  - `test`는 `instrument`에 의존되기 때문에 테스트 전에 `Offline instrument`를 미리 생성한다.

---

```bash
$ ./gradlew test
...
Please supply original non-instrumented classes.
...
```
- 이제 커버리지가 제대로 나오지만 위와 같은 메세지가 나온다. `Offline` 방식은 비선호하는 거 같다.
  - task 중 해당 메세지가 너무 많이 나오기 때문에 보고 싶지 않으면 `$ ./gradlew test -q` 옵션을 사용한다. 

---

**자세한 예제는 https://github.com/SangHakLee/jacoco-offline-gradle에서 확인 가능하다.**

### Conclusion
필자는 Java에 익숙하지 않다. JavaScript, Python 언어를 좋아하고 업무에서도 해당 언어를 주로 다뤘다.
부서 이동으로 Java 환경에서 작업이 필요했고 개발하면서 Java에 대해 다시 알게 된 내용들이 많다.

대학 졸업 후 잊고 있었던 Java의 JVM, 컴파일, 바이트코드 등 Java의 동작 원리에 대해서 자세하게 알아본 좋은 기회였다.
또한, 분명 학부 시절 Java는 컴파일 언어라 배웠고 그냥 그렇게 외우기만 했다. (보통 수업에 관심 없는 1학년 때 배우기 때문에 주요 개념만 외웠던 거 같다.)
사실, Java는 소스코드(.java)를 클래스 파일(.class)로 변환할 땐 컴파일하고 JVM이 런타임에선 인터프리터와 [JIT 컴파일러](https://medium.com/@ahn428/java-jit-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC-c7d068e29f45)를 사용한다는 사실도 다시 알게 됐다. (학부 때 배우긴 했을 것이다.)

코드 커버리지를 얻기 위해 대략 일주일을 이 문제 해결에 사용한 것 같다. 
만약, PowerMock이 `Javassist -> ByteBuddy` 변경하여 이 문제를 겪지 않았다면 JVM 동작 방식에 대해서 이렇게 자세히 알아보려 하지 않았을 것이다.
문제 해결 과정은 정말 힘들었지만, Java가 코드 커버리지를 측정하는 원리와 JVM 동작 원리에 대해서 깊게 공부할 수 있었다.


### References
- https://www.jacoco.org/jacoco/trunk/doc/
- https://github.com/powermock/powermock/wiki/Code-coverage-with-JaCoCo
- https://github.com/gradle/gradle/issues/2429
- https://github.com/jacoco/jacoco/issues/51
- https://github.com/SurpSG/jacoco-offline-instrumentation
- https://github.com/raphw/byte-buddy/issues/296
- https://github.com/AureaMohammadAlavi/gradle-jacoo-offline-instrumentation
- https://www.eclemma.org/jacoco/trunk/doc/offline.html
- [Java BCI(Byte Code Instrumentation) 간단 예제와 설명](https://deidesheim.tistory.com/entry/%EC%9E%90%EB%B0%94-BCIByte-Code-Instrumentation-%EA%B0%84%EB%8B%A8-%EC%98%88%EC%A0%9C%EC%99%80-%EC%84%A4%EB%AA%85)
- http://tcpschool.com/java/java_intro_programming
- https://iamsang.com/blog/2012/08/19/introduction-to-java-bytecode/
- http://www.semdesigns.com/Company/Publications/TestCoverage.pdf

---

Java에 대해서 필자가 잘못 이해한 내용 혹은 잘못된 해석이 있다면 알려주세요.
Java 동작 원리에 대해 완벽하게 이해하지 못하여 잘못된 정보를 설명했을 수도 있습니다.
