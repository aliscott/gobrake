# Airbrake Golang Notifier [![Build Status](https://circleci.com/gh/airbrake/gobrake.png?circle-token=4cbcbf1a58fa8275217247351a2db7250c1ef976)](https://circleci.com/gh/airbrake/gobrake)

<img src="http://f.cl.ly/items/3J3h1L05222X3o1w2l2L/golang.jpg" width=800px>

# Example

```go
package main

import (
	"errors"

	"github.com/aliscott/gobrake"
)

var airbrake = gobrake.NewNotifier(1234567, "FIXME")

func init() {
	airbrake.AddFilter(func(notice *gobrake.Notice) *gobrake.Notice {
		notice.Context["environment"] = "production"
		return notice
	})
}

func main() {
	defer airbrake.WaitAndClose(5*time.Second)
	defer airbrake.NotifyOnPanic()

	airbrake.Notify(errors.New("operation failed"), nil)
}
```

## Ignoring notices

```go
airbrake.AddFilter(func(notice *gobrake.Notice) *gobrake.Notice {
	if notice.Context["environment"] == "development" {
		// Ignore notices in development environment.
		return nil
	}
	return notice
})
```

## Logging

You can use [glog fork](https://github.com/airbrake/glog) to send your logs to Airbrake.
