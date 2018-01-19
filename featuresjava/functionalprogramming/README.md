# Basic definitions
- In imperative programming we develop programs where clases are the most importan estructures.
- In functional programming we develop programs where functions are the most important estructures.
- In OOP we have:
	- Classes.
	- Encapsulation (method visibility, attributes, ...).
	- Composition.
	- Inheritance.
	- Polymorphism (ability of functions, arrays, ... of accepting objects of a certain class and its children).
- In functional programming we have:
	- Pure functions: functions that only take certains parameters and return a velue.
	- Higher order functions: functions that accept or return functions as parameters.
	- Immutability: objects that functions accept can't be modified, they are immutables.
	- Recursive functions.
	- Lazy evaluation.
- Why is functional programming important?
	- Because it focuses on purely mathematical concepts and because of its immutability there isn't any collateral damage.
	- Concurrency is more important than ever: now all devices are multi-core so atomic execution (with no collateral damage) can take advantage of all avaliable resources.
	- Scala is the best example.
- What are funcional interfaces?
	- Sometomes we need configure the behaviour of a method in runtime. Everyone has ever used Comparator to indicate how a comparison must work, or Runnable to define how a code must execute.
	- They are interfaces that only exist to wrap a simple method.
- What are anonymous classes??
	- It is a class with no name, which is defined in the same code where object of the class is created.

# Lambdas
- Anonymous class creation has a lot of redundant information. It can be observed in the following example:
_&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public void foo(List<Person> users){
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Collection.sort(users, new Comparator<Person>()) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;@Override
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public int compare(Person one, Person two) {
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return one.getLastName().compareTo(two.getLastName()));
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}_
- In the previous method we can see that it is redundant in compare method to define parameters type, we are comparing obviously people. Return is algo redundant because we must return something. Even ";", "{", "}" are redundant.
- Lambda expressions are a way to create anonymous methods in a much simpler way.
- Lambdas can be used wherever you can use a anonymous class (such as the previous Comparator).
- They are basically anonymous functions.
- The previous example would be like that:
_&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public void foo(List<Person> users) {
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;sort(users, (one, two) -> one.getLastName().compareTo(two.getLastName()));
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}_
- Bytecode generated by a lambda expression is not equivalent to the one generated for a anonymous class. For lambdas, compilator uses invoke_dynamic which has, among other things, better performance
- A lambda is not the same that a clousure.
- They are really important because it makes easier to use higher order methods.
- What other thing can we do with methods that accept functions as parameters? Monads.
- A lambda expression can be used to create a instance of a functional interface.
- The type of the functional interface is deduced according to the context and it works both assignment contexts and method invocations (parameters).
- Compilator deduces the type of the lambda expression parameters based on functional interface abstract method definition, so there is no need to write its type.
- To deduce a lambda expression type:
	- Context identification: assignment or method invocation.
	- To identify target type: variable or parameter.
	- To identify functional interface of the target type.
	- To identify the descriptor of the function. 
	- To verify that function descriptor is coherent with lambda expression.
	- There are two special cases:
		- The same lambda expression can suit different function descriptors, I mean, different functional interfaces.
		- Compatibility rule for not return. If a lambda expression is made by a sentence, then is compatible with descriptors of functions which don't jave any return, I mean, void.
