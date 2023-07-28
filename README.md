Chromedp Cheatsheet
===============================
## Installing

Install in the usual Go way:

```sh
$ go get -u github.com/chromedp/chromedp
```

## Helloworld

Command click is a chromedp example demonstrating how to use a selector to
click on an element.
```go
package main

import (
	"context"
	"log"
	"time"

	"github.com/chromedp/chromedp"
)

func main() {
	// create chrome instance
	ctx, cancel := chromedp.NewContext(
		context.Background(),
		// chromedp.WithDebugf(log.Printf),
	)
	defer cancel()

	// create a timeout
	ctx, cancel = context.WithTimeout(ctx, 15*time.Second)
	defer cancel()

	// navigate to a page, wait for an element, click
	var example string
	err := chromedp.Run(ctx,
		chromedp.Navigate(`https://pkg.go.dev/time`),
		// wait for footer element is visible (ie, page is loaded)
		chromedp.WaitVisible(`body > footer`),
		// find and click "Example" link
		chromedp.Click(`#example-After`, chromedp.NodeVisible),
		// retrieve the text of the textarea
		chromedp.Value(`#example-After textarea`, &example),
	)
	if err != nil {
		log.Fatal(err)
	}
	log.Printf("Go's time.After example:\n%s", example)
}
```
## Open new tab
## Disable headless mode
```go
options := append(chromedp.DefaultExecAllocatorOptions[:],
  chromedp.Flag(`headless`, false),
  chromedp.DisableGPU,
  chromedp.Flag(`disable-extensions`, false),
  chromedp.Flag(`enable-automation`, false),
  chromedp.UserAgent(`Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36`),
)

allocCtx, cancel := chromedp.NewExecAllocator(context.Background(), options...)
defer cancel()

ctx, cancel := chromedp.NewContext(allocCtx)
defer cancel()
```
## Disable headless mode
