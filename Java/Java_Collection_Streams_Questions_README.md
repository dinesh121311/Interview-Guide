# Java Collections and Streams Practice Questions

## Basic Operations

1.  **Remove Duplicates from an ArrayList using Streams**

    **Question:**
    Given an `ArrayList<Integer>`, remove duplicate elements while maintaining the original order.

    **Solution:**
    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class RemoveDuplicates {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(4, 2, 3, 2, 4, 5, 6, 3);
            
            List<Integer> uniqueNumbers = numbers.stream()
                                                 .distinct()
                                                 .collect(Collectors.toList());
    
            System.out.println(uniqueNumbers); // Output: [4, 2, 3, 5, 6]
        }
    }
    ```

    **Explanation:**
    -   `.stream()` creates a stream from the list.
    -   `.distinct()` filters out duplicate elements, maintaining the original order.
    -   `.collect(Collectors.toList())` collects the resulting distinct elements into a new list.

2.  **Count Character Frequency in a String using Streams**

    **Question:**
    Given a string, count the frequency of each character using Streams.

    **Solution:**
    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class CharacterFrequency {
        public static void main(String[] args) {
            String str = "hello world";
            
            Map<Character, Long> frequencyMap = str.chars()
                                                   .mapToObj(c -> (char) c)
                                                   .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    
            System.out.println(frequencyMap);
        }
    }
    ```

    **Explanation:**
    -   `.chars()` creates an `IntStream` of ASCII values for each character in the string.
    -   `.mapToObj(c -> (char) c)` converts each ASCII value back to a `Character` object, creating a `Stream<Character>`.
    -   `.collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))` groups the characters by their identity (the character itself) and counts the occurrences of each character.

3.  **Merge Two Map Objects Efficiently**

    **Question:**
    Given two `Map<String, Integer>`, merge them such that duplicate keys sum their values.

    **Solution:**
    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.Stream;
    
    public class MergeMaps {
        public static void main(String[] args) {
            Map<String, Integer> map1 = Map.of("A", 1, "B", 2, "C", 3);
            Map<String, Integer> map2 = Map.of("B", 3, "C", 4, "D", 5);
    
            Map<String, Integer> mergedMap = Stream.concat(map1.entrySet().stream(), map2.entrySet().stream())
                                                   .collect(Collectors.toMap(Map.Entry::getKey, 
                                                                             Map.Entry::getValue, 
                                                                             Integer::sum));
    
            System.out.println(mergedMap); // Output: {A=1, B=5, C=7, D=5}
        }
    }
    ```

    **Explanation:**
    -   `Stream.concat(map1.entrySet().stream(), map2.entrySet().stream())` combines the entry sets of both maps into a single stream.
    -   `.collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue, Integer::sum))` collects the entries into a new map. If duplicate keys are encountered, the `Integer::sum` merge function is used to sum the values.

4.  **Check If All Elements in a List Are Unique**

    **Question:**
    Given a list of integers, check if all elements are unique.

    **Solution:**
    ```java
    import java.util.*;
    
    public class UniqueElements {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);
    
            boolean isUnique = numbers.size() == numbers.stream().distinct().count();
    
            System.out.println("Are all elements unique? " + isUnique);
        }
    }
    ```

    **Explanation:**
    -   `numbers.stream().distinct().count()` counts the number of unique elements in the stream.
    -   The size of the original list is compared to the count of distinct elements. If they are equal, all elements are unique.

5.  **Process Large Data Sets Using Parallel Streams**

    **Question:**
    Given a large dataset, sum all elements efficiently using `parallelStream()`.

    **Solution:**
    ```java
    import java.util.*;
    import java.util.stream.IntStream;
    
    public class ParallelProcessing {
        public static void main(String[] args) {
            List<Integer> numbers = IntStream.rangeClosed(1, 1000000).boxed().toList();
    
            long sum = numbers.parallelStream().mapToLong(Integer::longValue).sum();
    
            System.out.println("Sum: " + sum);
        }
    }
    ```

    **Explanation:**
    -   `numbers.parallelStream()` creates a parallel stream, allowing the operation to be performed concurrently across multiple threads.
    -   `.mapToLong(Integer::longValue)` converts each `Integer` to a `long`.
    -   `.sum()` calculates the sum of all the `long` values in the stream.

6.  **Partition a List into Even and Odd Numbers**

    **Question:**
    Given a list of numbers, separate even and odd numbers using Streams.

    **Solution:**
    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class PartitioningExample {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
    
            Map<Boolean, List<Integer>> partitioned = numbers.stream()
                                                             .collect(Collectors.partitioningBy(n -> n % 2 == 0));
    
            System.out.println("Even Numbers: " + partitioned.get(true));
            System.out.println("Odd Numbers: " + partitioned.get(false));
        }
    }
    ```

    **Explanation:**
    -   `.collect(Collectors.partitioningBy(n -> n % 2 == 0))` partitions the stream into two lists based on whether the elements are even or odd. `n -> n % 2 == 0` is the predicate used for partitioning.

7.  **Group Elements by Length**

    **Question:**
    Given a list of words, group them by their length.

    **Solution:**
    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class GroupByLength {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("apple", "bat", "car", "dog", "elephant");
    
            Map<Integer, List<String>> grouped = words.stream()
                                                      .collect(Collectors.groupingBy(String::length));
    
            System.out.println(grouped);
        }
    }
    ```

    **Explanation:**
    -   `.collect(Collectors.groupingBy(String::length))` groups the words by their length, creating a map where the keys are the lengths and the values are lists of words with that length.

8.  **Convert List of Objects to Map**

    **Question:**
    Given a list of `Person` objects, convert it to a `Map<Integer, String>` where the key is id and value is name.

    **Solution:**
    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    class Person {
        int id;
        String name;
        
        Person(int id, String name) {
            this.id = id;
            this.name = name;
        }
        
        public int getId() { return id; }
        public String getName() { return name; }
    }
    public class ConvertListToMap {
        public static void main(String[] args) {
            List<Person> people = Arrays.asList(new Person(1, "Alice"), new Person(2, "Bob"), new Person(3, "Charlie"));
    
            Map<Integer, String> personMap = people.stream()
                                                   .collect(Collectors.toMap(Person::getId, Person::getName));
    
            System.out.println(personMap);
        }
    }
    ```

    **Explanation:**
    -   `.collect(Collectors.toMap(Person::getId, Person::getName))` collects the `Person` objects into a map. `Person::getId` provides the keys (IDs), and `Person::getName` provides the values (names).

## Advanced Java Collections and Streams Questions

