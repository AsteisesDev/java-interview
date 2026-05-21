# Changelog: Многопоточность
Дата: 2026-05-09
Вопросов в теме: 70
Обработано: 70 из 70

## Ревизия v2 (аудит + редактура)

### Общие изменения
- [заголовки] Убраны `_«»_` и `**` из всех `##` заголовков
- [грейды] Добавлены `<!-- grade: -->` ко всем 70 вопросам
- [структура] Добавлены `###` подзаголовки, `> **На собеседовании:**`, `---` разделители
- [структура] Длинные примеры кода обёрнуты в `<details>`
- [структура] Сравнения переведены в табличный формат
- [контент] Добавлены аналогии из жизни
- [объём] 2772 → 6353 строк

### Распределение грейдов
- **Junior (~20):** Процесс vs поток, создание потока, Thread vs Runnable, start vs run, состояния потока, sleep/yield, join, приоритет, демоны, sleep, Runnable vs Callable, остановка потока, interrupted vs isInterrupted и др.
- **Middle (~35):** JMM, потокобезопасность, конкуренция vs параллелизм, монитор, синхронизация, wait/notify, deadlock, livelock, volatile, Atomic, CAS, race condition, пул потоков, ThreadLocal, synchronized vs ReentrantLock, ReadWriteLock, Fork/Join, Semaphore, CyclicBarrier vs CountDownLatch, double-checked locking, CompletableFuture и др.
- **Senior (~15):** ordering/happens-before/visibility, busy spin, неблокирующий стек/ArrayList, Virtual Threads, Structured Concurrency, ScopedValue, Concurrent Collections, Actor Model, корутины и др.

### Фактические исправления
- [fix] Определение синхронизации — исправлена неточность: «процесс, который позволяет выполнять потоки параллельно» → «механизм координации потоков для корректного доступа к общим ресурсам»

### Таблицы добавлены
- Thread vs Runnable
- sleep() vs yield()
- notify() vs notifyAll()
- Состояния потока (Thread.State)
- volatile vs Atomic
- compareAndSwap vs weakCompareAndSwap
- synchronized vs ReentrantLock
- CyclicBarrier vs CountDownLatch
- submit() vs execute()
- Stack vs Heap в многопоточности
- Platform Threads vs Virtual Threads vs Корутины
- Phaser vs CyclicBarrier vs CountDownLatch
- CompletableFuture методы

### Аналогии добавлены
- JMM = сотрудники с копиями документа
- Монитор = ключ от комнаты
- Deadlock = машины на узком мосту
- Livelock = два человека в коридоре
- Пул потоков = бригада рабочих
- ThreadLocal = личный ящик на рабочем месте
- Semaphore = парковка с ограниченным числом мест
- Fork/Join = делегирование задач в команде
- Virtual Threads = лёгкие нити в ткацком станке
