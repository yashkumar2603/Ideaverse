[How to Use FastAPI: A Detailed Python Tutorial - YouTube](https://www.youtube.com/watch?v=SORiTsvnU28)
[GitHub - ArjanCodes/2023-fastapi](https://github.com/ArjanCodes/2023-fastapi)
Really nice repo having all the code needed essentially

[Python FastAPI Tutorial: Build a REST API in 15 Minutes - YouTube](https://www.youtube.com/watch?v=iWS9ogMPOI0)
Another goat tutorial, covered everything needed basics



# Advantages
- Uses decorators and is hence easier to understand.
- Auto converts from Pydantic to JSON and bach to Pydantic after operations and at fast speed.
- Validation is in the core of fastapi, to limit the integers we can pass, there are also checks in the query params such as gt(greater than); lt(less than); ge(greater than equal); le.
- Async is built in, unlike flask and using something other than uvicorn like gunicorn we can also make it scalable and multithreaded(ig, not sure).
- Authentication built in


#### To start the server
```bash
uvicorn main:app --reload
```
basically `<file_name>:<appname_inside_the_file>`, the flag --reload makes the server watch for file changes.

```Python
# BOILERPLATE CODE

from enum import Enum
from fastapi import FastAPI, HTTPException, Path, Query
from pydantic import BaseModel

app = FastAPI()


class Category(Enum):
    TOOLS = 'tools'
    CONSUMABLES = 'consumables'


class Item(BaseModel):
    name: str
    price: float
    count: int
    id: int
    category: Category


items = {
    0: Item(name="Hammer", price=9.99, count=20, id=0, category=Category.TOOLS),
    1: Item(name="Pliers", price=5.99, count=20, id=1, category=Category.TOOLS),
    2: Item(name="Nails", price=1.99, count=100, id=2, category=Category.CONSUMABLES),
}


# FastAPI handles JSON serialization and deserialization for us.
# We can simply use built-in python and Pydantic types, in this case dict[int, Item].
@app.get("/")
def index() -> dict[str, dict[int, Item]]:
    return {"items": items}
```

`def index() -> dict[str, dict[int, Item]]:`
In this line, we have the function then -> the return type 

### Path Params
```Python
# Path parameters can be specified with {} directly in the path (similar to f-string syntax)
# These parameters will be forwarded to the decorated function as keyword arguments.
@app.get("/items/{item_id}")
def query_item_by_id(item_id: int) -> Item:
    if item_id not in items:
        raise HTTPException(status_code=404, detail=f"Item with {item_id=} does not exist.")
    return items[item_id]
```

### Query params
```Python
# Function parameters that are not path parameters can be specified as query parameters in the URL
# Here we can query items like this /items?count=20

Selection = dict[
    str, str | int | float | Category | None
]  # dictionary for response, key as string & value can be anything of the given types


@app.get("/items/")
def query_item_by_parameters(
    name: str | None = None,
    price: float | None = None, 
    count: int | None = None,  
    category: Category | None = None,  # Making optional
) -> dict[str, Selection | list[Item]]:  #return type
    def check_item(item: Item):
        """Check if the item matches the query arguments from the outer scope."""
        return all(
            (
                name is None or item.name == name,
                price is None or item.price == price,
                count is None or item.count != count,
                category is None or item.category is category,
            )
        )

    selection = [item for item in items.values() if check_item(item)]
    return {
        "query": {"name": name, "price": price, "count": count, "category": category},
        "selection": selection,
    }
```

Code explanation at
[Selection = dict[ str, str int float Category None ] # dictionary...](https://www.perplexity.ai/search/function-parameters-that-are-n-is6ghBNvTTq8se8zLJ9Cxg)

the query params are set as -> `/items?count=20`
All parameters are optional (`= None`). If not provided in the request, they default to `None`.

The function returns a dictionary where:
    
    - Keys are strings.
    - Values are either:
        
        - A dictionary (`Selection`) containing query arguments.
        - A list of matching items (`list[Item]`).

### POST
```Python
@app.post("/")
def add_item(item: Item) -> dict[str, Item]:

    if item.id in items:
        HTTPException(status_code=400, detail=f"Item with {item.id=} already exists.")

    items[item.id] = item
    return {"added": item}
```

### PUT
```python
@app.put("/update/{item_id}")
def update(
    item_id: int,
    name: str | None = None,
    price: float | None = None,
    count: int | None = None,
) -> dict[str, Item]:

    if item_id not in items:
        HTTPException(status_code=404, detail=f"Item with {item_id=} does not exist.")
    if all(info is None for info in (name, price, count)):
        raise HTTPException(
            status_code=400, detail="No parameters provided for update."
        )

    item = items[item_id]
    if name is not None:
        item.name = name
    if price is not None:
        item.price = price
    if count is not None:
        item.count = count

    return {"updated": item}
```

### DELETE
```python
@app.delete("/delete/{item_id}")
def delete_item(item_id: int) -> dict[str, Item]:

    if item_id not in items:
        raise HTTPException(
            status_code=404, detail=f"Item with {item_id=} does not exist."
        )

    item = items.pop(item_id)
    return {"deleted": item}
```

#### Testing -
We can make a simple test script like -
```Python
import requests

print("Adding an item:")
print(
    requests.post(
        "http://127.0.0.1:8000/",
        json={"name": "Screwdriver", "price": 3.99, "count": 10, "id": 4, "category": "tools"},
    ).json()
)
print(requests.get("http://127.0.0.1:8000/").json())
print()

print("Updating an item:")
print(requests.put("http://127.0.0.1:8000/update/0?count=9001").json())
print(requests.get("http://127.0.0.1:8000/").json())
print()

print("Deleting an item:")
print(requests.delete("http://127.0.0.1:8000/delete/0").json())
print(requests.get("http://127.0.0.1:8000/").json())
```
Refactor this to make URL swapping easier and other imporvements, but this can test it easily and log all the responses it gets

---
So the thing we notice is that the URL is the same but the type of request determines what will happen, this is the best practice to follow for abstraction.

### Query and path validation and constraints
```Python
@app.put("/update/{item_id}")
def update(
    item_id: int = Path(ge=0),
    name: str | None = Query(defaut=None, min_length=1, max_length=8),
    price: float | None = Query(default=None, gt=0.0),
    count: int | None = Query(default=None, ge=0),
)
```

### Making nice documentation easily (OpenAPI)
```Python
app = FastAPI(
    title="Arjan's Handyman Emporium",
    description="Arjan does not only code but also helps you fix things. **See what's in stock!**",
    version="0.1.0",
) #Add API metadata while initializing

# Docstrings of classes will be reflected in the API documentation in the 'Schemas' section
class Category(Enum):
    """Category of an item"""

    TOOLS = "tools"
    CONSUMABLES = "consumables"

# You can add metadata to attributes using the Field class.
# This information will also be shown in the auto-generated documentation.
class Item(BaseModel):
    """Representation of an item in the system."""

    name: str = Field(description="Name of the item.")
    price: float = Field(description="Price of the item in Euro.")
    count: int = Field(description="Amount of instances of this item in stock.")
    id: int = Field(description="Unique integer that specifies this item.")
    category: Category = Field(description="Category this item belongs to.")
```

Adding Docstrings to the functions will also reflect in the documentation that is generated.

Documentaion is served at `https://127.0.0.1:8000/docs` always

on a per-function basis ->
```Python
# The 'responses' keyword allows you to specify which responses a user can expect from this endpoint.
@app.put(
    "/update/{item_id}",
    responses={
        404: {"description": "Item not found"},
        400: {"description": "No arguments specified"},
    },
)
# The Query and Path classes also allow us to add documentation to query and path parameters.
def update(
    item_id: int = Path(
        title="Item ID", description="Unique integer that specifies an item.", ge=0
    ),
    name: str
    | None = Query(
        title="Name",
        description="New name of the item.",
        default=None,
        min_length=1,
        max_length=8,
    ),
    price: float
    | None = Query(
        title="Price",
        description="New price of the item in Euro.",
        default=None,
        gt=0.0,
    ),
    count: int
    | None = Query(
        title="Count",
        description="New amount of instances of this item in stock.",
        default=None,
        ge=0,
    ),
):
```
This all per query descriptions will be shown in the documentation.