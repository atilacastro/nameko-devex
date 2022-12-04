# Deploying nameko-devex docker envinroment
![Airship Ltd](airship.png)


## Important Notes
* Deploy nameko microservice in docker
```sh
'EntryPoints' object has no attribute 'get'
```

### Solution
To solve 'EntryPoints' object has no attribute 'get' issue, I needed to do the downgrade from  "importlib-metadata" module. 
Because the version installed was not compatible with the function required.
Was necessary to insert in the Dockerfile of each application microservices the follow command
```sh
RUN pip install importlib-metadata==4.13.0 
```

### Test output
* Deploy nameko microservice in Docker
```sh
(nameko-devex)  ✘ atila@MacBook-Pro-de-Saulo  ~/Projects/nameko-devex   master ±  make smoke-test
./test/nex-smoketest.sh http://localhost:8003
Remote Smoke Test in CF
STD_APP_URL=http://localhost:8003
=== Creating a product id: the_odyssey ===
{"id": "the_odyssey"}
=== Getting product id: the_odyssey ===
{
  "passenger_capacity": 101,
  "in_stock": 10,
  "maximum_speed": 5,
  "id": "the_odyssey",
  "title": "The Odyssey"
}
=== Creating Order ===
{"id": 1}
=== Getting Order ===
{
  "order_details": [
    {
      "quantity": 1,
      "image": "http://www.example.com/airship/images/the_odyssey.jpg",
      "price": "100000.99",
      "id": 1,
      "product": {
        "passenger_capacity": 101,
        "in_stock": 9,
        "maximum_speed": 5,
        "id": "the_odyssey",
        "title": "The Odyssey"
      },
      "product_id": "the_odyssey"
    }
  ],
  "id": 1
}
```

Curl Output
```sh
(nameko-devex)  ✘ atila@MacBook-Pro-de-Saulo  ~/Projects/nameko-devex   master ±  curl -s "http://localhost:8003/products/the_odyssey" | jq .
{
  "passenger_capacity": 101,
  "in_stock": 9,
  "maximum_speed": 5,
  "id": "the_odyssey",
  "title": "The Odyssey"
}
```

### References

* Issue Solution:
    - [Stackoverflow](hhttps://stackoverflow.com/questions/73929564/entrypoints-object-has-no-attribute-get-digital-ocean)