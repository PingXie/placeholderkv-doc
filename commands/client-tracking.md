This command enables the tracking feature of the Valkey server, that is used
for [server assisted client side caching](/topics/client-side-caching).

When tracking is enabled Valkey remembers the keys that the connection
requested, in order to send later invalidation messages when such keys are
modified. Invalidation messages are sent in the same connection (only available
when the RESP3 protocol is used) or redirected in a different connection
(available also with RESP2 and Pub/Sub). A special *broadcasting* mode is
available where clients participating in this protocol receive every
notification just subscribing to given key prefixes, regardless of the
keys that they requested. Given the complexity of the argument please
refer to [the main client side caching documentation](/topics/client-side-caching) for the details. This manual page is only a reference for the options of this subcommand.

In order to enable tracking, use:

    CLIENT TRACKING on ... options ...

The feature will remain active in the current connection for all its life,
unless tracking is turned off with `CLIENT TRACKING off` at some point.

The following are the list of options that modify the behavior of the
command when enabling tracking:

* `REDIRECT <id>`: send invalidation messages to the connection with the specified ID. The connection must exist. You can get the ID of a connection using `CLIENT ID`. If the connection we are redirecting to is terminated, when in RESP3 mode the connection with tracking enabled will receive `tracking-redir-broken` push messages in order to signal the condition.
* `BCAST`: enable tracking in broadcasting mode. In this mode invalidation messages are reported for all the prefixes specified, regardless of the keys requested by the connection. Instead when the broadcasting mode is not enabled, Valkey will track which keys are fetched using read-only commands, and will report invalidation messages only for such keys.
* `PREFIX <prefix>`: for broadcasting, register a given key prefix, so that notifications will be provided only for keys starting with this string. This option can be given multiple times to register multiple prefixes. If broadcasting is enabled without this option, Valkey will send notifications for every key. You can't delete a single prefix, but you can delete all prefixes by disabling and re-enabling tracking. Using this option adds the additional time complexity of O(N^2), where N is the total number of prefixes tracked. 
* `OPTIN`: when broadcasting is NOT active, normally don't track keys in read only commands, unless they are called immediately after a `CLIENT CACHING yes` command.
* `OPTOUT`: when broadcasting is NOT active, normally track keys in read only commands, unless they are called immediately after a `CLIENT CACHING no` command.
* `NOLOOP`: don't send notifications about keys modified by this connection itself.
