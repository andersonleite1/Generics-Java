# Tutorial de exemplo de genéricos Java - Método genérico, classe, interface

___


**O Java Genrics** é um dos recursos mais importantes introduzidos no Java 5.

Se você trabalha com Java Collections e com a versão 5 ou superior, tenho certeza de que você o usou.

**Os genéricos em Java** com classes de coleção são muito fáceis, mas oferecem muito mais recursos do que apenas criar o tipo de coleção.

Vamos tentar aprender os recursos dos genéricos neste artigo. Às vezes, entender os genéricos pode ficar confuso se seguirmos palavras de jargão, então eu tentaria mantê-lo simples e fácil de entender.

**Veremos abaixo os tópicos sobre genéricos em java.**

1. **Genéricos em Java**
2. **Classe genérica Java**
3. **Interface genérica Java**
4. **Tipo genérico Java**
5. **Método genérico Java**
6. **Parâmetros de tipo limitado de genéricos Java**
7. **Genéricos e herança de Java**
8. **Classes genéricas e subtipagem Java**
9. **Curingas Java Generics**

	9.1) *Curinga Java Generics Superior Limitada*
	
	9.2) *Curinga não genérico Java Generics*
	
	9.3) *Curinga Java Generics com limite inferior*
	
10. **Subtipagem usando curinga genérica**
11. **Eliminação de tipo genérico Java**
12. **Perguntas frequentes sobre genéricos**


### 1. Genéricos em Java

Os genéricos foram adicionados no Java 5 para fornecer a **verificação do tipo em tempo de compilação** e a remoção de riscos `ClassCastExceptioncomuns` ao trabalhar com classes de coleta. Toda a estrutura da coleção foi reescrita para usar genéricos para segurança de tipo. Vamos ver como os genéricos nos ajudam a usar as classes de coleção com segurança.

```
List list = new ArrayList();
list.add("abc");
list.add(new Integer(5)); //OK

for(Object obj : list){
	//conversão de tipo levando a ClassCastException em tempo de execução
    String str=(String) obj; 
}
```

O código acima compila bem, mas lança ClassCastException em tempo de execução porque estamos tentando converter Object na lista para String, enquanto um dos elementos é do tipo Integer. Após o Java 5, usamos classes de coleção como abaixo.

```
List<String> list1 = new ArrayList<String>(); // java 7 ? List<String> list1 = new ArrayList<>(); 
list1.add("abc");
//list1.add(new Integer(5)); //erro ao compilar

for(String str : list1){
     //nenhum tipo de conversão é necessário, evita ClassCastException
}
```

Observe que no momento da criação da lista, especificamos que o tipo de elementos na lista será String. Portanto, se tentarmos adicionar qualquer outro tipo de objeto na lista, o programa lançará um erro em tempo de compilação. Observe também que, no loop for, não precisamos da conversão de tipo do elemento na lista, removendo a ClassCastException em tempo de execução.


### 2. Classe genérica Java

Podemos definir nossas próprias classes com o tipo genérico. Um tipo genérico é uma classe ou interface que é parametrizada sobre tipos. Usamos colchetes angulares (<>) para especificar o parâmetro type.

Para entender o benefício, digamos que temos uma classe simples como:

```
package com.journaldev.generics;

public class GenericsTypeOld {

	private Object t;

	public Object get() {
		return t;
	}

	public void set(Object t) {
		this.t = t;
	}

        public static void main(String args[]){
		GenericsTypeOld type = new GenericsTypeOld();
		type.set("Pankaj"); 
		String str = (String) type.get(); //tipo de conversão, propenso a erros e pode causar ClassCastException
	}
}
```

Observe que, ao usar esta classe, precisamos usar conversão de tipo e ela pode produzir ClassCastException em tempo de execução. Agora usaremos a classe genérica java para reescrever a mesma classe, como mostrado abaixo.

```
package com.journaldev.generics;

public class GenericsType<T> {

	private T t;
	
	public T get(){
		return this.t;
	}
	
	public void set(T t1){
		this.t=t1;
	}
	
	public static void main(String args[]){
		GenericsType<String> type = new GenericsType<>();
		type.set("Pankaj"); //valid
		
		GenericsType type1 = new GenericsType(); //raw type
		type1.set("Pankaj"); //válidp
		type1.set(10); //suporte válido e autoboxing
	}
}
```


Observe o uso da classe `GenericsType` no método principal. Não precisamos fazer conversão de tipo e podemos remover `ClassCastException` em tempo de execução. Se não fornecermos o tipo no momento da criação, o compilador produzirá um aviso de que `GenericsType` é um tipo bruto.

