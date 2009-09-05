* How to run sample

- Run sample Web Socket server (echo server) with:
  $ ruby web_socket.rb server ws://localhost:10081

- Run sample Web Socket client and type something:
  $ ruby web_socket.rb client ws://localhost:10081
  Ready
  hoge
  Sent: "hoge"
  Received: "hoge"


* Usage example

Server:

  # Runs the server at port 10081.
  server = WebSocketServer.new("ws://example.com:10081")
  server.run() do |ws|
    # The block is called for each connection.
    # Checks origin (host name of the Web page the Web Socket is used).
    if ws.origin == "http://example.com"
      # Checks requested path.
      if ws.path == "/"
        # Call ws.handshake() without argument first.
        ws.handshake()
        # Receives one message from the client as String.
        while data = ws.receive()
          puts(data)
          # Sends the message to the client.
          ws.send(data)
        end
      else
        # You can call ws.handshake() with argument to return error status.
        ws.handshake("404 Not Found")
      end
    else
      ws.handshake("403 Forbidden")
    end
  end

Client:

  # Connects to Web Socket server at host example.com port 10081.
  client = WebSocket.new("ws://example.com:10081/")
  # Sends a message to the server.
  client.send("Hello")
  # Receives a message from the server.
  data = client.receive()
  puts(data)


* Tips: JavaScript client implementation

No browser natively supports Web Socket so far as far as I know. For now, you can use an implementation using Flash:
http://github.com/gimite/web-socket-js/tree/master


* License

New BSD License.