# dxc-apigateway-tyk-training

## Deploying opensource tyk

1- To clone tyk repo 

    git clone https://github.com/TykTechnologies/tyk-gateway-docker

2- To go to the tyk directory 

    cd tyk-gateway-docker

3- To up and run tyk and redis

    docker-compose up -d

4- Ensure that docker-tyk-gateway and redis are up and running

    docker ps

5-  To ensure that tyk and redis are up and runnning

    curl localhost:8080/hello

6- tyk secret is stored in tyk.conf file as an entry as secret.tyk gives more priorty to the following environement variable TYK_GW_SECRET=foo

7- this secret has to be supplied as a header to create a new tyk api

8- To add new api

```bash
        curl --location 'http://localhost:58080/tyk/apis' \
        --header 'x-tyk-authorization: foo' \
        --header 'Content-Type: application/json' \
        --data '{
            "name": "demo-app",
            "slug": "demo-app",
            "api_id": "demo-app",
            "org_id": "1",
            "use_keyless": true,
            "auth": {
            "auth_header_name": "Authorization"
            },
            "definition": {
            "location": "header",
            "key": "x-api-version"
            },
            "version_data": {
            "not_versioned": true,
            "versions": {
                "Default": {
                "name": "Default",
                "use_extended_paths": true
                }
            }
            },
            "proxy": {
            "listen_path": "/demo-app/",
            "target_url": "http://echo.tyk-demo.com:8080/",
            "strip_listen_path": true
            },
            "active": true
        }'
```

9- Reload or refresh the url

    curl --location 'http://localhost:58080/tyk/reload/group' \
    --header 'x-tyk-authorization: foo'
10- Access api

```bash
   curl --location 'http://localhost:58080/tyk/keys/create' \
--header 'x-tyk-authorization: foo' \
--header 'Content-Type: application/json' \
--data '{
    "allowance": 1000,
    "rate": 1000,
    "per": 1,
    "expires": -1,
    "quota_max": -1,
    "org_id": "1",
    "quota_renews": 1449051461,
    "quota_remaining": -1,
    "quota_renewal_rate": 60,
    "access_rights": {
      "demo-app": {
        "api_id": "demo-app",
        "api_name": "demo-app",
        "versions": ["Default"]
      }
    },
    "meta_data": {}
  }'
  ```