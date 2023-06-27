# Como escolher o melhor garbage Collector para a aplicação utilizando o Java 17

## A partir do Java 9, o Garbage Collector padrão é o G1 (Garbage-First).

Pode-se definir o coletor de lixo desejado usando a opção -XX:+Use[GCName] na linha de comando ao iniciar a aplicação Java. 

**Os GCs disponíveis são:**

| Garbage Collector | Descrição | Comando para habilitar |
| --- | --- | --- |
| Serial Garbage Collector | Essa é a implementação mais simples do GC, pois basicamente trabalha com um único thread. Como resultado, essa implementação de GC congela todos os threads da aplicação quando é executada. Portanto, não é uma boa ideia usá-lo em aplicações multi-thread, como ambientes de servidor. | `java -XX:+UseSerialGC -jar Application.java` |
| Parallel Garbage Collector | É o GC padrão da JVM e, às vezes, chamado de Throughput Collectors. Ao contrário do Serial Garbage Collector, ele usa vários threads para gerenciar o espaço do heap, mas também congela outros threads da aplicação durante a execução do GC. | `java -XX:+UseParallelGC -jar Application.java` |
| CMS Garbage Collector | A implementação do Concurrent Mark Sweep (CMS) usa vários threads de coletor de lixo para a coleta de lixo. É projetado para aplicações que preferem pausas mais curtas na coleta de lixo e podem compartilhar recursos do processador com o coletor de lixo enquanto a aplicação está em execução. | `java -XX:+UseConcMarkSweepGC -jar Application.java` |
| G1 Garbage Collector | O G1 é uma coleta de lixo paralela, concorrente e com pausas previsíveis. É projetado para aplicações que precisam de grandes quantidades de memória heap (mais de 4GB). | `java -XX:+UseG1GC -jar Application.java` |
| Z Garbage Collector | O ZGC é uma coleta de lixo escalável, com pausas previsíveis e baixa sobrecarga. Ele é projetado para aplicações que precisam de grandes quantidades de memória heap e/ou tem requisitos rigorosos de tempo de pausa. | `java -XX:+UseZGC -jar Application.java` |
Cada um desses coletores tem prós e contras e pode ser mais adequado para certas situações do que outros. Por exemplo, o G1 GC é melhor para aplicações com metas de tempo de pausa muito rigorosas e uma taxa de transferência geral modesta, enquanto o ZGC é bom para aplicações que precisam de tempos de pausa muito curtos e têm grandes quantidades de memória​1​.

Você pode escolher um coletor de lixo específico usando opções de linha de comando ao iniciar sua aplicação. Por exemplo, para usar o G1 GC, você forneceria -XX:+UseG1GC na linha de comando. Para o ZGC, você forneceria -XX:+UnlockExperimentalVMOptions -XX:+UseZGC​1​​2​.

Peço desculpas por não ter conseguido informações detalhadas sobre a instalação e o uso do VisualVM, mas estou trabalhando nisso agora.
