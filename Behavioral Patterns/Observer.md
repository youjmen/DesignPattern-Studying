# Observer  
> 의도 : 객체 사이에 일 대 다의 의존 관계를 정의해 두어, 어떤 객체의 상태가 변할 때 그 객체에 의존성을 가진 다른 객체들이 그 변화를 통지받고 자동으로 갱신될 수 있게 만듭니다. -Gof의 디자인 패턴

보통 Subject, Observer, ConcreteSubject, ConcreteObserver로 이루어져 있다.

![](https://gmlwjd9405.github.io/images/design-pattern-observer/observer-pattern.png)

Observer는 Subject를 observe하여 Subject의 변경이 일어날 때 마다 Subject가 Observer에 Notify를 해주면 Observer가 Update되는 구조다.

Observer 패턴을 사용하는 이유는 데이터를 가지고 있는 객체에 의존성을 가진 객체가 여러 개 일때, 그 객체의 변경으로 인해 얼마나 많은 객체가 변경되야 할지 몰라도 되거나, 그 데이터에 관심이 있는 객체들이 누구인지 몰라도 통지할 수 있을때 사용한다.

주체가 정보를 줄 때 정보를 받는 구독자와의 의존성이 없어진다. 주체는 정보를 갱신하기만 하고 정보를 직접 주지 않는다, 그를 관찰하는 Observer들이 갱신된 정보를 가져가기 때문에 주체와의 의존성이 없어진다.

## *예제*
```kt
interface Observer {
    var score : Int
    fun update()
    fun subscribe(subject: Subject)
}
class ConcreteObserver : Observer{

    lateinit var subject: Subject
    override var score = 0


    override fun update() {
        score= subject.score


    }

    override fun subscribe(subject: Subject) {
        this.subject=subject
        subject.attach(this)
    }


}
class ConcreteObserver2 : Observer{
    lateinit var subject: Subject
    override var score=0

    override fun update() {
        score= subject.score+5


    }

    override fun subscribe(subject: Subject) {
        this.subject=subject
        subject.attach(this)
    }


}
```
Observer들은 Subject를 구독한다. Subject를 구독하면 Subject의 Observer 목록에 Attach가 된다.
```kt
abstract class Subject{


    private val observers = arrayListOf<Observer>()
    open var score =0

    open fun addScore(score : Int) {}

    fun attach(o: Observer){
        observers.add(o)
    }
    fun detach(o: Observer){
        observers.remove(o)
    }
    fun notifyToObservers(){
       observers.forEach{
           o->o.update()
       }
    }

}
class ConcreteSubject : Subject() {
    override var score =0

    override fun addScore(score : Int){
        this.score+=score
        notifyToObservers()
    }


}
```
addScore로 Subject에 변경이 생기면, Observer들에게 Notify되고, Attach된 Observer들이 Update된다.
```kt
fun main(){
    val observer = ConcreteObserver()
    val observer2 = ConcreteObserver2()

    val subject = ConcreteSubject()

    observer.subscribe(subject)
    observer2.subscribe(subject)
    
    subject.addScore(5)

    println(observer.score)
    println(observer2.score)



}

```
