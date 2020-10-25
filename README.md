# Refactoring
## CountWord and CountSyllable
From Thornthep's Readablity code: https://github.com/Raikirieiei/PA4-Readability

**IsVowel methods**

In the ```/src/readability/CountSyllable.java``` class

https://github.com/Raikirieiei/PA4-Readability/blob/master/src/readability/CountWord.java 
 
and in ```/src/readability/CountWord.java``` class

https://github.com/Raikirieiei/PA4-Readability/blob/master/src/readability/CountSyllable.java

consider this code:
```java
private boolean IsVowel(String alphabet) {
     return (alphabet.equalsIgnoreCase("a") || alphabet.equalsIgnoreCase("e") || alphabet.equalsIgnoreCase("i")
     || alphabet.equalsIgnoreCase("o") || alphabet.equalsIgnoreCase("u"));
}
```
- Refactoring Signs:
    - many calls string.equalsIgnoreCase().
    - equation is long and complex.
    - duplicate method in two class.
    
- Refactoring: introduce named constant for the vowels.

```java
private boolean IsVowel(String alphabet){
    final String VOWELS = "aeiouAEIOU";
    return VOWELS.contains(alphabet);
}
```
- Refactoring: use polymorphism by extract superclass to reduce duplicate codes.
```java
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
```java
public boolean isVowel(String word){
```
## FleschReadability
**IndexCalculator methods**

In the ```src/readability/FleschReadability.java```

https://github.com/Raikirieiei/PA4-Readability/blob/master/src/readability/FleschReadability.java

consider this code:
```java
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
    - hard to understand the Flesch index formula.
    - have unnecessary temporary variable that’s assigned the result of a simple expression.
    
- Refactoring: replace the references to the variable with the expression itself.
```java
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
```java
public double indexCalculator(String source){
```
In the same class there is a ```GradeCalculator``` method. that too hard to understand.
```java
public String GradeCalculator(double index){
    if (index > 100)    return "4th grade student (elementary school) ";
    else if (90 <= index && index <= 100) return "5th grade student";
    else if (80 <= index && index <= 90) return "6th grade student";
    else if (70 <= index && index <= 80) return "7th grade student";
    else if (65 <= index && index <= 70) return "8th grade student";
    else if (60 <= index && index <= 65) return "9th grade student";
    else if (50 <= index && index <= 60) return "High school student";
    else if (30 <= index && index <= 50) return "College student";
    else if (0 <= index && index <= 30) return "College graduate";
    else return "Advance degree graduate";
}
```
- Refactoring Signs:
    - the "if" expression is long and complex.
    - too many hard code.

- Refactoring: create enum class to collect all readability level.
```java
public enum ReadabilityLevel {
    INDEX_100("4th grade student (elementary school)", 100),
    INDEX_90("5th grade student", 90),
    INDEX_80("6th grade student", 80),
    INDEX_70("7th grade student", 70),
    INDEX_65("8th grade student", 65),
    INDEX_60("9th grade student", 60),
    INDEX_50("High school student", 50),
    INDEX_30("College student", 30),
    INDEX_0("College graduate", 0),
    INDEX_MINUS("Advance degree graduate", -1);


    private final String levelText;
    private final int levelValue;


    private ReadabilityLevel(String levelText, int levelValue) {
        this.levelText = levelText;
        this.levelValue = levelValue;
    }

    public String getLevelText() {
        return this.levelText;
    }

    public static ReadabilityLevel getLevel(double index) {
        for (ReadabilityLevel g : ReadabilityLevel.values()) {
            if (index > g.levelValue) {
                return g;
            }
        }
        return INDEX_MINUS;
    }
}
```
- Refactoring: make GradeCalculator in FleschReadability class use Grade enum.
```java
public String GradeCalculator(double index){
          return ReadabilityLevel.getLevel(index).getLevelText();
    }
```
- Refactoring: rename method.```GradeCalculator``` violates the Java naming convention.
```java
public String gradeCalculator(double index){
```
