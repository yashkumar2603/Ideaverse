[Microservices with FastAPI – Full Course - YouTube](https://www.youtube.com/watch?v=Cy9fAvsXGZA)
make a redis DB with redisJson and redisSearch(similar to elastic search) enabled.
Next we connect to it using the python package redis-om
using the following code
```Python
from fastapi import FastAPI
from redis_om import get_redis_connection, HashModel

app = FastAPI()

redis = get_redis_connection(
    host="redis-",
    port=11844,
    password="",
    decode_responses=True
)
print(redis.ping())
```

Now to push and communicate ,

```Python
class Product(HashModel):
	name : str
	price: float
	quantity: int

	class Meta:
		database = redis  #This part is purely for redis
```

To make this communicate with the redis DB actually, we need to add this ->
```Python
@app.get('/products')
def all():
	return [format(pk) for pk in Product.all_pks()]


def format(pk: str):
	product = Product.get(pk)
	return {
		'id': product.pk,
		'name': product.name,
		'price': product.price,
		'quantity': product.quantity
	}

#This will return all the primary keys of all the products in the DB.

@app.post('/products')
def create(product: Product)
	return product.save()

#Adds a product to the RedisDB
```

before we can start sending and recieving data we need the middleware
```Python
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

app.add_middleware(
	CORSMiddleware,
	allow_origins['http://localhost:3000']
	allow_methods['*'],
	allow_headers['*']
)
```