9.  **Find the First Non-Repeating Character in a String**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class FirstNonRepeating {
        public static void main(String[] args) {
            String input = "swiss";
            
            Character result = input.chars()
                                    .mapToObj(c -> (char) c)
                                    .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()))
                                    .entrySet()
                                    .stream()
                                    .filter(entry -> entry.getValue() == 1)
                                    .map(Map.Entry::getKey)
                                    .findFirst()
                                    .orElse(null);
    
            System.out.println(result); // Output: 'w'
        }
    }
    ```

    **Explanation:**
    -   Uses `LinkedHashMap` to preserve insertion order, ensuring the first non-repeating character is found.
    -   Groups characters by identity and counts occurrences, then filters for entries with a count of 1 and gets the first one.

10. **Find the Second-Highest Number in a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class SecondHighest {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(10, 20, 30, 40, 50);
    
            Integer secondHighest = numbers.stream()
                                           .distinct()
                                           .sorted(Comparator.reverseOrder())
                                           .skip(1)
                                           .findFirst()
                                           .orElse(null);
    
            System.out.println(secondHighest); // Output: 40
        }
    }
    ```

    **Explanation:**
    -   Removes duplicates with `distinct()`, sorts the list in reverse order, skips the first element (the highest), and retrieves the first remaining element (the second-highest).

11. **Convert a Set to a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class SetToList {
        public static void main(String[] args) {
            Set<String> set = Set.of("A", "B", "C");
    
            List<String> list = set.stream().collect(Collectors.toList());
    
            System.out.println(list);
        }
    }
    ```

    **Explanation:**
    -   Converts the elements of a `Set` to a `List` using a stream. This is straightforward and useful when you need list-specific operations on a set's elements.

12. **Check if a List Contains Duplicates**

    ```java
    import java.util.*;
    
    public class ContainsDuplicates {
        public static void main(String[] args) {
            List<Integer> list = Arrays.asList(1, 2, 3, 2, 4);
    
            boolean hasDuplicates = list.size() != new HashSet<>(list).size();
    
            System.out.println("Contains duplicates? " + hasDuplicates);
        }
    }
    ```

    **Explanation:**
    -   Compares the size of the original `List` with the size of a `HashSet` created from the list. Since `HashSet` does not allow duplicates, a size difference indicates duplicates were present.

13. **Find Duplicates in a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class FindDuplicates {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 2, 4, 3);
    
            Set<Integer> duplicates = numbers.stream()
                                             .filter(i -> Collections.frequency(numbers, i) > 1)
                                             .collect(Collectors.toSet());
    
            System.out.println(duplicates); // Output: [2, 3]
        }
    }
    ```

    **Explanation:**
    -   Filters the stream to include only elements that appear more than once in the list, using `Collections.frequency()`. The result is collected into a `Set` to ensure uniqueness of the duplicate elements.

14. **Flatten a List of Lists using flatMap()**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class FlattenList {
        public static void main(String[] args) {
            List<List<String>> listOfLists = Arrays.asList(
                Arrays.asList("A", "B"),
                Arrays.asList("C", "D")
            );
    
            List<String> flatList = listOfLists.stream()
                                               .flatMap(Collection::stream)
                                               .collect(Collectors.toList());
    
            System.out.println(flatList); // Output: [A, B, C, D]
        }
    }
    ```

    **Explanation:**
    -   Uses `flatMap()` to flatten a `List<List<String>>` into a single `List<String>`. `Collection::stream` converts each inner list into a stream, which `flatMap()` then merges.

15. **Join Strings with a Delimiter**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class JoinStrings {
        public static void main(String[] args) {
            List<String> list = Arrays.asList("Java", "Python", "C++");
    
            String result = list.stream().collect(Collectors.joining(", "));
    
            System.out.println(result); // Output: Java, Python, C++
        }
    }
    ```

    **Explanation:**
    -   Uses `Collectors.joining(", ")` to concatenate the strings in the list into a single string, with each element separated by ", ".

16. **Find the Maximum Value in a List**

    ```java
    import java.util.*;
    
    public class MaxValue {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(10, 20, 5, 50);
    
            Integer max = numbers.stream().max(Integer::compareTo).orElse(null);
    
            System.out.println("Max Value: " + max);
        }
    }
    ```

    **Explanation:**
    -   Uses `max(Integer::compareTo)` to find the maximum element in the stream, relying on the natural ordering of integers. `orElse(null)` handles the case where the stream is empty.

17. **Filter Null Values from a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class FilterNulls {
        public static void main(String[] args) {
            List<String> list = Arrays.asList("A", null, "B", null, "C");
    
            List<String> filtered = list.stream()
                                        .filter(Objects::nonNull)
                                        .collect(Collectors.toList());
    
            System.out.println(filtered); // Output: [A, B, C]
        }
    }
    ```

    **Explanation:**
    -   Uses `filter(Objects::nonNull)` to remove null elements from the stream. `Objects::nonNull` is a method reference that checks if an object is not null.

18. **Count Occurrences of Each Word in a List**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class WordFrequency {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("apple", "banana", "apple");
    
            Map<String, Long> frequency = words.stream()
                                               .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    
            System.out.println(frequency); // Output: {apple=2, banana=1}
        }
    }
    ```

    **Explanation:**
    -   Groups the words by their identity and counts the occurrences of each word using `Collectors.groupingBy(Function.identity(), Collectors.counting())`.

19. **Convert Map to List of Keys**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class MapKeys {
        public static void main(String[] args) {
            Map<String, Integer> map = Map.of("A", 1, "B", 2);
    
            List<String> keys = map.keySet().stream().collect(Collectors.toList());
    
            System.out.println(keys); // Output: [A, B]
        }
    }
    ```

    **Explanation:**
    -   Converts the keys of a `Map` to a `List` by streaming the key set and collecting the elements into a list.

20. **Parallel Stream to Sum Elements**

    ```java
    import java.util.*;
    import java.util.stream.IntStream;
    
    public class ParallelSum {
        public static void main(String[] args) {
            int sum = IntStream.rangeClosed(1, 1000).parallel().sum();
            System.out.println("Sum: " + sum);
        }
    }
    ```

    **Explanation:**
    -   Uses a parallel stream to sum a range of integers. `parallel()` enables concurrent processing, which can significantly improve performance for large datasets.

21. **Filter and Sort a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class FilterSort {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(5, 3, 8, 1, 9);
    
            List<Integer> result = numbers.stream()
                                          .filter(n -> n > 5)
                                          .sorted()
                                          .collect(Collectors.toList());
    
            System.out.println(result); // Output: [8, 9]
        }
    }
    ```

    **Explanation:**
    -   Filters the stream to include only numbers greater than 5 and then sorts the remaining elements in ascending order.

22. **Group Objects by a Custom Property**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    class Product {
        String category;
        String name;
    
        Product(String category, String name) {
            this.category = category;
            this.name = name;
        }
    
        public String getCategory() { return category; }
    }
    
    public class GroupByCategory {
        public static void main(String[] args) {
            List<Product> products = Arrays.asList(new Product("Electronics", "TV"),
                                                   new Product("Furniture", "Table"));
    
            Map<String, List<Product>> grouped = products.stream()
                                                         .collect(Collectors.groupingBy(Product::getCategory));
    
            System.out.println(grouped);
        }
    }
    ```

    **Explanation:**
    -   Groups `Product` objects by their `category` property, creating a map where the keys are categories and the values are lists of products in each category.

23. **Skip and Limit in Streams**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class SkipLimit {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);
    
            List<Integer> result = numbers.stream().skip(2).limit(3).collect(Collectors.toList());
    
            System.out.println(result); // Output: [3, 4, 5]
        }
    }
    ```

    **Explanation:**
    -   Skips the first two elements of the stream and then limits the stream to the next three elements.

