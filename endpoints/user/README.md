# **Endpoint /user/**


| Method | Name | Request parameters | Mock data |
|-|-|-|-|
| GET  | liked | String user | [Movie Details](./liked.json)
| PUT  | liked | String user, Number movieId, Bool liked | None
| GET  | suggested | String user | [Movie Details](/.liked.json)

### Bijde endpoints maken een user aan als hij niet bestaat.
### enpoinst worden aangeroepen via [url]/users/[name]
### waar url de main java server is en name de name column.
### suggested is extra

