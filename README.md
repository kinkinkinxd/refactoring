# Refactoring
## Readability
From Thornthep's Readablity code: https://github.com/Raikirieiei/PA4-Readability

**IsVowel methods**

In the ```/src/readability/CountSyllable.java``` class

https://github.com/Raikirieiei/PA4-Readability/blob/master/src/readability/CountWord.java 
 
and in ```/src/readability/CountWord.java``` class

https://github.com/Raikirieiei/PA4-Readability/blob/master/src/readability/CountSyllable.java

consider this code
```
private boolean IsVowel(String alphabet) {
     return (alphabet.equalsIgnoreCase("a") || alphabet.equalsIgnoreCase("e") || alphabet.equalsIgnoreCase("i")
     || alphabet.equalsIgnoreCase("o") || alphabet.equalsIgnoreCase("u"));
}
```
- Refactoring Signs:
    - many calls string.equalsIgnoreCase().
    - equation is long and complex.
    - duplicate method in two class.
    
- Refactoring: introduce named constant for the vowels

```
private boolean IsVowel(String alphabet){
    final String VOWELS = "aeiouAEIOU";
    return VOWELS.contains(alphabet);
}
```
- Refactoring: use polymorphism by extract superclass to reduce duplicate codes.
```
public class Count {
     public boolean IsVowel(String alphabet){
        final String VOWELS = "aeiouAEIOU";
        return VOWELS.contains(alphabet);
}

public class CountSyllable extends Count implements CountStrategy {
...
}

public class CountWord extends Count implements CountStrategy {
...
}
```
- Refactoring: rename method.```IsVowel```violates the Java naming convention.
```
public boolean isVowel(String word){
```
**IndexCalculator methods**

In the ```src/readability/FleschReadability.java```

https://github.com/Raikirieiei/PA4-Readability/blob/master/src/readability/FleschReadability.java

consider this code:
```
public double IndexCalculator(String source){
        ReadAndCount read = new ReadAndCount();
        read.Read(source);
        double words = read.getWord();
        double syllables = read.getSyllable();
        double sentences = read.getSentence();
        double readabilityIndex = (206.835 - 84.6*(syllables/words) - 1.015*(words/sentences)); // Formula to count index
        return readabilityIndex;
```
- Refactoring Signs:
    - hard to understand the Flesch index formula
    - have unnecessary temporary variable that’s assigned the result of a simple expression
    
- Refactoring: Replace the references to the variable with the expression itself.
```
public double IndexCalculator(String source){
        ReadAndCount read = new ReadAndCount();
        read.Read(source);
        double words = read.getWord();
        double syllables = read.getSyllable();
        double sentences = read.getSentence();
        return readabilityIndex(syllables, words, sentences);
    
double readabilityIndex(syllables, words, sentences) {
        return (206.835 - 84.6*(syllables/words) - 1.015*(words/sentences)); 
}
```
- Refactoring: rename method.```IndexCalculator``` violates the Java naming convention.
```
public double indexCalculator(String source){
```