24. **Check if Any Element Matches a Condition**

    ```java
    import java.util.*;
    
    public class AnyMatch {
        public static void main(String[] args) {
            List<String> list = Arrays.asList("Apple", "Banana", "Orange");
    
            boolean hasBanana = list.stream().anyMatch(s -> s.equalsIgnoreCase("banana"));
    
            System.out.println(hasBanana); // Output: true
        }
    }
    ```

    **Explanation:**
    -   Checks if any element in the stream matches the given condition (case-insensitive "banana").

25. **Convert List to Set to Remove Duplicates**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class ListToSet {
        public static void main(String[] args) {
            List<Integer> list = Arrays.asList(1, 2, 2, 3, 3, 4);
    
            Set<Integer> set = list.stream().collect(Collectors.toSet());
    
            System.out.println(set); // Output: [1, 2, 3, 4]
        }
    }
    ```

    **Explanation:**
    -   Converts a `List` to a `Set` to remove duplicate elements.

## 10 Additional Real-World Java Collections and Streams Questions

26. **Check if a String is a Palindrome using Streams**

    ```java
    import java.util.stream.IntStream;
    
    public class PalindromeCheck {
        public static void main(String[] args) {
            String input = "madam";
    
            boolean isPalindrome = IntStream.range(0, input.length() / 2)
                                            .allMatch(i -> input.charAt(i) == input.charAt(input.length() - i - 1));
    
            System.out.println("Is Palindrome? " + isPalindrome); // Output: true
        }
    }
    ```

    **Explanation:**
    -   Iterates only till the middle of the string.
    -   Compares the characters from both ends for equality.

27. **Check if Two Strings are Anagrams**

    ```java
    import java.util.*;
    
    public class AnagramCheck {
        public static void main(String[] args) {
            String str1 = "listen";
            String str2 = "silent";
    
            boolean isAnagram = Arrays.equals(
                str1.chars().sorted().toArray(),
                str2.chars().sorted().toArray()
            );
    
            System.out.println("Is Anagram? " + isAnagram); // Output: true
        }
    }
    ```

    **Explanation:**
    -   Convert strings to character streams, sort them, and compare arrays.

28. **Find the Frequency of Each Digit in an Integer**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class DigitFrequency {
        public static void main(String[] args) {
            int number = 1122334455;
    
            Map<Integer, Long> digitFrequency = String.valueOf(number).chars()
                    .map(Character::getNumericValue)
                    .boxed()
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()));
    
            System.out.println(digitFrequency);
        }
    }
    ```

    **Explanation:**
    -   Convert the number to a string, stream the digits, and count occurrences.

29. **Find All Palindromic Words in a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class PalindromicWords {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("madam", "level", "world", "java");
    
            List<String> palindromes = words.stream()
                    .filter(word -> word.equalsIgnoreCase(new StringBuilder(word).reverse().toString()))
                    .collect(Collectors.toList());
    
            System.out.println(palindromes); // Output: [madam, level]
        }
    }
    ```

    **Explanation:**
    -   Filters the list to include only words that are palindromes (reads the same forwards and backwards).

30. **Group Words by Their First Character**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class GroupByFirstLetter {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("apple", "banana", "avocado", "blueberry");
    
            Map<Character, List<String>> grouped = words.stream()
                    .collect(Collectors.groupingBy(word -> word.charAt(0)));
    
            System.out.println(grouped);
        }
    }
    ```

    **Explanation:**
    -   Groups words based on their first character.

31. **Find Common Elements Between Two Lists**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class CommonElements {
        public static void main(String[] args) {
            List<Integer> list1 = Arrays.asList(1, 2, 3, 4);
            List<Integer> list2 = Arrays.asList(3, 4, 5, 6);
    
            List<Integer> common = list1.stream()
                                        .filter(list2::contains)
                                        .collect(Collectors.toList());
    
            System.out.println(common); // Output: [3, 4]
        }
    }
    ```

    **Explanation:**
    -   Filters the first list to include only elements that are also present in the second list.

32. **Find the First Repeating Element in a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class FirstRepeatingElement {
        public static void main(String[] args) {
            List<Integer> list = Arrays.asList(5, 1, 2, 3, 2, 1);
    
            Integer firstRepeating = list.stream()
                                         .filter(i -> Collections.frequency(list, i) > 1)
                                         .findFirst()
                                         .orElse(null);
    
            System.out.println(firstRepeating); // Output: 1
        }
    }
    ```

    **Explanation:**
    -   Filters the list to include only elements that appear more than once, then retrieves the first such element.

33. **Find the Longest String in a List**

    ```java
    import java.util.*;
    
    public class LongestString {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("Java", "Python", "JavaScript");
    
            String longest = words.stream()
                                  .max(Comparator.comparingInt(String::length))
                                  .orElse("");
    
            System.out.println("Longest String: " + longest); // Output: JavaScript
        }
    }
    ```

    **Explanation:**
    -   Finds the string with the maximum length using a comparator that compares string lengths.

34. **Partition a List of Numbers into Prime and Non-Prime**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;
    
    public class PrimePartition {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(2, 3, 4, 5, 6, 7, 8, 9, 10);
    
            Map<Boolean, List<Integer>> partitioned = numbers.stream()
                    .collect(Collectors.partitioningBy(PrimePartition::isPrime));
    
            System.out.println("Primes: " + partitioned.get(true));
            System.out.println("Non-Primes: " + partitioned.get(false));
        }
    
        private static boolean isPrime(int number) {
            return number > 1 && IntStream.rangeClosed(2, (int) Math.sqrt(number))
                                          .allMatch(n -> number % n != 0);
        }
    }
    ```

    **Explanation:**
    -   Partitions the list into two lists: prime numbers and non-prime numbers, based on the `isPrime` predicate.

35. **Find Elements Occurring More Than Once in a List**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class Duplicates {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana");
    
            Set<String> duplicates = words.stream()
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                    .entrySet()
                    .stream()
                    .filter(entry -> entry.getValue() > 1)
                    .map(Map.Entry::getKey)
                    .collect(Collectors.toSet());
    
            System.out.println(duplicates); // Output: [apple, banana]
        }
    }
    ```

    **Explanation:**
    -   Groups the words by identity and counts occurrences, then filters for entries with a count greater than 1 and collects the keys into a set.

## 10 More Real-World Java Collections and Streams Questions

