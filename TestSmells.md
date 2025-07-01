# Test Smells

## Assertion Roulette
**Descrição:** Múltiplas asserções sem mensagens descritivas dificultam a identificação do motivo da falha do teste.

### Detecção:
Um método de teste contém **mais de uma declaração de asserção sem uma explicação/mensagem** (parâmetro no método de asserção).

Em Java com JUnit, as asserções são feitas através de métodos cujo nome começa com `assert` (ex: `assertTrue(1 == 1)`, `assertFalse(2 == 3)`, `assertEquals(3, 3)`, etc). Quando uma asserção contém explicação/mensagem, ela sempre é passada como o primeiro parâmetro para o método de asserção (ex: `assertTrue("1 é igual a 1", 1 == 1)`, `assertFalse("2 é diferente de 3", 2 == 3)`, `assertEquals("3 é igual a 3", 3, 3)`).

**Exemplo Java com JUnit:**
```java
@Test
public void test() {
    // ...
    assertTrue(1 == 1); // Sem mensagem
    assertFalse(2 == 3); // Sem mensagem
    assertEquals(3, 3); // Sem mensagem
    // ...
}
```

Em Python com Pytest, as asserções geralmente são feitas através da declaração `assert` (ex: `assert 1 == 1`, `assert 2 == 3`, `assert 3 == 3`). Quando uma asserção contém explicação/mensagem, ela é passada depois do parâmetro da asserção (ex: `assert 1 == 1, "1 é igual a 1"`, `assert 2 == 3, "2 é diferente de 3"`, `assert 3 == 3, "3 é igual a 3"`).

**Exemplo Python com Pytest:**
```python
def test_assertion_roulette():
    # ...
    assert 1 == 1 # Sem mensagem
    assert 2 == 3 # Sem mensagem
    assert 3 == 3 # Sem mensagem
    # ...
```

## Conditional Test Logic
**Descrição:** Condições em testes podem fazer com que partes do código não sejam executadas, resultando em testes incompletos e difíceis de entender.

### Detecção:
Um método de teste que contém **uma ou mais declarações de controle** (ou seja, if, switch, expressão condicional, for, foreach e while).

**Exemplo Java com JUnit:**

```java
@Test
public void test() {
    // ...
    if (condition) { // Condição
        // ...
    }
    // ...
    for (int i = 0; i < 10; i++) { // Laço
        // ...
    }
    // ...
}
```

**Exemplo Python com Pytest:**

```python
def test_conditional_test_logic():
    # ...
    if condition: # Condição
        # ...
    # ...
    for i in range(10): # Laço
        # ...
    # ...
```

## Duplicate Assert
**Descrição:** Testar a mesma condição múltiplas vezes no mesmo método indica má organização do teste. Cada condição diferente deve ter seu próprio método de teste.

### Detecção:
Um método de teste que contém **mais de uma declaração de asserção com os mesmos parâmetros**.

**Exemplo Java com JUnit:**

```java
@Test
public void testDuplicateAssert() {
    // ...
    assertEquals(param1, 1); // Duplicado
    assertEquals(param2, 2);
    assertEquals(param1, 1); // Duplicado
    // ...
}
```

**Exemplo Python com Pytest:**

```python
def test_duplicate_assert():
    # ...
    assert param1 == 1 # Duplicado
    assert param2 == 2
    assert param1 == 1 # Duplicado
    # ...
```

## Empty Test
**Descrição:** Testes vazios passam automaticamente, criando uma falsa sensação de segurança e não detectando problemas no código.

### Detecção:
Um método de teste que **não contém uma única declaração executável**.

**Exemplo Java com JUnit:**

Podemos considerar um Empty Test quando não há nada no método de teste ou quando há somente comentários.

```java
@Test
public void test() {
    // Não há declarações executáveis, somente comentários
}
```

**Exemplo Python com Pytest:**

Podemos considerar um Empty Test quando há somente um `pass` ou somente comentários.

```python
def test_empty_test():
    pass
    # Não há declarações executáveis, somente comentários
```

## Exception Handling
**Descrição:** O tratamento manual de exceções em testes é desnecessário quando o framework de teste já fornece mecanismos para isso.

