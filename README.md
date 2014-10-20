# twilio-gae

An unofficial Go helper library for [Twilio](http://twilio.com) that works with Google App Engine (using urlfetch transport over the standard net/http package).

This package is an adaptation of the [twiliogo](https://github.com/carlosdp/twiliogo) package (a much more complete package for working with twilio services) that has been stripped out and left only with the capability to send SMS messages, while replacing the use of the net/http package with Google's appengine/urlfetch package (the only way to perform post/get requests from within the gae sandbox)


# usage

```go
package example

import (
	"appengine"
	"flag"
	"fmt"
	twilio "github.com/streamrail/twilio-gae"
	"net/http"
)

var (
	twilioSID    = flag.String("twilioSID", "111111111", "twilio sid auth")
	twilioToken  = flag.String("twilioToken", "111111111", "twilio token auth")
	twilioNumber = flag.String("twilioNumber", "+111111111", "twilio sms capable number")
	twilioClient = twilio.NewClient(*twilioSID, *twilioToken)
)

func init() {
	http.HandleFunc("/sms", SmsHandler)
}


func SmsHandler(w http.ResponseWriter, r *http.Request) {
	qs := r.URL.Query()
	to := qs.Get("to")
	body := qs.Get("body")
	c := appengine.NewContext(r)
	if err := SendSMS(c, to, body); err != nil {
		c.Errorf(err.Error())
	} else {
		c.Infof("sms sent succesfully to %s", to)
	}
}

func SendSMS(c appengine.Context, to string, body string) error {
	if len(body) == 0 {
		return fmt.Errorf("must contain 'body'")
	}
	if len(to) == 0 {
		return fmt.Errorf("must contain 'to' number")
	}
	message, err := twilio.NewMessage(c, twilioClient, *twilioNumber, to, twilio.Body(body))

	if err != nil {
		return err
	} else {
		fmt.Println(message.Status)
	}
	return nil
}
```

# license

MIT (see LICENSE file)