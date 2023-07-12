
# SAVI Testbed APIs

## List Endpoints

This repository provides guidelines for available APIs to access the SAVI testbed.

> **Note:** You will need to route your traffic through **client1** *(i.e. client1.savitestbed.ca)* machine 
> to access the endpoints. You may follow this guide and run commands directly 
> on client1 machine or you may use [sshtunnel](https://pypi.org/project/sshtunnel/) to route entire or part 
> of your traffic through client1 machine


## Authentication
Once you have obtained the endpoints and ensured that you have access to them, attempt to call the `keystone` endpoint: `http://iamv3.savitestbed.ca:5000/v3`. First, create a file named `credentials.json` and place the following json data inside it:
```json
{ "auth": {
    "identity": {
      "methods": ["password"],
      "password": {
        "user": {
          "name": "USERNAME",
          "domain": { "id": "default" },
          "password": "PASSWORD"
        }
      }
    },
    "scope": {
      "project": {
        "name": "PROJECT_NAME",
        "domain": { "id": "default" }
      }
    }
  }
}
```
Then use `curl` to call the endpoint. Replace the `{{KEYSTONE_URI}}` with the `keystone` endpoint value:

```bash
curl -H "Content-Type: application/json" \
   --location '{{KEYSTONE_URI}}/auth/tokens' \
   --data @credentials.json -D headers.txt
```
Please make a note of your `user ID` and `project ID` from the output. To retrieve the token value, locate the `headers.txt` file and find the value of the `X-Subject-Token` field within it.

## Sample Call using the Token
Replace the `TOKEN` and `USER_ID` in the following command:
```
curl -X GET -H "Content-Type: application/json" -H "X-Auth-Token: TOKEN" {{KEYSTONE_URI}}/users/USER_ID
```

You can find the list of available APIs in the [openstack Queens API guide](https://docs.openstack.org/queens/api/) directory page.