As referências ao tipo genérico `GenericsType <T>` devem ser parametrizadas. Quando não fornecemos o tipo, o tipo se torna `Object` e, portanto, está permitindo os objetos `String` e `Integer`. Porém, devemos sempre tentar evitar isso, pois teremos que usar conversão de tipo enquanto trabalhamos no tipo bruto que pode produzir erros de tempo de execução.
> Dica : podemos usar a `@SuppressWarnings("rawtypes")`anotação para suprimir o aviso do compilador, consulte o [tutorial sobre anotações em java](https://www.journaldev.com/721/java-annotations).

Observe também que ele suporta [java autoboxing](https://www.journaldev.com/1005/autoboxing-java).


### 3. Interface genérica Java

[Interface comparable]() é um ótimo exemplo de genéricos em interfaces e está escrita como:

```
package java.lang;
import java.util.*;

public interface Comparable<T> {
    public int compareTo(T o);
}
```


De maneira semelhante, podemos criar interfaces genéricas em java. Também podemos ter vários parâmetros de tipo, como na interface do mapa. Novamente, também podemos fornecer valor parametrizado para um tipo parametrizado, por exemplo, `new HashMap<String, List<String>>();`é válido.

### 4. Tipo genérico Java

A convenção de nomenclatura de tipo genérico Java nos ajuda a entender o código facilmente e ter uma convenção de nomenclatura é uma das melhores práticas da linguagem de programação Java. Portanto, os genéricos também vêm com suas próprias convenções de nomenclatura. Normalmente, os nomes dos parâmetros de tipo são letras maiúsculas e simples, para torná-lo facilmente distinguível das variáveis ​​java. Os nomes dos parâmetros de tipo mais usados ​​são:

+ Elemento E (usado extensivamente pelo [Java Collections  Framework](https://www.journaldev.com/1260/collections-in-java-tutorial) , por exemplo, `ArrayList, Set` etc.)
+ Chave K (usada no mapa)
+ N - Número
+ Tipo T
+ V - Valor (usado no mapa)
+ S, U, V etc. - 2º, 3º, 4º tipos

### 5. Método Genérico Java

Às vezes, não queremos que toda a classe seja parametrizada; nesse caso, podemos criar o método genérico java. Como o construtor é um tipo especial de método, também podemos usar o tipo genérico nos construtores.

Aqui está uma classe que mostra um exemplo de método genérico java.

```
package com.journaldev.generics;

public class GenericsMethods {

	//Método genérico Java
	public static <T> boolean isEqual(GenericsType<T> g1, GenericsType<T> g2){
		return g1.get().equals(g2.get());
	}
	
	public static void main(String args[]){
		GenericsType<String> g1 = new GenericsType<>();
		g1.set("Pankaj");
		
		GenericsType<String> g2 = new GenericsType<>();
		g2.set("Pankaj");
		
		boolean isEqual = GenericsMethods.<String>isEqual(g1, g2);
		//declaração acima pode ser escrita simplesmente como
		isEqual = GenericsMethods.isEqual(g1, g2);
		//Esse recurso, conhecido como inferência de tipo, permite chamar um método genérico como um método comum, sem especificar um tipo entre colchetes angulares.
		//O compilador inferirá o tipo necessário
	}
}
```

Observe a `isEqual` assinatura do método mostrando sintaxe para tipo de uso de genéricos nos métodos. Além disso, observe como usar esses métodos em nosso programa java. Podemos especificar o tipo ao chamar esses métodos ou podemos invocá-los como um método normal. O compilador Java é inteligente o suficiente para determinar o tipo de variável a ser usada, esse recurso é chamado de **inferência de tipo**.

### 6. Parâmetros do tipo vinculado de genéricos Java

Suponha que desejamos restringir o tipo de objetos que podem ser usados ​​no tipo parametrizado, por exemplo, em um método que compara dois objetos e queremos garantir que os objetos aceitos sejam comparáveis. Para declarar um parâmetro de tipo delimitado, liste o nome do parâmetro de tipo, seguido pela palavra-chave extends, seguida pelo seu limite superior, semelhante ao método abaixo.

```
public static <T extends Comparable<T>> int compare(T t1, T t2){
		return t1.compareTo(t2);
}
```

A invocação desses métodos é semelhante ao método ilimitado, exceto que, se tentarmos usar qualquer classe que não seja Comparável, ocorrerá um erro em tempo de compilação.

Os parâmetros do tipo delimitado podem ser usados ​​com métodos, bem como classes e interfaces.

O Java Generics também suporta vários limites, ou seja, `<T extends A & B & C>`. Nesse caso, `A` pode ser uma interface ou classe. Se `A` é classe, `B` e `C` devem ser uma interface. Não podemos ter mais de uma classe em vários limites.

### 7. Genéricos e herança em Java

Sabemos que a [herança Java](https://www.journaldev.com/644/inheritance-java-example) nos permite atribuir uma variável A a outra variável B se A for subclasse de B. Portanto, podemos pensar que qualquer tipo genérico de A pode ser atribuído ao tipo genérico de B, mas não é o caso. Vamos ver isso com um programa simples.

```
package com.journaldev.generics;

public class GenericsInheritance {

	public static void main(String[] args) {
		String str = "abc";
		Object obj = new Object();
		obj=str; //funciona porque String é um objeto, herança em java
		
		MyClass<String> myClass1 = new MyClass<String>();
		MyClass<Object> myClass2 = new MyClass<Object>();
		//myClass2=myClass1; //erro de compilação desde MyClass<String> não é um MyClass<Object>
		obj = myClass1; // MyClass<T> é objeto
	}
	
	public static class MyClass<T>{}

}
```

Não temos permissão para atribuir a variável `MyClass <>` à variável `MyClass <Object>` porque elas não estão relacionadas, na verdade o pai `MyClass <T>` é Object.

### 8. Classes e subtipagem genéricas de Java

Podemos subtipo uma classe ou interface genérica estendendo-a ou implementando-a. O relacionamento entre os parâmetros de tipo de uma classe ou interface e os parâmetros de tipo de outra é determinado pelas cláusulas estende e implementa.

Por exemplo, *`ArrayList <E>` implementa a `Lista <E>` que estende a `Coleção <E>`, portanto `ArrayList <String>` é um subtipo de `Lista <String>` e a `Lista <String>` é o subtipo da `Coleção <String>`*.

O relacionamento de subtipagem é preservado, desde que não alteremos o argumento type, abaixo mostra um exemplo de vários parâmetros de tipo.


```
interface MyList<E,T> extends List<E>{
}
```

Os subtipos de `Lista <>` podem ser `MyList <String, Object>`, `MyList <String, Inteiro>` e assim por diante.

### 9. Curingas Java Generics

O ponto de interrogação (?) É o curinga nos genéricos e representa um tipo desconhecido. O curinga pode ser usado como o tipo de parâmetro, campo ou variável local e, às vezes, como tipo de retorno. Não podemos usar curingas enquanto invocamos um método genérico ou instanciamos uma classe genérica. Nas seções a seguir, aprenderemos sobre curingas com limite superior, curingas com limite inferior e captura de curinga.


#### 9.1) Curinga superior genérica de Java

Os curingas com limite superior são usados ​​para relaxar a restrição do tipo de variável em um método. Suponha que desejemos escrever um método que retorne a soma dos números na lista; portanto, nossa implementação será algo assim.

```
public static double sum(List<Number> list){
		double sum = 0;
		for(Number n : list){
			sum += n.doubleValue();
		}
		return sum;
}
```

Agora, o problema com a implementação acima é que ele não funcionará com a Lista de números inteiros ou duplos, porque sabemos que a `lista <Integer>` e a `lista <Double>` não estão relacionadas, é quando um curinga com limite superior é útil. Usamos o curinga genérico com a palavra-chave **`extends`** e a classe ou interface do **limite superior** que nos permitirá transmitir argumentos dos tipos de limite superior ou de subclasses.

A implementação acima pode ser modificada como o programa abaixo.

```
package com.journaldev.generics;

import java.util.ArrayList;
import java.util.List;

public class GenericsWildcards {

	public static void main(String[] args) {
		List<Integer> ints = new ArrayList<>();
		ints.add(3); ints.add(5); ints.add(10);
		double sum = sum(ints);
		System.out.println("Sum of ints="+sum);
	}

	public static double sum(List<? extends Number> list){
		double sum = 0;
		for(Number n : list){
			sum += n.doubleValue();
		}
		return sum;
	}
}
```
É semelhante a escrever nosso código em termos de interface, no método acima, podemos usar todos os métodos da classe Número limite superior. Observe que, na lista com limites superiores, não temos permissão para adicionar nenhum objeto à lista, exceto nulo. Se tentarmos adicionar um elemento à lista dentro do método sum, o programa não será compilado.


#### 9.2) Curinga Java Generics Unbounded

Às vezes, temos uma situação em que queremos que nosso método genérico trabalhe com todos os tipos; nesse caso, um curinga ilimitado pode ser usado. É o mesmo que usar `<? extends Object>`.

```
public static void printData(List<?> list){
	for(Object obj : list){
		System.out.print(obj + "::");
	}
}
```
Podemos fornecer `List <>` ou `List <Integer>` ou qualquer outro tipo de argumento da lista de objetos para o método `printData`. Semelhante à lista de limites superiores, não temos permissão para adicionar nada à lista.


#### 9.3) Curinga genérico Java com limite inferior

Suponha que desejemos adicionar números inteiros a uma lista de números inteiros em um método, podemos manter o tipo de argumento como `List <Integer>`, mas ele será vinculado a números inteiros, enquanto `List <Number>` e `List <Object>` também podem conter números inteiros, portanto podemos usar um curinga de limite inferior para conseguir isso. Usamos curinga genérica (?) Com **super** palavra - chave e classe de limite inferior para conseguir isso.

Podemos passar um limite inferior ou qualquer super tipo de limite inferior como argumento; nesse caso, o compilador java permite adicionar tipos de objetos com limite inferior à lista.

```
public static void addIntegers(List<? super Integer> list){
		list.add(new Integer(50));
	}
```

### 10. Subtipagem usando curinga genérica

```
List<? extends Integer> intList = new ArrayList<>();
List<? extends Number>  numList = intList;  // OK. List<? extends Integer> é um subtipo de List<? extends Number>
```

### 11. Eliminação de tipo genérico Java

Os genéricos em Java foram adicionados para fornecer verificação de tipo em tempo de compilação e não podem ser utilizados em tempo de execução; portanto, o compilador java usa o recurso de **apagamento de tipo** para remover todo o código de verificação de tipo genérico no código de bytes e inserir a conversão de tipo, se necessário. O apagamento de tipo garante que nenhuma nova classe seja criada para tipos parametrizados; consequentemente, os genéricos não sofrem sobrecarga de tempo de execução.

Por exemplo, se tivermos uma classe genérica como abaixo;

```
public class Test<T extends Comparable<T>> {

    private T data;
    private Test<T> next;

    public Test(T d, Test<T> n) {
        this.data = d;
        this.next = n;
    }

    public T getData() { return this.data; }
}
```

O compilador Java substitui o parâmetro do tipo limitado T pela primeira interface vinculada, Comparable, conforme o código abaixo:

```
public class Test {

    private Comparable data;
    private Test next;

    public Node(Comparable d, Test n) {
        this.data = d;
        this.next = n;
    }

    public Comparable getData() { return data; }
}
```

### 12. Perguntas frequentes sobre genéricos

#### 12.1) Por que usamos genéricos em Java?

Os genéricos fornecem uma forte verificação do tipo em tempo de compilação e reduzem o risco de `ClassCastException` e a conversão explícita de objetos.

#### 12.2) O que é T em genéricos?

Usamos `<T>` para criar uma classe, interface e método genéricos. O *T* é substituído pelo tipo real quando o usamos.

#### 12.3) Como os Genéricos funcionam em Java?

O código genérico garante a segurança do tipo. O compilador usa apagamento de tipo para remover todos os parâmetros de tipo no tempo de compilação para reduzir a sobrecarga no tempo de execução.

### 13. Genéricos em Java - leituras adicionais

+ Os genéricos não suportam a sub-digitação; portanto `List<Number> numbers = new ArrayList<Integer>();`, não serão compilados; saiba [por que os genéricos não suportam a sub-digitação](https://www.journaldev.com/1330/java-collections-interview-questions-and-answers#generics-sub-typing).

+ Não podemos criar uma matriz genérica; portanto `List<Integer>[] array = new ArrayList<Integer>[10]`, não iremos compilar; leia [por que não podemos criar uma matriz genérica?](https://www.journaldev.com/1330/java-collections-interview-questions-and-answers#generics-array).

Isso é tudo para **genéricos em java**, java genéricos é um tópico realmente vasto e requer muito tempo para entender e usá-lo efetivamente. Este post aqui é uma tentativa de fornecer detalhes básicos de genéricos e como podemos usá-lo para estender nosso programa com segurança de tipo.

[**Referência JournalDev**](https://www.journaldev.com/1663/java-generics-example-method-class-interface)
