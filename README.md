# HookServe

http://godoc.org/github.com/chappjc/hookserve/hookserve

HookServe is a small golang utility for receiving github webhooks, originally by
[phayes](https://github.com/phayes). It's easy to use, flexible, and provides
strong security though GitHub's HMAC webhook verification scheme.

```go
server := hookserve.NewServer()
server.Port = 8888
server.Secret = "supersecretcode"
server.GoListenAndServe()

// Every time the server receives a webhook event, print the results
for event := range server.Events {
    fmt.Println(event.Owner + " " + event.Repo + " " + event.Branch + " " + event.Commit)
}
```

## Command Line Utility

It also comes with a command-line utility that lets you pass webhook push events
to other commands.

```sh
hookserve --port=8888 logger -t PushEvent #log github webhook push event to the system log (/var/log/message) via the logger command
```

Example output in response to a push event:

```text
web hook received: event type push on chappjc/webfiles, branch master, [90b7cc2e3]
Launching command: /home/ubuntu/go/src/github.com/chappjc/webfiles/cmd/webfiles/relaunch.sh
```

### Building From Source

First install Go, then:

```bash
go get -u github.com/chappjc/hookserve/util/hookserve
```

## GitHub Webhooks

Setting up webhooks on GitHub is easy. Navigate to
`github.com/<name>/<repo>/settings/hooks` and create a new webhook. Be sure to use `application/json` as the content type, and don't forget the `/postreceive` part of the Payload URL. Setting up your webhook should look something like this:

![Configuring webhooks in GitHub](https://i.imgur.com/u3ciUD7.png)
