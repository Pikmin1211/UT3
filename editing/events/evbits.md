# EVBITs

EVBITs are a set of 13 flags that do various event-related things when set. They can be set using the opcodes `EVBIT_T evbit`, unset with `EVBIT_F evbit`. They are as follows:

| EVBIT | Effect                       |
|-------|------------------------------|
| 0     | Ignore queued scenes         |
| 1     | Scene is active              |
| 2     | Skipping scene               |
| 3     | Skipping text                |
| 4     | Prevent skipping scenes      |
| 5     | Prevent skipping text        |
| 6     | Prevent fast-forwarding text |
| 7     | No end fade                  |
| 8     | Faded in                     |
| 9     | Camera follows moving units  |
| 10    | Moving to another chapter    |
| 11    | Changing game mode           |
| 12    | GFX locked                   |


Most of these are controlled automatically in the use of other event codes and have no reason to be manually flipped, but some are only flipped manually. EVBIT 9, when enabled, causes the camera to follow moving units, which is useful during cutscene events where you want this to happen. EVBIT 7 is perhaps the most ubiquitous EVBIT: when set just before the end of the event, it will prevent the fade to black and back that otherwise occurs at the end of events.
Others are tied directly to the effects of different opcodes: 0 is `ENDB`, 10 and 11 are the `MNCH` family, 12 is a pair of dedicated opcodes, 8 is used by fades, etc. All of the skipping-related EVBITs can be controlled using the opcode `EVBIT_MODIFY`, which takes the following parameters:
| Parameter | Scene skipping | Dialogue skipping | Dialogue fast-forwarding |
|-----------|----------------|-------------------|--------------------------|
| 0         | ✔              | ✔                 | ✔                        |
| 1         | ❌              | ❌                 | ❌                        |
| 2         | ✔              | ✔                 | ❌                        |
| 3         | ❌              | ✔                 | ✔                        |
| 4         | ❌              | ❌                 | ✔                        |



