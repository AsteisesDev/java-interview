[Вопросы для собеседования](README.md)

# Реактивное программирование

+ [Что такое реактивное программирование и чем оно отличается от процедурного программирования?](#что-такое-реактивное-программирование-и-чем-оно-отличается-от-процедурного-программирования)
+ [Объясните концепцию потоков данных в реактивном программировании](#объясните-концепцию-потоков-данных-в-реактивном-программировании)
+ [Что такое паттерн Observer и как он лежит в основе реактивного программирования?](#что-такое-паттерн-observer-и-как-он-лежит-в-основе-реактивного-программирования)
+ [Опишите роль Observable и Observer в реактивном программировании](#опишите-роль-observable-и-observer-в-реактивном-программировании)
+ [Что такое backpressure в контексте реактивного программирования?](#что-такое-backpressure-в-контексте-реактивного-программирования)
+ [Объясните разницу между Hot и Cold Observable](#объясните-разницу-между-hot-и-cold-observable)
+ [Какова роль Подписки в реактивном программировании?](#какова-роль-подписки-в-реактивном-программировании)
+ [Как отписаться от потока для предотвращения утечки памяти?](#как-отписаться-от-потока-для-предотвращения-утечки-памяти)
+ [Какие есть операторы в Project Reactor и для чего они используются?](#какие-есть-операторы-в-project-reactor-и-для-чего-они-используются)
+ [Что такое Reactive Streams Specification?](#что-такое-reactive-streams-specification)
+ [Чем отличаются Mono и Flux в Project Reactor?](#чем-отличаются-mono-и-flux-в-project-reactor)
+ [Что такое Scheduler в Project Reactor? Какие типы существуют?](#что-такое-scheduler-в-project-reactor-какие-типы-существуют)
+ [Как обрабатывать ошибки в реактивных потоках?](#как-обрабатывать-ошибки-в-реактивных-потоках)
+ [Что такое Spring WebFlux и чем он отличается от Spring MVC?](#что-такое-spring-webflux-и-чем-он-отличается-от-spring-mvc)
+ [Как тестировать реактивный код?](#как-тестировать-реактивный-код)
+ [Что такое Sinks в Project Reactor?](#что-такое-sinks-в-project-reactor)
+ [Как реализовать Server-Sent Events (SSE) с помощью WebFlux?](#как-реализовать-server-sent-events-sse-с-помощью-webflux)
+ [Что такое R2DBC и как он связан с реактивным программированием?](#что-такое-r2dbc-и-как-он-связан-с-реактивным-программированием)
+ [Реактивный vs императивный подход — когда что использовать?](#реактивный-vs-императивный-подход--когда-что-использовать)
+ [Что такое Context в Project Reactor?](#что-такое-context-в-project-reactor)
+ [Как работает WebClient и чем он отличается от RestTemplate?](#как-работает-webclient-и-чем-он-отличается-от-resttemplate)

## Что такое реактивное программирование и чем оно отличается от процедурного программирования?
<!-- grade: junior -->

Реактивное программирование — это парадигма, в которой программа реагирует на изменения в потоках данных и событиях, автоматически распространяя эти изменения через систему.

> Аналогия из жизни: процедурное программирование — это когда вы вручную проверяете почтовый ящик каждый час. Реактивное — когда почтальон сам звонит в дверь, и вы реагируете на звонок.

Процедурное программирование строит программу как последовательность инструкций, выполняемых одна за другой. Данные представлены в виде единственного значения в переменной.

Реактивное программирование фокусируется на непрерывных потоках данных, на которые могут подписаться несколько наблюдателей. Оно позволяет создавать гибкие и эффективные системы, способные адаптироваться к изменениям без необходимости явного управления асинхронными задачами.

| Критерий | Процедурное | Реактивное |
|----------|------------|------------|
| Модель данных | Единственное значение в переменной | Непрерывный поток данных |
| Управление | Явное, последовательное | Автоматическая реакция на события |
| Асинхронность | Требует явного управления | Встроена в парадигму |
| Подписка | Нет | Множественные наблюдатели |

> **На собеседовании:** интервьюер хочет услышать понимание ключевого отличия: процедурный код pull-based (вы запрашиваете данные), реактивный — push-based (данные приходят к вам). Частая ошибка — путать реактивное программирование с асинхронным; реактивность шире и включает концепции потоков, backpressure и composability.

[к оглавлению](#реактивное-программирование)

---

## Объясните концепцию потоков данных в реактивном программировании
<!-- grade: junior -->

Поток данных (data stream) — это последовательность событий, каждое из которых может содержать новое значение или изменение состояния, автоматически распространяющееся через систему.

В отличие от традиционного императивного подхода, где данные обрабатываются как отдельные элементы и изменения инициируются явно, в реактивном программировании система автоматически реагирует на изменения, обновляя своё состояние. Реактивное программирование основано на шаблоне Наблюдатель (Observer).

### Ключевые компоненты

| Компонент | Роль |
|-----------|------|
| Observable (Наблюдаемый) | Источник данных; при изменении состояния передаёт данные наблюдателям |
| Observer (Наблюдатель) | Подписывается на Observable и получает уведомления об изменениях |
| Subscription (Подписка) | Устанавливает взаимосвязь между Observable и Observer |
| Operators (Операторы) | Функции преобразования данных до передачи наблюдателю |
| Schedulers (Планировщики) | Управляют временем и порядком выполнения операций |
| Subjects | Совмещают роли источника и потребителя данных |

### Процесс передачи данных

- Emission — данные создаются в Observable и отправляются наблюдателям
- Filtering — операторы пропускают только данные, соответствующие критериям
- Transformation — данные преобразуются (например, через map) перед передачей
- Notification — наблюдатели информируются при поступлении новых данных

### Основные характеристики потоков

- Непрерывность (Continuous) — поток сохраняется, обеспечивая взаимодействие в реальном времени
- Асинхронность (Asynchronous) — порядок событий не гарантирован, операции неблокирующие
- Однонаправленность (One-directional) — данные передаются от Observable к подписчикам

### Типы потоков

| Тип | Описание |
|-----|----------|
| Unicast | У каждого наблюдателя эксклюзивное подключение к источнику |
| Broadcast | Несколько наблюдателей подписаны на один источник |
| Hot Observable | Передаёт данные независимо от наличия наблюдателя; новый подписчик получает данные с момента подписки |
| Cold Observable | Передача начинается только после подписки; каждый наблюдатель получает данные с начала |

### Backpressure

Механизм регулирования скорости публикации данных в поток. Необходим для предотвращения переполнения при разнице скоростей обработки. В RxJava интерфейс Flowable (в отличие от Observable) включает поддержку backpressure.

### Практическое применение

Потоки данных используются для обработки асинхронных вызовов API, управления пользовательским вводом, работы с событиями в реальном времени. Операторы map, filter, debounce и throttle позволяют преобразовывать и манипулировать данными в потоке.

> **На собеседовании:** важно показать понимание всей цепочки: источник -> операторы -> подписчик. Частая ошибка — описывать потоки как просто коллекции; поток — это временная последовательность событий, а не набор данных в памяти.

[к оглавлению](#реактивное-программирование)

---

## Что такое паттерн Observer и как он лежит в основе реактивного программирования?
<!-- grade: junior -->

Паттерн Observer (Наблюдатель) — поведенческий шаблон проектирования, позволяющий объектам следить за изменениями в других объектах и автоматически реагировать на них.

Также известен как Publish/Subscribe или Event Listener. Паттерн обеспечивает слабую связность (loose coupling) между компонентами, что делает его основой реактивного программирования, где система должна реагировать на изменения внешних источников данных.

### Ключевые компоненты

- Subject — источник данных или событий, управляет подписками наблюдателей
- Observer — получает уведомления при изменении состояния Subject

В реактивной конфигурации Subject публикует изменения, а Observers подписываются на них, что подчёркивает модель потока данных (datastream model).

<details><summary>Пример кода паттерна Observer</summary>

```java
import java.util.ArrayList;
import java.util.List;

interface Observer {
    void update();
}

class Subject {
    private List<Observer> observers = new ArrayList<>();
    private int state;

    public int getState() {
        return state;
    }

    public void setState(int state) {
        this.state = state;
        notifyAllObservers();
    }

    public void attach(Observer observer) {
        observers.add(observer);
    }

    public void notifyAllObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }
}

class ConcreteObserver implements Observer {
    private String name;
    private Subject subject;

    public ConcreteObserver(String name, Subject subject) {
        this.name = name;
        this.subject = subject;
    }

    @Override
    public void update() {
        System.out.println("Observer " + name + " updated. New state: " + subject.getState());
    }
}

public class Main {
    public static void main(String[] args) {
        Subject subject = new Subject();
        ConcreteObserver observer1 = new ConcreteObserver("One", subject);
        ConcreteObserver observer2 = new ConcreteObserver("Two", subject);

        subject.attach(observer1);
        subject.attach(observer2);

        subject.setState(5);
    }
}
```

</details>

В данном примере Subject ведет список подписавшихся Observers и уведомляет их при изменении его состояния.

### Практическое применение

- Обработка событий пользовательского интерфейса
- Системы уведомлений
- Отслеживание изменений состояния приложения

Важно помнить, что чрезмерное использование паттерна может привести к усложнению кода и затруднить отладку.

> **На собеседовании:** интервьюер ожидает связку: Observer — это фундамент реактивного программирования, расширенный поддержкой backpressure, операторов и управления потоками. Частая ошибка — не упомянуть, чем реактивный Observer отличается от классического GoF-паттерна (добавлены backpressure и терминальные сигналы onComplete/onError).

[к оглавлению](#реактивное-программирование)

---

## Опишите роль Observable и Observer в реактивном программировании
<!-- grade: junior -->

Observable — источник (производитель) данных, Observer — потребитель данных. Их взаимодействие через подписку формирует основу реактивного потока.

### Ключевые концепции

| Компонент | Роль |
|-----------|------|
| Observable (Наблюдаемый) | Передаёт данные или сигналы любых типов |
| Observer (Наблюдатель) | Получает уведомления при отправке данных |
| Subscription (Подписка) | Связующее звено между Observable и Observer |
| Operators (Операторы) | Преобразуют, фильтруют, комбинируют поток данных |

### Пример: Java 9+ (Flow API)

<details><summary>Subscriber</summary>

```java
import java.util.concurrent.Flow;

public class SimpleSubscriber implements Flow.Subscriber<String> {
    private Flow.Subscription subscription;

    @Override
    public void onSubscribe(Flow.Subscription subscription) {
        this.subscription = subscription;
        subscription.request(1); // Запрашиваем первый элемент
    }

    @Override
    public void onNext(String item) {
        System.out.println("Received: " + item);
        subscription.request(1); // Запрашиваем следующий элемент
    }

    @Override
    public void onError(Throwable throwable) {
        System.err.println("Error: " + throwable.getMessage());
    }

    @Override
    public void onComplete() {
        System.out.println("All items received");
    }
}
```

</details>

<details><summary>Publisher</summary>

```java
import java.util.concurrent.Flow;
import java.util.concurrent.SubmissionPublisher;
import java.util.concurrent.TimeUnit;

public class SimplePublisher {

    public static void main(String[] args) throws InterruptedException {
        SubmissionPublisher<String> publisher = new SubmissionPublisher<>();
        SimpleSubscriber subscriber = new SimpleSubscriber();

        publisher.subscribe(subscriber);

        System.out.println("Publishing data items...");
        String[] items = {"item1", "item2", "item3"};
        for (String item : items) {
            publisher.submit(item);
            TimeUnit.SECONDS.sleep(1);
        }

        publisher.close();
        TimeUnit.SECONDS.sleep(3);
    }
}
```

</details>

### Пример: Project Reactor

```java
import reactor.core.publisher.Flux;

public class ReactorExample {

    public static void main(String[] args) {
        Flux<String> flux = Flux.just("Hello", "World", "From", "Reactor");

        flux.subscribe(
            item -> System.out.println("Received: " + item),
            error -> System.err.println("Error: " + error),
            () -> System.out.println("All items received")
        );
    }
}
```

> **На собеседовании:** покажите, что понимаете протокол взаимодействия: Publisher.subscribe -> onSubscribe -> request(n) -> onNext (до n раз) -> onComplete/onError. Частая ошибка — забыть про request(n), без которого данные не будут отправлены.

[к оглавлению](#реактивное-программирование)

---

## Что такое backpressure в контексте реактивного программирования?
<!-- grade: middle -->

Backpressure — механизм управления потоком данных, когда производитель (Publisher) генерирует данные быстрее, чем потребитель (Subscriber) может их обработать.

> Аналогия из жизни: backpressure — это как кран с водой и стакан. Если лить быстрее, чем стакан наполняется и опустошается, вода переливается. Backpressure позволяет потребителю регулировать напор крана.

### Почему важно backpressure

Без управления backpressure возникают:

- Memory Overflow — необработанные элементы накапливаются в памяти
- Latency Increase — растёт очередь необработанных элементов
- Resource Exhaustion — исчерпание CPU и памяти, сбои системы

### Стратегии управления backpressure в Project Reactor

| Стратегия | Оператор | Описание |
|-----------|----------|----------|
| Буферизация | `onBackpressureBuffer(100)` | Элементы хранятся в буфере до обработки |
| Отбрасывание | `onBackpressureDrop()` | Лишние элементы отбрасываются |
| Новейшие | `onBackpressureLatest()` | Сохраняется только последний элемент |
| Ошибка | `onBackpressureError()` | Сигнал об ошибке при переполнении |
| Контроль запроса | `request(n)` | Явный контроль скорости запроса элементов |

<details><summary>Пример: буферизация</summary>

```java
import reactor.core.publisher.Flux;
import reactor.core.scheduler.Schedulers;

public class BackpressureExample {

    public static void main(String[] args) {
        Flux<Integer> flux = Flux.range(1, 1000)
                .onBackpressureBuffer(100);

        flux.publishOn(Schedulers.boundedElastic())
            .subscribe(
                item -> {
                    try {
                        Thread.sleep(10); // Имитируем медленного потребителя
                        System.out.println("Received: " + item);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                },
                error -> System.err.println("Error: " + error),
                () -> System.out.println("All items received")
            );
    }
}
```

</details>

<details><summary>Пример: контроль скорости запроса через BaseSubscriber</summary>

```java
import reactor.core.publisher.BaseSubscriber;
import reactor.core.publisher.Flux;

public class ControlledRequestExample {

    public static void main(String[] args) {
        Flux<Integer> flux = Flux.range(1, 1000);

        flux.subscribe(new BaseSubscriber<Integer>() {
            @Override
            protected void hookOnSubscribe(Subscription subscription) {
                request(1); // Запрашиваем один элемент при подписке
            }

            @Override
            protected void hookOnNext(Integer value) {
                try {
                    Thread.sleep(10);
                    System.out.println("Received: " + value);
                    request(1); // Запрашиваем следующий элемент после обработки
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            @Override
            protected void hookOnComplete() {
                System.out.println("All items received");
            }

            @Override
            protected void hookOnError(Throwable throwable) {
                System.err.println("Error: " + throwable.getMessage());
            }
        });
    }
}
```

</details>

> **На собеседовании:** интервьюер хочет услышать, что backpressure — это не просто буфер, а протокол: потребитель через request(n) сообщает производителю, сколько данных он готов принять. Частая ошибка — назвать только одну стратегию (обычно буферизацию), не упомянув drop, latest, error и явный request(n).

[к оглавлению](#реактивное-программирование)

---

## Объясните разницу между Hot и Cold Observable
<!-- grade: middle -->

Cold Observable начинает эмитировать данные только после подписки; каждый подписчик получает полную последовательность с начала. Hot Observable эмитирует данные независимо от наличия подписчиков; новый подписчик получает только данные, отправленные после его подписки.

### Cold Observable

Характеристики:
- Ленивое исполнение (Lazy Evaluation) — данные не производятся без подписчика
- Множественные подписки — каждый подписчик получает полную последовательность независимо
- Воспроизводимость — каждый подписчик видит одну и ту же последовательность с начала

```java
Flux<String> coldFlux = Flux.just("A", "B", "C", "D")
        .doOnSubscribe(s -> System.out.println("Subscribed to Cold Observable"));

coldFlux.subscribe(item -> System.out.println("Subscriber 1: " + item));
coldFlux.subscribe(item -> System.out.println("Subscriber 2: " + item));
// Оба подписчика получат A, B, C, D
```

### Hot Observable

Характеристики:
- Жадное исполнение (Eager Evaluation) — публикация начинается сразу, даже без подписчиков
- Общий поток данных — все подписчики используют один источник
- Невоспроизводимость — подписчики видят разные части последовательности

<details><summary>Пример Hot Observable</summary>

```java
import reactor.core.publisher.Flux;
import reactor.core.publisher.ConnectableFlux;
import java.time.Duration;

public class HotObservableExample {

    public static void main(String[] args) throws InterruptedException {
        ConnectableFlux<String> hotFlux = Flux.just("A", "B", "C", "D")
                                              .delayElements(Duration.ofMillis(500))
                                              .publish();

        hotFlux.connect(); // Запускаем публикацию

        hotFlux.subscribe(item -> System.out.println("Subscriber 1: " + item));
        Thread.sleep(750);

        // Второй подписчик пропустит первый элемент
        hotFlux.subscribe(item -> System.out.println("Subscriber 2: " + item));

        Thread.sleep(2000);
    }
}
```

</details>

### Ключевые отличия

| Критерий | Hot Observable | Cold Observable |
|----------|---------------|-----------------|
| Обмен данными | Один поток для всех подписчиков | У каждого подписчика свой поток |
| Время подписки | Данные зависят от момента подписки | Все данные доступны любому подписчику |
| Жизненный цикл | Работает независимо от подписок | Запускается только при подписке |
| Асинхронность | Генерирует данные без подписчиков | Передача начинается после подписки |

### Практическое применение

Cold Observable:
- Чтение из файла, HTTP-запрос, генерация последовательности
- Сценарии, где каждый подписчик должен получить полные данные

Hot Observable:
- События мыши, обновления цен на акции, данные датчиков
- Сценарии, где важны только текущие и будущие события

> **На собеседовании:** хороший маркер знаний — привести примеры из реальных проектов. Cold: HTTP-запрос (каждый subscribe делает новый запрос). Hot: WebSocket-соединение, Kafka-топик. Частая ошибка — не упомянуть ConnectableFlux как способ превращения Cold в Hot.

[к оглавлению](#реактивное-программирование)

---

## Какова роль Подписки в реактивном программировании?
<!-- grade: junior -->

Subscription (Подписка) — интерфейс, действующий как контракт между источником данных (Publisher) и потребителем (Subscriber), обеспечивающий управление потоком, backpressure и ресурсами.

### Основные методы

| Метод | Назначение |
|-------|------------|
| `request(long n)` | Информирует источник о количестве элементов, которые потребитель готов получить |
| `cancel()` | Останавливает поток данных, освобождая ресурсы |

### Управление backpressure

Метод `request` — основной канал, по которому подписчик сообщает источнику о своей текущей возможности принимать данные. Реализации Publisher оценивают готовность подписчика и адаптируют скорость передачи данных, предотвращая перегрузку.

### Управление ресурсами

Для источников данных, связанных с файлами, потоками ввода-вывода или базами данных, Subscription предоставляет средства для освобождения ресурсов. После вызова `cancel()` источник данных может закрыть файл или прекратить сетевое взаимодействие.

> **На собеседовании:** ключевое — Subscription связывает Publisher и Subscriber, и именно через request(n) реализуется backpressure. Частая ошибка — описывать Subscription как просто «подписку», забывая про управление потоком и ресурсами.

[к оглавлению](#реактивное-программирование)

---

## Как отписаться от потока для предотвращения утечки памяти?
<!-- grade: junior -->

Отписка от реактивного потока выполняется через вызов `dispose()` на объекте Disposable, полученном из подписки, или через автоматические операторы ограничения потока.

### Способы отписки

- Disposable — получить Disposable из подписки и вызвать `dispose()` для отмены
- BaseSubscriber — наследоваться от BaseSubscriber и вызвать `dispose()`
- Операторы при создании потока:
  - `take(n)` — автоматически отменяет подписку после получения n элементов
  - `timeout(Duration)` — автоматически отменяет подписку при истечении времени ожидания

### Best Practice

Настраивайте отмену подписки, связывая её с жизненным циклом компонента (например, @PreDestroy в Spring).

> **На собеседовании:** интервьюер проверяет понимание утечек памяти в реактивном коде. Частая ошибка — забыть про Disposable и считать, что потоки автоматически завершаются. Бесконечные потоки (interval, SSE) требуют явной отписки.

[к оглавлению](#реактивное-программирование)

---

## Какие есть операторы в Project Reactor и для чего они используются?
<!-- grade: middle -->

Операторы в Project Reactor — функции для создания, преобразования, фильтрации и объединения реактивных потоков данных.

### Операторы создания

| Оператор | Описание |
|----------|----------|
| `just` | Создаёт Flux/Mono из указанных элементов |
| `fromArray` / `fromIterable` / `fromStream` | Создаёт поток из массива, итерируемого объекта или Stream |
| `fromCallable` / `fromRunnable` / `fromSupplier` | Создаёт Mono из Callable, Runnable или Supplier |
| `range` | Создаёт Flux из диапазона целых чисел |
| `interval` | Создаёт Flux, эмитирующий long через регулярные промежутки |
| `empty` / `error` / `never` | Пустой поток / поток с ошибкой / поток без сигналов |
| `defer` | Создаёт новый Flux/Mono для каждой подписки |

### Операторы трансформации

| Оператор | Описание |
|----------|----------|
| `map` | Синхронное преобразование каждого элемента |
| `flatMap` | Преобразует элемент в поток и объединяет результаты (без гарантии порядка) |
| `concatMap` | Как flatMap, но с сохранением порядка |
| `switchMap` | Отписывается от предыдущего потока при получении нового |
| `scan` | Накапливает состояние для каждого элемента |
| `buffer` | Собирает элементы в списки внутри Flux |
| `window` | Публикует коллекции элементов как вложенный Flux |
| `groupBy` | Группирует элементы по ключу |

### Операторы фильтрации

| Оператор | Описание |
|----------|----------|
| `filter` | Пропускает элементы, удовлетворяющие условию |
| `distinct` | Пропускает только уникальные элементы |
| `take` / `takeWhile` / `takeUntil` | Ограничивает поток по количеству или условию |
| `skip` / `skipWhile` / `skipUntil` | Пропускает элементы по количеству или условию |

### Операторы объединения

| Оператор | Описание |
|----------|----------|
| `merge` | Объединяет потоки параллельно (активная подписка) |
| `concat` | Объединяет потоки последовательно (отложенная подписка) |
| `zip` | Объединяет элементы из нескольких потоков в пары |
| `combineLatest` | Объединяет последние значения из нескольких потоков |
| `startWith` | Добавляет элементы в начало потока |

> **На собеседовании:** достаточно знать основные операторы каждой группы и уметь объяснить разницу map vs flatMap, merge vs concat, take vs takeWhile. Частая ошибка — путать map (синхронный, 1:1) и flatMap (асинхронный, 1:N).

[к оглавлению](#реактивное-программирование)

---

## Что такое Reactive Streams Specification?
<!-- grade: middle -->

Reactive Streams — спецификация, определяющая стандарт для асинхронной потоковой обработки данных с поддержкой неблокирующего backpressure. Разработана совместно инженерами Netflix, Pivotal, Lightbend и Oracle; с Java 9 включена в JDK как `java.util.concurrent.Flow`.

### Четыре ключевых интерфейса

```java
// 1. Publisher — источник данных
public interface Publisher<T> {
    void subscribe(Subscriber<? super T> subscriber);
}

// 2. Subscriber — потребитель данных
public interface Subscriber<T> {
    void onSubscribe(Subscription subscription); // вызывается при подписке
    void onNext(T item);                         // получение элемента
    void onError(Throwable throwable);           // сигнал об ошибке (терминальный)
    void onComplete();                           // сигнал о завершении (терминальный)
}

// 3. Subscription — связь Publisher-Subscriber, управление backpressure
public interface Subscription {
    void request(long n); // запросить n элементов
    void cancel();        // отменить подписку
}

// 4. Processor — комбинация Publisher и Subscriber
public interface Processor<T, R> extends Subscriber<T>, Publisher<R> {
}
```

### Протокол взаимодействия

1. Subscriber вызывает `publisher.subscribe(subscriber)`
2. Publisher вызывает `subscriber.onSubscribe(subscription)`
3. Subscriber вызывает `subscription.request(n)` — запрашивает n элементов
4. Publisher вызывает `subscriber.onNext(item)` не более n раз
5. Publisher завершает поток вызовом `onComplete()` или `onError()`

### Реализации спецификации

| Реализация | Компания | Где используется |
|-----------|----------|-----------------|
| Project Reactor | Pivotal/VMware | Spring WebFlux |
| RxJava | Netflix | Android-разработка |
| Mutiny | Red Hat | Quarkus |
| Akka Streams | Lightbend | Экосистема Akka |

### Частые ошибки

- Не вызывать `request(n)` — без запроса Publisher не отправит ни одного элемента
- Вызывать `onNext` после терминального сигнала — нарушение протокола
- Бросать исключения из `onNext` — вместо этого нужно вызвать `subscription.cancel()`
- Путать `Flow` API из JDK и Reactive Streams API — интерфейсы идентичны, но в разных пакетах

### Как используется в 2026

- Reactive Streams — зрелый стандарт, входящий в JDK
- Project Reactor — доминирующая реализация в Spring-экосистеме
- С появлением Virtual Threads (Java 21) часть задач решается проще через виртуальные потоки
- Reactive Streams незаменимы для streaming-сценариев (SSE, WebSocket, Kafka) и работы с backpressure

> **На собеседовании:** ключевое — назвать четыре интерфейса и объяснить протокол, особенно роль request(n) в backpressure. Частая ошибка — знать только Mono/Flux из Reactor, но не понимать, что под ними лежит стандартная спецификация с чётким протоколом.

[к оглавлению](#реактивное-программирование)

---

## Чем отличаются Mono и Flux в Project Reactor?
<!-- grade: junior -->

Mono — реактивный поток, эмитирующий 0 или 1 элемент. Flux — реактивный поток, эмитирующий от 0 до N элементов. Оба реализуют интерфейс Publisher и являются ленивыми — ничего не происходит до подписки.

```java
// Flux: 0..N элементов
Flux<String> flux = Flux.just("Spring", "Reactor", "WebFlux");
Flux<Integer> range = Flux.range(1, 100);
Flux<Long> interval = Flux.interval(Duration.ofSeconds(1)); // бесконечный поток

// Mono: 0..1 элемент
Mono<String> mono = Mono.just("Hello");
Mono<User> user = Mono.fromCallable(() -> userRepository.findById(1L));
Mono<Void> empty = Mono.empty(); // сигнал без данных (аналог void)
```

### Ключевые различия

| Критерий | Mono | Flux |
|----------|------|------|
| Количество элементов | 0 или 1 | 0..N |
| Аналог в блокирующем мире | Optional, CompletableFuture | List, Stream |
| Типичное применение | Запрос к БД по ID, HTTP-запрос | Список сущностей, поток событий |
| Backpressure | Не актуально (максимум 1 элемент) | Критически важно |

### Конвертация между Mono и Flux

```java
// Flux -> Mono
Mono<String> first = Flux.just("A", "B", "C").next();           // первый элемент
Mono<List<String>> all = Flux.just("A", "B", "C").collectList(); // все в список
Mono<Long> count = Flux.just("A", "B", "C").count();             // количество

// Mono -> Flux
Flux<String> flux = Mono.just("A").flux();                       // Mono как Flux из 1 элемента
Flux<String> repeated = Mono.just("A").repeat(3);                // повторить Mono
```

<details><summary>Типичные паттерны использования в контроллере</summary>

```java
@RestController
public class UserController {

    @GetMapping("/users/{id}")
    public Mono<User> getUser(@PathVariable Long id) {
        return userRepository.findById(id); // Mono — один пользователь
    }

    @GetMapping("/users")
    public Flux<User> getAllUsers() {
        return userRepository.findAll(); // Flux — список пользователей
    }

    @PostMapping("/users")
    public Mono<Void> createUser(@RequestBody Mono<User> user) {
        return user.flatMap(userRepository::save).then(); // Mono<Void> — результат без тела
    }
}
```

</details>

### Частые ошибки

- Использовать Flux там, где достаточно Mono — если результат всегда один объект (findById), используйте Mono
- Вызывать `.block()` в реактивном контексте — может привести к deadlock
- Игнорировать возвращённый Mono/Flux — без подписки операция не выполнится
- Путать `map` и `flatMap` — map для синхронных преобразований, flatMap когда трансформация возвращает Mono/Flux

### Как используется в 2026

- Mono и Flux — основа реактивного стека Spring (WebFlux, Spring Data Reactive, Spring Security Reactive)
- С Virtual Threads часть сценариев заменяется обычным блокирующим кодом
- Mono/Flux незаменимы для streaming: SSE, WebSocket, R2DBC
- Тренд: использование Mono/Flux только там, где действительно нужна реактивность

> **На собеседовании:** минимум — назвать отличие (0..1 vs 0..N) и привести примеры использования каждого. Частая ошибка — не упомянуть ленивость (lazy): Mono/Flux без subscribe ничего не делают, а также забыть про Mono<Void> как реактивный аналог void.

[к оглавлению](#реактивное-программирование)

---

## Что такое Scheduler в Project Reactor? Какие типы существуют?
<!-- grade: middle -->

Scheduler — абстракция в Project Reactor, определяющая, в каком потоке (или пуле потоков) будет выполняться реактивный конвейер. По умолчанию цепочка выполняется в потоке, который вызвал `subscribe()`.

### Два ключевых оператора

| Оператор | Поведение |
|----------|-----------|
| `subscribeOn(Scheduler)` | Определяет поток для всей цепочки с начала (upstream). Место вызова не важно |
| `publishOn(Scheduler)` | Переключает выполнение последующих операторов (downstream). Место вызова важно |

```java
Flux.range(1, 10)
    .map(i -> {
        System.out.println("map1: " + Thread.currentThread().getName());
        return i * 2;
    })
    .publishOn(Schedulers.parallel())
    .map(i -> {
        System.out.println("map2: " + Thread.currentThread().getName());
        return i + 1;
    })
    .subscribeOn(Schedulers.boundedElastic())
    .subscribe();
// map1 выполнится в boundedElastic, map2 — в parallel
```

### Типы Scheduler

| Scheduler | Пул потоков | Назначение |
|-----------|-------------|------------|
| `Schedulers.immediate()` | Текущий поток | Без переключения |
| `Schedulers.single()` | 1 поток | Последовательные задачи с малой нагрузкой |
| `Schedulers.parallel()` | N потоков (N = CPU cores) | CPU-bound вычисления, без блокировок |
| `Schedulers.boundedElastic()` | Эластичный пул (до 10xCPU, TTL 60с) | Блокирующие I/O (JDBC, файлы, legacy API) |
| `Schedulers.fromExecutor(executor)` | Пользовательский Executor | Кастомный пул потоков |

<details><summary>Пример: оборачивание блокирующего вызова</summary>

```java
Mono<User> user = Mono.fromCallable(() -> {
        // Блокирующий вызов к БД через JDBC
        return jdbcTemplate.queryForObject("SELECT * FROM users WHERE id = ?",
                                            userRowMapper, userId);
    })
    .subscribeOn(Schedulers.boundedElastic()); // выполнить в эластичном пуле
```

</details>

### Частые ошибки

- Блокирующие вызовы в `Schedulers.parallel()` — приводит к голоданию потоков (пул ограничен числом CPU-ядер)
- Многократный `subscribeOn` — работает только ближайший к источнику, остальные игнорируются
- Отсутствие `subscribeOn` при блокирующем источнике — блокирующий вызов выполнится в event-loop потоке Netty
- Создание нового Scheduler на каждый запрос — утечка потоков; используйте `Schedulers.*` или кэшируйте

### Как используется в 2026

- С Virtual Threads (Java 21) потребность в `boundedElastic()` для I/O снижается
- Reactor 3.6+ поддерживает `Schedulers.fromExecutor(Executors.newVirtualThreadPerTaskExecutor())`
- Для CPU-bound вычислений `Schedulers.parallel()` остаётся актуальным
- В полностью реактивных приложениях (R2DBC, WebClient) Scheduler нужен реже

> **На собеседовании:** ключевое — объяснить разницу subscribeOn vs publishOn и назвать типы Scheduler с их назначением. Частая ошибка — не знать, что subscribeOn действует на весь upstream (место вызова не важно), а publishOn — только на downstream (место вызова критично).

[к оглавлению](#реактивное-программирование)

---

## Как обрабатывать ошибки в реактивных потоках?
<!-- grade: middle -->

В реактивном программировании ошибка — терминальный сигнал, после которого поток прекращает работу. Project Reactor предоставляет набор операторов для перехвата, замены и восстановления после ошибок.

### Основные операторы обработки ошибок

| Оператор | Назначение | Пример |
|----------|------------|--------|
| `onErrorReturn` | Заменить ошибку значением по умолчанию | `flux.onErrorReturn(-1)` |
| `onErrorResume` | Заменить ошибку альтернативным потоком | `mono.onErrorResume(e -> fallback())` |
| `onErrorMap` | Преобразовать ошибку в другой тип | `mono.onErrorMap(e -> new ApiException(e))` |
| `doOnError` | Побочный эффект (логирование) без перехвата | `mono.doOnError(e -> log.error(...))` |
| `retry` / `retryWhen` | Повторная попытка подписки | `mono.retry(3)` |
| `onErrorComplete` | Преобразовать ошибку в сигнал завершения | `flux.onErrorComplete()` |

```java
// onErrorReturn — значение по умолчанию
Flux.just(1, 2, 0)
    .map(i -> 10 / i)
    .onErrorReturn(-1)
    .subscribe(System.out::println);
// Вывод: 10, 5, -1
```

```java
// onErrorResume — разная обработка для разных типов ошибок
Mono<User> user = userService.findById(id)
    .onErrorResume(NotFoundException.class, e -> Mono.empty())
    .onErrorResume(ServiceException.class, e -> fallbackService.findById(id));
```

```java
// onErrorMap — преобразование типа ошибки
Mono<User> user = userRepository.findById(id)
    .onErrorMap(DataAccessException.class,
                e -> new ServiceException("Ошибка БД", e));
```

<details><summary>Пример: retryWhen с backoff</summary>

```java
Mono<Response> response = webClient.get()
    .uri("/api/data")
    .retrieve()
    .bodyToMono(Response.class)
    .retryWhen(Retry.backoff(3, Duration.ofSeconds(1))
        .maxBackoff(Duration.ofSeconds(10))
        .filter(e -> e instanceof WebClientResponseException.ServiceUnavailable)
        .onRetryExhaustedThrow((spec, signal) ->
            new ServiceException("Сервис недоступен после 3 попыток")));
```

</details>

<details><summary>Пример: комбинированная обработка ошибок</summary>

```java
Mono<OrderDto> result = orderService.createOrder(request)
    .doOnError(e -> log.error("Ошибка создания заказа", e))
    .onErrorMap(DataAccessException.class,
                e -> new ApiException(500, "Ошибка базы данных"))
    .onErrorMap(ValidationException.class,
                e -> new ApiException(400, e.getMessage()))
    .retryWhen(Retry.backoff(2, Duration.ofMillis(500))
        .filter(e -> e instanceof TransientDataAccessException));
```

</details>

### Частые ошибки

- `onErrorReturn` в середине цепочки — поток завершится после возврата значения, последующие элементы Flux не будут эмитированы
- Бесконечный `retry` без фильтра — бесконечный цикл при постоянной ошибке
- Retry для неидемпотентных операций — повтор POST-запроса может создать дубликаты
- Игнорирование ошибок без `doOnError` — если ошибка «проглатывается», важно хотя бы залогировать

### Как используется в 2026

- `retryWhen` с `Retry.backoff` — стандартный паттерн для устойчивости микросервисов
- Интеграция с Resilience4j для circuit breaker + retry
- Spring WebFlux: `@ExceptionHandler` и `WebExceptionHandler` для глобальной обработки
- Structured error handling через `onErrorResume` с разными типами исключений — best practice

> **На собеседовании:** покажите знание нескольких операторов и объясните разницу: onErrorReturn заменяет значением, onErrorResume — потоком, onErrorMap — типом ошибки, doOnError — побочный эффект. Частая ошибка — не знать, что порядок операторов важен: ошибка перехватывается ближайшим оператором ниже по цепочке.

[к оглавлению](#реактивное-программирование)

---

## Что такое Spring WebFlux и чем он отличается от Spring MVC?
<!-- grade: middle -->

Spring WebFlux — реактивный веб-фреймворк в Spring, работающий на неблокирующем сервере (Netty по умолчанию). Является альтернативой Spring MVC для построения асинхронных, неблокирующих веб-приложений.

### Архитектурные различия

| Критерий | Spring MVC | Spring WebFlux |
|----------|-----------|----------------|
| Модель выполнения | Один поток на запрос (thread-per-request) | Event loop (цикл событий) |
| Сервер | Tomcat, Jetty (Servlet API) | Netty, Undertow (неблокирующий) |
| Типы возврата | Object, ResponseEntity | Mono, Flux |
| Блокирующие вызовы | Допустимы | Запрещены в event-loop потоках |
| Потоковая передача | Ограничена | Нативная (SSE, WebSocket) |
| Потребление памяти | ~1 MB stack на поток | Минимальное (несколько event-loop потоков) |
| Макс. concurrent запросов | 200-500 (пул потоков) | Десятки тысяч соединений |

### Аннотированные контроллеры (общий стиль для MVC и WebFlux)

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @GetMapping("/{id}")
    public Mono<ResponseEntity<User>> getUser(@PathVariable Long id) {
        return userService.findById(id)
            .map(ResponseEntity::ok)
            .defaultIfEmpty(ResponseEntity.notFound().build());
    }

    @GetMapping
    public Flux<User> getAllUsers() {
        return userService.findAll();
    }
}
```

<details><summary>Функциональный стиль (Router Functions) — только WebFlux</summary>

```java
@Configuration
public class RouterConfig {

    @Bean
    public RouterFunction<ServerResponse> routes(UserHandler handler) {
        return RouterFunctions.route()
            .GET("/api/users/{id}", handler::getUser)
            .GET("/api/users", handler::getAllUsers)
            .POST("/api/users", handler::createUser)
            .build();
    }
}

@Component
public class UserHandler {

    private final UserService userService;

    public Mono<ServerResponse> getUser(ServerRequest request) {
        Long id = Long.parseLong(request.pathVariable("id"));
        return userService.findById(id)
            .flatMap(user -> ServerResponse.ok().bodyValue(user))
            .switchIfEmpty(ServerResponse.notFound().build());
    }

    public Mono<ServerResponse> getAllUsers(ServerRequest request) {
        return ServerResponse.ok().body(userService.findAll(), User.class);
    }
}
```

</details>

### Когда выбирать WebFlux

- Высокая конкурентность (тысячи одновременных соединений)
- Streaming-сценарии (SSE, WebSocket, реактивные потоки)
- Полностью неблокирующий стек (R2DBC, WebClient, reactive Redis/Mongo)
- Микросервисы с интенсивным межсервисным взаимодействием

### Когда оставаться на Spring MVC

- Блокирующие зависимости (JDBC, JPA/Hibernate)
- Простая CRUD-логика без требований к высокой конкурентности
- Команда не знакома с реактивным программированием
- Уже работающее приложение на MVC

### Частые ошибки

- Использование JDBC/JPA в WebFlux — блокирующие вызовы заблокируют event-loop; нужен R2DBC или `Schedulers.boundedElastic()`
- Выбор WebFlux «потому что это новее» — если нет требований к конкурентности, MVC проще
- Смешивание блокирующего и реактивного кода без `subscribeOn` — незаметно блокирует event-loop
- Отладка реактивных цепочек — нужен `Hooks.onOperatorDebug()` или `checkpoint()`

### Как используется в 2026

- С Virtual Threads (Java 21) Spring MVC покрывает большинство сценариев, ранее требовавших WebFlux
- Spring Boot 3.2+: `spring.threads.virtual.enabled=true` для MVC на виртуальных потоках — проще, чем WebFlux
- WebFlux остаётся для: streaming (SSE, WebSocket), реактивных БД, приложений с очень высокой конкурентностью
- Тренд: WebFlux для edge-сервисов и API Gateway, MVC + Virtual Threads для бизнес-логики

> **На собеседовании:** важно показать понимание trade-offs, а не просто сказать «WebFlux — реактивный». Назовите модель выполнения (event-loop vs thread-per-request), и когда каждый вариант оправдан. Частая ошибка — не упомянуть, что WebFlux поддерживает два стиля: аннотированные контроллеры и Router Functions.

[к оглавлению](#реактивное-программирование)

---

## Как тестировать реактивный код?
<!-- grade: middle -->

Реактивный код тестируется через StepVerifier — инструмент из reactor-test, который подписывается на Mono/Flux и пошагово проверяет сигналы. Нельзя просто вызвать метод и проверить результат, потому что Mono и Flux ленивы.

### StepVerifier — основной инструмент

```java
// Тестирование Mono
@Test
void testFindById() {
    Mono<User> userMono = userService.findById(1L);

    StepVerifier.create(userMono)
        .expectNextMatches(user -> user.getName().equals("John"))
        .verifyComplete();
}

// Тестирование Flux
@Test
void testFindAll() {
    Flux<User> usersFlux = userService.findAll();

    StepVerifier.create(usersFlux)
        .expectNextCount(3)
        .verifyComplete();
}

// Тестирование ошибок
@Test
void testError() {
    Mono<User> errorMono = userService.findById(-1L);

    StepVerifier.create(errorMono)
        .expectError(NotFoundException.class)
        .verify();
}
```

### Тестирование с виртуальным временем

```java
@Test
void testWithVirtualTime() {
    StepVerifier.withVirtualTime(() ->
            Flux.interval(Duration.ofHours(1)).take(3))
        .expectSubscription()
        .thenAwait(Duration.ofHours(3))
        .expectNext(0L, 1L, 2L)
        .verifyComplete();
}
```

<details><summary>TestPublisher — управляемый источник для тестов</summary>

```java
@Test
void testWithTestPublisher() {
    TestPublisher<String> testPublisher = TestPublisher.create();

    Flux<String> flux = testPublisher.flux()
        .map(String::toUpperCase);

    StepVerifier.create(flux)
        .then(() -> testPublisher.emit("hello", "world"))
        .expectNext("HELLO", "WORLD")
        .verifyComplete();
}

// Симуляция ошибки
@Test
void testErrorWithTestPublisher() {
    TestPublisher<String> testPublisher = TestPublisher.create();

    StepVerifier.create(testPublisher.flux())
        .then(() -> {
            testPublisher.next("data");
            testPublisher.error(new RuntimeException("test error"));
        })
        .expectNext("data")
        .expectError(RuntimeException.class)
        .verify();
}
```

</details>

<details><summary>WebTestClient — тестирование WebFlux-контроллеров</summary>

```java
@WebFluxTest(UserController.class)
class UserControllerTest {

    @Autowired
    private WebTestClient webTestClient;

    @MockitoBean
    private UserService userService;

    @Test
    void shouldReturnUser() {
        when(userService.findById(1L)).thenReturn(Mono.just(new User(1L, "John")));

        webTestClient.get().uri("/api/users/1")
            .exchange()
            .expectStatus().isOk()
            .expectBody(User.class)
            .value(user -> assertThat(user.getName()).isEqualTo("John"));
    }

    @Test
    void shouldReturnAllUsers() {
        when(userService.findAll()).thenReturn(Flux.just(
            new User(1L, "John"), new User(2L, "Jane")));

        webTestClient.get().uri("/api/users")
            .exchange()
            .expectStatus().isOk()
            .expectBodyList(User.class)
            .hasSize(2);
    }
}
```

</details>

### Частые ошибки

- Забыть вызвать `.verify()` — StepVerifier не подпишется, тест будет «зелёным» без проверки
- Использовать `.block()` вместо StepVerifier — теряется возможность проверки сигналов и порядка
- Не использовать `withVirtualTime` для `interval`/`delay` — тест будет ждать реальное время
- Моки, возвращающие null — нужно возвращать `Mono.empty()` или `Flux.empty()`, а не null

### Как используется в 2026

- StepVerifier — стабильный стандарт де-факто
- WebTestClient используется не только для WebFlux, но и для интеграционных тестов Spring MVC
- Testcontainers + R2DBC — стандартная связка для интеграционных тестов реактивных репозиториев
- AssertJ интеграция через StepVerifier + reactor-test

> **На собеседовании:** минимум — знать StepVerifier и его основные методы (expectNext, verifyComplete, expectError). Частая ошибка — не упомянуть withVirtualTime для тестирования операторов с задержкой и WebTestClient для интеграционных тестов контроллеров.

[к оглавлению](#реактивное-программирование)

---

## Что такое Sinks в Project Reactor?
<!-- grade: middle -->

Sinks — программно управляемые издатели данных в Project Reactor, пришедшие на замену deprecated FluxProcessor и MonoProcessor (Reactor 3.4+). Sinks позволяют императивно отправлять данные в реактивный поток.

> Аналогия из жизни: Sinks — это как микрофон на сцене. Вы говорите в микрофон (императивно отправляете данные), а все слушатели (подписчики) одновременно слышат вас через динамики (реактивный поток).

### Зачем нужны Sinks

Когда источник данных не является «естественно реактивным» (callback-API, WebSocket, очередь событий), Sinks мостят императивный и реактивный миры.

### Типы Sinks

| Тип | Аналог | Описание |
|-----|--------|----------|
| `Sinks.One<T>` | Mono | Эмитирует 0 или 1 элемент |
| `Sinks.Many<T>` unicast | Flux (1 подписчик) | Только один подписчик |
| `Sinks.Many<T>` multicast | Flux (N подписчиков) | Каждый получает элементы с момента подписки |
| `Sinks.Many<T>` replay | Flux (N подписчиков + история) | Новые подписчики получают N последних элементов |

```java
// Sinks.One — аналог Mono
Sinks.One<String> sink = Sinks.one();
Mono<String> mono = sink.asMono();
sink.tryEmitValue("result");

// Sinks.Many — multicast
Sinks.Many<String> sink = Sinks.many().multicast().onBackpressureBuffer();
Flux<String> flux = sink.asFlux();
sink.tryEmitNext("event1");
sink.tryEmitNext("event2");
sink.tryEmitComplete();

// Sinks.Many — replay (с историей)
Sinks.Many<String> replay = Sinks.many().replay().limit(10);
```

<details><summary>Практический пример — чат через WebSocket</summary>

```java
@Component
public class ChatService {
    private final Sinks.Many<ChatMessage> chatSink =
        Sinks.many().multicast().onBackpressureBuffer();

    public void sendMessage(ChatMessage message) {
        chatSink.tryEmitNext(message);
    }

    public Flux<ChatMessage> getMessages() {
        return chatSink.asFlux();
    }
}
```

</details>

### Обработка результата эмиссии

```java
Sinks.Many<String> sink = Sinks.many().multicast().onBackpressureBuffer();

Sinks.EmitResult result = sink.tryEmitNext("data");
if (result.isFailure()) {
    // FAIL_ZERO_SUBSCRIBER, FAIL_OVERFLOW, FAIL_CANCELLED, FAIL_TERMINATED
    log.warn("Не удалось отправить: {}", result);
}

// Или с автоматической обработкой:
sink.emitNext("data", Sinks.EmitFailureHandler.FAIL_FAST);
```

### Частые ошибки

- Использовать deprecated `FluxProcessor` — заменён на Sinks с Reactor 3.4
- Игнорировать `EmitResult` — без подписчиков или при переполнении данные теряются молча
- Использовать `unicast` с несколькими подписчиками — второй получит `IllegalStateException`
- Не вызывать `tryEmitComplete()` — подписчики будут ждать бесконечно

### Как используется в 2026

- Sinks — стандартный способ мостить императивный и реактивный код
- Широко используется в WebSocket/SSE-обработчиках
- Для event-driven архитектуры внутри приложения
- С Virtual Threads потребность снижается, но для broadcast-сценариев Sinks оптимальны

> **На собеседовании:** ключевое — объяснить, зачем нужны Sinks (мост между императивным и реактивным кодом) и назвать три режима Many: unicast, multicast, replay. Частая ошибка — не знать про EmitResult и не проверять результат эмиссии.

[к оглавлению](#реактивное-программирование)

---

## Как реализовать Server-Sent Events (SSE) с помощью WebFlux?
<!-- grade: middle -->

Server-Sent Events (SSE) — стандарт HTML5 для однонаправленной потоковой передачи данных от сервера к клиенту через HTTP. В отличие от WebSocket, SSE работает поверх обычного HTTP и поддерживает автоматическое переподключение браузером.

### Простейший SSE-эндпоинт

```java
@RestController
public class SseController {

    @GetMapping(value = "/events", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<String> streamEvents() {
        return Flux.interval(Duration.ofSeconds(1))
            .map(i -> "Event #" + i);
    }
}
```

### SSE с типизированными событиями

```java
@GetMapping(value = "/notifications", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
public Flux<ServerSentEvent<NotificationDto>> streamNotifications() {
    return notificationService.getNotifications()
        .map(notification -> ServerSentEvent.<NotificationDto>builder()
            .id(notification.getId().toString())
            .event(notification.getType())
            .data(notification)
            .retry(Duration.ofSeconds(5))
            .build());
}
```

<details><summary>Реальный пример — лента обновлений с Sinks + SSE</summary>

```java
@Service
public class UpdateService {
    private final Sinks.Many<Update> sink =
        Sinks.many().multicast().onBackpressureBuffer(100);

    public void publishUpdate(Update update) {
        sink.tryEmitNext(update);
    }

    public Flux<Update> getUpdates() {
        return sink.asFlux();
    }
}

@RestController
public class UpdateController {

    private final UpdateService updateService;

    @GetMapping(value = "/updates/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<ServerSentEvent<Update>> stream() {
        return updateService.getUpdates()
            .map(update -> ServerSentEvent.builder(update)
                .id(UUID.randomUUID().toString())
                .event("update")
                .build());
    }
}
```

</details>

### Клиент на JavaScript

```javascript
const eventSource = new EventSource('/updates/stream');

eventSource.addEventListener('update', (event) => {
    const data = JSON.parse(event.data);
    console.log('Получено обновление:', data);
});

eventSource.onerror = (error) => {
    console.error('SSE ошибка:', error);
    // Браузер автоматически переподключится
};
```

### Частые ошибки

- Не указать `produces = TEXT_EVENT_STREAM_VALUE` — клиент получит JSON-массив вместо потока
- Отправлять слишком большие объекты — SSE предназначен для лёгких уведомлений
- Забывать про CORS — SSE-запросы подчиняются политике CORS

### Как используется в 2026

- SSE — стандартный подход для push-уведомлений, дашбордов, live-лент
- Для двунаправленного обмена — WebSocket, для однонаправленного — SSE
- SSE хорошо работает с CDN и прокси (в отличие от WebSocket, требующего Upgrade)
- В микросервисной архитектуре SSE используется совместно с Kafka

> **На собеседовании:** покажите знание практической реализации: MediaType.TEXT_EVENT_STREAM_VALUE + Flux. Частая ошибка — путать SSE с WebSocket. SSE — однонаправленный (сервер -> клиент), работает поверх HTTP, браузер переподключается автоматически. WebSocket — двунаправленный, требует Upgrade.

[к оглавлению](#реактивное-программирование)

---

## Что такое R2DBC и как он связан с реактивным программированием?
<!-- grade: middle -->

R2DBC (Reactive Relational Database Connectivity) — спецификация для реактивного, неблокирующего доступа к реляционным базам данных, решающая проблему блокирующего JDBC в реактивном стеке.

### Проблема JDBC в реактивном контексте

JDBC полностью блокирующий — каждый запрос к БД блокирует поток до получения ответа. В реактивных приложениях (WebFlux + Netty) это блокирует event-loop поток и нивелирует все преимущества реактивности.

### Архитектура

```
Приложение -> Spring Data R2DBC -> R2DBC SPI -> R2DBC Driver -> База данных
```

### Поддерживаемые базы данных

- PostgreSQL (`r2dbc-postgresql`)
- MySQL / MariaDB (`r2dbc-mysql`, `r2dbc-mariadb`)
- Microsoft SQL Server (`r2dbc-mssql`)
- H2 (`r2dbc-h2`)
- Oracle (`oracle-r2dbc`)

<details><summary>Настройка и использование Spring Data R2DBC</summary>

```yaml
spring:
  r2dbc:
    url: r2dbc:postgresql://localhost:5432/mydb
    username: user
    password: secret
```

```java
// Сущность
@Table("users")
public record User(
    @Id Long id,
    String name,
    String email
) {}

// Реактивный репозиторий
public interface UserRepository extends ReactiveCrudRepository<User, Long> {

    Flux<User> findByName(String name);

    @Query("SELECT * FROM users WHERE email = :email")
    Mono<User> findByEmail(String email);
}

// Использование в сервисе
@Service
public class UserService {
    private final UserRepository userRepository;

    public Mono<User> createUser(User user) {
        return userRepository.save(user);
    }

    public Flux<User> findAll() {
        return userRepository.findAll();
    }
}
```

</details>

<details><summary>DatabaseClient — низкоуровневый доступ</summary>

```java
@Service
public class CustomUserRepository {
    private final DatabaseClient databaseClient;

    public Flux<User> searchUsers(String query) {
        return databaseClient.sql("SELECT * FROM users WHERE name LIKE :query")
            .bind("query", "%" + query + "%")
            .map(row -> new User(
                row.get("id", Long.class),
                row.get("name", String.class),
                row.get("email", String.class)))
            .all();
    }
}
```

</details>

### R2DBC vs JDBC

| Критерий | JDBC | R2DBC |
|----------|------|-------|
| Модель | Блокирующая | Неблокирующая, реактивная |
| Возвращаемые типы | ResultSet, List | Mono, Flux |
| Пул соединений | HikariCP | r2dbc-pool |
| ORM | Hibernate/JPA | Spring Data R2DBC (без lazy loading) |
| Зрелость | 25+ лет | ~5 лет |
| Транзакции | @Transactional | @Transactional (реактивный менеджер) |

### Частые ошибки

- Ожидать от R2DBC возможностей JPA — нет lazy loading, entity graph, dirty checking
- Использовать блокирующие драйверы рядом с R2DBC — один блокирующий вызов нивелирует реактивность
- Не настраивать пул соединений — без r2dbc-pool создаётся новое соединение на каждый запрос
- Сложные JOIN-запросы — R2DBC не маппит связи автоматически; нужен DatabaseClient или проекции

### Как используется в 2026

- R2DBC зрелый для PostgreSQL и MySQL
- С Virtual Threads (Java 21) обычный JDBC + HikariCP — конкурентная альтернатива
- R2DBC оправдан в полностью реактивных приложениях (WebFlux + reactive messaging + R2DBC)
- Тренд: для новых проектов без жёстких требований к реактивности — JDBC + Virtual Threads проще

> **На собеседовании:** ключевое — объяснить, зачем нужен R2DBC (JDBC блокирует event-loop в WebFlux) и чем он отличается от JPA (нет lazy loading, нет каскадирования). Частая ошибка — не упомянуть альтернативу: JDBC + Virtual Threads, которая проще и покрывает большинство сценариев.

[к оглавлению](#реактивное-программирование)

---

## Реактивный vs императивный подход — когда что использовать?
<!-- grade: senior -->

Выбор между реактивным и императивным подходом — архитектурное решение, зависящее от требований к конкурентности, стека технологий и опыта команды.

### Сравнение подходов

| Критерий | Императивный | Реактивный |
|----------|-------------|-----------|
| Читаемость | Высокая, линейный код | Ниже, цепочки операторов |
| Отладка | Простая, понятный стектрейс | Сложная, длинные трейсы |
| Порог входа | Низкий | Высокий |
| Throughput при I/O | Ограничен пулом потоков | Высокий (event-loop) |
| Потребление памяти | ~1 MB на поток | Минимальное |
| Streaming | Ограниченный | Нативный |
| Экосистема | Полная (JPA, JDBC, все библиотеки) | Ограниченная (R2DBC, WebClient) |

<details><summary>Сравнение кода: императивный vs реактивный vs Virtual Threads</summary>

```java
// Императивный (Spring MVC)
@GetMapping("/orders/{id}")
public OrderDto getOrder(@PathVariable Long id) {
    Order order = orderRepository.findById(id)
            .orElseThrow(() -> new NotFoundException("Order not found"));
    User user = userService.getUser(order.getUserId());
    List<Item> items = itemService.getItems(order.getId());
    return OrderDto.from(order, user, items);
}

// Реактивный (Spring WebFlux)
@GetMapping("/orders/{id}")
public Mono<OrderDto> getOrder(@PathVariable Long id) {
    return orderRepository.findById(id)
        .switchIfEmpty(Mono.error(new NotFoundException("Order not found")))
        .flatMap(order -> Mono.zip(
            userService.getUser(order.getUserId()),
            itemService.getItems(order.getId()).collectList(),
            (user, items) -> OrderDto.from(order, user, items)
        ));
}

// Virtual Threads (Java 21+ Spring MVC)
// application.yml: spring.threads.virtual.enabled=true
@GetMapping("/orders/{id}")
public OrderDto getOrder(@PathVariable Long id) {
    // Тот же блокирующий код, но на виртуальных потоках
    Order order = orderRepository.findById(id).orElseThrow();
    User user = userService.getUser(order.getUserId());
    List<Item> items = itemService.getItems(order.getId());
    return OrderDto.from(order, user, items);
}
```

</details>

### Когда выбирать реактивный подход

- Streaming-сценарии (SSE, WebSocket, бесконечные потоки данных)
- Уже реактивный стек (R2DBC, reactive MongoDB, reactive Kafka)
- API Gateway / BFF с высокой конкурентностью
- Сложная оркестрация асинхронных вызовов с backpressure

### Когда выбирать императивный подход

- Типичные CRUD-приложения
- Работа с JPA/Hibernate, блокирующими библиотеками
- Команда без опыта реактивного программирования
- Java 21+ с Virtual Threads доступна

### Частые ошибки

- «Реактивный = быстрый» — реактивный подход не ускоряет отдельный запрос, он повышает throughput при высокой конкурентности
- Частичная реактивность — один блокирующий вызов в реактивной цепочке уничтожает все преимущества
- Переход на WebFlux без реактивной БД — JDBC через `boundedElastic` — компромисс, а не решение
- Выбор технологии по моде — технический долг от неоправданного усложнения стоит дорого

### Как используется в 2026

- Чёткое разделение: WebFlux — для streaming и edge-сервисов, MVC + Virtual Threads — для бизнес-логики
- Spring Boot 3.2+: `spring.threads.virtual.enabled=true` переключает MVC на Virtual Threads одной строкой
- Прагматичный тренд: «используй реактивность только там, где она даёт измеримое преимущество»
- Можно комбинировать: MVC-контроллеры + WebClient для исходящих запросов

> **На собеседовании:** это вопрос senior-уровня — интервьюер ожидает аргументированный ответ с trade-offs, а не «WebFlux лучше». Назовите три сценария для реактивности и три для императивного подхода. Частая ошибка — не упомянуть Virtual Threads как третий путь, который в 2026 покрывает 80% сценариев.

[к оглавлению](#реактивное-программирование)

---

## Что такое Context в Project Reactor?
<!-- grade: middle -->

Context — неизменяемая (immutable) структура данных в Project Reactor, привязанная к конкретной подписке (Subscription). Замена ThreadLocal для реактивных цепочек, где один запрос может исполняться в разных потоках.

> Аналогия из жизни: Context — это как бейджик участника конференции. Не важно, в какой зал (поток) вы перейдёте — бейджик (контекст) остаётся при вас и идентифицирует вас в любом месте.

### Проблема

В реактивном коде ThreadLocal не работает, потому что цепочка операторов может переключаться между потоками через `publishOn`/`subscribeOn`. Context решает эту проблему.

### Запись и чтение контекста

```java
Mono<String> mono = Mono.deferContextual(ctx -> {
        String userId = ctx.get("userId");
        return Mono.just("User: " + userId);
    })
    .contextWrite(Context.of("userId", "12345"));
// Результат: "User: 12345"
```

<details><summary>Практический пример — передача correlation ID</summary>

```java
@Component
public class CorrelationWebFilter implements WebFilter {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, WebFilterChain chain) {
        String correlationId = exchange.getRequest().getHeaders()
            .getFirst("X-Correlation-ID");
        if (correlationId == null) {
            correlationId = UUID.randomUUID().toString();
        }
        String finalCorrelationId = correlationId;
        return chain.filter(exchange)
            .contextWrite(Context.of("correlationId", finalCorrelationId));
    }
}

// Использование в сервисе
@Service
public class OrderService {

    public Mono<Order> createOrder(OrderRequest request) {
        return Mono.deferContextual(ctx -> {
            String correlationId = ctx.getOrDefault("correlationId", "unknown");
            log.info("[{}] Создание заказа", correlationId);
            return orderRepository.save(new Order(request));
        });
    }
}
```

</details>

### Важные особенности

- Context immutable — каждый `contextWrite` создаёт новый Context
- Context распространяется снизу вверх (от subscribe к источнику) — `contextWrite` должен быть ниже точки чтения
- `deferContextual` — для чтения контекста при создании элементов
- `transformDeferredContextual` — для трансформации в середине цепочки

```java
// Ближний к подписке contextWrite перезаписывает дальний
Mono<String> mono = Mono.deferContextual(ctx ->
        Mono.just(ctx.get("key")))
    .contextWrite(ctx -> ctx.put("key", "value2"))  // ближе к подписке
    .contextWrite(ctx -> ctx.put("key", "value1")); // дальше от подписки
// Результат: "value2"
```

### Частые ошибки

- Использовать ThreadLocal в реактивном коде — значение потеряется при смене потока
- Помещать `contextWrite` выше точки чтения — Context распространяется снизу вверх
- Хранить мутабельные объекты в Context — лучше использовать immutable типы
- Злоупотреблять Context — это не замена параметров метода; Context для cross-cutting concerns (tracing, auth, MDC)

### Как используется в 2026

- Reactor Context — стандарт для передачи correlation ID, trace ID, информации об аутентификации
- Micrometer + Reactor автоматически пробрасывает observation context через Reactor Context
- Spring Security Reactive использует Context для хранения SecurityContext
- С Virtual Threads потребность снижается (можно использовать ThreadLocal через ScopedValue)

> **На собеседовании:** ключевое — объяснить, зачем нужен Context (замена ThreadLocal) и что он распространяется снизу вверх. Частая ошибка — не знать направление распространения Context и разместить contextWrite выше точки чтения.

[к оглавлению](#реактивное-программирование)

---

## Как работает WebClient и чем он отличается от RestTemplate?
<!-- grade: middle -->

WebClient — неблокирующий, реактивный HTTP-клиент из Spring WebFlux, пришедший на замену блокирующему RestTemplate. Начиная со Spring 6.1, появился также RestClient — блокирующий клиент с fluent API.

### Эволюция HTTP-клиентов в Spring

| Клиент | Версия Spring | Модель | Статус в 2026 |
|--------|--------------|--------|---------------|
| RestTemplate | Spring 3 (2009) | Блокирующий | Maintenance mode |
| WebClient | Spring 5 (2017) | Реактивный (неблокирующий) | Актуален |
| RestClient | Spring 6.1 (2023) | Блокирующий, fluent API | Рекомендуемый для блокирующего кода |

### WebClient — базовое использование

```java
WebClient webClient = WebClient.builder()
    .baseUrl("https://api.example.com")
    .defaultHeader(HttpHeaders.CONTENT_TYPE, MediaType.APPLICATION_JSON_VALUE)
    .build();

// GET — Mono
Mono<User> user = webClient.get()
    .uri("/users/{id}", 1)
    .retrieve()
    .bodyToMono(User.class);

// GET — Flux (список)
Flux<User> users = webClient.get()
    .uri("/users")
    .retrieve()
    .bodyToFlux(User.class);

// POST
Mono<User> created = webClient.post()
    .uri("/users")
    .bodyValue(new CreateUserRequest("John", "john@example.com"))
    .retrieve()
    .bodyToMono(User.class);
```

<details><summary>Обработка ошибок HTTP</summary>

```java
Mono<User> user = webClient.get()
    .uri("/users/{id}", userId)
    .retrieve()
    .onStatus(HttpStatusCode::is4xxClientError, response ->
        response.bodyToMono(ErrorResponse.class)
            .flatMap(error -> Mono.error(new ApiException(error.getMessage()))))
    .onStatus(HttpStatusCode::is5xxServerError, response ->
        Mono.error(new ServiceUnavailableException("Сервис недоступен")))
    .bodyToMono(User.class)
    .retryWhen(Retry.backoff(3, Duration.ofSeconds(1))
        .filter(e -> e instanceof ServiceUnavailableException));
```

</details>

<details><summary>Таймауты и конфигурация</summary>

```java
HttpClient httpClient = HttpClient.create()
    .responseTimeout(Duration.ofSeconds(5))
    .option(ChannelOption.CONNECT_TIMEOUT_MILLIS, 3000);

WebClient webClient = WebClient.builder()
    .baseUrl("https://api.example.com")
    .clientConnector(new ReactorClientHttpConnector(httpClient))
    .filter(ExchangeFilterFunction.ofRequestProcessor(request -> {
        log.info("Request: {} {}", request.method(), request.url());
        return Mono.just(request);
    }))
    .build();
```

</details>

### RestClient (Spring 6.1+) — блокирующая альтернатива

```java
RestClient restClient = RestClient.builder()
    .baseUrl("https://api.example.com")
    .build();

User user = restClient.get()
    .uri("/users/{id}", 1)
    .retrieve()
    .body(User.class);
```

### Сравнение

| Критерий | RestTemplate | WebClient | RestClient |
|----------|-------------|-----------|------------|
| Блокирующий | Да | Нет | Да |
| API стиль | Методы (getForObject) | Fluent chain | Fluent chain |
| Streaming | Нет | Да | Нет |
| Зависимость | spring-web | spring-webflux + reactor-netty | spring-web |
| Тестирование | MockRestServiceServer | MockWebServer | MockRestServiceServer |

### Частые ошибки

- Создавать новый WebClient на каждый запрос — WebClient потокобезопасный, создавайте один раз
- Вызывать `.block()` в реактивном контексте — deadlock; допустим только в блокирующем коде
- Не обрабатывать HTTP-ошибки — без `onStatus` WebClient бросает неинформативное исключение
- Использовать WebClient только потому, что RestTemplate deprecated — для блокирующего кода лучше RestClient

### Как используется в 2026

- RestClient — стандартный HTTP-клиент для Spring MVC (Spring 6.1+)
- WebClient — для реактивных приложений (WebFlux) и streaming
- RestTemplate встречается в legacy-коде
- Spring HTTP Interface (`@HttpExchange`) работает и с WebClient, и с RestClient — декларативный стиль

> **На собеседовании:** знайте три клиента (RestTemplate, WebClient, RestClient) и когда какой использовать. Частая ошибка — говорить «RestTemplate deprecated, используйте WebClient» без нюансов. RestTemplate в maintenance mode, но для блокирующего кода лучше RestClient, а не WebClient с .block().

[к оглавлению](#реактивное-программирование)