36. **Find the First Non-Repeating Word in a Sentence**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class FirstNonRepeatingWord {
        public static void main(String[] args) {
            String sentence = "java python java c python go";
    
            String firstNonRepeating = Arrays.stream(sentence.split(" "))
                    .collect(Collectors.groupingBy(Function.identity(), LinkedHashMap::new, Collectors.counting()))
                    .entrySet()
                    .stream()
                    .filter(entry -> entry.getValue() == 1)
                    .map(Map.Entry::getKey)
                    .findFirst()
                    .orElse(null);
    
            System.out.println(firstNonRepeating); // Output: c
        }
    }
    ```

    **Explanation:**
    -   Splits the sentence into words, groups them by identity, counts occurrences, filters for words with a count of 1, and retrieves the first such word.

37. **Remove Null and Empty Strings from a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class RemoveNullsAndEmpty {
        public static void main(String[] args) {
            List<String> list = Arrays.asList("Java", "", null, "Python", "  ", "C++");
    
            List<String> cleaned = list.stream()
                    .filter(s -> s != null && !s.trim().isEmpty())
                    .collect(Collectors.toList());
    
            System.out.println(cleaned); // Output: [Java, Python, C++]
        }
    }
    ```

    **Explanation:**
    -   Filters the list to remove null and empty strings, including strings with only whitespace.

38. **Find the Longest Palindromic Substring**

    ```java
    public class LongestPalindrome {
        public static void main(String[] args) {
            String input = "babad";
            String longest = "";
    
            for (int i = 0; i < input.length(); i++) {
                for (int j = i + 1; j <= input.length(); j++) {
                    String sub = input.substring(i, j);
                    if (sub.equals(new StringBuilder(sub).reverse().toString()) && sub.length() > longest.length()) {
                        longest = sub;
                    }
                }
            }
    
            System.out.println("Longest Palindrome: " + longest); // Output: bab or aba
        }
    }
    ```

    **Explanation:**
    -   Finds the longest substring that is a palindrome. It iterates through all possible substrings and checks if they are palindromes.

39. **Find All Anagrams of a Word in a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class FindAnagrams {
        publicstatic void main(String[] args) {
            String word = "listen";
            List<String> candidates = Arrays.asList("enlist", "google", "silent", "tinsel", "java");
    
            List<String> anagrams = candidates.stream()
                    .filter(s -> s.length() == word.length() &&
                                 Arrays.equals(s.chars().sorted().toArray(), word.chars().sorted().toArray()))
                    .collect(Collectors.toList());
    
            System.out.println(anagrams); // Output: [enlist, silent, tinsel]
        }
    }
    ```

    **Explanation:**
    -   Filters the list to include only words that are anagrams of the given word. It checks if the words have the same length and if their sorted character arrays are equal.

40. **Group Strings by Their Length**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class GroupByLength {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("Java", "Python", "C", "Go", "Ruby");
    
            Map<Integer, List<String>> grouped = words.stream()
                    .collect(Collectors.groupingBy(String::length));
    
            System.out.println(grouped);
        }
    }
    ```

    **Explanation:**
    -   Groups strings by their length, creating a map where the keys are lengths and the values are lists of strings with that length.

41. **Sum of Digits in a List of Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class SumOfDigits {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(123, 456, 789);
    
            List<Integer> sumOfDigits = numbers.stream()
                    .map(n -> String.valueOf(n).chars().map(Character::getNumericValue).sum())
                    .collect(Collectors.toList());
    
            System.out.println(sumOfDigits); // Output: [6, 15, 24]
        }
    }
    ```

    **Explanation:**
    -   Calculates the sum of digits for each number in the list by converting the number to a string, streaming its characters, converting them to numeric values, and summing them.

42. **Find Elements Occurring Exactly Twice in a List**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class ElementsOccurringTwice {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "banana", "banana");
    
            Set<String> result = words.stream()
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                    .entrySet()
                    .stream()
                    .filter(entry -> entry.getValue() == 2)
                    .map(Map.Entry::getKey)
                    .collect(Collectors.toSet());
    
            System.out.println(result); // Output: [apple]
        }
    }
    ```

    **Explanation:**
    -   Groups the words by identity, counts occurrences, filters for words with a count of exactly 2, and collects the keys into a set.

43. **Find the Most Frequent Character in a String**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class MostFrequentChar {
        public static void main(String[] args) {
            String input = "aabbcccddd";
    
            Character mostFrequent = input.chars()
                    .mapToObj(c -> (char) c)
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                    .entrySet()
                    .stream()
                    .max(Map.Entry.comparingByValue())
                    .map(Map.Entry::getKey)
                    .orElse(null);
    
            System.out.println("Most Frequent Character: " + mostFrequent); // Output: d
        }
    }
    ```

    **Explanation:**
    -   Groups characters by identity, counts occurrences, finds the entry with the maximum count, and retrieves the corresponding character.

44. **Reverse Each Word in a Sentence**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class ReverseWords {
        public static void main(String[] args) {
            String sentence = "hello world";
    
            String reversed = Arrays.stream(sentence.split(" "))
                    .map(word -> new StringBuilder(word).reverse().toString())
                    .collect(Collectors.joining(" "));
    
            System.out.println(reversed); // Output: olleh dlrow
        }
    }
    ```

    **Explanation:**
    -   Splits the sentence into words, reverses each word using `StringBuilder`, and joins the reversed words back into a sentence.

45. **Count Unique Elements in a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class CountUnique {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 4, 4, 5);
    
            long uniqueCount = numbers.stream()
                    .collect(Collectors.groupingBy(n -> n, Collectors.counting()))
                    .entrySet()
                    .stream()
                    .filter(entry -> entry.getValue() == 1)
                    .count();
    
            System.out.println("Unique Count: " + uniqueCount); // Output: 3 (1, 3, 5)
        }
    }
    ```

    **Explanation:**
    -   Groups the elements by identity, counts occurrences, filters for elements with a count of 1 (unique elements), and counts the number of such elements.
- # Java Collections and Streams Practice Questions (Continued)

## More Advanced Java Collections and Streams Questions

46. **Find the Intersection of Multiple Lists**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class ListIntersection {
        public static void main(String[] args) {
            List<Integer> list1 = Arrays.asList(1, 2, 3, 4, 5);
            List<Integer> list2 = Arrays.asList(3, 4, 5, 6, 7);
            List<Integer> list3 = Arrays.asList(5, 6, 7, 8, 9);
    
            List<Integer> intersection = list1.stream()
                    .filter(list2::contains)
                    .filter(list3::contains)
                    .collect(Collectors.toList());
    
            System.out.println(intersection); // Output: [5]
        }
    }
    ```

    **Explanation:**
    -   Filters `list1` to include only elements that are present in both `list2` and `list3`, effectively finding the intersection of the lists.

