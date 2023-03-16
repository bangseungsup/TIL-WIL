### 인스턴스 멤버
객체 마다 가지고 있는 멤버
- 객체를 생성한 후 사용할 수 있는 필드와 메소드 (객체 안에 존재하는 필드와 메소드)
- 인스턴스 필드 : 힙 영역의 객체마다 가지고 있는 멤버, 객체마다 다른 데이터를 저장
- 인스턴스 메소드 : 객체가 있어야 호출 가능한 메소드,
	클래스 코드(메소드 영역)에 위치하지만, 이해하기 쉽도록 객체마다 가지고 있는 메소드라고 생각해도 됨
    
```
public class Car {
	
    int gas;     -----------> 인스턴스 필드(gas)
    
    void setSpeed(int speed){   ------> 인스턴스 메소드(setSpeed)
    
    }

```

gas 필드와 setSpeed() 메소드는 인스턴스 멤버이기 때문에 외부 클래스에서 사용하기 위해서는 
우선 Car 객체(인스턴스)를 생성하고 참조변수 myCar 또는 yourCar로 접근해야 함

```
Car myCar = new Car();
myCar.gas = 10;
myCar.setSpeed(60);

Car yourCar = new Car();
yourCar.gas = 20;
yourCar.setSpeed(80);
```

gas 라는 (인스턴스) 필드와 setSpeed라는 (인스턴스) 메소드를 인스턴스 멤버라고 한다. 
이 인스턴스 멤버들은 객체가 있어야 사용이 가능.


#### this
객체 외부에서 인스턴스 멤버에 접근하기 위해 참조 변수를 사용하는 것과 마찬가지로 
객체 내부에서도 인스턴스 멤버에 접근하기 위해 this를 사용할 수 있음 (객체는 객체 자신을 this 라고 함)
```
Car(String model) {
 this.model = model; 
 }
 Void setModel(String model){
 	this.model = model;
    }
```

매개변수와 필드의 이름이 다를 경우에는 this를 사용하지 않아도 된다.
```
public class Vehicle;	
    String Car;

	Car(String m){
    	model = m;
        }
```




### 정적 멤버
객체와 상관없는 멤버, 클래스 코드(메소드 영역)에 위치 (정적 멤버란 어딘가에 고정된 멤버, 
 - 정적 필드 및 상수 : 객체 없이 클래스만으로도 사용 가능한 필드
 - 정적 메소드 : 객체가 없이 클래스만으로도 호출 가능한 메소드

#### 정적 멤버 선언
 - 정적필드와 정적 메소드를 선언하려면 필드와 메소드 선언 시 static 키워들 추가적으로 붙이면 됨
 
 
 
####  정적 멤버 사용
- 클래스가 메모리로 로딩되면 정적 멤버를 바로 사용할 수 있음. 클래스 이름과 함께 도트(.)연산자로 접근
```
public class Calculator {
	static double pi = 3.14159;                 ------------> 클래스.필드;
    static int plus(int x, int y) {...}        -------------> 클래스.메소드(매개값,..);
    static int miuns(int x, int y) {...}
}


double result1 = 10 * 10 * Calculator.pi;
int result2 = Calculator.plus(10, 5);
int result3 = Calculator,minus(10. 5);
```


정적 필드와 정적 메소드는 원칙적으로 클래스 이름으로 접근해야 하지만 다음과 같이 객체 참조 변수로도 접근 가능


```
Calculator myCalcu = new Calculator();
double result1 = 10 * 10 * myCalcu.pi;
int result2 = myCalcu.plus(10,5);
int result3 = myCalcu.plus(10,5);
```
 
위와 동일한 같이 나오지만 이클립스에서는 경고 표시가 날 수 있으니, 정적요소는 클래스 이름 자체로 접근하는 것이 좋음



#### 인스턴스 멤버와 정적 멤버 선택 기준?
객체마다 다를 수 있는 필드 값은 _**'인스턴스 필드**_'로 선언
객체마다 다를 필요가 없는 필드 값은_** '정적 필드'**_로 선언



```
public Class Calculator{
String Color;               ---------> 다를 수 있는 값
static double pi = 3.14159; ----------> 다를 수 없는 값
```



```
public Class Calculator{
	string color;
    void setColor(Stirng Color) {this.color = color}  -----> 인스턴스 필드가 사용된다면,
    														static을 붙이지 않고 인스턴스 메소드로 선언
    static int plus(int x, int y) {return x+ y}      -------> 매개 변수만으로 사용되었거나,
    												--------> static 필드만 사용됐을 경우, 정적 메소드 선언
}
```






### 정적 메소드 선언시 주의할 점
- 정적 메소드 선언 시 그 내부에 인스턴스 필드 및 메소드 사용 불가
- 정적 메소드 선언 시 그 객체 자신 참조인 this 키워드 사용 불가



#### 만약, 정적 메소드에서 인스턴스 멤버를 사용하려는 경우, 객체 우선 생성 후 참조 변수로 접근
```
static void Method3(){
	CalssName obj = new CalssName();
    obj.field1 = 10;
    obj.method1();
}
```
