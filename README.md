# Dingo Redis example app in Ruby

This app is an example of how you can consume the Dingo Redis service within an application.

This example app provides an API to end users (for example `curl` commands) to get/set/delete key value pairs. The data is stored in a Dingo Redis service instance.

## Getting Started

### Using with Pivotal Cloud Foundry

Check that Dingo Redis has been installed by your platform operators:

```
cf marketplace -s dingo-redis
```

The output might look like:

```
Getting service plan information for service dingo-redis as admin...
OK

service plan   description   free or paid
ready          Redis node    free
```

Deploy the app by pushing it to your Pivotal Cloud Foundry and binding with the Dingo Redis service:

```
git clone https://github.com/dingotiles/dingo-redis-example-ruby-app
cd dingo-redis-example-ruby-app
cf push --no-start --random-route
cf create-service dingo-redis ready redis
cf bind-service dingo-redis-example-ruby-app redis
cf start dingo-redis-example-ruby-app
```

The final terminal output will include the URL:

```
requested state: started
instances: 1/1
usage: 256M x 1 instances
urls: dingo-redis-example-ruby-app-noncash-bisphenoid.apps.pcf110.starkandwayne.com
last uploaded: Sat Apr 15 18:26:03 UTC 2017
stack: cflinuxfs2
buildpack: ruby_buildpack
```

In the example above, the application is now running at `https://dingo-redis-example-ruby-app-noncash-bisphenoid.apps.pcf110.starkandwayne.com`.


### Usage

The app's API can be interacted with using `curl` to `PUT`, `GET`, and `DELETE` values, which in turn are stored/fetched/deleted from a Dingo Redis service instance.

#### PUT /:key

Sets the value stored in Redis at the specified key to a value posted in the 'data' field. Example:

```
$ export APP=https://dingo-redis-example-ruby-app-noncash-bisphenoid.apps.pcf110.starkandwayne.com
$ curl -k -X PUT $APP/foo -d 'data=bar'
success
```

#### GET /:key

Returns the value stored in Redis at the key specified by the path. Example:

```
$ curl -k -X GET $APP/foo
bar
```

#### DELETE /:key

Deletes a Redis key spcified by the path. Example:

```
$ curl -k -X DELETE $APP/foo
success
$ curl -k -X GET $APP/foo
key not present
```
