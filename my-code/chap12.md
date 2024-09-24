# 📌 인상 깊었던 내용

# 📌 이해가 가지 않았던 내용

## **📚 커맨드/쿼리 분리**

```jsx
int gainAndGetPoint() {
	point += 10;
	return point;
}
```

> 위의 메서드는 상태 변경과 추출을 동시에 하고 있습니다. 
상태 변경과 추출을 동시에 하는 메서드는 여러 문제의 원인이 됩니다. 또한 사용자도 쓰기 힘든 메서드가 되어 버립니다. 

📕 265p 1번째 (12장)
> 

### **🧐 : 메서드를 기능에 맞게 분리하는건 동의하는데 결국 상단에 취합하는 메서드는 필요할것 같은데.. 이는 괜찮을까..?**

## **📚 ‘자료형’을 사용해서 리턴 값의 의도 나타내기**

```jsx
class Price {
	// 생략
	int add(final Price other) {
		return amount + other.amount;
	}
}

// 의도한 값 전달
int price = productPrice.add(otherPrice); // 상품 가격 합계
int discountedPrice = calcDiscountedPrice(price); // 할인 금액
int deliveryPrice = calcDeliveryPrice(discountedPrice); // 배송비

// 잘못된 값 전달 실수
// 배송 수수료 DeliveryCharge에는 배송비가 전달되어야 하는데
// 상품 가격 합계가 전달되고 있음.
DeliveryCharge deliveryCharge = new DeliveryCharge(price);
```

> Int 자료형처럼 단순한 기본 자료형으로는 리턴한 값의 의미를 호출하는 쪽에 전달할 수 없습니다. 왜 그럴까요? Product Price는 Price 자료형이며, add 메서드로 int 자료형의 가격을 리턴합니다. 그런데 가격뿐만 아니라 할인 금액과 배송비까지 모두 int 자료형을 사용하고 있습니다. 
금액을 계산할 때는 어떤 가격이 다른 가격 계산에 사용되는 등, 다양한 종류의 가격을 함께 다루는 경우가 많습니다. Int 자료형을 리턴하게 만들면, 어떠값이 어떤 금액을 의미하는지 알기 힘듭니다. 따라서 매개변수를 잘못 전달하는 등의 실수가 발생할 수 있습니다. 
따라서 기본 자료형을 사용하지 말고, 독자적인 자료형을 사용해서 의도를 명확하게 나타내는 것이 좋습니다

📕 269p 1번째 (12장)
> 

### **🧐 : 같은 가격이라는 의미를 각각의 데이터 클래스를 만드는게 맞을까..? Int 보다는 데이터 클래스를 만드는게 좀더 유연하고 확장성 있는 부분은 이해했는데.. 어떻게 생각하는지..?**

# 📌 논의해보고 싶었던 내용

## **📚 null 전달하지 않기**

> null을 활용하는 로직은 NullPointerException이 발생할 수 있으며, null을 확인해야 하므로 로직이 복잡해지는 등 다양한 문제를 일으킵니다. 
매개변수로 null을 전달하지 않게 설계하세요. 차라리 null로 나타내지 않고 Equipment.EMPTY로 표현했던 것처럼 구현하세요.

📕 267p 12번째 (12장)
> 

### **🧐 : 매개변수에 nullable하지 않게만 제공할수 있을까..? 만약 nullable하다면 그 케이스는 어떻게 기준을 잡아야 할까..?**
