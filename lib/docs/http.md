[[Docs](https://iojs.org/api/http.html)]

-  http server timing change (Use `'finish'` instead of `'prefinish'` when ending a response)
  - When doing HTTP pipelining of requests, the server creates new request and response objects for each incoming HTTP request on the socket. Starting from 3.0.0, these objects are now created a couple of ticks later, when we are certainly done processing the previous request. This could change the observable timing with certain HTTP server coding patterns.
  - The switch to `'finish'` is intended to prevent the socket being detached from the response until after the data is actually sent to the other side.
  - Refs: [`6020d2`](https://github.com/nodejs/io.js/commit/6020d2a2fb75de6766c807864fa8f1c0fba88ec9), [#1411](https://github.com/nodejs/io.js/pull/1411)
- Removed default chunked encoding on `DELETE` and `OPTIONS` requests.
  - Refs: [`aef0960`](https://github.com/nodejs/node/commit/aef09601b4320648f4b6fac0baba3e5c54b0406a), [`1df32af`](https://github.com/nodejs/node/commit/1df32af74a450d456e264afbbaf6201a388d7572)
- [`http.STATUS_CODES`](https://iojs.org/api/http.html#http_http_status_codes) now maps to the regulated [IANA standard](http://www.iana.org/assignments/http-status-codes/http-status-codes.xhtml) set of codes.
  - Updating the code mappings to conform to the IANA standard was a backwards incompatible change to consumers who depend on the text value of a header. The most significant of these changes was the text for `302`, which was previously `Moved Temporarily` but is now `Found`.
  - Refs: [`235036e`](https://github.com/nodejs/io.js/commit/235036e7faa537469c3600850bdd533c095c392a), [#1470](https://github.com/nodejs/io.js/pull/1470)
- [`http.Agent.prototype.getName()`](https://iojs.org/api/http.html#http_agent_getname_options) no longer returns an extra trailing colon.
  - Refs: [`06cc919`](https://github.com/nodejs/io.js/commit/06cc919a0cd41380a83e4ea699f1a9ea30881266), [#1617](https://github.com/nodejs/io.js/pull/1617)
- `http.OutgoingMessage.prototype.flush()` has been deprecated in favor of the new [`flushHeaders()`](https://iojs.org/api/http.html#http_request_flushheaders) method. The behavior remains the same.
  - Note: these methods reside on **both** the common http `request` and `response` objects.
  - Refs: [`b2e00e3`](https://github.com/nodejs/node/commit/b2e00e38dcac23cf564636e8fad880829489286c), [`joyent/node@89f3c90`](https://github.com/joyent/node/commit/89f3c9037faf19eb32c464b2e02a0a9191156c36)
- [`http.OutgoingMessage.prototype.write()`](https://iojs.org/api/http.html#http_response_write_chunk_encoding_callback) will now emit an error on the `OutgoingMessage` (`response`) object if it has been called after [`end()`](https://iojs.org/api/http.html#http_response_end_data_encoding_callback).
  - Refs: [`64d6de9`](https://github.com/nodejs/node/commit/64d6de9f34abe63bf7602ab0b55ff268cf480e45)
- [`http.request()`](https://iojs.org/api/http.html#http_http_request_options_callback) and [`http.get()`](https://iojs.org/api/http.html#http_http_get_options_callback) wrongly decode multi-byte string characters as `'binary'`.
  - Notes: This bug still exists in 4.0.0.
  - Refs: [#2114](https://github.com/nodejs/node/issues/2114), [joyent/node#25634 (comment)](https://github.com/joyent/node/issues/25634#issuecomment-118970793)
- [`http.request()`](https://iojs.org/api/http.html#http_http_request_options_callback) and [`http.get()`](https://iojs.org/api/http.html#http_http_get_options_callback) now throw a TypeError if the requested path contains illegal characters. (Currently only `' '`).
- Refs: [`7124387`](https://github.com/nodejs/node/commit/7124387b3414c41533078f14a84446e2e0a6ff95)