47. **Find the Difference Between Two Lists**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class ListDifference {
        public static void main(String[] args) {
            List<Integer> list1 = Arrays.asList(1, 2, 3, 4, 5);
            List<Integer> list2 = Arrays.asList(3, 4, 5, 6, 7);
    
            List<Integer> difference = list1.stream()
                    .filter(i -> !list2.contains(i))
                    .collect(Collectors.toList());
    
            System.out.println(difference); // Output: [1, 2]
        }
    }
    ```

    **Explanation:**
    -   Filters `list1` to include only elements that are not present in `list2`, effectively finding the elements that are in `list1` but not in `list2`.

48. **Group Objects by Multiple Properties**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    class Product {
        String category;
        String brand;
        String name;
    
        Product(String category, String brand, String name) {
            this.category = category;
            this.brand = brand;
            this.name = name;
        }
    
        public String getCategory() { return category; }
        public String getBrand() { return brand; }
    
        @Override
        public String toString() {
            return "Product{category='" + category + "', brand='" + brand + "', name='" + name + "'}";
        }
    }
    
    public class GroupByMultipleProperties {
        public static void main(String[] args) {
            List<Product> products = Arrays.asList(
                    new Product("Electronics", "Samsung", "TV"),
                    new Product("Electronics", "LG", "TV"),
                    new Product("Furniture", "IKEA", "Table"),
                    new Product("Furniture", "IKEA", "Chair")
            );
    
            Map<String, Map<String, List<Product>>> grouped = products.stream()
                    .collect(Collectors.groupingBy(Product::getCategory,
                            Collectors.groupingBy(Product::getBrand)));
    
            System.out.println(grouped);
        }
    }
    ```

    **Explanation:**
    -   Groups `Product` objects first by `category` and then by `brand`, creating a nested map structure. This allows for grouping objects by multiple levels of properties.

49. **Find the Median of a List of Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class ListMedian {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 3, 2, 4, 5);
    
            List<Integer> sortedNumbers = numbers.stream().sorted().collect(Collectors.toList());
            int size = sortedNumbers.size();
            double median;
    
            if (size % 2 == 0) {
                median = (sortedNumbers.get(size / 2 - 1) + sortedNumbers.get(size / 2)) / 2.0;
            } else {
                median = sortedNumbers.get(size / 2);
            }
    
            System.out.println("Median: " + median); // Output: 3.0
        }
    }
    ```

    **Explanation:**
    -   Sorts the list and calculates the median. If the list size is even, the median is the average of the middle two elements. If the list size is odd, the median is the middle element.

50. **Split a List into Chunks of a Given Size**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;
    
    public class ListChunk {
        public static void main(String[] args) {
            List<Integer> numbers = IntStream.rangeClosed(1, 10).boxed().collect(Collectors.toList());
            int chunkSize = 3;
    
            List<List<Integer>> chunks = IntStream.range(0, (numbers.size() + chunkSize - 1) / chunkSize)
                    .mapToObj(i -> numbers.subList(i * chunkSize, Math.min((i + 1) * chunkSize, numbers.size())))
                    .collect(Collectors.toList());
    
            System.out.println(chunks); // Output: [[1, 2, 3], [4, 5, 6], [7, 8, 9], [10]]
        }
    }
    ```

    **Explanation:**
    -   Splits the list into sublists of a given size. It calculates the number of chunks needed and uses `subList` to create each chunk.

51. **Find the Most Common Element in a List**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;
    
    public class MostCommonElement {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("apple", "banana", "apple", "orange", "apple");
    
            String mostCommon = words.stream()
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                    .entrySet().stream()
                    .max(Map.Entry.comparingByValue())
                    .map(Map.Entry::getKey)
                    .orElse(null);
    
            System.out.println("Most Common Element: " + mostCommon); // Output: apple
        }
    }
    ```

    **Explanation:**
    -   Groups the elements by identity, counts occurrences, finds the entry with the maximum count, and retrieves the corresponding element.

52. **Check if a List is Sorted**

    ```java
    import java.util.*;
    
    public class ListSorted {
        public static void main(String[] args) {
            List<Integer> sortedList = Arrays.asList(1, 2, 3, 4, 5);
            List<Integer> unsortedList = Arrays.asList(1, 3, 2, 4, 5);
    
            boolean isSorted1 = IntStream.range(1, sortedList.size())
                    .allMatch(i -> sortedList.get(i - 1) <= sortedList.get(i));
    
            boolean isSorted2 = IntStream.range(1, unsortedList.size())
                    .allMatch(i -> unsortedList.get(i - 1) <= unsortedList.get(i));
    
            System.out.println("Sorted List: " + isSorted1); // Output: true
            System.out.println("Unsorted List: " + isSorted2); // Output: false
        }
    }
    ```

    **Explanation:**
    -   Checks if each element in the list is less than or equal to the next element. If all pairs satisfy this condition, the list is sorted.

53. **Find the First N Prime Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;
    
    public class FirstNPrimes {
        public static void main(String[] args) {
            int n = 10;
    
            List<Integer> primes = IntStream.iterate(2, i -> i + 1)
                    .filter(FirstNPrimes::isPrime)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());
    
            System.out.println(primes); // Output: [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
        }
    
        private static boolean isPrime(int number) {
            return number > 1 && IntStream.rangeClosed(2, (int) Math.sqrt(number))
                                          .allMatch(i -> number % i != 0);
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers starting from 2, filters out non-prime numbers, limits the stream to the first N prime numbers, and collects them into a list.

54. **Find the Running Average of a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;
    
    public class RunningAverage {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);
    
            List<Double> runningAverages = IntStream.range(0, numbers.size())
                    .mapToDouble(i -> numbers.subList(0, i + 1).stream().mapToInt(Integer::intValue).average().orElse(0.0))
                    .boxed()
                    .collect(Collectors.toList());
    
            System.out.println(runningAverages); // Output: [1.0, 1.5, 2.0, 2.5, 3.0]
        }
    }
    ```

    **Explanation:**
    -   Calculates the running average of the list. For each index, it calculates the average of the sublist from the beginning to that index.55. **Find the Longest Common Prefix in a List of Strings**

    ```java
    import java.util.*;
    
    public class LongestCommonPrefix {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("flower", "flow", "flight");
    
            String prefix = words.stream()
                    .reduce((s1, s2) -> {
                        int i = 0;
                        while (i < Math.min(s1.length(), s2.length()) && s1.charAt(i) == s2.charAt(i)) {
                            i++;
                        }
                        return s1.substring(0, i);
                    })
                    .orElse("");
    
            System.out.println("Longest Common Prefix: " + prefix); // Output: fl
        }
    }
    ```

    **Explanation:**
    -   Uses `reduce` to iteratively find the common prefix between pairs of strings. The result is the longest common prefix among all strings in the list.

