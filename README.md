[![GoDoc](https://godoc.org/github.com/pdf/websocketrwc?status.svg)](http://godoc.org/github.com/pdf/websocketrwc) ![License-MIT](http://img.shields.io/badge/license-MIT-red.svg)

# websocketrwc
--
    import "github.com/pdf/websocketrwc"

Package websocketrwc wraps Gorilla websocket in an io.ReadWriteCloser compatible
interface, allowing it to be used as a net/rpc or similar connection. Only the
server side is currently implemented.

## Usage

```go
var (
	// ErrClosing is returned for operations during close phase.
	ErrClosing = errors.New(`Closing`)
	// DefaultUpgrader will be used if a nil upgrader is passed to Upgrade().
	DefaultUpgrader = &websocket.Upgrader{
		HandshakeTimeout:  defaultHandshakeTimeout,
		ReadBufferSize:    defaultBufferSize,
		WriteBufferSize:   defaultBufferSize,
		EnableCompression: true,
		CheckOrigin:       func(r *http.Request) bool { return true },
	}
	// WriteTimeout determines the period of time a write will wait to complete
	// before producing an error.
	WriteTimeout = 10 * time.Second
	// PongTimeout determines the period of time a connection will wait for a
	// Pong response to Pings sent to the client.
	PongTimeout = 60 * time.Second
	// PingInterval determins the interval at which Pings are sent to the
	// client.
	PingInterval = (PongTimeout * 9) / 10
)
```

#### type Conn

```go
type Conn struct {
}
```

Conn wraps gorilla websocket to provide io.ReadWriteCloser.

#### func  Upgrade

```go
func Upgrade(w http.ResponseWriter, r *http.Request, h http.Header, upgrader *websocket.Upgrader) (*Conn, error)
```
Upgrade a HTTP connection.

#### func (*Conn) Close

```go
func (c *Conn) Close() error
```
Close implements io.Closer and closes the underlying connection.

#### func (*Conn) Read

```go
func (c *Conn) Read(p []byte) (n int, err error)
```
Read implements io.Reader by wrapping websocket messages in a buffer.

#### func (*Conn) Write

```go
func (c *Conn) Write(p []byte) (n int, err error)
```
Write implements io.Writer and sends binary messages only.
