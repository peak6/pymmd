# pymmd
A python module for communicating over MMD.

## Installation


To install, run:

```sh
$ python setup.py build
$ sudo python setup.py install
```

## Getting started: calling echo2


The mmd2 repo comes with an `echo2` service. Let's call it:

```python
>>> import pymmd
>>> c = pymmd.connect()
>>> c.echo2("Hello, world!")
MMDChannelClose(chan_id=UUID('8d6db6c6-5fd2-11e0-900d-00219b38831e'), body='Hello, world!')
>>> c.echo2({1: 1, 2: 4, 3: 9})
MMDChannelClose(chan_id=UUID('b27a0cd0-5fd2-11e0-900d-00219b38831e'), body={1: 1, 2: 4, 3: 9})
````


## Writing a service


You can find an example service in `examples/echo3.py`. You'll need two
python windows to run the example. In the first window start the service:

```python
>>> import pymmd
>>> import echo3
>>> c = pymmd.connect()
>>> c.register(service="echo3", handler=echo3.echo3)
```

And in the client window:

```python
>>> import pymmd
>>> c = pymmd.connect()
>>> c.echo3("This is a test")
MMDChannelClose(chan_id=UUID('6e197948-5fde-11e0-92a5-00219b38831e'), body=['pong', 'This is a test'])
```

You can also try subscribing, sending and recieving channel messages,
and closing the channel:

```python
>>> ch = c.echo3.subscribe(handler=echo3.echo3, body="Test123")
>>> ch("Ping")
handle_message(MMDChannelMessage(chan_id=UUID('c5f1b7c0-5fde-11e0-9c3e-00219b38831e'), body=['pong2', 'Ping']))
>>> ch.close("Goodbye")
```

You should see the following on the service side:

```python
handle_subscribe(MMDChannelCreate(chan_id=UUID('c5f1b7c0-5fde-11e0-9c3e-00219b38831e'), chan_type='subscribe', service='echo3', body='Test123', auth_id=UUID('c5f187a0-5fde-11e0-9c3e-00219b38831e'), timeout=0))
handle_message(MMDChannelMessage(chan_id=UUID('c5f1b7c0-5fde-11e0-9c3e-00219b38831e'), body='Ping'))
handle_close(MMDChannelClose(chan_id=UUID('c5f1b7c0-5fde-11e0-9c3e-00219b38831e'), body='Goodbye'))
```
