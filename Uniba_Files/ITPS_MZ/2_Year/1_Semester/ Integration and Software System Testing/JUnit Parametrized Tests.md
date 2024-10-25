# Parametrized Tests


I test parametrizzati sono dichiarati come metodi con l’annotazione **@ParameterizedTest**.

```java
@ParameterizedTest
@ValueSource(ints = { 1, 2, 3 })
void testWithValueSource(int argument) {
   assertTrue(argument > 0 && argument < 4);
}
```

Bisogna dichiarare almeno una fonte che fornirà gli argomenti per ogni invocazione, che saranno poi consumati nel metodo di test.

---

- I test **Unhappy Path** sono spesso fatti con valori `NULL` o `Empty`.

Questi valori sono usati nei test parametrizzati per verificare casi limite e/o il comportamento del software quando riceve un input invalido.

<br>

### **Test con @NullSource e @EmptySource**

**@NullSource** fornisce un singolo argomento `null` al metodo annotato con @ParameterizedTest.

```java
@ParameterizedTest
@NullSource
void testWithNullSource(String argument) {
    assertNull(argument);
}
```

**@EmptySource** fornisce un singolo argomento vuoto per parametri dei seguenti tipi:
- *Stringhe, Liste, Set, Map, Array primitivi (es. int[], char[], etc.), Array di oggetti (es. Integer[], String[], etc.)*

```java
@ParameterizedTest
@EmptySource
void testWithEmptySource(List<String> argument) {
    assertTrue(argument.isEmpty());
}
```

Usato per fornire un argomento vuoto a vari tipi, tra cui **String**, **List**, **Set**, **Map**, array di tipi primitivi o oggetti.

---

### **Annotazioni speciali**

**@NullAndEmptySource**: combina i test **@NullSource** e **@EmptySource** in un'unica notazione. 

Si usano i tag separati solo se non si vuole eseguire entrambi i test.

> Se i risultati per `NULL` e `Empty` sono tali che un test fallisca e uno no, allora i tag devono essere separati.

```java
@ParameterizedTest
@NullAndEmptySource
void testWithNullAndEmptySource(String argument) {
   if (argument == null) {
       assertNull(argument);
    } else {
        assertTrue(argument.isEmpty());
    }
}
```

---

## **@MethodSource**

Il tag **@MethodSource** permette di riferirsi a uno o più metodi, detti *“factory methods”*, che generano un flusso di argomenti `Stream<Arguments>`. 

> Ogni set di argomenti nel flusso sarà il set di argomenti per un metodo di test parametrizzato.

### Esempio di utilizzo di @MethodSource

Se si dichiara un metodo **range()**, lo si può richiamare prima del test con **@MethodSource("range")**.  
Verrà eseguito il metodo, e il risultato sarà dato in input al test.

```java
@ParameterizedTest
@MethodSource("stringProvider")
void testWithExplicitLocalMethodSource(String argument) {
    assertNotNull(argument);
}
```

---

## **@CsvSource**

**@CsvSource** esprime la lista di argomenti come valori separati da virgole, come un file CSV.

```java
@ParameterizedTest(name = "[{index}] {arguments}")
@CsvSource(useHeadersInDisplayName = true, textBlock = """
    FRUIT,         RANK
    apple,         1
    banana,        2
    'lemon, lime', 0xF1
    strawberry,    700_000
    """)
void testWithCsvSource(String fruit, int rank) {
    assertNotNull(fruit);
    assertNotEquals(0, rank);
}
```

### Proprietà del tag @CsvSource

1. Ogni stringa rappresenta un record di un file CSV, e quindi un’invocazione del test.
2. Per default, se ci sono spazi tra i valori, questi vengono eliminati.  
   Per mantenerli, si deve impostare l'attributo:
   >**ignoreLeadingAndTrailingWhiteSpace = false**.
4. Il primo record di valori può essere usato per identificare gli header del file CSV.  
   Si può fare con l'attributo:
   > **useHeadersInDisplayName = true**
   
   ignorando gli header con **numLinesToSkip = 1** per saltare la prima riga.

---

## **Nome dei test parametrizzati**

Se vicino al tag **@ParameterizedTest** si specificano i valori, come:

```java
@ParameterizedTest(name = "[{index}] {arguments}")
```

durante l’esecuzione del test si avranno più chiari i valori usati.

---



