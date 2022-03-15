# hikariPool의 Thread starvation or clock leap detected (housekeeper delta=51s20ms282µs452ns) 이슈는 왜 발생 할까?

```txt
WARN 30747 --- [or-http-epoll-1] app.hikari.pool.PoolBase          : HikariPool-1 - Failed to validate connection com.mysql.cj.jdbc.ConnectionImpl@1066d1f0 (No operations allowed after connection closed.). Possibly consider using a shorter maxLifetime value.
WARN 30747 --- [l-1 housekeeper] app.hikari.pool.HikariPool        : HikariPool-1 - Thread starvation or clock leap detected (housekeeper delta=1m13s50ms252µs185ns).
WARN 30747 --- [l-1 housekeeper] app.hikari.pool.HikariPool        : HikariPool-1 - Thread starvation or clock leap detected (housekeeper delta=1m49s539ms302µs551ns).
WARN 30747 --- [or-http-epoll-1] o.h.engine.jdbc.spi.SqlExceptionHelper   : SQL Error: 0, SQLState: 08003
ERROR 30747 --- [or-http-epoll-1] o.h.engine.jdbc.spi.SqlExceptionHelper   : HikariPool-1 - Connection is not available, request timed out after 491632ms.
ERROR 30747 --- [or-http-epoll-1] o.h.engine.jdbc.spi.SqlExceptionHelper   : No operations allowed after connection closed.
WARN 30747 --- [l-1 housekeeper] app.hikari.pool.HikariPool        : HikariPool-1 - Thread starvation or clock leap detected (housekeeper delta=51s20ms282µs452ns).
```


