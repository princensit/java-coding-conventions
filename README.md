# Java Coding Conventions

This document serves the coding standards for source code in Java language. It is intended for use by software engineering teams to employ a common style when writing Java code.

Code conventions improve the readability of the software, allowing engineers to understand new code more quickly and thoroughly.

## Contents
1. Source file structure
2. Coding guidelines
3. Ordering of class contents
4. JUnit Tests method ordering
5. Javadoc conventions
6. Cyclomatic complexity

### Source file structure
1. License or copyright information, if present
2. Package statement
3. Import statements
4. Exactly one top-level class

Exactly one blank line separates each section that is present.

### Coding guidelines
1. Java code has a column limit of 100 characters. This will be enforced by code formatter xml file, imported in IDE. Other than below exceptions, any line that would exceed this limit must be line-wrapped.
```java
Exceptions:
1. Lines where obeying the column limit is not possible. Ex- a long URL in Javadoc
2. package and import statements
```
2. Package and Import statements are not line-wrapped. The column limit does not apply to import statements.
3. Class and member modifiers, when present, appear in the order recommended by the Java Language Specification:
```java
public protected private abstract default static final transient volatile synchronized native strictfp
```
4. Wildcard imports, static or otherwise are not allowed. Ex-
```java
// bad practice
import java.util.*;
```
5. One variable per declaration. Ex- int a, b; are not allowed.
6. The square brackets form a part of the type, not the variable: String[] args, not String args[]
7. Class names are typically nouns or noun phrases, and is written in UpperCamelCase. Test classes are named starting with the name of the class they are testing, and ending with Test. Ex- ImmutableList, ImmutableListTest
8. Method names are wriiten in lowerCamelCase. Method names are typically verbs or verb phrases. Ex- sendMessage or stop
9. Package names are all lowercase, with consecutive words simply concatenated together (no underscores).
```java
com.example.deepspace, not com.example.deepSpace or com.example.deep_space.
```
10. Braces are used with if, else, for, do and while statements, even when the body is empty or contains only a single statement. Ex-
```java
if (number%2 == 0) {
   System.out.println("even number");
}
11. One annotation per line. Ex-
@Override
@Nullable
public String doSomething() { 
}
```
12. Underscores may appear in JUnit test method names to separate logical components of the name, with each component written in lowerCamelCase. One typical pattern is <methodUnderTest>_<state>, for example pop_emptyStack.
13. Use pipe in catch block that handles multiple exceptions: catch (FooException | BarException e)
14. Use foreach whenever possible. Ex-
```java
List<String> words = Arrays.asList("hello", "world");
for (String word : words) {
    System.out.println(word);
}
```
15. Use Enum classes for defined values. Ex-
```java
private enum Suit { CLUBS, HEARTS, SPADES, DIAMONDS }
```
16. Constants are in all-caps with underscores. Ex-
```java
private static final long ONE_SECOND = 1000;
private static final long ONE_MINUTE = 60 * ONE_SECOND;
private static final ImmutableList<String> NAMES = ImmutableList.of("Ed", "Ann");
private static final ImmutableMap<String, Integer> AGES = ImmutableMap.of("Ed", 35, "Ann", 32);
private static final Joiner COMMA_JOINER = Joiner.on(','); // because Joiner is immutable
private static final SomeMutableType[] EMPTY_ARRAY = {};
private enum SomeEnum { ENUM_CONSTANT }

// Not constants
private static String nonFinal = "non-final";
private final String nonStatic = "non-static";
private static final Set<String> mutableCollection = new HashSet<String>();
private static final ImmutableSet<SomeMutableType> mutableElements = ImmutableSet.of(mutable);
private static final ImmutableMap<String, SomeMutableType> mutableValues =
    ImmutableMap.of("Ed", mutableInstance, "Ann", mutableInstance2);
private static final Logger logger = Logger.getLogger(MyClass.getName());
private static final String[] nonEmptyArray = {"these", "can", "change"};
```
17. Member variables never have prefixes such as m_, s_, etc.
18. Member variables containing acronyms should contain the lowerCamelCase version. Ex-
```java
shortUrl instead of shortURL
xmlHttpRequest instead of XMLHTTPRequest
customerId instead of customerID
```
19. At a minimum, javadoc should be present for top-level class descriptions, interfaces, and important public APIs.
20. Long-valued integer literals use an uppercase L suffix, never lowercase (to avoid confusion with the digit 1). Ex-
3000000000L rather than 3000000000l
21. Ensure that each method has only one return statement for better code readability and untimely exit.
22. It is always better to extract section of code in another method to improve code readability and reduce complexity of original method.
23. Static members should always be accessed using class name. Ex-
```java
Foo.aStaticMethod();
```
24. Annotate implemented methods of implemented interfaces or extended classes with @Override, but do not add redundant javadoc.
25. Use @Nullable, @Nonnull from javax.annotation where desired in order to avoid redundant null-checking everywhere.
26. It is very rarely correct to do nothing in response to a caught exception. If still no action has to be taken, then support that by a comment. Adder logger or comment, as necessary.
```java
try {
  int i = Integer.parseInt(response);
  return handleNumericResponse(i);
} catch (NumberFormatException ex) {
  // it's not numeric; that's fine, just continue
}

return handleTextResponse(response);
```
27. It is extremely rare to override Object.finalize(). Avoid finalizers. Read "Effective Java" for more details.
28. Always specify types for Generics whenever possible.
29. Add @SupressWarnings("unchecked") whenever there is an unavoidable cast (i.e. using external API). The most common source of "unchecked" warnings is the use of raw types. "unchecked" warnings are issued when an object is accessed through a raw type variable, because the raw type does not provide enough type information to perform all necessary type checks. Ex- 
```java
@SuppressWarnings("unchecked")
private void getJobName() {
    jobData = cacheManager.getData("templateName", Map.class);
}

@SuppressWarnings("unchecked")
void uncheckedGenerics() {
    List words = new ArrayList();
 
    // this causes unchecked warning
    words.add("hello");
}

In this case, it is always better to specify type instead of using @SuppressWarnings annotation. Ex- List<String> words = new ArrayList<>();
```
30. Don't log password in log files.
31. Use @JsonIgnore annotation on variables, whenever necessary.
32. Each switch statement includes a default statement group, even if it contains no code. Exception: A switch statement for an enum type may omit the default statement group, if it includes explicit cases covering all possible values of that type. This enables IDEs or other static analysis tools to issue a warning if any cases were missed.
33. Commenting done using double slash (//) should be above the code instead of inline. Ex-
```java
private String getFirstWord() { 
    // comment here
    return words.get(0);
}
```
34. When writing multi-line comments, use the /* ... */ style if you want automatic code formatters to re-wrap the lines when necessary (paragraph-style). Most formatters don't re-wrap lines in // ... style comment blocks.

### Ordering of class contents
1. Any declared logger as "private static final Logger logger = Logger.getLogger(MyClass.getName());" or better use @Slf4j lombok annotation.
2. All static final members come first, ordered from least to most restrictive access qualifiers (i.e. String constants, static lookup tables, etc.)
3. All other member variables come next, ordered from least to most restrictive access qualifiers.
4. static initialization blocks
5. non-static initialization blocks 
6. main() if it exists
7. Constructor(s) ordered from least to most arguments. Better use lombok annotation.
8. Getters/setters (pairing the getter with the setter). Better use lombok annotation.
9. Abstract methods
10. Member methods ordered from least to most restrictive access qualifiers generally grouped by functionality.  In general, methods in a class should be ordered from highest-level to lowest-level in terms of logic.
11. Inner classes ordered from least to most restrictive access qualifiers.

### JUnit Tests method ordering
1. All @BeforeClass methods
2. All @AfterClass methods
3. All @Before methods (and/or setUp() for JUnit 4 style tests)
4. All @After methods (and/or tearDown() for JUnit 4 style tests)
5. All public test cases
6. All other helper methods

### Javadoc conventions
1. Use (/**) whenever writing javadoc. Ex- 
```java
/**
 * Multiple lines of Javadoc text are written here,
 * wrapped normally...
 */
public int method(String p1) {
}
```
2. Use <p> paragraph tag to separate multiple paragraphs.
```java
/**
 * Lorem ipsum dolor sit amet, consectetur adipiscing elit.
 *
 * <p>Fusce placerat at nisl in molestie. Vestibulum rhoncus, mi ac 
 * viverra sollicitudin.
 *
 * <p>
 * Nulla non mollis leo. Aliquam efficitur lectus eros, sit amet ultrices 
 * ex fringilla at.
 *
 * @return user issued books
 */
public Book[] getBooks(User user) {
    return dao.getBooks(user);
}
```
3. Use <pre> preformatted text tag inside javadoc to avoid code formatting in that block. Ex-
```java
/**
<pre>
   Following scenarios are possible:
   case 1: ABC
   case 2: DEF

   case 1 and case 2 are applicable for merchants.
 </pre>
 */
private void triggerReport() {
}
```
4. Use // @formatter:off and // @formatter:on to disable the code formatting anywhere in code. This has to be configured in IDE. Ex-
```java
// @formatter:off
/**
 * alpha          
 *     beta
 *        gamma
 */
// @formatter:on
private void trigger() {
}
```
5. For more Javadoc tags, refer [wikipedia](https://en.wikipedia.org/wiki/Javadoc).

### Cyclomatic complexity
It is a software metric, used to indicate the complexity of a program. It is a quantitative measure of the number of linearly independent paths through a program's source code. More details to be shared!
