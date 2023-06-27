## Como escolher o melhor Garbage Collector para a aplicação utilizando o Java 17

### A partir do Java 9, o Garbage Collector padrão é o G1 (Garbage-First).

Pode-se definir o coletor de lixo desejado usando a opção -XX:+Use[GCName] na linha de comando ao iniciar a aplicação Java. 

**Os GCs disponíveis são:**

| Garbage Collector | Descrição | Comando para habilitar |
| --- | --- | --- |
| **Serial Garbage Collector** | Essa é a implementação mais simples do GC, pois basicamente trabalha com um único thread. Como resultado, essa implementação de GC congela todos os threads da aplicação quando é executada. Portanto, não é uma boa ideia usá-lo em aplicações multi-thread, como ambientes de servidor. | _**`java -XX:+UseSerialGC -jar Application.java`**_ |
| **Parallel Garbage Collector** | É o GC padrão da JVM e, às vezes, chamado de Throughput Collectors. Ao contrário do Serial Garbage Collector, ele usa vários threads para gerenciar o espaço do heap, mas também congela outros threads da aplicação durante a execução do GC. | `java -XX:+UseParallelGC -jar Application.java` |
| **CMS Garbage Collector** | A implementação do Concurrent Mark Sweep (CMS) usa vários threads de coletor de lixo para a coleta de lixo. É projetado para aplicações que preferem pausas mais curtas na coleta de lixo e podem compartilhar recursos do processador com o coletor de lixo enquanto a aplicação está em execução. | _**`java -XX:+UseConcMarkSweepGC -jar Application.java`**_ |
| **G1 Garbage Collector** | O G1 é uma coleta de lixo paralela, concorrente e com pausas previsíveis. É projetado para aplicações que precisam de grandes quantidades de memória heap (mais de 4GB). | _**`java -XX:+UseG1GC -jar Application.java`**_ |
| **Z Garbage Collector** | O ZGC é uma coleta de lixo escalável, com pausas previsíveis e baixa sobrecarga. Ele é projetado para aplicações que precisam de grandes quantidades de memória heap e/ou tem requisitos rigorosos de tempo de pausa. | _**`java -XX:+UseZGC -jar Application.java`**_ |

Cada um desses coletores tem prós e contras e pode ser mais adequado para certas situações do que outros. Por exemplo, o G1 GC é melhor para aplicações com metas de tempo de pausa muito rigorosas e uma taxa de transferência geral modesta, enquanto o ZGC é bom para aplicações que precisam de tempos de pausa muito curtos e têm grandes quantidades de memória​1​.

Você pode escolher um coletor de lixo específico usando opções de linha de comando ao iniciar sua aplicação. Por exemplo, para usar o G1 GC, você forneceria -XX:+UseG1GC na linha de comando. Para o ZGC, você forneceria -XX:+UnlockExperimentalVMOptions -XX:+UseZGC​1​​2​.

As opções do Garbage Collector podem ser especificadas diretamente na linha de comando quando você inicia um aplicativo Java, como mencionado anteriormente. No entanto, se você quiser configurar a opção do Garbage Collector em um arquivo de configuração ou como uma variável de ambiente, aqui estão algumas maneiras de fazê-lo:

## **Arquivo de configuração:**
Você pode criar um arquivo de configuração de opções JVM (por exemplo, jvm.options) que contém todas as opções de linha de comando desejadas para a JVM. Cada opção é escrita em uma linha separada. Por exemplo:

```
-XX:+UseSerialGC
```
 O arquivo **jvm.options** pode ser colocado em um diretório específico do seu aplicativo e você pode referenciá-lo quando iniciar seu aplicativo. Como isso é feito depende do seu ambiente de aplicativo específico.

_Por exemplo, se você estiver usando o ElasticSearch, pode colocar o arquivo **jvm.options** no diretório config do ElasticSearch e o ElasticSearch o lerá ao iniciar._

## **Variável de ambiente:**
Você também pode definir a opção do Garbage Collector como uma variável de ambiente. No entanto, isso pode variar dependendo do sistema operacional.

Por exemplo, no Linux ou no macOS, você pode adicionar a seguinte linha ao seu arquivo **~/.bashrc** ou **~/.bash_profile**:

**```
export JAVA_OPTS="-XX:+UseSerialGC"
```
**
E então, ao iniciar seu aplicativo Java, você pode referenciar essa variável de ambiente:

```
**java $JAVA_OPTS -jar seuAplicativo.jar**
```

No Windows, você pode definir uma variável de ambiente no Painel de Controle:

1. Abra o Painel de Controle -> Sistema e Segurança -> Sistema -> Configurações avançadas do sistema.
2. Clique em "Variáveis de ambiente...".
3. Clique em "Novo..." sob "Variáveis de usuário para [seu nome de usuário]".
4. Digite JAVA_OPTS para o nome da variável e -XX:+UseSerialGC para o valor da variável.
5. Clique em "OK" em todas as janelas abertas.
6. Depois disso, a variável de ambiente JAVA_OPTS pode ser usada ao iniciar o aplicativo Java.

As configurações de linha de comando têm precedência sobre as variáveis de ambiente. Portanto, se a opção do Garbage Collector for definida tanto na linha de comando quanto como uma variável de ambiente, a opção definida na linha de comando será usada.

As opções de configuração da JVM pode depender do seu ambiente de aplicação específico e da forma como o aplicativo é iniciado. 
Consultar a documentação do ambiente de aplicação para obter as melhores práticas para configurar as opções da JVM.
