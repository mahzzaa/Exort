---
title: Serial Monitor & Serial Plotter
description: Read board logs, adjust baud rate, and plot numeric streams from the serial connection.
order: 7
section: Serial Monitor & Serial Plotter
---

# Serial Monitor & Serial Plotter

Serial Monitor shows live output from your connected board. Serial Plotter takes numeric streams from that same connection and turns them into a graph.

## How To Connect

Select the board and port first, then open the serial view for the active workspace.

If nothing appears, check:

- the board is powered
- the selected port is correct
- the sketch was uploaded successfully
- another app is not holding the port open

## Baud Rate

The baud rate in Exort must match the baud rate in your sketch.

For example, if your code uses:

```cpp
Serial.begin(115200);
```

then the monitor should also use `115200`.

## Viewing Logs

- Debug prints
- Boot logs
- Sensor values
- Calibration messages
- Runtime state
- Error messages
- Communication output

Example firmware:

```cpp
void setup() {
  Serial.begin(115200);
  Serial.println("Device started");
}

void loop() {
  int value = analogRead(A0);
  Serial.println(value);
  delay(500);
}
```

## Plotting Numeric Streams

Serial Plotter works best when you print plain numbers, one sample per line, or a stable set of comma-separated numbers.

Good plotter output:

```text
123
124
125
```

Or:

```text
123,456
124,458
125,459
```

Bad plotter output:

```text
Sensor value is: 123
Temperature now equals 24.3 C
```

## Common Serial Problems

- no output because the wrong port is selected
- unreadable text because the baud rate does not match
- nothing updates because the board did not finish booting
- data freezes because another app owns the port
