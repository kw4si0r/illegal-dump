# Build & run

```
docker build -f Dockerfile -t illegal-dump/api-gateway-mock .

docker run -i --rm -p 8081:1080 illegal-dump/api-gateway-mock
```

go to browser 
```
http://localhost:8081/api/coordinates
```
# Mock data

Please se *.json files. 

More information at [https://www.mock-server.com](https://www.mock-server.com)

