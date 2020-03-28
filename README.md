# PA communication protocol

This document describes the protocol used to communicate between
components of PA service.

## Basics

The protocol is message-based. Each message is a separate unit of
communication and has a source and a destination.

Each message is either sent by the "brain" of PA and should be
delivered to user, or sent by user and should be delivered to the
"brain". Messages sent by schedulers to notify "brain" about certain
events are considered to be sent by user.

Each message is a single sequence of bytes (which may include unicode)
ending with a single `LF` (`0x0A`) byte. Message cannot contain any
other `LF` bytes.

All beginning and trailing whitespace bytes should be discarded.

Empty message (only whitespace characters) should be ignored (the
"exactly one" message sent by service means there is exactly one
non-empty message).

Message body must be a valid JSON.

## Routing

To properly route messages within PA service these should have the
following two fields:

- `from`
- `to`

Messages sent by "brain" should have `from` field with the following
fields:

- `channel`: equal to "brain"
- `name`: name of PA instance

Messages sent by "brain" should have `to` field with at least the
following fields:

- `channel`: specific communication media

The `to` field might have other fields related to specific media type.

User-originated messages must have `to` field similar to
brain-originated `from` and vice-versa.

Messages can have other fields specific to different endpoint types.

## Specific protocol details
### Telegram messages

Messages from Telegram will have their `from` field having the
following fields:

- `channel`: equal to "tg"
- `user_id`: telegram user id of person who sent the message
- `chat_id`: telegram channel id from which the message is forwarded

Messages sent to Telegram channel should have at least the following
fields in their `to` field:

- `channel`: equal to "tg"
- `chat_id`: telegram channel id where the messages(s) should be posted
