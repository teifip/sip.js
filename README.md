This is an experimental fork of the [sip.js](https://github.com/kirm/sip.js) library authored and maintained by [Kirill Mikhailov](https://github.com/kirm).

This forked version is intended to enable support for binary content, as needed for the transport of SMS messages (`content-type` equal to `application/vnd.3gpp.sms`).

All the changes introduced by this fork are in the `sip.js` module.

# API changes

All APIs remain as in the [original library](https://github.com/kirm/sip.js/blob/master/doc/api.markdown) except for the following backward compatible changes.

### sip.start(options, onRequest)

A new optional property has been defined for the `options` object:
- `binaryTypes` - array of strings specifying the content types that should be handled as binary content when parsing a message; example `options.binaryTypes = ['application/vnd.3gpp.sms']`. Default to the empty list.

### sip.send(message[, callback])

The `content` property of the `message` object is now handled in the following manner:
- If the specified `content` is a string, then the behavior of the original [kirm/sip.js](https://github.com/kirm/sip.js) library is retained;
- If the specified `content` is a buffer, then it is handled as binary content. This holds true regardless of whether the `message` object has a `content-type` property, and - if so - regardless of the value of the `content-type` property.

### sip.stringify(message)

In case the `message` object does not have a `content` property or the `content` property is a string, then the behavior of the original [kirm/sip.js](https://github.com/kirm/sip.js) library is retained. Instead, if the `content` property is a buffer, then it is converted into a string of hex characters so that it can be conveniently displayed/logged.


### sip.parse(message[, binaryTypes])

A new optional input parameter `binaryTypes` has been introduced. This has exactly the same definition as the one used with `sip.start` (see above).

# Remarks

The original [kirm/sip.js](https://github.com/kirm/sip.js) library is compatible with Node.js v0.2.2 or later while - in its present experimental form - this fork restricts compatibility to Noje.js v6 or later.
This restriction can be easily relaxed by reverting from the use of `Buffer.from()` and `Buffer.alloc()` to the deprecated use of the `new Buffer()` constructor. However, this fork also uses the `Buffer.indexOf()` method which is not available in Node.js versions prior to v1.5.0.
