Subscribes the client to the given patterns.

Supported glob-style patterns:

* `h?llo` subscribes to `hello`, `hallo` and `hxllo`
* `h*llo` subscribes to `hllo` and `heeeello`
* `h[ae]llo` subscribes to `hello` and `hallo,` but not `hillo`

Use `\` to escape special characters if you want to match them verbatim.

Once the client enters the subscribed state it is not supposed to issue any other commands, except for additional `SUBSCRIBE`, `SSUBSCRIBE`, `PSUBSCRIBE`, `UNSUBSCRIBE`, `SUNSUBSCRIBE`, `PUNSUBSCRIBE`, `PING`, `RESET` and `QUIT` commands.
However, if RESP3 is used (see `HELLO`) it is possible for a client to issue any commands while in subscribed state.

Note that `RESET` can be called to exit subscribed state.

For more information, see [Pub/sub](/docs/interact/pubsub/).

