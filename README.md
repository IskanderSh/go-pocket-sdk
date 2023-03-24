# GetPocket API Golang SDK
## https://getpocket.com/developer/

### imports
```
go get github.com/pkg/errors
```

### Example usage:

```go
package main

import (
	"context"
	"fmt"
	pocket "github.com/IskanderSh/go-pocket-sdk"
	"log"
)

func main()  {
	ctx := context.Background()

	client := pocket.NewClient("<your-consumer-key>") // you can generate key at https://getpocket.com/developer/apps/
	requestToken, err := client.GetRequestToken(ctx, "http://example.com/")
	if err != nil {
		log.Fatalf("failed to get request token: %s", err.Error())
	}

	url	:= client.GetAuthorizationURL(requestToken, "http://example.com/")
	fmt.Println(url)

	authResp, err := client.Authorize(ctx, requestToken)
	if err != nil {
		log.Fatalf("failed to authorize: %s", err)
	}

	err = client.Add(ctx, pocket.AddInput{
		URL: "https://github.com/IskanderSh/go-pocket-sdk",
		AccessToken: authResp.AccessToken,
	})
	if err != nil {
		log.Fatalf("failed to add item: %s", err)
	}
}
```
