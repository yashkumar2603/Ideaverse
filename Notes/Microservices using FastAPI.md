make a redis DB with redisJson and redisSearch(similar to elastic search) enabled.
Next we connect to it using the python package redis-om
using the following code
```Python
from fastapi import FastAPI
from redis_om import get_redis_connection

app = FastAPI()

redis = get_redis_connection(
    host="redis-",
    port=11844,
    password="",
    decode_responses=True
)

print(redis.ping())
```

