# ASIO Networking `<asio.hpp>`
- Socket: Comms Endpoint
- Bind: Attach local address (server only)
- Listen: Announce ready to accept (server only)
- Accept: Block until connection arrives (server only)
- Connect: Establish connection (client only)
- Read: Read data
- Write: Write data
- Close: Terminate connection

## Client setup
Context and Socket:
```C++
asio::io_context context{};
asio::ip::tcp::socket socket{context};
```
DNS and Connect:
```C++
auto address = asio::ip::make_address("127.0.0.1");
auto endpoint = asio::ip::tcp::endpoint(address, <port>);
asio::connect(endpoint);
//with dns:
asio::ip::tcp::resolver{context};
auto endpoints = resolver.resolve(<domain>, "<port_string>");
asio::connect(socket, endpoints);
```

## Termination
```C++
//close stream
//alt: shutdown_read/shutdown_write
socket.shutdown(asio::ip::tcp::socket::shutdown_both);
socket.close();
```

## Server Setup
Context, Endpoint and Socket:
```C++
asio::io_context context{};
asio::ip::tcp::endpoint localEndpoint{asio::ip::tcp::v4(), <port>};
asio::ip::tcp::acceptor acceptor{context, localEndpoint};
```

Synchronous-Accept:
```C++
asio::ip::tcp::endpoint peerEndpoint{};
asio::ip::tcp::socket peerSocket = acceptor.accept(peerEndpoint);
```

## Buffers
```C++
//fixed size with standard containers as backend
std::ostringstream request{};
request << "...";
asio::buffer{request.str()};

//dynamically sized with std::vector / std::string
std::string memory{};
asio::dynamic_buffer{memory};
//streambuf buffers
auto transfer_buffer = asio::streambuf{};
auto request_stream = std::ostream {&transfer_buffer};
auto response_stream = std::istream{&transfer_buffer};
```

## Reads & Writes
```C++
asio::write(socket, <buffer>[,ec]);
asio::read(socket, <buffer>[,ec][,condition]);
asio::read_until(socket, <buffer>, <delimiter>[,ec]);

asio::async_write(socket, <buffer>, <handler>);
asio::async_read(socket, <buffer>, <handler>);
asio::async_read_until(socket, <buffer>, <delimiter>, <handler>);

```
- condition: `asio::transfer_all()` / `_at_least(std::size_t bytes)` / `exactly(std::size_t bytes)`

## Asynchronous Connect
```C++
asio::async_connect(socket, <endpoint>, <handler(ec)>)
```

## Asynchronous Acceptor
```C++
struct Server {
	using tcp = asio::ip::tcp;
	Server(asio::io_context & context, unsigned short port)
		: acceptor{context, tcp::endpoint{tcp::v4(), port}}{
			accept();
		}
private:
	void accept() {
		auto acceptHandler = [this] (asio::error_code ec, tcp::socket peer) {
			if(!ec) {
				auto session = std::make_shared<Session>(std::move(peer));
				session->start();
			}
			accept();
		};
		acceptor.async_accept(acceptHandler);
	}
	tcp::acceptor acceptor;
}
int main() {
	asio::io_context context{};
	Server server{context, <port>};
	context.run(); //blocks until no async op is left
}
```
- Should be kept alive by caller
## Asynchronous Session
```c++
struct Session : std::enable_shared_from_this<Session> {
	explicit Session(asio::ip::tcp::socket socket)
		
private:
	void read() {
		auto handler = [self = shared_from_this()](asio::error_code ec, size_t length) {/*... write ...*/};
		asio::async_read(socket, buffer, handler);
	}
	void write(std::string input) {
		auto data = std::make_shared<std::string>(data);
		auto handler = [self = shared_from_this(), data](asio::error_code, size_t length) {/*... read ...*/};
		asio::async_write(socket, buffer, handler);
	}
	asio::streambuf buffer{};
	asio::istream input{&buffer};
	asio::ip::tcp::socket socket;
}
```
- Keeps itself alive by pointer