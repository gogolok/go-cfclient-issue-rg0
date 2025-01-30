The hard coded assumption is that the refresh token expires after 4 minutes.
But you can change the following line in main.go to change the refreshInterval to your UAA's refresh token expiration time.

```
refreshInterval := 4 * 60 * time.Second // 4 minutes
```

If you set up the environment variables
CF_API
CF_USERNAME
CF_PASSWORD
you could run the demo code and hopefully observe the following error:

```
$ go run main.go
stacks = [0x1400019e630 0x1400019e6c0] ; err = <nil>
Sleeping for time interval 4m0s .

stacks = [0x140002f8480 0x140002f8510] ; err = <nil>
Sleeping for time interval 4m0s .

err = error executing GET request for /v3/stacks?page=1&per_page=50: error executing request, failed during HTTP request send: Get "https://api.system.00.cf.eu01.example.com/v3/stacks?page=1&per_page=50": Post "https://uaa.system.00.cf.eu01.example.com/oauth/token": context canceled
stacks = [] ; err = error executing GET request for /v3/stacks?page=1&per_page=50: error executing request, failed during HTTP request send: Get "https://api.system.00.cf.eu01.example.com/v3/stacks?page=1&per_page=50": Post "https://uaa.system.00.cf.eu01.example.com/oauth/token": context canceled
Sleeping for time interval 4m0s .
```


The UAA does not issue new refresh tokens when getting a new access token with the refresh_token request.
This is because of the setting `config.tokenPolicy.refreshTokenRotate = false` (default value) I guess.
