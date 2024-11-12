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

