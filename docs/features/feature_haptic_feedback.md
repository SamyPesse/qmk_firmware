# Haptic Feedback

## Haptic feedback rules.mk options

The following options are currently available for haptic feedback in `rules.mk`:

`HAPTIC_ENABLE += DRV2605L`

`HAPTIC_ENABLE += SOLENOID`

## Known Supported Hardware

| Name | Description |
| :--- | :--- |
| [LV061228B-L65-A](https://www.digikey.com/product-detail/en/jinlong-machinery-electronics-inc/LV061228B-L65-A/1670-1050-ND/7732325) | z-axis 2v LRA |
| [Mini Motor Disc](https://www.adafruit.com/product/1201) | small 2-5v ERM |

## Haptic Keycodes

Not all keycodes below will work depending on which haptic mechanism you have chosen.

| Name | Description |
| :--- | :--- |
| `HPT_ON` | Turn haptic feedback on |
| `HPT_OFF` | Turn haptic feedback off |
| `HPT_TOG` | Toggle haptic feedback on/off |
| `HPT_RST` | Reset haptic feedback config to default |
| `HPT_FBK` | Toggle feedback to occur on keypress, release or both |
| `HPT_BUZ` | Toggle solenoid buzz on/off |
| `HPT_MODI` | Go to next DRV2605L waveform |
| `HPT_MODD` | Go to previous DRV2605L waveform |
| `HPT_CONT` | Toggle continuous haptic mode on/off |
| `HPT_CONI` | Increase DRV2605L continous haptic strength |
| `HPT_COND` | Decrease DRV2605L continous haptic strength |
| `HPT_DWLI` | Increase Solenoid dwell time |
| `HPT_DWLD` | Decrease Solenoid dwell time |

### Solenoids

First you will need a build a circuit to drive the solenoid through a mosfet as most MCU will not be able to provide the current needed to drive the coil in the solenoid.

[Wiring diagram provided by Adafruit](https://playground.arduino.cc/uploads/Learning/solenoid_driver.pdf)

| Settings | Default | Description |
| :--- | :--- | :--- |
| `SOLENOID_PIN` | _Not defined_ | Configures the pin that the Solenoid is connected to. |
| `SOLENOID_DEFAULT_DWELL` | `12` ms | Configures the default dwell time for the solenoid. |
| `SOLENOID_MIN_DWELL` | `4` ms | Sets the lower limit for the dwell. |
| `SOLENOID_MAX_DWELL` | `100` ms | Sets the upper limit for the dwell. |

?&gt; Dwell time is how long the "plunger" stays activated. The dwell time changes how the solenoid sounds.

Beware that some pins may be powered during bootloader \(ie. A13 on the STM32F303 chip\) and will result in the solenoid kept in the on state through the whole flashing process. This may overheat and damage the solenoid. If you find that the pin the solenoid is connected to is triggering the solenoid during bootloader/DFU, select another pin.

### DRV2605L

DRV2605L is controlled over i2c protocol, and has to be connected to the SDA and SCL pins, these varies depending on the MCU in use.

#### Feedback motor setup

This driver supports 2 different feedback motors. Set the following in your `config.h` based on which motor you have selected.

**ERM**

Eccentric Rotating Mass vibration motors \(ERM\) is motor with a off-set weight attached so when drive signal is attached, the off-set weight spins and causes a sinusoidal wave that translate into vibrations.

```text
#define FB_ERM_LRA 0
#define FB_BRAKEFACTOR 3 /* For 1x:0, 2x:1, 3x:2, 4x:3, 6x:4, 8x:5, 16x:6, Disable Braking:7 */
#define FB_LOOPGAIN 1 /* For  Low:0, Medium:1, High:2, Very High:3 */

/* Please refer to your datasheet for the optimal setting for your specific motor. */
#define RATED_VOLTAGE 3
#define V_PEAK 5
```

**LRA**

Linear resonant actuators \(LRA, also know as a linear vibrator\) works different from a ERM. A LRA has a weight and magnet suspended by springs and a voice coil. When the drive signal is applied, the weight would be vibrate on a single axis \(side to side or up and down\). Since the weight is attached to a spring, there is a resonance effect at a specific frequency. This frequency is where the LRA will operate the most efficiently. Refer to the motor's datasheet for the recommanded range for this frequency.

```text
#define FB_ERM_LRA 1
#define FB_BRAKEFACTOR 3 /* For 1x:0, 2x:1, 3x:2, 4x:3, 6x:4, 8x:5, 16x:6, Disable Braking:7 */
#define FB_LOOPGAIN 1 /* For  Low:0, Medium:1, High:2, Very High:3 */

/* Please refer to your datasheet for the optimal setting for your specific motor. */
#define RATED_VOLTAGE 2
#define V_PEAK 2.8
#define V_RMS 2.0 
#define V_PEAK 2.1
#define F_LRA 205 /* resonance freq */
```

#### DRV2605L waveform library

DRV2605L comes with preloaded library of various waveform sequences that can be called and played. If writing a macro, these waveforms can be played using `DRV_pulse(*sequence name or number*)`

List of waveform sequences from the datasheet:

| seq\# | Sequence name | seq\# | Sequence name | seq\# | Sequence name |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | strong\_click | 43 | lg\_dblclick\_med\_60 | 85 | transition\_rampup\_med\_smooth2 |
| 2 | strong\_click\_60 | 44 | lg\_dblsharp\_tick | 86 | transition\_rampup\_short\_smooth1 |
| 3 | strong\_click\_30 | 45 | lg\_dblsharp\_tick\_80 | 87 | transition\_rampup\_short\_smooth2 |
| 4 | sharp\_click | 46 | lg\_dblsharp\_tick\_60 | 88 | transition\_rampup\_long\_sharp1 |
| 5 | sharp\_click\_60 | 47 | buzz | 89 | transition\_rampup\_long\_sharp2 |
| 6 | sharp\_click\_30 | 48 | buzz\_80 | 90 | transition\_rampup\_med\_sharp1 |
| 7 | soft\_bump | 49 | buzz\_60 | 91 | transition\_rampup\_med\_sharp2 |
| 8 | soft\_bump\_60 | 50 | buzz\_40 | 92 | transition\_rampup\_short\_sharp1 |
| 9 | soft\_bump\_30 | 51 | buzz\_20 | 93 | transition\_rampup\_short\_sharp2 |
| 10 | dbl\_click | 52 | pulsing\_strong | 94 | transition\_rampdown\_long\_smooth1\_50 |
| 11 | dbl\_click\_60 | 53 | pulsing\_strong\_80 | 95 | transition\_rampdown\_long\_smooth2\_50 |
| 12 | trp\_click | 54 | pulsing\_medium | 96 | transition\_rampdown\_med\_smooth1\_50 |
| 13 | soft\_fuzz | 55 | pulsing\_medium\_80 | 97 | transition\_rampdown\_med\_smooth2\_50 |
| 14 | strong\_buzz | 56 | pulsing\_sharp | 98 | transition\_rampdown\_short\_smooth1\_50 |
| 15 | alert\_750ms | 57 | pulsing\_sharp\_80 | 99 | transition\_rampdown\_short\_smooth2\_50 |
| 16 | alert\_1000ms | 58 | transition\_click | 100 | transition\_rampdown\_long\_sharp1\_50 |
| 17 | strong\_click1 | 59 | transition\_click\_80 | 101 | transition\_rampdown\_long\_sharp2\_50 |
| 18 | strong\_click2\_80 | 60 | transition\_click\_60 | 102 | transition\_rampdown\_med\_sharp1\_50 |
| 19 | strong\_click3\_60 | 61 | transition\_click\_40 | 103 | transition\_rampdown\_med\_sharp2\_50 |
| 20 | strong\_click4\_30 | 62 | transition\_click\_20 | 104 | transition\_rampdown\_short\_sharp1\_50 |
| 21 | medium\_click1 | 63 | transition\_click\_10 | 105 | transition\_rampdown\_short\_sharp2\_50 |
| 22 | medium\_click2\_80 | 64 | transition\_hum | 106 | transition\_rampup\_long\_smooth1\_50 |
| 23 | medium\_click3\_60 | 65 | transition\_hum\_80 | 107 | transition\_rampup\_long\_smooth2\_50 |
| 24 | sharp\_tick1 | 66 | transition\_hum\_60 | 108 | transition\_rampup\_med\_smooth1\_50 |
| 25 | sharp\_tick2\_80 | 67 | transition\_hum\_40 | 109 | transition\_rampup\_med\_smooth2\_50 |
| 26 | sharp\_tick3\_60 | 68 | transition\_hum\_20 | 110 | transition\_rampup\_short\_smooth1\_50 |
| 27 | sh\_dblclick\_str | 69 | transition\_hum\_10 | 111 | transition\_rampup\_short\_smooth2\_50 |
| 28 | sh\_dblclick\_str\_80 | 70 | transition\_rampdown\_long\_smooth1 | 112 | transition\_rampup\_long\_sharp1\_50 |
| 29 | sh\_dblclick\_str\_60 | 71 | transition\_rampdown\_long\_smooth2 | 113 | transition\_rampup\_long\_sharp2\_50 |
| 30 | sh\_dblclick\_str\_30 | 72 | transition\_rampdown\_med\_smooth1 | 114 | transition\_rampup\_med\_sharp1\_50 |
| 31 | sh\_dblclick\_med | 73 | transition\_rampdown\_med\_smooth2 | 115 | transition\_rampup\_med\_sharp2\_50 |
| 32 | sh\_dblclick\_med\_80 | 74 | transition\_rampdown\_short\_smooth1 | 116 | transition\_rampup\_short\_sharp1\_50 |
| 33 | sh\_dblclick\_med\_60 | 75 | transition\_rampdown\_short\_smooth2 | 117 | transition\_rampup\_short\_sharp2\_50 |
| 34 | sh\_dblsharp\_tick | 76 | transition\_rampdown\_long\_sharp1 | 118 | long\_buzz\_for\_programmatic\_stopping |
| 35 | sh\_dblsharp\_tick\_80 | 77 | transition\_rampdown\_long\_sharp2 | 119 | smooth\_hum1\_50 |
| 36 | sh\_dblsharp\_tick\_60 | 78 | transition\_rampdown\_med\_sharp1 | 120 | smooth\_hum2\_40 |
| 37 | lg\_dblclick\_str | 79 | transition\_rampdown\_med\_sharp2 | 121 | smooth\_hum3\_30 |
| 38 | lg\_dblclick\_str\_80 | 80 | transition\_rampdown\_short\_sharp1 | 122 | smooth\_hum4\_20 |
| 39 | lg\_dblclick\_str\_60 | 81 | transition\_rampdown\_short\_sharp2 | 123 | smooth\_hum5\_10 |
| 40 | lg\_dblclick\_str\_30 | 82 | transition\_rampup\_long\_smooth1 |  |  |
| 41 | lg\_dblclick\_med | 83 | transition\_rampup\_long\_smooth2 |  |  |
| 42 | lg\_dblclick\_med\_80 | 84 | transition\_rampup\_med\_smooth1 |  |  |

### Optional DRV2605L defines

```text
#define DRV_GREETING *sequence name or number*
```

If haptic feedback is enabled, the keyboard will vibrate to a specific sqeuence during startup. That can be selected using the following define:

```text
#define DRV_MODE_DEFAULT *sequence name or number*
```

This will set what sequence HPT\_RST will set as the active mode. If not defined, mode will be set to 1 when HPT\_RST is pressed.

### DRV2605L Continuous Haptic Mode

This mode sets continuous haptic feedback with the option to increase or decrease strength.

