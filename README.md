# 🚀 모던 C++11 주요 기능 완벽 가이드

## 1. 타입추론

### 1.1 auto 키워드
#### 1.1.1 auto 키워드
변수 선언을 할때 타입에 auto를 사용하면 컴파일 타임에 그 변수의 타입을 자동으로 추론하여 쓰인다 ex) auto x = 20; // int타입으로 추론된다
이렇게 간단한 변수를 사용할때 쓰이기보단 std::map<int, std::vector<std::string>>::iterator it = myMap.begin(); 같이 복잡한 타입을 auto it = myMap.begin();이렇게 표현 가능하다.
템플릿 코드에서도 타입을 유연하게 처리가 가능하다 
template <typename T, typename U>
auto add(T a, U b) {
    return a + b;  // 컴파일러가 반환 타입을 자동으로 추론
}
#### 1.1.2 for문에서의 auto
범위기반 for문에서 자주 사용한다
std::vector<int> vec = {1, 2, 3};
for (auto& element : vec) {
    // element는 int& 타입으로 추론됨
}
#### 1.1.3 auto 장단점
장점: 가독성 향상, 유지보수 강화, 템플릿 및 제네릭코드에서 유용하게 쓰인다
단점: 의도하지않은 타입으로 추론이 될수있다, 코드만 보고 타입의 정보가 무엇인지 명확하지않다, 디버깅 어렵다.

주의점: 참조와 const 한정자가 제거되어 값의 복사가 일어난다.

### 1.2 decltype 키워드

#### 1.2.1 decltype 키워드
decltype 는 인수로 지정한 표현식의 타입을 알아낼수 있다. ex) double a = 10.0; decltype(x) y = 11.0;
auto와 달리 참조나 const값을 유지한다.
#### 1.2.2 템플릿에서의 decltype
template <typename T, typename U>
auto add(T a, U b) -> decltype(a + b) {
    return a + b;
}
같이 함수의 반환타입을 표현식에 따라 자동으로 추론이 가능하다.

## 2. 스마트 포인터
스마트 포인터는 memory헤더에 포함되어있다
### 1.1 unique_ptr
#### 1.1.1 unique_ptr 특징
unique_ptr은 단독 소유를 보장하는 스마트 포인터로 한 시점에 하나의 객체만 가리킬수있다
소유권을 다른 unique_ptr로 이전할수있지만 복사는 불가능하다.
std::unique_ptr<int> ptr1 = std::make_unique<int>(10);  // 새로운 int(10) 생성 및 소유
std::unique_ptr<int> ptr2 = std::move(ptr1);            // ptr1에서 ptr2로 소유권 이전
// ptr1은 더 이상 유효하지 않음
**std::make_unique는 c++14에 추가된 함수다**
#### 1.1.2 unique_ptr 생성자
1. 기본생성자 std::unique_ptr<int> ptr; // ptr은 nullptr을 가르킨다
2. 원시 포인터를 생성자에 직접 전달 int* x = new int(10); std::unique_ptr<int> ptr(x);
** 원시포인터를 직접 전당하면 다른코드에서 delete x;를 호출할 가능성이 있어 포인터가 이중삭제될 위험성이 있어 쓰지않는게 좋다**
3. 소유할 객체를 받아들이는 생성자 std::unique_ptr<int> ptr(new int(10));  // int(10)을 가리키는 unique_ptr 생성
4. 이동 생성자 std::unique_ptr<int> ptr2 = std::move(ptr1);  // ptr1에서 ptr2로 소유권 이동 이때 ptr1은 유니크포인터여야한다
5. 커스텀 삭제자를 받는 생성자
커스텀 삭제자를 사용 하는 이유는 파일이나 소켓 데이터베이스등 의 자원은 단순히 delete로 메모리 헤제가 불가능하므로 커스텀 삭제자를 이용하여야 한다
예를 들어 fopen으로 파일포인터를 유니크 포인터로 지정했을경우 delte가 아닌 fclose함수로 파일을 닫아주어야한다
