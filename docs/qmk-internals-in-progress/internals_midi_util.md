# Midi Util

## Summary

| Members | Descriptions |
| :--- | :--- |
| `enum`[`midi_packet_length_t`](internals_midi_util.md#group__midi__util_1gae29ff56aee2b430ffe53933b97e5e79e) | An enumeration of the possible packet length values. |
| `public bool`[`midi_is_statusbyte`](internals_midi_util.md#group__midi__util_1ga12e3b42ff9cbb4b4f2bc455fc8743ee5)`(uint8_t theByte)` | Test to see if the byte given is a status byte. |
| `public bool`[`midi_is_realtime`](internals_midi_util.md#group__midi__util_1gad2f52c363e34a8000d80c983c324e2d7)`(uint8_t theByte)` | Test to see if the byte given is a realtime message. |
| `public`[`midi_packet_length_t`](internals_midi_util.md#group__midi__util_1gae29ff56aee2b430ffe53933b97e5e79e)```[``midi\_packet\_length`](#group__midi__util_1gaa168b43af6ae9de0debce1625e4b8175)`\(uint8\_t status\)\` | Find the length of the packet associated with the status byte given. |

## Members

### `enum`[`midi_packet_length_t`](internals_midi_util.md#group__midi__util_1gae29ff56aee2b430ffe53933b97e5e79e) <a id="group__midi__util_1gae29ff56aee2b430ffe53933b97e5e79e"></a>

| Values | Descriptions |
| :--- | :--- |
| UNDEFINED |  |
| ONE |  |
| TWO |  |
| THREE |  |

An enumeration of the possible packet length values.

### `public bool`[`midi_is_statusbyte`](internals_midi_util.md#group__midi__util_1ga12e3b42ff9cbb4b4f2bc455fc8743ee5)`(uint8_t theByte)` <a id="group__midi__util_1ga12e3b42ff9cbb4b4f2bc455fc8743ee5"></a>

Test to see if the byte given is a status byte.

### Parameters

* `theByte` the byte to test 

### Returns

true if the byte given is a midi status byte

### `public bool`[`midi_is_realtime`](internals_midi_util.md#group__midi__util_1gad2f52c363e34a8000d80c983c324e2d7)`(uint8_t theByte)` <a id="group__midi__util_1gad2f52c363e34a8000d80c983c324e2d7"></a>

Test to see if the byte given is a realtime message.

### Parameters

* `theByte` the byte to test 

### Returns

true if it is a realtime message, false otherwise

### `public`[`midi_packet_length_t`](internals_midi_util.md#group__midi__util_1gae29ff56aee2b430ffe53933b97e5e79e)```[``midi\_packet\_length`](#group__midi__util_1gaa168b43af6ae9de0debce1625e4b8175)`\(uint8\_t status\)\` <a id="group__midi__util_1gaa168b43af6ae9de0debce1625e4b8175"></a>

Find the length of the packet associated with the status byte given.

### Parameters

* `status` the status byte 

### Returns

the length of the packet, will return UNDEFINED if the byte is not a status byte or if it is a sysex status byte

