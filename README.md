# installredisonmacbook
How to install and configure Redis on a macbook, and set up a local master and slave

## Installing

Using homebrew:

```
brew install redis
```

Or to build from source, follow the instructions [here](http://redis.io/download).

## Configuring

Check that you have a file `/usr/local/etc/redis.conf`.

Copy the file `io.redis.redis-server.plist` to `/Library/LaunchDaemons`. If `redis.conf` is in a different location from `/usr/local/etc`, edit the plist file accordingly.

Install the redis server as a service:

```
sudo launchctl load /Library/LaunchDaemons/io.redis.redis-slave.plist
```

The server will now start automatically whenever your macbook reboots. To start immediately by hand:

```
sudo launchctl start io.redis.redis-slave
```

## Creating a slave Redis server

It doesn't usually make sense to have a redis master and slave running on the same host, but you might need to do this to replicate a production environment on localhost for development/testing.

```
cd /usr/local/etc
cp redis.conf redis-slave.conf
```

Edit `redis-slave.conf` and make the following changes:

- Change `port 6379` to `port 8379` (e.g.)
- Add a line: `slaveof 127.0.0.1 6379`
- Copy the file `io.redis.redis-server.plist` to `/Library/LaunchDaemons`
- Install the slave server as a service as described above.
