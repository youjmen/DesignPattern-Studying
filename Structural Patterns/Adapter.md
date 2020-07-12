# Adapter

> 의도 : 오직 한 개의 클래스 인스턴스만을 갖도록 보장하고, 이에 대한 전역적인 접근점을 제공합니다. - GoF의 디자인 패턴

서로 일치하지 않는 인터페이스를 갖는 클래스들을 함께 동작시킴.

A 클래스를 구현할 때 다른 클래스를 재사용하려 한다. 그런데 A 클래스는 다른걸 상속중이다. 그래서, A 클래스의 상위 클래스와 다른 클래스가 호환이 안돼서, 
둘을 호환시키기 위한 것이 어댑터다.

어댑터 패턴에는 클래스 어댑터, 객체 어댑터가 있다.

클래스 어댑터는 재사용하려는 다른 클래스와 상위 클래스를 둘 다 상속하는 다중 상속을 이용하여, 한 클래스를 다른 클래스에 호환되게 변형시키는 것이다.

객체 어댑터는, 상위 클래스의 인터페이스를 변형시켜, 재사용되는 클래스의 인터페이스를 동작하게 한다.



```kt
class Baker {
    fun bake(){
        println("빵 구워야징")
    }
}

interface Chef {
    fun cook()
}

class Cook : Chef {
    override fun cook(){
        println("요리중")
    }
}

class CookAdapter(val baker: Baker) : Chef{
    override fun cook(){
        baker.bake()
    }
}

fun main(){
    val baker = CookAdapter(Baker())
    val cook = Cook()
 
    val chefList = listOf<Chef>(cook,baker)

    chefList.forEach{
        chefs-> chefs.cook()
    }
   


}
```
cheflist에서는 똑같은 cook 메서드를 사용하여 요리사들에게 요리를 시키려 한다. 하지만 Baker 클래스는 bake 메서드를 사용하여 cook으로 실행을 하면 cook 메서드가 없어 실행을 할 수 없다. 그래서 CookAdapter로 Chef의 메서드를 오버라이드 하여 bake를 하게 하면 bake 메서드의 이름을 바꾸지 않고도 동일하게 cook 메서드로 모든 일을 처리할 수 있다.