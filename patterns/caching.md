# Caching

## Что это
Кэширование — это хранение часто используемых данных в быстром хранилище для ускорения доступа и снижения нагрузки на основное хранилище (БД).


## Какую проблему решает
Без кэша все запросы идут в медленную БД, что приводит к высоким задержкам и перегрузке при пиках нагрузки. Кэш решает проблему "горячих" данных — тех, к которым обращаются чаще всего.

## Когда применять
- Чтение >> записи (90%+ запросов — чтение)
- Есть "горячие" данные (топ-1% ключей дают 99% запросов)
- БД становится бутылочным горлышком
- Нужно снизить latency для пользователей
## Как работает
```text
Client → App → Cache Check
↓              ↓
↓ HIT          ↓ MISS
↓              ↓
Return data    DB → Cache → Return data
```
Основные стратегии:
- Cache-Aside (Lazy Loading): App проверяет кэш → miss → DB → записать в кэш
- Write-Through: Запись сразу в кэш + DB (консистентно, но медленно)
- Write-Behind (Write-Back): Запись в кэш → асинхронно в DB (быстро, но риск потери)
- Refresh Ahead: Кэш сам обновляется перед истечением TTL

## Плюсы
- Снижение latency в 10-1000x
- Разгрузка БД на 80-99%
- Простая реализация
- Масштабируется горизонтально (Redis Cluster)

## Минусы
- Cache Stampede: Массовые miss'ы перегружают БД
- Thundering Herd: Все ждут один ключ
- Stale Data: Устаревшие данные в кэше
- Memory overhead: Кэш жрёт RAM
- Cache Invalidation — самая сложная часть

## Пример
Сценарий: Соцсеть, профили пользователей.
```text
GET /user/123 → Redis: "user:123" → HIT → 0.5ms
               ↓ MISS
             DB → {"name": "Иван"} → Redis.setex("user:123", 300, data) → 50ms
PUT /user/123 → Redis.del("user:123") → DB UPDATE
```
Go код (redis/go-redis):
```go
key := fmt.Sprintf("user:%d", userID)
if val, err := rdb.Get(ctx, key).Result(); err == nil {
    return parseUser(val) // HIT
}
// MISS
user := db.GetUser(userID)
rdb.Set(ctx, key, userJSON, 5*time.Minute)
return user
```
Проблемы в примере:
- Race condition при UPDATE (2 записи → только 1 инвалидация)
- Cache stampede при рестарте Redis

## Типичные ошибки
1. Race Condition: UPDATE → DEL → UPDATE (данные вернулись в кэш)
2. No TTL → Memory leak
3. Hot Keys → Один Redis node под 100% нагрузкой
4. Leaky Abstractions → App не знает про кэш, но страдает от miss'ов