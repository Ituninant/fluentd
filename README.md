# Сервис логов
* [Fluentd](https://www.fluentd.org/) - собирает логи
* [ElasticSearch](https://www.elastic.co/) - ищет в логах
* [Kibana](https://www.elastic.co/kibana) - UI

## Начало работы
1) cd ENV
2) docker-compose up -d
3) открыть kibana: http://localhost:5601
4) Вкладка "Discover", создать индекс
5) Вернуться на вкладку "Discover"

Просмотр консоли fluentd: `docker logs fluentd`

## Добавление в микросервис
Рекомендуется выставить в `application.properties` следующие настройки, чтобы не засорять общие логи.
```
    logging.level.org.apache=WARN
    logging.level.org.springframework=WARN
```
### Необходимые зависимости
```
    <dependency>
        <groupId>com.sndyuk</groupId>
        <artifactId>logback-more-appenders</artifactId>
        <version>1.8.0</version>
    </dependency>
    <dependency>
        <groupId>org.fluentd</groupId>
        <artifactId>fluent-logger</artifactId>
        <version>0.3.4</version>
    </dependency>
    <dependency>
        <groupId>org.komamitsu</groupId>
        <artifactId>fluency-core</artifactId>
        <version>2.4.1</version>
        <type>pom</type>
        <scope>runtime</scope>
    </dependency>
    <dependency>
        <groupId>org.komamitsu</groupId>
        <artifactId>fluency-fluentd</artifactId>
        <version>2.4.1</version>
        <type>pom</type>
    </dependency>
```
### Logback
Пример конфигурации Logback в папке [resources](resources). 
Достаточно добавить эти файлы в ресурсы приложения.
В файле [logback-spring.xml](resources/logback-spring.xml) пример настройки разных логгеров (классический лог в файл, лог в консоль и fluentd) для разных профилей спринга.
Чтобы проще фильровать логи для  каждого отдельного микросервиса, 
в [logback-appenders-fluentd.xml](resources/logback-appenders-fluentd.xml) проставить значение ${MS_NAME}.
