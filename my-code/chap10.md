# 📌 인상 깊었던 내용

## **📚 관심사 분리**

![KakaoTalk_Photo_2024-09-09-22-53-38](https://github.com/user-attachments/assets/5bb4539f-0866-4833-bed2-73552c01d4ab)


> 이렇게 거대해진 상품 클래스에 사양 변경이 발생하면 어떻게 될까요? 변경된 부분의 영향을 받아 동작에 버그가 생기지는 않는지, 상품 클래스와 관련된 클래스를 모두 확인해 봐야 합니다. 그런데 영향 범위가 너무 넓어 개발 생산성이 저하될 것입니다
…
그림을 자세히 살펴보면 상품이 예약, 주문, 발송 등 다양한 관심사에 관한 로직을 갖고 있습니다. 상품 클래스가 다양한 관심사와 결합되어, 온갖것과 관련된 로직을 품게 되었습니다.

📕 212p 3번째 (10장)
> 

### **🧐 : 관심사를 분리하는건 중요하지만.. 많은 애플리케이션이 이와 같은 구조로 되어있지 않을까..? 다른 분들은 이런 상황을 겪은적 있는지..? 또는 개선한 경험이 있는지..?**

## **📚 이름 설계하기 - 목적 중심 이름 설계**

> 저는 클래스와 메서드에 이름을 붙이는 것을 ‘명명’이라고 부르지 않고, 이름설계라고 부릅니다. 여기에서 설계는 ‘어떤 문제를 해결하기 위한 구조를 생각하거나 만들어 내는 것’을 의미합니다. 
프로그래밍에서 이름은 가독성을 높이는 역할만 하는 게 아닙니다. 
…
중요한 포인트를 정리하겠습니다.
- 최대한 구체적이고, 의미 범위가 좁고, 특화된 이름 선택하기
- 존재가 아니라 목적을 기반으로 하는 이름 생각하기
- 어떤 관심사가 있는지 분석하기
- 소리 내어 이야기해 보기
- 이용 약관 읽어 보기
- 다른 이름으로 대체할 수 없는지 검토하기
- 결합이 느슨하고 응집도가 높은 구조인지 검토하기

📕 216p 8번째 (10장)
> 

### **🧐 : 명명이 아닌 이름 설계 관점으로 접근하는건 중요한 포인트 같다..**

## **📚 부적절한 위치에 있는 boolean 메서드**

```jsx
// as-is
class Common {
	// 멤버가 혼란 상태라면 true를 리턴
	static boolean isMemeInConfusion(Member member) {
		return member.states.contains(StateType.confused);
	}
}

// to-be
class Member {
	private final States states;
	
	boolean isInConfusion() {
		return states.contains(StateType.confused);
	}
}
```

> 멤버의 상태는 멤버와 관련된 관심사입니다. 따라서 Memeber 클래스에 정의하는 것이 자연스럽습니다. 

📕 248p 2번째 (10장)
> 

### **🧐 : 관련된 관심사로 이동하는것도 중요하지만 메서드 체이닝 구조면 의심하는 습관도 중요할것 같다**

# 📌 이해가 가지 않았던 내용

## **📚 로직 구조를 나타내는 이름**

```jsx
// AS-IS
class Magic {
	boolean isMemeberHpMoreThanZeroAndIsMemberCanActAndIsMemberMpMoreThanMagicCostMp(Member member) {
		 if (0 < member.hitPoint) {
			 if(member.canAct()) {
				 if(costMagicPoint <= member.magicPoint) {
					 return true;
				 }
			 }
		 }
		 
		 return false;
	 }
 }
 
 //TO-BE
 class Magic {
	boolean canEnchant(Member member) {
		 if (0 >= member.hitPoint) return false;
		 if (!member.canAct()) return false;
		 if(costMagicPoint > member.magicPoint) return false;
		 
		 return true;
	 }
 }
```

> 이는 게임에서 멤버가 마법을 사용할 수 있는 상태인지 판정하는 로직입니다. 
- 히트포인트가 0보다 큰가(생존해 있나)?
- 행동 가능한 상태(canAct)인가?
- 매직포인트가 남아 있는가?

그런데 메서드의 이름은 로직 구조를 그대로 드러내고 있습니다. 무엇을 의도하는지 메서드 이름만 보고는 알기 힘듭니다. 구현하려는 대상이 무엇을 하려는지 제대로 이해하고 있지못할 때, 이와 같은 이름을 짓기 쉽습니다.

📕 232p 2번째 (10장)
> 

### **🧐 : 메서드 이름도 개선이 필요하지만… 파라미터에 Memeber 객체를 다 넘길 필요가 있을까..? 또는 저 메서드가 필요할까..? 멤버 객체내에서 해당 조건에 해당하는 메서드를 제공해줘도 되지 않을까..?**

## **📚 구조에 악영향을 미치는 이름**

```jsx
class ProductInfo {
	int id;
	String name;
	int price;
	String productCode;
}
```

> ~Info, ~Data 같은 이름의 클래스는 읽는 사람에게 ‘데이터만 갖는 클래스니까, 로직을 구현하면 안 되는구나’라는 이미지를 심어 줄 수 있습니다. 이러면 응집도가 낮은 구조가 되기 쉽습니다.

📕 235p 6번째 (10장)
> 

### **🧐 : Info, Data라는 후행자는 많이 쓰는 패턴인데… 앞으로 쓰지 말아야 할까..? 로직이 없다고 이해하려나..?**

# 📌 논의해보고 싶었던 내용

## **📚 포괄적이고 의미가 불분명한 이름**

> 개발 초기에는 포괄적인 이름을 붙이는 경우가 많습니다. 이름을 이렇게 포괄적으로 붙이면 어떤 문제가 생길까요?

직원 A : “이번 사양 변경으로 (개발하고 있는 온라인 쇼핑몰에) 예약 기능을 추가해야 합니다. 상품 예약 로직을 구현해야 하는데, 어디에 구현해야 할까요?
지원 B : “상품 클래스가 이미 있잖아요? 상품 클래스에 구현하세요”

📕 215p 1번째 (10장)
> 

### **🧐 : 추상적인 이름도 문제긴 하지만 너무 구체적인 이름도 문제라고 생각되는데..? 적절한 지점은 어디일까..?**

## **📚 놀람 최소화 원칙**

```jsx
class Order {
	private final OrderId id;
	private final Items items;
	private GiftPoint giftPoint;
	
	int itemCount() {
		int count = items.count();
		
		// 주문 상품 수가 10 이상일 때, 기프트 포인트를 100만큼 추가
		if (10 <= count) {
			giftPoint = giftPoint.add(new GiftPoint(100));
		}
		
		return count;
	}
}
```

> 놀랍게도 주문 상품 수를 리턴할 뿐만 아니라, 기프트 포인트까지 추가하고 있습니다. 아마 itemCount 메서드를 사용했던 사람은 이런 동작까지 예측하지 못했을 터라, 실제로 하고 있는 일을 깨달으면 놀랄 것입니다.

📕 233p 4번째 (10장)
> 

### **🧐 : 읽고 쓰기가 공존하는 메서드는 항상 오류 발생가능성을 높이는 위험한 안티패턴이라고 생각한다.. 혹시 당신의 코드엔 이런 코드가 있지는 않은가..? 🔫**
