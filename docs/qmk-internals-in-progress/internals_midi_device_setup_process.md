# Midi Device Setup Process

These are method that you must use to initialize and run a device.

## Summary

| Members | Descriptions |
| :--- | :--- |
| `public void`[`midi_device_init`](internals_midi_device_setup_process.md#group__midi__device__setup__process_1gaf29deddc94ea98a59daa0bde1aefd9d9)`(`[`MidiDevice`](internals_midi_device_setup_process.md#struct__midi__device)`* device)` | Initialize a device. |
| `public void`[`midi_device_process`](internals_midi_device_setup_process.md#group__midi__device__setup__process_1gaa3d5993d0e998a1b59bbf5ab9c7b492b)`(`[`MidiDevice`](internals_midi_device_setup_process.md#struct__midi__device)`* device)` | Process input data. |

## Members

### `public void`[`midi_device_init`](internals_midi_device_setup_process.md#group__midi__device__setup__process_1gaf29deddc94ea98a59daa0bde1aefd9d9)`(`[`MidiDevice`](internals_midi_device_setup_process.md#struct__midi__device)`* device)` <a id="group__midi__device__setup__process_1gaf29deddc94ea98a59daa0bde1aefd9d9"></a>

Initialize a device.

You must call this before using the device in question.

### Parameters

* `device` the device to initialize

### `public void`[`midi_device_process`](internals_midi_device_setup_process.md#group__midi__device__setup__process_1gaa3d5993d0e998a1b59bbf5ab9c7b492b)`(`[`MidiDevice`](internals_midi_device_setup_process.md#struct__midi__device)`* device)` <a id="group__midi__device__setup__process_1gaa3d5993d0e998a1b59bbf5ab9c7b492b"></a>

Process input data.

This method drives the input processing, you must call this method frequently if you expect to have your input callbacks called.

### Parameters

* `device` the device to process

