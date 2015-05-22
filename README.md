# twilio-gae

Update GAE libraries 

An unofficial Go helper library for [Twilio](http://twilio.com) that works with Google App Engine (using urlfetch transport over the standard net/http package).

This package is an adaptation of the [twiliogo](https://github.com/carlosdp/twiliogo) package (a much more complete package for working with twilio services) that has been stripped out and left only with the capability to send SMS messages, while replacing the use of the net/http package with Google's appengine/urlfetch package (the only way to perform post/get requests from within the gae sandbox)


# usage

# license

MIT (see LICENSE file)