### Detecção:
Um método de teste que **contém uma declaração de lançamento de exceção (throw em Java ou raise em Python) ou uma cláusula try/catch**.

**Exemplo Java com JUnit:**

```java
@Test
public void test() {
    // ...
    try { // Tratamento de exceção
        // ...
    } catch (Exception e) {
        // ...
    }
    // ...
    throw new Exception("Exceção lançada"); // Lançamento de exceção
}
```

**Exemplo Python com Pytest:**

```python
def test_exception_handling():
    # ...
    try: # Tratamento de exceção
        # ...
    except Exception as e:
        # ...
    # ...
    raise Exception("Exceção lançada") # Lançamento de exceção
```

## Ignored Test
**Descrição:** Testes ignorados aumentam a complexidade do código e o tempo de compilação sem trazer benefícios.

### Detecção:
Um método de teste ou classe que **contém um comando para ignorar o teste** (ex: `@Ignore` em Java ou `@pytest.mark.skip(reason="Teste ignorado")` em Python).

**Exemplo Java com JUnit:**

Há diversas anotações para ignorar um teste em Java com JUnit como: `@Ignore`, `@EnabledIf`, `@DisabledIf`, etc.

```java
@Test
@Ignore
public void test() {
    // ...
}
```

**Exemplo Python com Pytest:**

Há diversas maneiras de ignorar um teste em Python com Pytest como: `@pytest.mark.skip()`, `@pytest.mark.skipif(condition)`, etc.

```python
@pytest.mark.skip(reason="Teste ignorado")
def test_ignored_test():
    # ...
}
```

## Magic Number Test
**Descrição:** Números mágicos em testes dificultam a compreensão do propósito do teste e devem ser substituídos por constantes nomeadas.

### Detecção:
Um **método de asserção que contém um literal numérico como argumento** (ex: `assertEquals(1, 1)`, `assert 1 == 1`, etc).

É importante destacar que este test smell busca números mágicos em métodos de asserção e não no restante do código de teste. Além disso, este test smell não considera outros tipos de dados como strings, booleanos, etc.

**Exemplo Java com JUnit:**

```java
@Test
public void test() {
    // ...
    assertEquals(1, 1); // Número mágico
    // ...
}
```


**Exemplo Python com Pytest:**

```python
def test_magic_number():
    # ...
    assert 1 == 1 # Número mágico
    # ...
```

## Redundant Print
**Descrição:** Impressões em testes são desnecessárias pois os testes são executados automaticamente sem intervenção humana.

### Detecção:
Um método de teste que **invoca os métodos de impressão** (ex: `print`, `println`, `printf` ou `write` da classe System em Java ou `print` ou `pprint` em Python).

**Exemplo Java com JUnit:**

```java
@Test
public void test() {
    // ...
    System.out.println("Teste"); // Impressão
    // ...
}
```

**Exemplo Python com Pytest:**

```python
def test_redundant_print():
    # ...
    print("Teste") # Impressão
    # ...
```

## Sleepy Test
**Descrição:** Dormir em testes pode causar resultados inconsistentes devido a diferenças no tempo de processamento entre ambientes.

### Detecção:
Um método de teste que **invoca o método de suspensão de thread** (ex: `Thread.sleep()` em Java ou `time.sleep()` ou `asyncio.sleep()` em Python).

**Exemplo Java com JUnit:**

```java
@Test
public void test() {
    // ...
    Thread.sleep(1000); // Suspensão de thread
    // ...
}
```

**Exemplo Python com Pytest:**

```python
def test_sleepy_test():
    # ...
    time.sleep(1) # Suspensão de thread
    # ...
```

## Unknown Test
**Descrição:** Testes sem asserções não verificam efetivamente o comportamento esperado, dificultando a compreensão do seu propósito.

### Detecção: 
Um método de teste que **não contém uma única declaração de asserção**.

**Exemplo Java com JUnit:**

```java
@Test
public void test() { // Teste sem asserção
    SystemManager systemManager = new SystemManager();
    // ...
    systemManager.doSomething();
    // ...
}
```

**Exemplo Python com Pytest:**

```python
def test_unknown_test(): # Teste sem asserção
    systemManager = SystemManager()
    # ...
    systemManager.doSomething()
    # ...
```