56. **Find the First N Fibonacci Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.Stream;
    
    public class FirstNFibonacci {
        public static void main(String[] args) {
            int n = 10;
    
            List<Integer> fibonacci = Stream.iterate(new int[]{0, 1}, f -> new int[]{f[1], f[0] + f[1]})
                    .limit(n)
                    .map(f -> f[0])
                    .collect(Collectors.toList());
    
            System.out.println(fibonacci); // Output: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
        }
    }
    ```

    **Explanation:**
    -   Uses `iterate` to generate an infinite stream of Fibonacci pairs, limits the stream to the first N pairs, extracts the first element of each pair, and collects them into a list.

57. **Group Anagrams Together in a List of Strings**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class GroupAnagrams {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("eat", "tea", "tan", "ate", "nat", "bat");
    
            Map<String, List<String>> groupedAnagrams = words.stream()
                    .collect(Collectors.groupingBy(word -> {
                        char[] chars = word.toCharArray();
                        Arrays.sort(chars);
                        return new String(chars);
                    }));
    
            System.out.println(groupedAnagrams);
            // Output: {aet=[eat, tea, ate], ant=[tan, nat], abt=[bat]}
        }
    }
    ```

    **Explanation:**
    -   Groups the words by their sorted character representation, effectively grouping anagrams together.

58. **Find the Kth Largest Element in a List**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    
    public class KthLargestElement {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(3, 2, 1, 5, 6, 4);
            int k = 2;
    
            Integer kthLargest = numbers.stream()
                    .sorted(Comparator.reverseOrder())
                    .skip(k - 1)
                    .findFirst()
                    .orElse(null);
    
            System.out.println("Kth Largest Element: " + kthLargest); // Output: 5
        }
    }
    ```

    **Explanation:**
    -   Sorts the list in reverse order, skips the first k-1 elements, and retrieves the first remaining element, which is the kth largest.

59. **Find All Subsets of a Set**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;
    
    public class SubsetsOfSet {
        public static void main(String[] args) {
            List<Integer> set = Arrays.asList(1, 2, 3);
    
            List<List<Integer>> subsets = IntStream.range(0, 1 << set.size())
                    .mapToObj(mask -> IntStream.range(0, set.size())
                            .filter(i -> (mask & (1 << i)) != 0)
                            .mapToObj(set::get)
                            .collect(Collectors.toList()))
                    .collect(Collectors.toList());
    
            System.out.println(subsets);
            // Output: [[], [1], [2], [1, 2], [3], [1, 3], [2, 3], [1, 2, 3]]
        }
    }
    ```

    **Explanation:**
    -   Generates all possible subsets using bit manipulation. Each bit in the mask represents whether an element is included in the subset.

60. **Implement a Simple Moving Average (SMA) Calculation**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;
    
    public class SimpleMovingAverage {
        public static void main(String[] args) {
            List<Double> data = Arrays.asList(1.0, 2.0, 3.0, 4.0, 5.0, 6.0);
            int windowSize = 3;
    
            List<Double> sma = IntStream.range(windowSize - 1, data.size())
                    .mapToObj(i -> data.subList(i - windowSize + 1, i + 1).stream()
                            .mapToDouble(Double::doubleValue)
                            .average()
                            .orElse(0.0))
                    .collect(Collectors.toList());
    
            System.out.println(sma); // Output: [2.0, 3.0, 4.0, 5.0]
        }
    }
    ```

    **Explanation:**
    -   Calculates the simple moving average of a list of numbers using a sliding window. For each position, it calculates the average of the elements within the window.
- # Java Collections and Streams Practice Questions (Continued)

## More Advanced Java Collections and Streams Questions (Continued)

61. **Find the First Non-Consecutive Number in a List**

    ```java
    import java.util.*;
    import java.util.stream.IntStream;

    public class FirstNonConsecutive {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 5, 6, 7);

            Integer nonConsecutive = IntStream.range(1, numbers.size())
                    .filter(i -> numbers.get(i) - numbers.get(i - 1) != 1)
                    .mapToObj(numbers::get)
                    .findFirst()
                    .orElse(null);

            System.out.println("First Non-Consecutive: " + nonConsecutive); // Output: 5
        }
    }
    ```

    **Explanation:**
    -   Checks each pair of consecutive numbers to see if their difference is not 1. Returns the first non-consecutive number found.

62. **Find the Missing Number in a List of Consecutive Numbers**

    ```java
    import java.util.*;
    import java.util.stream.IntStream;

    public class MissingNumber {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 5, 6);

            Integer missing = IntStream.range(1, numbers.size())
                    .filter(i -> numbers.get(i) - numbers.get(i - 1) != 1)
                    .map(i -> numbers.get(i - 1) + 1)
                    .findFirst()
                    .orElse(null);

            System.out.println("Missing Number: " + missing); // Output: 4
        }
    }
    ```

    **Explanation:**
    -   Checks each pair of consecutive numbers to see if their difference is not 1. Returns the missing number that should have been present.

63. **Partition a List into Sublists Based on a Condition**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;

    public class PartitionByCondition {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

            List<List<Integer>> partitioned = numbers.stream()
                    .collect(Collectors.partitioningBy(n -> n % 3 == 0))
                    .values().stream()
                    .collect(Collectors.toList());

            System.out.println(partitioned);
            // Output: [[3, 6, 9], [1, 2, 4, 5, 7, 8, 10]]
        }
    }
    ```

    **Explanation:**
    -   Partitions the list into two sublists based on whether the number is divisible by 3. Collects the values of the partitioned map into a list of lists.

64. **Find the Mode (Most Frequent Element) in a List**

    ```java
    import java.util.*;
    import java.util.function.Function;
    import java.util.stream.Collectors;

    public class ListMode {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 2, 4, 2, 5);

            Integer mode = numbers.stream()
                    .collect(Collectors.groupingBy(Function.identity(), Collectors.counting()))
                    .entrySet().stream()
                    .max(Map.Entry.comparingByValue())
                    .map(Map.Entry::getKey)
                    .orElse(null);

            System.out.println("Mode: " + mode); // Output: 2
        }
    }
    ```

    **Explanation:**
    -   Groups the elements by identity, counts occurrences, finds the entry with the maximum count, and retrieves the corresponding element.

65. **Check if a List Contains a Sublist**

    ```java
    import java.util.*;

    public class ContainsSublist {
        public static void main(String[] args) {
            List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6);
            List<Integer> sublist = Arrays.asList(3, 4, 5);

            boolean contains = Collections.indexOfSubList(list, sublist) != -1;

            System.out.println("Contains Sublist: " + contains); // Output: true
        }
    }
    ```

    **Explanation:**
    -   Uses `Collections.indexOfSubList` to check if a sublist exists within a larger list. Returns true if the sublist is found.

