# Compilando e gerenciando dependências dos projetos

## Maven

Andes de saber o que é Maven, vamos pensar em um projeto do zero. É comum que não saibamos qual proporção ele tomará com o tempo. Ao precisar de uma biblioteca ou um framework apenas importamos no classpath, numa pasta ``lib``, adicionamos no classpath e tudo funciona normalmente.

Entretanto, conforme nosso projeto vai crescendo, precisamos gerenciar cada pacote necessário, cada biblioteca e também as dependências.

Dependendo do projeto, ainda precisamos nos preocupar com os testes unitários e de integração, com _build_ e _deploy_. Fazer tudo isso começa a se tornar extremamente trabalhoso.

O Maven é uma ferramenta de integração de projetos. É responsável por gerenciar dependências, controlar versão de artefatos, gerar relatórios de produtividade, garantir execução de testes, manter nível de qualidade do código dentre outras. Ela fornece às equipes de desenvolvimento uma forma padronizada de automação, construção e publicação de suas aplicações, agregando agilidade e qualidade. Por ser extremamente flexível, permite que sejam adicionados plugins.

As principais tarefas de uma ferramenta de gerenciamento de projetos incluem:
- Construir o Código-Fonte
- Testar o Código-Fonte
- Empacotar o Código-Fonte em um artefato (``ZIP``, ``JAR``, ``WAR`` ou ``EAR``)
- Lidar com controle de versão e releases dos artefatos
- Gerar JavaDocs a partir do Código-Fonte
- Gerenciar Dependências do Projeto

### Arquivo pom.xml

Quando utilizamos o Maven, configuramos um arquivo ``xml`` chamado **POM** (Project Object Model), que fornece todas as configurações necessárias para o projeto em um único arquivo.

````
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.anbruvic</groupId>
    <artifactId>Gerenciador-Dependencias</artifactId>
    <version>1.0-SNAPSHOT</version>
    <name>Gerenciador-Dependencias</name>
````

- project
    - É a tag de nível superior de nosso arquivo ``pom.xml``, que encapsula todas as informações relacionadas ao nosso projeto Maven
- modelVersion
  - Representa qual a versão do POM você está usando. O modelVersion para Maven 3 é sempre 4.0. Isso nunca mudará, a menos que você esteja usando outra versão principal do Maven.
- groupId
  - É um nome base exclusivo da empresa ou grupo que criou o projeto
- artifactId
  - É o nome exclusivo do projeto
- version
  - É a versão do projeto

### Estrutura de um projeto

- **src/main/java:** aqui fica o Código-Fonte do sistema ou biblioteca.
- **src/main/resources:** arquivos auxiliares do sistema, como ``.properties``, ``XMLs`` e configurações.
- **src/main/webapp:** se for uma aplicação web, os arquivos ``JSP``, ``HTML``, ``JavaScript`` e ``CSS`` vão aqui, incluindo o ``web.xml``.
- **src/test/java:** as classes com seus testes unitários ficam aqui sendo executadas automaticamente com _JUnit_ e _TestNG_. Outros frameworks podem exigir configuração adicional.
- **src/test/resources:** arquivos auxiliares usados nos testes. Você pode ter properties e configurações alternativas, por exemplo.
- **pom.xml:** é o arquivo que concentra as informações do seu projeto.
- **target:** é o diretório onde fica tudo que é gerado, isto é, onde vão parar s arquivos compilados, ``Jar``, ``WAR``, ``JavaDoc``, etc. 

### Criando o projeto

Depois de instalar o Maven no PC
````
mvn archetype:generate -DgroupId=NomeExemplo -DartifactId=NomeExemplo -DarchetypeArticactlId=maven-archetype-quickstart -DinteractiveMode=false
````

**Projeto WEB**
````
mvn archetype:generate -DgroupId=NomeExemplo -DartifactId=NomeExemplo -DarchetypeArticactlId=maven-archetype-webapp -DinteractiveMode=false
````

**Projeto feito com InteractiveMode**
````
mvn archetype:generate
````

### Configurando as dependências

O maven baixará e gerenciará tudo que é necessário para o Hibernate funcionar!

Mas, e para descobrir o groupId, artifactId e version de cada dependência que quisermos adicionar?

