# system-design-patterns

| Паттерн | Где особенно полезен |
|---|---|
| [Caching](patterns/caching.md) | Ускорение чтения, снижение нагрузки на БД |
| [Load Balancing](patterns/load-balancing.md) | Распределение трафика между серверами |
| [Sharding / Partitioning](patterns/sharding.md) | Масштабирование больших баз данных |
| [Replication](patterns/replication.md) | Высокая доступность и быстрые чтения |
| [Pub/Sub](patterns/pub-sub.md.md) | Асинхронная коммуникация между сервисами |
| [Queue-Based Load Leveling](patterns/queues.md) | Выравнивание пиков нагрузки через очередь |
| [Long-Running Tasks / Async Jobs](patterns/async-jobs.md) | Загрузка файлов, видео, отчёты, фоновые задачи |
| [Circuit Breaker](patterns/circuit-breaker.md) | Защита от каскадных падений |
| [Backpressure / Rate Limiting](patterns/rate-limiting.md) | Контроль перегрузки системы |
| [Consistent Hashing](patterns/consistent-hashing.md) | Равномерное распределение ключей/сессий |
| [Leader Election](patterns/leader-election.md) | Координация в распределённых системах |
| [Event-Driven Architecture](patterns/event-driven-architecture.md) | Реакция на события, слабая связность |
| [CQRS](patterns/cqrs.md) | Разделение записи и чтения |
| [Event Sourcing](patterns/event-sourcing.md) | Хранение истории изменений как событий |
| [Saga](patterns/saga.md) | Распределённые бизнес-процессы без 2PC |
| [Idempotency](patterns/idempotency.md) | Распределённые бизнес-процессы без 2PC |
| [Retry with Backoff](patterns/retries.md) | Устойчивость к временным сбоям |
| [Bulkhead](patterns/bulkhead.md) | Изоляция ресурсов между частями системы |
| [Batching](patterns/batching.md) | Группировка запросов для эффективности |
| [Materialized Views / Precomputation](patterns/materialized-views.md) | Быстрые сложные чтения |