# ip.zxq.co

An ipinfo.io clone, without the rate limiting. And some other thingies.

## Why?

The main reason for this is: rate limiting. ipinfo.io sets a rate limiting of 1000 requests per day. I understand it, although that's a bit of a bummer. So I said, whatever. Let's just write our own clone. So, there you have it.

## Using

This is really similiar to the way ipinfo.io does it. Every response will be identical from ipinfo.io's, almost. So, basic usage: you will just make a GET request to <http://ip.zxq.co/[ip]>, like http://ip.zxq.co/8.8.8.8. Need to get something specific? http://ip.zxq.co/8.8.8.8/country.

```
$ curl ip.zxq.co/8.8.8.8
{"city":"Mountain View","continent":"NA","continent_full":"North America","country":"US","country_full":"United States","ip":"8.8.8.8","loc":"37.3860,-122.0838","postal":"94040","region":"California"}
```

That is a bit ugly, though. Especially for testing purposes. Let's make it pretty, shall we?

```
$ curl "localhost:8080/8.8.8.8?pretty=1"
{
  "city": "Mountain View",
  "continent": "NA",
  "continent_full": "North America",
  "country": "US",
  "country_full": "United States",
  "ip": "8.8.8.8",
  "loc": "37.3860,-122.0838",
  "postal": "94040",
  "region": "California"
}
```

Much better! :smile:

We're aren't done just yet! You want to use JSONP. You guess it, we are using the same system as ipinfo.io's. Just provide a `callback` parameter to your GET request.

```
$ curl "localhost:8080/8.8.8.8?pretty=1&callback=myFancyFunction"
/**/ typeof myFancyFunction === 'function' && myFancyFunction({
  "city": "Mountain View",
  "continent": "NA",
  "continent_full": "North America",
  "country": "US",
  "country_full": "United States",
  "ip": "8.8.8.8",
  "loc": "37.3860,-122.0838",
  "postal": "94040",
  "region": "California"
});
```

```html
<script>
var myFancyFunction = function(data) {
  alert("The city of the IP address 8.8.8.8 is: " + data.city);
}
</script>
<script src="http://ip.zxq.co/8.8.8.8?callback=myFancyFunction"></script>
```

## Features that aren't here (and not going to be implemented)

* Hostname. We would have to pick that data from another data source, which is too much effort.
* Organization field. Need another datasource. Effort. Performance. I'll just stick with not having it.

## Features that aren't on ipinfo.io but are here

* JSON minified, so it gets to your server quicker.
* Full name for the country!
* We also got continent info, with the full name too.

## Some advantages:

* We are using Go and not nodejs like them. Go is a compiled language, and therefore is amazingly fast. A response can be generated in a very short time.
* We get data only from one data source. Which means no lookups on other databases, which results in being faster overall.
* We are open source. Which means you can compile and put it on your own server!

## Data source

This product includes GeoLite2 data created by MaxMind, available from http://www.maxmind.com.

## License

MIT. Check LICENSE file.
