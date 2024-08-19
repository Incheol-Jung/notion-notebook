# 📌 인상 깊었던 내용

## **📚 왜 static 메서드를 사용할까?**

> static 메서드를 사용하는 이유는 객체 지향 언어를 사용할 때, C 언어 같은 절차 지향 언어의 접근 방법을 사용하려 하기 때문입니다. 절차 지향 언어세더는 데이터와 로직이 따로 존재하도록 설계합니다. 이러한 접근 방법을 객체 지향 언어에 적용하여 설계하면, 데이터와 로직을 별도의 클래스에 배치하게 됩니다. 그래서 클래스의 인스턴스를 생성하지 않고도 사용할 수 있는 static 메서드를 활용하는 것입니다

📕 63p 5번째 (5장)
> 

### **🧐 : static 메서드를 사용하는 취지에 대해서는 깊게 생각해보지 않았고 관성적으로 비즈니스 로직이 아닌 시스템 로직을 재사용하기 위함으로 알고 있었는데 좀 더 올바른 표현인것 같아 남겨둠..**

# 📌 이해가 가지 않았던 내용

## **📚 인스턴스 메서드**

```jsx
class OrderManager {
	static int add(int moneyAmount1, int moneyAmount2) {
		return moneyAmount1 + moneyAmount2;
	}
}

class PaymentManager {
	private int discountRate; // 할인율
	
	int add(int moneyAmount1, int moneyAmount2) {
		return moneyAmount1 + moneyAmount2;
	}
}
```

> 그런데 인스턴스 변수 discountRate를 전혀 사용하지 않습니다. 매개변수로 받은 값만 활용해서 계산하므로, OrderManager의 static 메서드였던 add와 차이가 없습니다.

📕 62p 10번째 (5장)
> 

### **🧐 : 멤버변수를 안쓸수도 있는데.. 굳이 안쓴다고 불필요한 메서드일까..? 응집도 측면에서 관련 로직들이 모여있다면 그것만으로 의미있지 않을까..?**

# 📌 논의해보고 싶었던 내용
