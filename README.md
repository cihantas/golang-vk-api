# VK API wrapper in golang

## Installing
go get github.com/himidori/golang-vk-api

## Example of usage

```go
package main

import (
	"fmt"

	"github.com/himidori/golang-vk-api"
)

func main() {
	//creating new VKClient using email(phone) and password for authorization
	client, err := vkapi.NewVKClient("xxx", "xxx")
	if err != nil {
		panic(err)
	}
	fmt.Printf("Successfuly authorized!\nToken:%s\nUID:%d\n", client.Self.AccessToken,
		client.Self.UID)

	//getting information about given user ids
	//if there's more than one id they have to be splitted with comma
	users, err := client.GetUsers("1,15")
	if err != nil {
		panic(err)
	}

	//getting 100 dialogs
	dialogs, err := client.GetDialogs(0, 100, 0, false)
	if err != nil {
		panic(err)
	}

	//getting channel for receiving messages from a longpoll server
	ch, err := client.ListenLongPollServer()
	if err != nil {
		panic(err)
	}

	//receiving messages from a longpoll server
	for msg := range ch {
		if msg.UserID != 0 {
			fmt.Printf("New message! %d: %s\n", msg.UserID, msg.Body)
		}
	}
}
```
