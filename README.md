# WebSocket Client for Ruby

WebSocket-EventMachine-Client is Ruby WebSocket client based on EventMachine.

- [Docs](http://rdoc.info/github/imanel/websocket-eventmachine-client/master/frames)

## Installation

``` bash
gem install websocket-eventmachine-client
```

or in Gemfile

``` ruby
gem 'websocket-eventmachine-client'
```

## Simple client example

```ruby
EM.run do

  ws = WebSocket::EventMachine::Server.connect(:host => "0.0.0.0", :port => 8080)

  ws.onopen do
    puts "Connected"
  end

  ws.onmessage do |msg, type|
    puts "Received message: #{msg}"
  end

  ws.onclose do
    puts "Disconnected"
  end

  ws.send "Hello Server!"

end
```

## Options

Following options can be passed to WebSocket::EventMachine::Server initializer:

- `[String] :host` - IP on which server should accept connections. '0.0.0.0' means all.
- `[Integer] :port` - Port on which server should accept connections.
- `[Boolean] :secure` - Enable secure WSS protocol. This will enable both SSL encryption and using WSS url and require `tls_options` key.
- `[Boolean] :secure_proxy` - Enable secure WSS protocol over proxy. This will enable only using WSS url and assume that SSL encryption is handled by some kind proxy(like [Stunnel](http://www.stunnel.org/))
- `[Hash] :tls_options` - Options for SSL(according to [EventMachine start_tls method](http://eventmachine.rubyforge.org/EventMachine/Connection.html#start_tls-instance_method))
  - `[String] :private_key_file` - URL to private key file
  - `[String] :cert_chain_file` - URL to cert chain file

## Methods

Following methods are available for WebSocket::EventMachine::Server object:

### onopen

Called after client is connected.

Example:

```ruby
ws.onopen do
  puts "Client connected"
end
```

### onclose

Called after client closed connection.

Example:

```ruby
ws.onclose do
  puts "Client disconnected"
end
```

### onmessage

Called when server receive message.

Parameters:

- `[String] message` - content of message
- `[Symbol] type` - type is type of message(:text or :binary)

Example:

```ruby
ws.onmessage do |msg, type|
  puts "Received message: #{msg} or type: #{type}"
end
```

### onerror

Called when server discovers error.

Parameters:

- `[String] error` - error reason.

Example:

```ruby
ws.onerror do |error|
  puts "Error occured: #{error}"
end
```

### onping

Called when server receive ping request. Pong request is sent automatically.

Parameters:

- `[String] message` - message for ping request.

Example:

```ruby
ws.onping do |message|
  puts "Ping received: #{message}"
end
```

### onpong

Called when server receive pong response.

Parameters:

- `[String] message` - message for pong response.

Example:

```ruby
ws.onpong do |message|
  puts "Pong received: #{message}"
end
```

### send

Sends message to client.

Parameters:

- `[String] message` - message that should be sent to client
- `[Hash] params` - params for message(optional)
  - `[Symbol] :type` - type of message. Valid values are :text, :binary(default is :text)

Example:

```ruby
ws.send "Hello Client!"
ws.send "binary data", :type => :binary
```

### close

Closes connection and optionally send close frame to client.

Parameters:

- `[Integer] code` - code of closing, according to WebSocket specification(optional)
- `[String] data` - data to send in closing frame(optional)

Example:

```ruby
ws.close
```

### ping

Sends ping request.

Parameters:

- `[String] data` - data to send in ping request(optional)

Example:

```ruby
ws.ping 'Hi'
```

### pong

Sends pong request. Usually there should be no need to send this request, as pong responses are sent automatically by server.

Parameters:

- `[String] data` - data to send in pong request(optional)

Example:

``` ruby
ws.pong 'Hello'
```

## Migrating from EM-WebSocket

This library is compatible with EM-WebSocket, so only thing you need to change is running server - you need to change from EM-WebSocket to WebSocket::EventMachine::Server in your application and everything will be working.

## License

The MIT License - Copyright (c) 2012 Bernard Potocki
