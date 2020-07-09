# Singleton

> 의도 : 오직 한 개의 클래스 인스턴스만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공합니다. - GoF의 디자인 패턴



```kt
object SingletonEx {
    private var count = 0

    fun testSingleton(){
        count +=5
        println("singleton Success"+ count)
    }
}

class SingletonEx2 private constructor() {

    private var count = 0
    companion object {
        private var a : SingletonEx2?=null
        fun getInstance() : SingletonEx2 {
            if(a ==null)
                a = SingletonEx2()

            return a!!
        }
    }
    fun testSingleton(){
        count+=5
        println("singleton Success"+count)
    }


}
```
코틀린에서는 object라는 키워드가 singleton을 자동으로 구현해줍니다.

 singleton을 구현하는 방법은 여러가지지만 저는 생성자를 private로 하여 객체를 생성하지 못하게 한 후, 동일한 객체를 반환하게 하는 함수를 사용하게 하였습니다.


```kt
fun main(){
    var test1 =SingletonEx.testSingleton()
    var test2 =SingletonEx.testSingleton()


    var test3 = SingletonEx2.getInstance().testSingleton()

    var test4 = SingletonEx2.getInstance().testSingleton()
}
// 실행 결과
singleton Success5
singleton Success10
singleton Success5
singleton Success10

```
하나의 객체만 존재해야 하는 클래스가 필요할 때 Singleton을 사용한다고 하는데, 저는 유저 정보같은 계속 변하지만, 유일해야 하는 정보를 저장할 때 보통 Singleton 패턴을 사용합니다.
