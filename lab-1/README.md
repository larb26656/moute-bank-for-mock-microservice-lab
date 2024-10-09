1. Run these command

```
docker compose up -d
```

2. Test add first imposter by call http reqest if success should got status 201

```
curl -i -X POST -H 'Content-Type: application/json' http://localhost:2525/imposters --data '{
  "port": 4545,
  "protocol": "http",
  "stubs": [{
    "responses": [
      { "is": { "statusCode": 400 }}
    ],
    "predicates": [{
      "and": [
        {
          "equals": {
            "path": "/test",
            "method": "POST",
            "headers": { "Content-Type": "application/json" }
          }
        },
        {
          "not": {
            "contains": { "body": "requiredField" },
            "caseSensitive": true
          }
        }
      ]
    }]
  }]
}'
```

3. Call http request for test stub if success should got status 400

```
curl -i -X POST -H 'Content-Type: application/json' http://localhost:4545/test --data '{"optionalField": true}'
```

4. Finally we can remove imposters by http request and define port number on url like these if success should return status 200

```
curl -X DELETE http://localhost:2525/imposters/4545
```