66. **Find the First N Even Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNEven {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> evens = IntStream.iterate(0, i -> i + 2)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(evens); // Output: [0, 2, 4, 6, 8]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of even numbers starting from 0, limits the stream to the first N numbers, and collects them into a list.

67. **Find the Difference Between Two Sets**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;

    public class SetDifference {
        public static void main(String[] args) {
            Set<Integer> set1 = Set.of(1, 2, 3, 4, 5);
            Set<Integer> set2 = Set.of(3, 4, 5, 6, 7);

            Set<Integer> difference = set1.stream()
                    .filter(i -> !set2.contains(i))
                    .collect(Collectors.toSet());

            System.out.println(difference); // Output: [1, 2]
        }
    }
    ```

    **Explanation:**
    -   Filters `set1` to include only elements that are not present in `set2`, effectively finding the elements that are in `set1` but not in `set2`.

68. **Find the Symmetric Difference Between Two Sets**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.Stream;

    public class SymmetricDifference {
        public static void main(String[] args) {
            Set<Integer> set1 = Set.of(1, 2, 3, 4, 5);
            Set<Integer> set2 = Set.of(3, 4, 5, 6, 7);

            Set<Integer> symmetricDifference = Stream.concat(
                    set1.stream().filter(i -> !set2.contains(i)),
                    set2.stream().filter(i -> !set1.contains(i))
            ).collect(Collectors.toSet());

            System.out.println(symmetricDifference); // Output: [1, 2, 6, 7]
        }
    }
    ```

    **Explanation:**
    -   Finds the elements that are in either `set1` or `set2`, but not in both.

69. **Find the Product of All Elements in a List**

    ```java
    import java.util.*;

    public class ListProduct {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

            int product = numbers.stream()
                    .reduce(1, (a, b) -> a * b);

            System.out.println("Product: " + product); // Output: 120
        }
    }
    ```

    **Explanation:**
    -   Uses `reduce` to multiply all elements in the list together.

70. **Find the Average Length of Strings in a List**

    ```java
    import java.util.*;

    public class AverageStringLength {
        public static void main(String[] args) {
            List<String> words = Arrays.asList("apple", "banana", "orange");

            double averageLength = words.stream()
                    .mapToInt(String::length)
                    .average()
                    .orElse(0.0);

            System.out# Java Collections and Streams Practice Questions (Continued)

## More Advanced Java Collections and Streams Questions (Continued)

71. **Find the First N Numbers Divisible by a Given Number**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNDivisible {
        public static void main(String[] args) {
            int n = 5;
            int divisor = 3;

            List<Integer> divisibleNumbers = IntStream.iterate(divisor, i -> i + divisor)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(divisibleNumbers); // Output: [3, 6, 9, 12, 15]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers divisible by the given divisor, limits the stream to the first N numbers, and collects them into a list.

72. **Find the Sum of Squares of Numbers in a List**

    ```java
    import java.util.*;

    public class SumOfSquares {
        public static void main(String[] args) {
            List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

            int sumOfSquares = numbers.stream()
                    .mapToInt(n -> n * n)
                    .sum();

            System.out.println("Sum of Squares: " + sumOfSquares); // Output: 55
        }
    }
    ```

    **Explanation:**
    -   Maps each number to its square and then sums the squares.

73. **Find the First N Palindrome Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNPalindromes {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> palindromes = IntStream.iterate(1, i -> i + 1)
                    .filter(FirstNPalindromes::isPalindrome)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(palindromes); // Output: [1, 2, 3, 4, 5] (Since we're starting at 1, these are the first 5 single digit palindromes)
        }

        private static boolean isPalindrome(int number) {
            String str = String.valueOf(number);
            return str.equals(new StringBuilder(str).reverse().toString());
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out non-palindrome numbers, limits the stream to the first N palindromes, and collects them into a list.

74. **Find the Union of Multiple Sets**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.Stream;

    public class SetUnion {
        public static void main(String[] args) {
            Set<Integer> set1 = Set.of(1, 2, 3);
            Set<Integer> set2 = Set.of(3, 4, 5);
            Set<Integer> set3 = Set.of(5, 6, 7);

            Set<Integer> union = Stream.of(set1, set2, set3)
                    .flatMap(Collection::stream)
                    .collect(Collectors.toSet());

            System.out.println(union); // Output: [1, 2, 3, 4, 5, 6, 7]
        }
    }
    ```

    **Explanation:**
    -   Flattens the stream of sets into a single stream of elements and collects them into a set, effectively finding the union of all sets.

75. **Find the First N Numbers That Are Perfect Squares**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNPerfectSquares {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> perfectSquares = IntStream.iterate(1, i -> i + 1)
                    .map(i -> i * i)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(perfectSquares); // Output: [1, 4, 9, 16, 25]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, maps each number to its square, limits the stream to the first N perfect squares, and collects them into a list.

76. **Find the First N Numbers with a Given Digit Sum**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNDigitSum {
        public static void main(String[] args) {
            int n = 5;
            int digitSum = 5;

            List<Integer> numbers = IntStream.iterate(1, i -> i + 1)
                    .filter(i -> digitSum(i) == digitSum)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(numbers); // Output: [5, 14, 23, 32, 41]
        }

        private static int digitSum(int number) {
            return String.valueOf(number).chars()
                    .map(Character::getNumericValue)
                    .sum();
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers whose digit sum does not match the given digit sum, limits the stream to the first N numbers, and collects them into a list.

77. **Find the First N Numbers with a Given Number of Set Bits**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNSetBits {
        public static void main(String[] args) {
            int n = 5;
            int setBits = 2;

            List<Integer> numbers = IntStream.iterate(1, i -> i + 1)
                    .filter(i -> Integer.bitCount(i) == setBits)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(numbers); // Output: [3, 5, 6, 9, 10]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers whose number of set bits does not match the given number of set bits, limits the stream to the first N numbers, and collects them into a list.

78. **Find the First N Numbers That Are Powers of Two**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNPowersOfTwo {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> powersOfTwo = IntStream.iterate(1, i -> i * 2)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(powersOfTwo); // Output: [1, 2, 4, 8, 16]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of powers of two, limits the stream to the first N powers of two, and collects them into a list.

79. **Find the First N Numbers That Are Multiples of Two Given Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNMultiples {
        public static void main(String[] args) {
            int n = 5;
            int num1 = 3;
            int num2 = 5;

            List<Integer> multiples = IntStream.iterate(1, i -> i + 1)
                    .filter(i -> i % num1 == 0 && i % num2 == 0)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(multiples); // Output: [15, 30, 45, 60, 75]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers that are multiples of both given numbers, limits the stream to the first N multiples, and collects them into a list.

80. **Find the First N Numbers That Are Prime Palindromes**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNPrimePalindromes {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> primePalindromes = IntStream.iterate(2, i -> i + 1)
                    .filter(FirstNPrimePalindromes::isPrimePalindrome)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(primePalindromes); // Output: [2, 3, 5, 7, 11] (Single digit primes and 11 which is the first double digit prime palindrome)
        }

        private static boolean isPrimePalindrome(int number) {
            return isPrime(number) && isPalindrome(number);
        }

        private static boolean isPrime(int number) {
            return number > 1 && IntStream.rangeClosed(2, (int) Math.sqrt(number))
                                          .allMatch(i -> number % i != 0);
        }

        private static boolean isPalindrome(int number) {
            String str = String.valueOf(number);
            return str.equals(new StringBuilder(str).reverse().toString());
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers that are not both prime and palindromes, limits the stream to the first N prime palindromes, and collects them into a list.

81. **Find the First N Numbers That Are Perfect Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNPerfectNumbers {
        public static void main(String[] args) {
            int n = 3;

            List<Integer> perfectNumbers = IntStream.iterate(2, i -> i + 1)
                    .filter(FirstNPerfectNumbers::isPerfectNumber)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(perfectNumbers); // Output: [6, 28, 496]
        }

        private static boolean isPerfectNumber(int number) {
            int sum = IntStream.range(1, number)
                    .filter(i -> number % i == 0)
                    .sum();
            return sum == number;
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers that are not perfect numbers, limits the stream to the first N perfect numbers, and collects them into a list.

82. **Find the First N Numbers That Are Armstrong Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNArmstrongNumbers {
        public static void main(String[] args) {
            int n = 3;

            List<Integer> armstrongNumbers = IntStream.iterate(1, i -> i + 1)
                    .filter(FirstNArmstrongNumbers::isArmstrongNumber)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(armstrongNumbers); // Output: [1, 153, 370]
        }

        private static boolean isArmstrongNumber(int number) {
            String str = String.valueOf(number);
            int power = str.length();
            int sum = str.chars()
                    .map(Character::getNumericValue)
                    .map(digit -> (int) Math.pow(digit, power))
                    .sum();
            return sum == number;
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers that are not Armstrong numbers, limits the stream to the first N Armstrong numbers, and collects them into a list.

83. **Find the First N Numbers That Are Happy Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNHappyNumbers {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> happyNumbers = IntStream.iterate(1, i -> i + 1)
                    .filter(FirstNHappyNumbers::isHappyNumber)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(happyNumbers); // Output: [1, 7, 10, 13, 19]
        }

        private static boolean isHappyNumber(int number) {
            Set<Integer> seen = new HashSet<>();
            while (number != 1 && !seen.contains(number)) {
                seen.add(number);
                number = String.valueOf(number).chars()
                        .map(Character::getNumericValue)
                        .map(digit -> digit * digit)
                        .sum();
            }
            return number == 1;
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers that are not happy numbers, limits the stream to the first N happy numbers, and collects them into a list.

84. **Find the First N Numbers That Are Smith Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNSmithNumbers {
        public static void main(String[] args) {
            int n = 3;

            List<Integer> smithNumbers = IntStream.iterate(4, i -> i + 1)
                    .filter(FirstNSmithNumbers::isSmithNumber)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(smithNumbers); // Output: [4, 22, 27]
        }

        private static boolean isSmithNumber(int number) {
            return number > 1 && sumOfDigits(number) == sumOfPrimeFactors(number);
        }

        private static int sumOfDigits(int number) {
            return String.valueOf(number).chars()
                    .map(Character::getNumericValue)
                    .sum();
        }

        private static int sumOfPrimeFactors(int number) {
            int sum = 0;
            for (int i = 2; i <= number; i++) {
                while (number % i == 0) {
                    sum += sumOfDigits(i);
                    number /= i;
                }
            }
            return sum;
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers that are not Smith numbers, limits the stream to the first N Smith numbers, and collects them into a list.

85. **Find the First N Numbers That Are Emirp Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNEmirpNumbers {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> emirpNumbers = IntStream.iterate(11, i -> i + 1)
                    .filter(FirstNEmirpNumbers::isEmirpNumber)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(emirpNumbers); // Output: [13, 17, 31, 37, 71]
        }

        private static boolean isEmirpNumber(int number) {
            return isPrime(number) && isPrime(reverse(number)) && number != reverse(number);
        }

        private static boolean isPrime(int number) {
            return number > 1 && IntStream.rangeClosed(2, (int) Math.sqrt(number))
                                          .allMatch(i -> number % i != 0);
        }

        private static int reverse(int number) {
            return Integer.parseInt(new StringBuilder(String.valueOf(number)).reverse().toString());
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, filters out numbers that are not Emirp numbers, limits the stream to the first N Emirp numbers, and collects them into a list.

86. **Find the First N Numbers That Are Carol Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNCarolNumbers {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> carolNumbers = IntStream.iterate(2, i -> i + 1)
                    .map(i -> (int) (Math.pow(2, i) - 1))
                    .map(i -> i * i - 2)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(carolNumbers); // Output: [7, 47, 223, 959, 3967]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, calculates the Carol numbers using the formula `(2^n - 1)^2 - 2`, limits the stream to the first N Carol numbers, and collects them into a list.

87. **Find the First N Numbers That Are Kynea Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNKyneaNumbers {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> kyneaNumbers = IntStream.iterate(2, i -> i + 1)
                    .map(i -> (int) (Math.pow(2, i) + 1))
                    .map(i -> i * i - 2)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(kyneaNumbers); // Output: [14, 62, 254, 1022, 4094]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, calculates the Kynea numbers using the formula `(2^n + 1)^2 - 2`, limits the stream to the first N Kynea numbers, and collects them into a list.

88. **Find the First N Numbers That Are Mersenne Primes**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNMersennePrimes {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> mersennePrimes = IntStream.iterate(2, i -> i + 1)
                    .map(i -> (int) (Math.pow(2, i) - 1))
                    .filter(FirstNMersennePrimes::isPrime)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(mersennePrimes); // Output: [3, 7, 31, 127, 8191]
        }

        private static boolean isPrime(int number) {
            return number > 1 && IntStream.rangeClosed(2, (int) Math.sqrt(number))
                                          .allMatch(i -> number % i != 0);
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, calculates the Mersenne numbers using the formula `2^n - 1`, filters out non-prime numbers, limits the stream to the first N Mersenne primes, and collects them into a list.

89. **Find the First N Numbers That Are Cullen Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNCullenNumbers {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> cullenNumbers = IntStream.iterate(1, i -> i + 1)
                    .map(i -> i * (int) Math.pow(2, i) + 1)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(cullenNumbers); // Output: [3, 9, 25, 65, 161]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, calculates the Cullen numbers using the formula `n * 2^n + 1`, limits the stream to the first N Cullen numbers, and collects them into a list.

90. **Find the First N Numbers That Are Woodall Numbers**

    ```java
    import java.util.*;
    import java.util.stream.Collectors;
    import java.util.stream.IntStream;

    public class FirstNWoodallNumbers {
        public static void main(String[] args) {
            int n = 5;

            List<Integer> woodallNumbers = IntStream.iterate(1, i -> i + 1)
                    .map(i -> i * (int) Math.pow(2, i) - 1)
                    .limit(n)
                    .boxed()
                    .collect(Collectors.toList());

            System.out.println(woodallNumbers); // Output: [1, 7, 23, 63, 159]
        }
    }
    ```

    **Explanation:**
    -   Generates an infinite stream of numbers, calculates the Woodall numbers using the formula `n * 2^n - 1`, limits the stream to the first N Woodall numbers, and collects them into a list.

These questions and answers provide a comprehensive set of practice problems for Java Collections and Streams, covering a wide range of topics and complexities.