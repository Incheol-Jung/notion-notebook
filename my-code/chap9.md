# 📌 인상 깊었던 내용

## **📚 null을 리턴/전달하지 말기**

```jsx
class Equipment {
	static final Equipment EMPTY = new Equipment("장비 없음", 0, 0, 0);
	...
}

// 모든 장비 해제
void takeOffAllEquipments() {
	head = Equipment.EMPTY;
	body = Equipment.EMPTY;
	arm = Equipment.EMPTY;
}
```

> null을 리턴하지 않는 설계란 메서드에서 null을 리턴하지 않게 작성하는 것을 말합니다. 그리고 null을 전달하지 않는 설계란 메서드에서 null을 변수에 할당하지 않는 것입니다. 
> 
> 📕 196p 5번째 (9장)
> 

### **🧐 : null일 경우 초기화할 수 있는 디폴트값이 필요하다.. 이 부분은 코틀린에서는 ?:로 처리되어 습관을 들이는것도 좋을것 같다**

## **📚 기술 중심 패키징**

```jsx
// AS-IS

├─UseCases
│├─재고_유스케이스.java
│└─주문_유스케이스.java
├─Entities
│├─입고_엔티티.java
│└─출고_엔티티.java
├─valueObjects
│├─입고_엔티티.java
│└─출고_엔티티.java

// TO-BE

├─재고
│├─재고_유스케이스.java
│├─입고_엔티티.java
│├─출고_엔티티.java
│├─입고_엔티티.java
│└─출고_엔티티.java
├─주문
│├─주문_유스케이스.java
│└─장바구니_엔티티.java
```

> 비즈니스 클래스를 기술 중심 패키징에 따라 폴더를 구분하면, 관련성을 알기 매우 힘들어집니다. 
> …
> 비즈니스 개념별로 묶어두면, 주문과 결제 등 관계없는 유스케이스에서 참조할 위험을 방지할 수 있습니다
> 
> 📕 206p 2번째 (9장)
> 

### **🧐 : 패키징의 중요성..**

# 📌 이해가 가지 않았던 내용

## **📚 설계 질서를 파괴하는 메타 프로그래밍**

```jsx
class Level {
	final int value;
	...
}

Level level = Level.initialize();
System.out.println("Level : " + level.value);

Field field = Level.class.getDeclaredField("value");
field.setAccessible(true);
field.setInt(level, 999);

System.out.println("Level : " + level.value);

// Level : 1
// Level : 999
```

> 자바에서 메타 프로그래밍을 활용해 클래스 구조를 읽고 쓸 때는 리플렉션 API를 사용합니다. 이를 통해 일반적인 프로그래밍에서는 접근할 수 없는 부분까지 접근할 수 있습니다
> 
> 📕 200p 2번째 (9장)
> 

### **🧐 : AOP를 제외하고.. 리플렉션을 우리가 직접 사용해야할 케이스가 있을까..?**

# 📌 논의해보고 싶었던 내용

## **📚 YAGNI 원칙**

> YAGIN라는 소프트웨어 원칙이 있습니다. ‘You Aren’t Gonna Need It.’의 약자로, 번역하면 ‘지금 필요 없는 기능을 만들지 말라!’입니다. 실제로 지금 당장 필요한 것들만 만들라는 방침입니다. 
> …
> 소프트웨어에 대한 요구는 매일매일 변합니다. 사양으로 확정되지 않고 명확하게 언어화되지 않은 요구를 미리 예측하고 구현해도, 이러한 예측은 대부분 맞지 않습니다.
> 
> 📕 188p 4번째 (9장)
> 

### **🧐 : 확장성을 고려하라는 이야기가 있는데.. 그 내용과 상충되는 뜻이 아닐까..?**

## **📚 예외를 catch하고서 무시하는 코드**

```jsx
try {
	reservations.add(product);
} catch (Exception e) {
	// nothing..
}
```

> 코드에서 try-catch로 예외를 catch해놓고도, 별다른 처리를 하고 있지 않습니다. 이처럼 예외를 무시하는 코드는 굉장히 사악한 로직입니다
> 
> 📕 198p 3번째 (9장)
> 

### **🧐 : catch 구문에 로직이 아무것도 없는것도 문제지만, 그보다 Exception이라는 추상 exception을 catch하여 각 예외사항에 대해서 적절히 처리하지 못한 것도 문제라고 생각한다**
