# Deploying nameko-devex docker envinroment
![Airship Ltd](airship.png)


## Important Notes
* I ran across with the error below, because the sleep time wasn't enough to condition met.
```sh
error: no matching resources found
make: *** [kind-setup-ingress] Error 1
```

### Solution
I needed to execute the command bellow manually

```sh
kubectl wait --namespace ingress-nginx \
		--for=condition=ready pod \
		--selector=app.kubernetes.io/component=controller \
		--timeout=90s 
```

After this first issue I needed to load all make commands before like a

```sh
make init-helm
make create-namespace
make deploy-dependences
make install-charts
```

### Test output
* Deploy nameko microservice in K8S
```sh
(nameko-devex)  atila@MacBook-Pro-de-Saulo  ~/Projects/nameko-devex/k8s   master ±  make smoke-test
../test/nex-smoketest.sh http://localhost
Remote Smoke Test in CF
STD_APP_URL=http://localhost
=== Creating a product id: the_odyssey ===
{"id": "the_odyssey"}
=== Getting product id: the_odyssey ===
{
  "maximum_speed": 5,
  "passenger_capacity": 101,
  "in_stock": 10,
  "title": "The Odyssey",
  "id": "the_odyssey"
}
=== Creating Order ===
{"id": 1}
=== Getting Order ===
{
  "id": 1,
  "order_details": [
    {
      "product_id": "the_odyssey",
      "quantity": 1,
      "product": {
        "maximum_speed": 5,
        "passenger_capacity": 101,
        "in_stock": 9,
        "title": "The Odyssey",
        "id": "the_odyssey"
      },
      "price": "100000.99",
      "id": 1,
      "image": "http://www.example.com/airship/images/the_odyssey.jpg"
    }
  ]
}
```

Curl Output
```sh
(nameko-devex)  atila@MacBook-Pro-de-Saulo  ~/Projects/nameko-devex/k8s   master ±  curl -s "http://localhost/products/the_odyssey" | jq . 
{
  "maximum_speed": 5,
  "passenger_capacity": 101,
  "in_stock": 9,
  "title": "The Odyssey",
  "id": "the_odyssey"
}


### References

* Issue Solution:
    - [Stackoverflow](hhttps://stackoverflow.com/questions/73929564/entrypoints-object-has-no-attribute-get-digital-ocean)