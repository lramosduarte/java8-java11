# Atualiza JDK 8 para JDK 11 app Java

Guia simples de como atualizar uma aplicação Java 8 para o novo LTS(Java 11).

Esse guia foi testado em algumas libs e aplicações spring.

## Contribuições
Contribuições são bem vindas, principalmente sobre relatos de problemas e correções no guia.

## Pré atualização
Antes de tudo, lembre-se de que é necessário possuir o JDK da versão que deseja, que no nosso caso seria `oracleJDK/openJDK 11`.

Configure se necessário o java default da sua maquina, exemplo:

$JAVA_HOME:
```bash
$: export JAVA_HOME=PATH_JDK_11
```

Utilizando update alternatives:
```bash
# Java
$: sudo update-alternatives --config java
# Javac
$: sudo update-alternatives --config javac
```

## Configurando a aplicação

### Utilizando Maven
Atualize a versão do seu java no `pom.xml`, por exemplo:

```xml
    ...
    <properties>
        ...
        <java.version>11</java.version>
        <maven.compiler.source>${java.version}</maven.compiler.source>
        <maven.compiler.target>${java.version}</maven.compiler.target>
        ...
    </properties>
    ...
```

## Problemas Comuns

### Classes que não existem ou beans que não iniciam 
Em alguns casos pode ser necessário fazer um import explicito de algumas dependencias, como por exemplo a falta do javaassists:

Exception exemplo:
```
Caused by: java.lang.NullPointerException: null
    at javassist.util.proxy.SecurityActions.setAccessib(SecurityActions.java:103) ~[javassist-3.22.0-GA.jar:na]
    at javassist.util.proxy.DefineClassHelper.toClas(DefineClassHelper.java:151) ~[javassist-3.22.0-GA.jar:na]
```

Lib necessária:
```xml
    <dependency>
        <groupId>org.javassist</groupId>
        <artifactId>javassist</artifactId>
        <version>3.23.1-GA</version>
    </dependency>
```

#### JAVA EE

Algumas libs do Java EE foram removidas, segunda proposta [JEP 320](http://openjdk.java.net/jeps/320#Java-EE-modules).
Então é necessário fazer imports explicitos, todas as libs podem ser encontradas no `maven central`:
```xml
    <dependency>
        <groupId>javax.xml.bind</groupId>
        <artifactId>jaxb-api</artifactId>
        <version>2.3.1</version>
    </dependency>
```
Links:

 * [Guia que fala principalmente sobre libs que dão problema](https://medium.com/criciumadev/its-time-migrating-to-java-11-5eb3868354f9)
