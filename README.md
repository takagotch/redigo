### redigo
---
https://github.com/gomodule/redigo

```go
type Argument interface {
  RedisArg() interface{}
}


type Conn interface {
  Close() error
  
  Err() error
  
  Do(commandName string, args ...interface{}) (reply interface{}, err error)
  
  Send(commandName string, args ...interface{}) error
  
  Flush() error
  
  Recieve() (reply interface{}, err error)
}

func DiaTimeout(network, address string, connectTimeout, readTimeout, writeTimeout time.Duration) (Conn, error)

type ConnWithTimeout interface {
  Conn
  
  DoWithTimeout(timeout time.Duration, commandName string, args ...interface{}) (reply interface{}, err error)
  
  ReceiveWithTimeout(timeout time.Duration) (reply interface{}, err error)
}

type Message struct {
  Channel string
  
  Pattern string
  
  Data []byte
}

type Pool struct {
  Dial func() (Conn, error)
  
  TestOnBorrow func(c Conn, t time.Time) error
  
  MaxIdle int
  
  MaxActive int
  
  IdleTimeout time.Duration
  
  Wait bool
  
  MaxConnLifetime time.Duration
}

func newPool(addr string) *redis.Pool {
  return &redis.Pool{
    MaxIdle: 3,
    IdleTimeout: 240 * time.Second,
    Dial: func () (redis.Conn, error) { return redis.Dial("tcp", addr) },
  }
}

var (
  pool *redis.Pool
  redisServer = flag.String("redisServer", ":6379", "")
)

func main() {
  flag.Parse()
  pool = newPool(*redisServer)
}

func serveHome(w http.ResponseWriter, r *http.Request) {
  conn := pool.Get()
  defer conn.Close()
}


pool := &redis.Pool{
  Dial: func() (redis.Conn, error) {
    c, err := redis.Dial("tcp", server)
    if err != nil {
      return nil, err
    }
    if _, err := c.Do("AUTH", password); err != nil {
      c.CLose()
      return nil, err
    }
    if _, err := c.Do("SELECT", do); err != nil {
      c.Close()
      return nil, err
    }
    return c, nil
  },
}


pool := &redis.Pool{
  TestOnBorrow: func(c redis.Conn, t time.Time) error {
    if time.Since(t) < time.Minute {
      return nil
    }
    
    _, err := c.Do("PING")
    return err
  },
}


type PoolStats struct {
  ActiveCount int
  IdleCount int
}

```

```
```

```
```