Para isso, podemos utilizar o site [MVN Repository](https://mvnrepository.com/). Lá podemos procurar pela dependência que quisermos. No próprio repositório já é disponibilizado o código pronto com a _dependency_ já configurada com o groupId, artifactId e a version!

### Escopo de dependências

O escopo auxilia na quantidade de dependências trazidas pelo Maven, limitando assim a inclusão de várias outras dependências.

Existem 6 escopos:
- compile
  - Este é o escopo padrão se nenhum for especificado. As dependências de tempo de compilação estão disponíveis no classpath do projeto.
- runtime
  - As dependências definidas com este escopo estarão disponíveis apenas em tempo de execução, mas não em tempo de compilação.
  - Um exemplo de uso: Imagine se você estiver usando o driver ``MySQL`` dentro do seu projeto, você pode adicionar a dependência com escopo como tempo de execução, para garantir que a abstração da _API JDBC_ seja usada em vez da _API do driver MySQL_ durante a implementação. Se o Código-Fonte incluir qualquer classe que faça parte da _API JDBC do MySQL_, o código não seá compilado, pois a dependência não está disponível no momento da compilação
- test
  - As dependências estão disponíveis apenas no momento da execução dos testes, exemplos típicos incluem Junit
- provided
  - Indica que esta dependência deve ser substituída por todas as dependências efetivas declaradas
- system
  - É semelhante ao escopo _provided_, mas a única diferença é que precisamos mencionar explicitamente onde a dependência pode ser encontrada no sistema, usando a tag ``systemPath``: 
  - ```
    <systemPath>${basedir}/lib/some-dependency.jar</systemPath>
    ```
- import

### Snapshot e Release

Uma dependência pode ser categorizada de duas maneiras:
- SNAPSHOT
- RELEASE

Se você estiver trabalhando em um projeto Java em uma equipe, é provável que esteja seguindo algum tipo de processo iterativo no qual passa pelas fases de desenvolvimento e em seguida, libera o software no final da fase.

Quando o projeto está em desenvolvimento geralmente usamos as dependências ``SNAPSHOT``, que se parecem com **1.0-SNAPSHOT**

Quando o software está pronto para o lançamento, geralmente criamos uma versão ``RELEASE`` que se parece com **1.0.RELEASE** ou **1.0**

### Repositórios

As dependências são armazenadas em um diretório especial chamado ``Repositório``. Existem basicamente 2 tipos de repositórios:

- Local Repository
- Remote Repository

#### Repositório Local

É um diretório em seu Pc onde os artefatos são armazenados após baixados de um repositório remoto na internet

O local padrão para o Repositório Local é:
```
~/.m2
 
C:\Users<user-name>.m2\repository
```

#### Repositório Remoto

Um Repositório remoto é um site onde podemos baixar dependências Maven. Podem ser um repositório fornecido pelo Maven(``repo.maven.org``) ou uma configuração de repositório customizada numa organização como, por exemplo, o [Nexus](https://www.sonatype.com/products/sonatype-nexus-oss)

### Ciclos de vida

O Maven possui 3 ciclos de vida e eles podem ser executados independente.

- clean
  - O ciclo de vida clean é o principal responsável por limpar o ``.class`` e metadados gerados pela fase de compilação.
- site
  - A fase do ciclo de vida site é responsável por gerar a documentação Java.
- default
  - O ciclo default é dividido em diferentes fases.

#### Fases de build
Fases do ciclo de vida default

- validate
  - Valida que os ``pom`` dos projetos envolvidos estão corretamente escritos e que todas as informações necessárias para a build estão presentes.
- compile
  - Compila todos os códigos do projeto, inclusive os códigos das classes teste;
- test
  - Roda os testes que estão em **src/test/java**, e certifica-se que todos estão passando, caso contrário a build é interrompida;
- package
  - Usa o código compilado e testado que está em **src/main/java** e cria um arquivo reusável, por exemplo ``jar``;
- integration-test
  - Nesta fase serão rodados os testes que necessitam do jar do projeto _"deploiado"_ para serem rodados 
- verify
  - Roda as verificações necessárias para se certificar que os pacotes gerados estão corretos e passam nos critérios de qualidade;
- install
  - Copia o arquivo gerado para o repositório local para estar disponível localmente para outros projetos;
- deploy
  - Copia o arquivo gerado para um repositório na rede ou remoto, para estar disponível para outros desenvolvedores.

### Projeto parent

Normalmente quando construímos projetos, nós os dividimos em módulos. Com o Maven existe a possibilidade de criarmos um projeto pai (_parent_) e criarmos vários outros projetos filhos, onde serão módulos do projeto pai.

Os projetos filhos herdam todas as configurações e dependências do projeto pai.

#### Criando projetos pai e filho

**Projeto Pai:**
```
mvn archetype:generate -DgroupId=NomeExemplo -DartifactId=parent-projec

//Altere o pom do projeto parent e coloque a tag
<packaging>pom</packaging>
```

**Projeto Filho:**
```
cd parent-project

mvn archetype:generate -DgroupId=NomeExemplo -DartifactId=core
```

##### Executando projeto filho

Adicione ao projeto filho a tag ``<package>jar</package>`` e execute o comando dentro da pasta do projeto:

```
mvn clen package

//Depis execute o comando:

java -jar target/core-1.0-SNAPSHOT.jar
```

### Gerenciamento de dependências

Quando trabalhamos com projetos com vários módulos no Maven pode acontecer de você utilizar diferentes versões da mesma biblioteca ou framework.

No Maven existe uma forma simples de gerenciar todas as versões utilizadas. Essa tag é chamada de ``dependencyManagement``

### Perfil ou Profiles

Os perfis(_Profiles_) são configurações personalizadas. Podemos criar perfis específicos para cada ambiente, como, por exemplo: Produção, Homologação e testes.

Exemplo de profile para pular a execução dos testes;
```
<profiles>
  <profile>
    <id>skip-tests</id>
    <properties>
      <maven.test.skip>true</maven.test.skip>
    </properties>
  </profile>
</profiles>
```

executando o profile skip-testes
```
mvn -Pskip-test clean package
```

## Gradle

É uma ferramenta de build open source que nos permite configurar arquivos de build por meio de ``DSL`` (_Domain Specific Language_) baseada na linguagem Groovy.

Ele tem o melhor do ``Ant`` e do ``Maven`` juntos.

**Instalando o** [Gradle](https://gradle.org/install/).

### Criando com Gradle

- Criar uma pasta com o nome do projeto

```
gradle init

gradle init --type java-application
```
