# all, any, count, find: 컬렉션에 술어 적용
컬렉션에 대해 자주 수행하는 연산으로 컬렉션의 모든 원소가 어떤 조건을 만족하는지 판단하는 연산이 있는데, 코틀린에서는 all과 any가 이런 연산이다. count 함수는 조건을 만족하는 원소의 개수를 반환하며, find 함수는 조건을 만족하는 첫 번째 원소를 반환한다. 이런 함수를 이용하여 나이가 27살 이하인지 판단하는 술어 함수를 만들어보자.

<code>
<pre>
val canBeInClub27 = {p: Person -> p.age <=27}
</code>
</pre>

모든 원소가 저 조건식에 만족하는지 궁금하다면 all 함수를 써서 전체를 검증한다.

<code>
<pre>
val people = listOf(Person("Alice", 27), Person("Bob", 31))
println(people.all(canBeInClub27))

false
</code>
</pre>

또는 하나라도 있는지 궁금하다면 any 함수를 사용한다.

<code>
<pre>
println(people.any(canBeInClub27))

true
</code>
</pre>

위와 같은 조건처럼 !all을 수행한 결과는 any와 같고 !any를 수행한 결과는 all과 같다. 헷갈리니까 앞에 !는 붙이지 말자.

<code>
<pre>
val list = listOf(1, 2, 3)
println(!list.all{ it == 3})
>>>true
println(list.any{ it != 3 })
>>>true
</code>
</pre>
첫 번째 식은 list의 모든 원소가 3인 것은 아니다 라는 뜻이고, 원소중 하나는 3이 아니라는 말과 같다.   
두 번째 식은 any를 사용하여 검사하고, 술어를 만족하는 원소의 갯수를 구하려면 count를 사용한다.
<code>
<pre>
val people = listOf(Person("Alice", 27), Person("Bob", 31))
println(people.count(canBeInClub27))
1
</code>
</pre>

size로도 구할 수 있지만, size를 사용하게 되면 조건을 만족하는 모든 원소가 들어가는 중간 컬렉션이 생기고, count는 조건을 만족하는 원소의 개수만을 추적하지 조건을 만족하는 원소를 따로 저장해두지 않는다. 따라서 조금 더 효율적이다.

술어를 만족하는 원소 하나만을 찾고 싶다면 find 함수를 사용한다.

<code>
<pre>
val people = listOf(Person("Alice", 27), Person("Bob", 31))
println(people.find(canBeInClub27))
Person(name=Alice, age=27)
</code>
</pre>

# groupBy: 리스트를 여러 그룹으로 이뤄진 맵으로 변경
컬렉션의 원소를 그룹핑해서 여러개로 나누고 싶다고 생각해보자. 예를 들어 사람을 나이에 따라 분류하려면 특성을 파라미터로 전달하여 컬렉션을 자동으로 구분해주는 함수가 있으면 편리하다. 그게 바로 groupBy 함수가 그런 역할이다.
<code>
<pre>
val people = listOf(Person("Alice", 31),
  ... Person("Bob", 29), Person("Carol", 31))
println(people.groupBy{ it.age })
</code>
</pre>
이 연산의 결과는 컬렉션의 원소를 구분하는 특성이 키가되고(여기서는 age) 키 값에 따른 각 그룹이 값인 맵이 만들어진다.
{29=[Person(name=Bob, age=29)],
31=[Person(name=Alice, age=31), Person(name=carol, age=31)]}