- Lambda expressions can use variables or parameters defined like constants (final) of really constants (a variable or parameter really constant is the one which is assigned just once, even if it hasn't been defined using final).
- In contrast to anonymous classes, in lambdas "this" refers to the class instance where lambda expression has been written.
_&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;class SessionManager{
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;long before = ...;
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;void expire(File root) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;root.listFiles(File p -> checkExpiry(p.lastModified(), this.before));
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;boolean checkExpiry(long time, long expiry) {...}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}_
- In the previous example, if before had been modified in any other way of the class, a compilation error would be launched by previous definition of "really constant". 

# Lambdas structure
- Parameters are separated of the funtion body by "->"-
- Parameters are separated by "," and between "(" and ")" (optional when there is only one parameter).
- Parameter type can be ommited when there is enough information for the compilator to deduce it.
- If the function body is comlex (it has more than one line) it must be between "{" and "}", to have ";" and to have a "return".
- If the function body is simple you can ommit "{", "}", ";" and "return".
- If the lambda expression only calls to a unique method we can use a reference to a method. The reference methods are the invocation of methods by its name. Lambda expressions compacter and easier to read:
	- To refer to a static class method: Class:staticMethodName
	- To refer to an object method: Object::instanceMethodName
	- To refer to an arbitrary object method of a concrete type: Type::methodName
	- To refer to a constructor: Class:new
- Examples
	- General:
		_&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public class Person {
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;String name;
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;LocalDate birthday;
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//getters ... setters ...
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public static int compareByAge(Person a, Person b) {
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return a.birthday.compareTo(b.birthday);
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}_
	- Static class method:
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Person[] personArray = new Person[1000];
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Order an array using lambdas
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arrays.sort(personArray, (a, b) -> Person.compareByAge(a, b));
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Order an array using reference methods
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arrays.sort(personArray, Person::compareByAge);_
	- Instance method:
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Person[] personArray = new Person[1000];
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;...
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ComparisonProvider comparisonProvider = new ComparisonProvider();
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Order an array using lambdas
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arrays.sort(personArray, (a, b) -> comparisonProvider.compareByAge(a, b));
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Order an array using reference methods
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arrays.sort(personArray, comparisonProvider::compareByAge);_
	- Type method:
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_String[] strings = {"asdf", "asdfas", ...};
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Order an array using lambdas
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arrays.sort(strings, (a, b) -> a.compareToIgnoreCase(b));
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Order an array using reference methods
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Arrays.sort(strings, String::compareToIgnoreCase);_
	- Constructor (with no parameters):
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_//Create a person using lambdas
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Supplier<Persona> s1 = () -> new Persona();
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Persona p1 = s1.get();
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Create a persons using reference methods
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Supplier<Persona> s1 = Persona::new;
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Persona p1 = s1.get();_
	- Constructor (with parameters):
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_//Create a person using lambdas
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Function<String, Persona> f1 = (w) -> new Persona(w);
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Persona p1 = f1.apply("Manolo");
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;//Create a persons using reference methods
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Function<String, Persona> f1 = Persona::new;
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Persona p1 = f1.apply("Manolo");_

# Pedicates
- A predicate is a functional interface which defines a condition that and object must accomplish.
- The predicate has an unique method named test which accepts the object and chech the condition.
- The execution code is always the same. You only need to implement the predicate (see example).
- The problem is that the more conditions, the more classes to mantain.
- It is extremely easy to implement strategy pattern with predicates.
- There is a lot of duplicate code when using predicates.

# Monads
- How much do you hate NullPointerException?
- Monads are parametrized types.
- They can wrap a value in a computational context.
- They have a method that from a <T> value can create a Monad<T>.
- They have binding method that from a function from U to T can transform Monat<T> into Monad<U>.
- There are other methods like bind, that can change the value but not the computational context, so they are safe.

# Optional
- An Optional is a Monad.
- It expresses the possible absence of a data type (there is no need of null).
- It is the Null Object patter for the T type.
- A Optional is empty when there is no value inside (it can be null).
- Its methods allow to operate with the value in a safe way. If you try an operation with an empty Optional, it will return an empty value without launching an exception.
- Traditional example:
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(computer != null) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SoundCard soundCard = computer.getSoundCard();
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(soundCard != null) {
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;USB usb = soundCard.getUSB();
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if(usb != null) {
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;name = usb.getVersion();
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
- Optional example:
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;String name = Optional.of(computer)	//Optional<Computer>
					&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.flatMap(Computer::getSoundCard)	//Optional<SoundCard>
					&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.flatMap(SoundCard::getUSB)	//Optional<USB>
					returns a String, we use map.
					&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.orElse("UNKNOW");	//String

# Map vs FlatMap
- FlatMap always returns an Object wrapped in an Optional.
- Map returns the object.
- FlatMap also makes the object simplier, what ensures that instead of returning Optional<Optionanl<T>>, it returns Optional<T>.
- Map example:
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public<U> Optional<U> map(Function<? super T, ? extends U> mapper) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objects.requireNonNull(mapper);
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if (!isPresent())
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return empty();
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;else {
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return Optional.ofNullable(mapper.apply(value));
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
- FlatMap example:
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public<U> Optional<U> flatMap(Function<? super T, Optional<U>> mapper) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Objects.requireNonNull(mapper);
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;if (!isPresent())
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return empty();
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;else {
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return Objects.requireNonNull(mapper.apply(value));
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
- As we can see, flatMap accepts an Optional whereas map accepts the object.

# Streams
- A stream is NOT a collection of elements (its content can not be saved in any place).
- A stream is NOT a sequence (the order can be or not important).
- A stream is an abstraction that represents zero or more values (a value means an immutable object, which is extremely important to concurrency).
- Immutability is essential in functional programming.
- A stream is like a strengthened iterator.
- Streams make pretty pipeline concept. A pipeline is a sequence of zero or more intermediate operations and a final operation with a stream.
- Pipelines are lazy: job is made just when a final operation is found. If there isn't a final operation, the stream do nothing.
- It exists some primitive streams: DoubleStream, IntStream and LongStream (it uses prevent from using of Autoboxing and Unboxing).
- Streams use Filter pattern --> Map (to map to other type) --> Reduce (return an unique value).
- Stream example:
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public long calculateDebt(List<User> users) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return users.stream()
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.filter(u -> u.getMemberShip() == PREMIUM)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.filter(u -> u.getDebt() > 0)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.sorted((one, two) -> (int)(one.getDebt() = two.getDebt()))
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.limit(50)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.map(User::getDebt)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.reduce(=L, (a, b) -> a + b);
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
- Another example:
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public long calculateDebt(List<User> users) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return users.stream()
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.filter(u -> u.getMemberShip() == PREMIUM)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.filter(u -> u.getDebt() > 0)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.sorted(Comparator.comparingLong(User::getDebt))
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.limit(50)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.mapToLong(User::getDebt)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.sum();
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
- Parallel operation:
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;public long calculateDebt(List<User> users) {
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;return users.parallelStream()
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.filter(u -> u.getMemberShip() == PREMIUM)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.filter(u -> u.getDebt() > 0)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.sorted(Comparator.comparingLong(User::getDebt))
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.limit(50)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.mapToLong(User::getDebt)
			&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;.sum();
	&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
- Anyway, to work concurrently:
	- To avoid mutations.
	- To avoid depencendy operations.
	- To avoid accumulated (a dependency type).
	- The best way to know if a parallel operation is better than a sequencial operation, the best is always to measure times.

# Stream methods
- Basic méthods:
	- Stream.of(T): Stream<T> --> Returns an ordered stream of the elements given as parameters.
	- Stream.empty(): Stream --> Returns an emtpy stream.
	- Arrays.stream(T[]): Stream<T> --> Returns an stream of the array given as a parameter. Primitive version returns DoubleStream, IntStrea, LongStream.
	- Collection<E>.stream(): Stream<E> --> Returns a stream of the elements of the collection. Parallel version: Collection<E>.parallelStream(): Stream<E>
	- Stream.iterate(T, UnaryOperation<T>): Stream<T> --> Returns a infinite stream, ordered and sequencial, from the initial value T and from applying the UnaryOperator function to the initial value to get the other elements. To limit its size, you can use the method +limit(long): Stream
	- Stream.generate(Supplier<T>): Stream<T> --> returns and infinite stream, sequential but not ordered, from and lambda expression which Supplier provides.

- Filter methods:
	- +filter(Predicate<T>): Stream<T> --> returns a stream which only contains the elements which fulfil with the Predicte.
	- +distinct(): Stream<T> --> returns a stream with no duplicate elements. It depends on the implementation of +equals(Object):boolean.
	- +limit(long): Stream<T> --> returns a stream whose size is equal or smaller to the parameter.
	- +skip(long): Stream<T> --> returns a stream which removes the first N elements (parameter). If the size is smaller than the parameter, it returns a empty stream.

- Mapping functions:
	- +map(Function<T, R>): Stream<R> --> returns a stream which contains the result of aplying the parameter function to all elements of the stream. It turns T elements into R elements.
	- +mapToDouble(ToDoubleFunction<T>): DoubleStream (primitive version) 
	- +mapToInt(ToIntFunction<T>): IntStream (primitive version)
	- +mapToLong(ToLongFunction<T>): LongStream (primitive version)
	- +flatMap(Function<T, Stream<R>>): Stream<R> --> it allows to turn each element into a stream and to concatenate all of them in just a final stream.
	- +flatMapToDouble(Function<T, DoubleStream>): DoubleStream (primitive version)
	- +flatMapToInt(Function<T, IntStream>): IntStream (primitive version)
	- +flatMapToLong(Function<T, LongStream>): LongStream (primitive version)

-  Métodos de reducción:
	- +count(): long --> returns the number of the elements of the stream.
	- +max(Comparator<T>): Optional<T> --> returns the maximum element of the stream based on the parameter comparator.
	- +min(Comparator<T>): Optional<T> --> returns the minimum element of the stream based on the parameter comparator.
	- +allMatch(Predicate<T>): boolean --> it verifies if all the elements of the stream suit with the parameter Predicate. If not, it stops the verification and returns false.
	- +anyMatch(Predicate<T>): boolean --> it verifies if a element of the stream suit with the parameter Predicate. If so (only one), it stops the verification and returns true.
	- +noneMatch(Predicate<T>): boolean --> the opposite of allMatch.
	- +findAny(): Optional<T> --> returns some element of the stream. It returns an optional.
	- +findFirst(): Optional<T> --> returns the first element of the stream. It returns an optional.
	- +reduce(BinaryOperator<T>): Optional<T> --> it reduces the stream using an associative function. It returns an optional..
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Optional<Integer> opt = numeros.stream().filter(x -> x % 2 == 0).reduce(Integer::max);_
	- +reduce(t, BinaryOperator<T>): T --> it execute the reduction of the stream using an initial value and d associative function.
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_Integer suma = numeros.stream().reduce(0, (x, y) -> x + y);ç_
	- +collect(?): ? --> final operation which allows to collect the elements of a Stream. Collectors is a class which provides static methods which return the most used collectors.
		- Collectors can be of three types:
			- Reduction and summary: reduce the stream and allow to get aggregate values.
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_int suma = numeros.stream().collect(summingInt(x -> x.intValue()));_
			- Grouping: group elements in a Map using a clasification function.
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_IntSummaryStatistics res = numeros.stream().collect(summarizintInt(Integer::intValue));_
			- Allocation: grouping where the clasification function is a Precicate. It groups the elements in a Map of 2 keys: true amd false.
				&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_String csv = numeros.stream().map(Object::toString).collect(joinning(", "));_

- Debug methos: an intermediate method which allows to debug.
	- +peek(Consumer<T>): Stream<T>
		&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;_List<String> unicas = palabras.stream().flatMap(w -> Stream.of(w.split(" "))).peek(s -> s). map(String::lowerCase).distinct().collect(Collectors.toList());_