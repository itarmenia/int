The stream library has specialized types IntStream, LongStream, and DoubleStream

As with object streams, you can also use the static generate and iterate methods. In addition, IntStream and LongStream 
have static methods range and rangeClosed that generate integer ranges with step size one:

When you have a stream of objects, you can transform it to a primitive type stream with the mapToInt, mapToLong, or mapToDouble methods.

```java
Stream<Integer> integers = IntStream.range(0, 100).boxed();

```

By default, parallel streams use the global fork-join pool returned by ForkJoinPool.commonPool. That is fine if your operations don’t block and you don’t share the pool with other tasks. There is a trick to substitute a different pool. Place your operations inside the submit method of a custom pool:
ForkJoinPool customPool = . . .;

result = customPool.submit(() ->
   stream.parallel().map(. . .).collect(. . .)).get();”



---


```java

words.stream().sorted(Comparator.comparing(String::length).reversed());

TreeSet<String> result = stream.collect(Collectors.toCollection(TreeSet::new));

```

------------------------------------------------------------------------------------------------------------------------

```java
IntSummaryStatistics summary = stream.collect(Collectors.summarizingInt(String::length));
double averageWordLength = summary.getAverage();
double maxWordLength = summary.getMax();”
```

“Since it is not possible to create a generic array at runtime, the expression stream.toArray() returns an Object[] 
array. If you want an array of the correct type, pass in the array constructor:

String[] result = stream.toArray(String[]::new);
   // stream.toArray() has type Object[]”

------------------------------------------------------------------------------------------------------------------------
## toMap   

In the common case when the values should be the actual elements, use Function.identity() for the second function

```java
Map<Integer, Person> idToPerson = people.collect(
   Collectors.toMap(Person::id, Function.identity()));
```

if there is more than one element with the same key

```java
Map<String, String> languageNames = locales.collect(
  Collectors.toMap(
     Locale::getDisplayLanguage,
     loc -> loc.getDisplayLanguage(loc),
     (existingValue, newValue) -> existingValue));
```

For each of the toMap methods, there is an equivalent toConcurrentMap method

------------------------------------------------------------------------------------------------------------------------
### groupingBy

groupingByConcurrent

```java
groupingBy(Locale::getCountry, toSet())


Map<Character, Integer> stringCountsByStartingLetter = strings.collect(
   groupingBy(s -> s.charAt(0),
      collectingAndThen(toSet(), Set::size)));



Map<String, List<Locale>> countryToLocales = locales.collect(
   Collectors.groupingBy(Locale::getCountry));

Map<Character, Set<Integer>> stringLengthsByStartingLetter = strings.collect(
   groupingBy(s -> s.charAt(0),
      mapping(String::length, toSet())));

Map<String, Set<City>> largeCitiesByState
    = cities.collect(
       groupingBy(City::state,
           filtering(c -> c.population() > 500000,
              toSet()))); 

      


```


```java
record Pair<S, T>(S first, T second) {}

Pair<List<String>, Double> result = cities
    .filter(c -> c.state().equals("NV"))
    .collect(
        teeing(
            mapping(City::name, toList()), // First downstream collector
            averagingDouble(City::population), // Second downstream collector
            (list, avg) -> new Result(list,  avg))); // Combining function”

```

When the classifier function is a predicate function (that is, a function returning a boolean value), the stream elements 
are partitioned into two lists: those where the function returns true and the complement. In this case, it is more 
efficient to use partitioningBy instead of groupingBy. For example, here we split all locales into those that use English and all others:

```java
Map<Boolean, List<Locale>> englishAndOtherLocales = locales.collect(
   Collectors.partitioningBy(l -> l.getLanguage().equals("en")));
```


## Reduction Operations

```java
Integer sum = values.stream().reduce(0, (x, y) -> x + y);
Optional<Integer> sum = values.stream().reduce((x, y) -> x + y);


int result = words.reduce(0,
   (total, word) -> total + word.length(),
   (total1, total2) -> total1 + total2);


```

1) A supplier to construct new instances of the target object
2) An accumulator that adds an element to the target
3) A combiner that merges two target objects into one

BitSet result = stream.collect(BitSet::new, BitSet::set, BitSet::or);







