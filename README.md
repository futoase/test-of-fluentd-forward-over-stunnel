test-of-fluentd-forward-over-stunnel
------------------------------------

![structure](https://cloud.githubusercontent.com/assets/72997/2545534/25e8df9e-b619-11e3-9105-078cf42d2450.jpg)

Prerequired
-----------

- [virtualbox](https://www.virtualbox.org/)
- [vagrant](http://www.vagrantup.com/)
- [packer](http://www.packer.io/)
- [ansible](http://www.packer.io/)

How to setup?
-------------

## server

```
> vagrant up --provision server
```

## client

```
> vagrant up --provision client
```

## testing of in_tail.

### on client

```
> cd /var/dummy-log
> node index.js -f /tmp/dummy.log -t 100
```

### on server

```
> tail -f /var/log/td-agent/app/base.log
```

You try send packet of fluentd from client to server
----------------------------------------------------

1. You try this command on the client.

```
> vagrant ssh client
> echo '{"message": "welcome to underground"}' | fluent-cat toshinou.kyoko.love
```

2. You check td-agent.log on the server.

```
> vagrant ssh server
> tail -f /var/log/td-agent/td-agent.log
2014-03-27T18:13:31+09:00       toshinou.kyoko.love     {"message":"welcome to underground"}
```

LICENSE
-------
MIT.
