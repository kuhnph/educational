
### Old message format

| Message                           | Bits | Decoder use                                               |
| --------------------------------- | ---- | --------------------------------------------------------- |
| `Raw_Data_Frame--Msg_Number`      | 24   | Frame sequencing and reconstructing multi-byte mux values |
| `Raw_Data_Frame--MeasurementTime` | —    | Timestamp associated with decoded data                    |
| `accel_mux_tag`                   | 8    | Identifies the accelerometer/IE mux value                 |
| `accel_mux_word`                  | 8    | Accelerometer/IE mux data byte                            |
| `gyro--status`                    | 8    | Packed gyro status + mux byte counter                     |
| `gyro--mux_tag`                   | 8    | Identifies gyro mux value                                 |
| `gyro--lsb`                       | 8    | Gyro theta LSB                                            |
| `gyro--msb`                       | 8    | Gyro theta MSB                                            |
| `gyro--mux_word`                  | 8    | Gyro mux data byte                                        |
| `gyro--checksum`                  | 8    | Gyro message checksum                                     |

### New message format

| Message                            | Bits | Decoder use                                                                                                             |
| ---------------------------------- | ---- | ----------------------------------------------------------------------------------------------------------------------- |
| `Raw_Data--Message_Number`         | 16   | Frame sequencing and reconstructing multi-byte mux values                                                               |
| `Raw_Data--Time_Of_Validity`       | —    | Timestamp associated with decoded data                                                                                  |
| `Accel--Mux_Tag`                   | 8    | Identifies accelerometer/IE mux value                                                                                   |
| `Accel--Mux_Data`                  | 8    | Accelerometer/IE mux data byte                                                                                          |
| `Gyro--Status--Status`             | 2    | Overall gyro status                                                                                                     |
| `Gyro--Status--Readout_BIT_Status` | 1    | Readout BIT flag                                                                                                        |
| `Gyro--Status--Current_BIT_Status` | 1    | Current-control BIT flag                                                                                                |
| `Gyro--Status--Dither_BIT_Status`  | 1    | Dither BIT flag *Part of ring laser gyro. Prevents Lock-in at low rotations*                                            |
| `Gyro--Status--PLC_BIT_Status`     | 1    | PLC BIT flag *Path length control. Keep optical path traveled by the two counter-rotating lasers at the correct length* |
| `Gyro--Status--Status_ID_Counter`  | 2    | Determines mux-byte position                                                                                            |
| `Gyro--Mux_Tag`                    | 8    | Identifies gyro mux value                                                                                               |
| `Gyro--Theta_LSB`                  | 8    | Gyro theta LSB                                                                                                          |
| `Gyro--Theta_MSB`                  | 8    | Gyro theta MSB                                                                                                          |
| `Gyro--Mux_Byte`                   | 8    | Actual Gyro Mux data Byte                                                                                               |
| `Gyro--Checksum`                   | 8    | Gyro checksum                                                                                                           |

# HG9900 Accelerometer Mux Example

## Example Raw Bytes

For one IMU frame, assume the accelerometer mux fields are:

```
Mux_Tag  = 0000 0000
Mux_Data = 0000 0101
```

Both values are **8-bit bytes**.

---

## 1. Interpret the Mux Tag

The first byte is:

```
Mux_Tag = 0000 0000
```

Interpreted as an unsigned binary number:

```
0000 0000 = decimal 0
```

The decoder uses this number as an index into its mux lookup table.

For this example:

```
Tag 0 -> accel_bit1
```

This tells the decoder:

> The accompanying `Mux_Data` byte contains the Accelerometer 1 Built-In Test (BIT) status.

The mux tag therefore answers:

> **What does this data byte represent?**

---

## 2. Interpret the Mux Data

The data byte is:

```
Mux_Data = 0000 0101
```

Because the mux tag told us this is `accel_bit1`, the individual bits are interpreted according to the Accelerometer 1 BIT definition.

```
Bit number:   7 6 5 4 3 2 1 0
Mux_Data:     0 0 0 0 0 1 0 1
```

The decoder assigns the bits as:

|Bit|Status|
|---|---|
|0|ATS|
|1|AOM|
|2|AAT|
|3|ANT|
|4|ADC|
|5|ASA|
|6|ASD|
|7|ASR|

Therefore:

```
Mux_Data = 0000 0101
```

produces:

```
ATS = 1
AOM = 0
AAT = 1
ANT = 0
ADC = 0
ASA = 0
ASD = 0
ASR = 0
```

Only bits 0 and 2 are set to `1`.

---

## Complete Interpretation

The complete sequence is:

```
Mux_Tag
0000 0000
    |
    v
Decimal 0
    |
    v
Mux lookup table
    |
    v
"Accelerometer 1 BIT status"
```

The second byte is then interpreted using that definition:

```
Mux_Data
0000 0101
    |
    v
Accelerometer 1 BIT interpretation
    |
    +--> Bit 0 = ATS = 1
    |
    +--> Bit 1 = AOM = 0
    |
    +--> Bit 2 = AAT = 1
    |
    +--> Remaining bits = 0
```

---

## Key Concept

The two bytes have different purposes:

```
Mux_Tag  = 0000 0000
           ^
           |
           Tells the decoder WHAT the data represents


Mux_Data = 0000 0101
           ^
           |
           Contains the actual value/status information
```

The `Mux_Tag` and `Mux_Data` bytes are **not combined together**.

Instead:

1. Read `Mux_Tag`.
    
2. Convert the tag into its numeric value.
    
3. Look up what that tag represents.
    
4. Interpret `Mux_Data` according to that definition.
    

### One-line summary

> The mux tag identifies the type of parameter being transmitted, while the mux data byte contains the actual data for that parameter.


# HG9900 Gyro Checksum Example

## Purpose

The gyro checksum is an **8-bit error-detection value** used to verify that the gyro message was received correctly.

The legacy decoder calculates the expected checksum from:

```
Gyro Status Byte
Gyro Mux Tag
Gyro Theta LSB
Gyro Theta MSB
Gyro Mux Byte
```

The process is:

```
Add the five values
        ↓
Keep only the lowest 8 bits
        ↓
Invert every bit
        ↓
Expected checksum
```

---

## Example Input Values

Assume one gyro frame contains:

```
Status Byte = 0xC1
Mux Tag     = 0x03
Theta LSB   = 0x20
Theta MSB   = 0x01
Mux Byte    = 0x10
```

Convert these hexadecimal values to decimal:

```
0xC1 = 193
0x03 =   3
0x20 =  32
0x01 =   1
0x10 =  16
```

---

## Step 1: Add the Values

```
193 + 3 + 32 + 1 + 16 = 245
```

245 in hexadecimal is:

```
245 = 0xF5
```

And in binary:

```
0xF5 = 1111 0101
```

---

## Step 2: Keep the Lowest 8 Bits

In this example, the result is already only 8 bits:

```
1111 0101
```

So the value remains:

```
0xF5
```

If the sum were larger than 255, only the lowest 8 bits would be retained.

Example:

```
300 decimal = 0000 0001 0010 1100
                         ^^^^^^^^
                         keep these
```

The retained byte would be:

```
0010 1100
```

---

## Step 3: Invert the Bits

The checksum uses the bitwise complement of the 8-bit sum.

Starting with:

```
1111 0101
```

Invert each bit:

```
1111 0101
↓
0000 1010
```

So:

```
0000 1010 = 0x0A
```

Therefore:

```
Expected Checksum = 0x0A
```

---

## Step 4: Compare Against the Received Checksum

If the received gyro message contains:

```
gyro_checksum = 0x0A
```

then:

```
Expected = 0x0A
Received = 0x0A
```

The checksum passes.

If instead the received checksum is:

```
gyro_checksum = 0x0B
```

then:

```
Expected = 0x0A
Received = 0x0B
```

The checksum fails.

---

## MATLAB Logic

The decoder performs the equivalent of:

```
gyroRawSum = uint16( ...
    gyroStatusByte + ...
    gyroMuxTag + ...
    gyroThetaLsb + ...
    gyroThetaMsb + ...
    gyroMuxWord);

gyroSum = uint8(bitand(gyroRawSum, uint16(0xFF)));

checksumExpected = bitcmp(gyroSum);

isValid = gyroChecksum == checksumExpected;
```

---

## Summary

```
Status Byte = 0xC1
Mux Tag     = 0x03
Theta LSB   = 0x20
Theta MSB   = 0x01
Mux Byte    = 0x10

Add:
0xC1 + 0x03 + 0x20 + 0x01 + 0x10
= 0xF5

Binary:
0xF5 = 1111 0101

Invert:
1111 0101
↓
0000 1010

Expected Checksum:
0x0A
```

> The gyro checksum is generated by summing the relevant gyro message bytes, keeping the lowest 8 bits, and taking the bitwise complement.

# Step 1 — Update Raw HG9900 Input Mapping

## Goal

Update `getHg9900ParameterList` so the verifier reads the **new HG9900 telemetry field names** while preserving the internal MATLAB names expected by the decoder.

## Main Changes

- Updated gyro axes from the old numbering scheme to the new telemetry naming:
    
    - `Gyro_U` → internal `gyro1`
    - `Gyro_V` → internal `gyro2`
    - `Gyro_W` → internal `gyro3`
- Updated the gyro mux fields:
    
    - old `mux_tag` → new `Mux_Tag`
    - old `mux_word` → new `Mux_Byte`
- Updated gyro angle fields:
    
    - old `lsb` → new `Theta_LSB`
    - old `msb` → new `Theta_MSB`
- Kept the gyro checksum mapped to the existing internal checksum field.
    
- Replaced the old packed 8-bit gyro `status` input with the new split status fields:
    
    - `Status`
    - `Readout_BIT_Status`
    - `Current_BIT_Status`
    - `Dither_BIT_Status`
    - `PLC_BIT_Status`
    - `Status_ID_Counter`

## Important Design Choice

The **external telemetry names changed**, but the existing internal MATLAB structure names were retained where practical.

```
New telemetry field
        ↓
getHg9900ParameterList
        ↓
existing internal MATLAB name
        ↓
decodeHg9900RawMux
```

This minimizes the amount of downstream decoder code that needs to change.

## Verification

A mapping test was added to confirm:

```
Gyro_U → gyro1
Gyro_V → gyro2
Gyro_W → gyro3
```

and that all expected new raw HG9900 fields are mapped correctly.

> **Step 1 result:** The verifier can now ingest the new HG9900 gyro message layout without requiring a complete rewrite of the decoder

# Step 2 — Update Gyro Status Handling

## Goal

Update `decodeHg9900RawMux` so it uses the **new split gyro status fields directly** instead of extracting them from the old packed 8-bit status byte.

## Main Changes

The old format provided one packed byte:

```
[ Status ][Readout][Current][Dither][PLC][Counter]
   2 bits     1       1       1      1    2 bits
```

The decoder previously used masks and bit shifts to separate those values.

The new message format provides them individually:

```
Status
Readout_BIT_Status
Current_BIT_Status
Dither_BIT_Status
PLC_BIT_Status
Status_ID_Counter
```

Step 2 therefore changed the decoder to read and store those fields directly.

## Old Behavior

```
8-bit packed status byte
        ↓
bit masks / bit shifts
        ↓
individual status values
```

For example:

```
STATUS_BYTE_CURCTRL_MASK = bin2dec('00010000');
```

was used to extract the current-control bit.

## New Behavior

```
individual telemetry fields
        ↓
read directly
        ↓
individual processed status outputs
```

No masking or shifting is required for these status values anymore.

The resulting processed fields remain the same conceptually, such as:

```
gyro_status
bit_readout_flg
bit_curr_ctrl_flg
bit_dither_flg
bit_plc_flg
```

## Important Design Choice

The decoder still produces the same processed status outputs expected downstream.

Only the **source of those values changed**:

```
Old:
packed byte → decode bits → processed fields

New:
split fields → processed fields
```

This keeps the rest of the verifier interface as unchanged as possible.

## Checksum Note

The packed status byte was also used by the legacy checksum calculation.

For now, checksum handling was intentionally left unresolved because it has not yet been confirmed whether the new message format uses the same legacy checksum definition.

> **Step 2 result:** The decoder now handles the new split gyro status fields directly while preserving the existing processed status outputs.


# Step 3 — Update Gyro Mux Counter Logic

## Goal

Update the gyro mux decoding logic so it uses the new **`Status_ID_Counter` field directly** to determine the byte position of each gyro mux value.

## Main Change

In the old message format, the mux-byte counter was stored in the lowest two bits of the packed gyro status byte:

```
[ Status ][R][C][D][P][ Counter ]
                         2 bits
```

The decoder therefore extracted the counter using a bit mask:

```
bitand(gyroStatusByte, STATUS_BYTE_CNT_MASK)
```

The counter identified mux-byte positions:

```
0 → byte 0
1 → byte 1
2 → byte 2
3 → byte 3
```

This allowed the decoder to reconstruct 16-bit and 32-bit gyro mux values from multiple 8-bit `Mux_Byte` fields. Pasted text(3)

## New Behavior

The new message format provides that 2-bit value directly as:

```
Status_ID_Counter
```

So the mux decoder no longer needs to recover it from the packed status byte.

Conceptually, the change is:

```
Old:
packed status byte
        ↓
mask bits 1:0
        ↓
mux-byte position

New:
Status_ID_Counter
        ↓
mux-byte position
```

The new field is loaded as `gyroStatusIdCounter` along with the other split status fields. Pasted text(3)

## Mux Reconstruction

For each gyro mux tag, the decoder uses the counter to identify the appropriate byte:

```
Mux_Tag + Counter 0 → byte 0
Mux_Tag + Counter 1 → byte 1
Mux_Tag + Counter 2 → byte 2
Mux_Tag + Counter 3 → byte 3
```

The existing logic that checks for **sequential message numbers** and stitches the bytes together into complete 16-bit or 32-bit mux values remains unchanged. Pasted text(3)

> **Step 3 result:** The gyro mux decoder now obtains mux-byte position directly from `Status_ID_Counter` rather than extracting the counter from the legacy packed gyro status byte.



# Step 4 — Verify New Decoder Output Against Legacy Behavior

## Goal

Verify that the updated `decodeHg9900RawMux` produces the **same processed gyro mux and status outputs** as the old decoder/mock when both are given equivalent input data.

## What Was Verified

The test compares the processed outputs from:

```
Legacy input format
        ↓
Old packed status handling
        ↓
Legacy processed outputs
```

against:

```
New input format
        ↓
Split status fields + Status_ID_Counter
        ↓
Updated processed outputs
```

The goal is not to make the raw inputs identical. The raw message format changed. The goal is to confirm that the **final decoded information remains equivalent**.

## Status Verification

The old packed status byte contained:

```
Status
Readout BIT
Current BIT
Dither BIT
PLC BIT
Status ID Counter
```

The new message supplies those fields separately.

Step 4 verifies that both approaches produce the same processed status values, such as:

```
gyro_status
bit_readout_flg
bit_curr_ctrl_flg
bit_dither_flg
bit_plc_flg
```

Conceptually:

```
Old packed byte ──decode──┐
                         ├── same processed status outputs
New split fields ────────┘
```

## Gyro Mux Verification

The test also verifies that replacing the old counter extraction with direct use of:

```
Status_ID_Counter
```

does not change the reconstructed gyro mux values.

For a multi-byte mux parameter:

```
Mux_Tag + byte position + Mux_Byte
                ↓
       reconstruct full word
                ↓
        apply existing scaling
                ↓
        processed mux value
```

The updated decoder should produce the same 16-bit or 32-bit mux values as the legacy implementation.

## What This Step Proves

Passing Step 4 shows that the changes made in Steps 1–3 altered **how the new telemetry is read**, but did not unintentionally alter the decoded engineering outputs.

```
New telemetry format
        ↓
updated mapping/status/counter handling
        ↓
same expected processed data
```

## Checksum Limitation

Checksum behavior is intentionally **not considered fully verified** in this step.

The checksum still depends on confirming whether the new split status fields should be repacked and used with the legacy checksum algorithm.

> **Step 4 result:** The updated decoder is checked for functional equivalence with the legacy implementation for processed status and gyro mux outputs, confirming that the new message format does not change the intended decoded results.

# HG9900 Legacy vs New Decoder Test Breakdown

## Purpose

The purpose of `test_decodeHg9900RawMux.m` is to verify that the updated HG9900 decoder produces the same processed outputs as the legacy decoder when both are given logically equivalent data.

The test specifically bypasses the telemetry-loading and raw-parameter-mapping stages and directly exercises:

```
decodeHg9900RawMux(...)
```

This isolates the decoder logic itself. Pasted text

---

## 1. Create 8 Synthetic HG9900 Frames

The test begins with:

```
N = 8;
```

So the input contains **8 artificial HG9900 raw-data frames**.

Each frame represents one complete raw sample at a point in time.

```
Frame 1
Frame 2
Frame 3
Frame 4
Frame 5
Frame 6
Frame 7
Frame 8
```

The test assigns each frame:

- received time
- message number
- HG9900 timestamp

For example:

```
s.AbsoluteReceivedTime = (0:N-1)' * 0.01;

s.HG9900_Raw_Data_Frame_Msg_Number = ...
    uint32((1:N)');

s.HG9900_Raw_Data_Frame_Timestamp = ...
    (0:N-1)' * 0.01;
```

Therefore:

```
Frame    Time
-----    ----
1        0.00
2        0.01
3        0.02
...
8        0.07
```

Pasted text

---

## 2. Create Synthetic Accelerometer Mux Data

The accelerometer mux tag cycles through:

```
0
1
2
3
4
5
6
7
```

The corresponding mux bytes are arbitrary test values:

```
0x10
0x20
0x30
0x40
0x50
0x60
0x70
0x80
```

These values are not intended to represent meaningful sensor data yet. They simply provide known inputs for the decoder. Pasted text

---

## 3. Construct the Legacy Packed Gyro Status Byte

The old HG9900 interface stores several pieces of status information inside **one 8-bit byte**.

The layout is:

```
bits:   7 6 | 5 | 4 | 3 | 2 | 1 0
        ----- --- --- --- --- -----
        Status RO  Cur Dit PLC Counter
```

The test fixes the status fields as:

```
Status  = 2 = binary 10
Readout = 1
Current = 0
Dither  = 1
PLC     = 0
```

The two-bit counter changes between frames:

```
Frame:     1  2  3  4  5  6  7  8
Counter:   0  1  2  3  0  1  2  3
```

Pasted text

This produces the legacy packed status sequence:

```
Counter 0 -> 10101000 -> 0xA8
Counter 1 -> 10101001 -> 0xA9
Counter 2 -> 10101010 -> 0xAA
Counter 3 -> 10101011 -> 0xAB
```

Therefore the eight frames contain:

```
A8
A9
AA
AB
A8
A9
AA
AB
```

Pasted text

---

## 4. Create Legacy Gyro Inputs

The test then creates data for:

```
Gyro 1
Gyro 2
Gyro 3
```

Each gyro receives the same packed status-byte sequence.

For example, Gyro 1 gets:

```
s.HG9900_Raw_Data_Frame_Raptor_Counts_gyro1_status = ...
    statusBytes;
```

### Gyro mux tag

The gyro mux tag is deliberately held constant:

```
1 1 1 1 1 1 1 1
```

The reason is that the **Status ID Counter** identifies which mux-byte position the current frame contains.

Across four frames:

```
Mux Tag = 1

Counter = 0 -> byte position 0
Counter = 1 -> byte position 1
Counter = 2 -> byte position 2
Counter = 3 -> byte position 3
```

Then the sequence repeats. Pasted text

---

## 5. Add Theta and Mux Data

Each gyro also receives:

- `mux_word`
- theta LSB
- theta MSB
- checksum

For Gyro 1, for example:

```
Frame       mux word
1           0x11
2           0x22
3           0x33
4           0x44
5           0x55
6           0x66
7           0x77
8           0x88
```

The theta bytes are also arbitrary values used to exercise the decoder

---

## 6. Current Checksum Setup

At present, the test assigns:

```
zeros(N,1,'uint8')
```

as the checksum for each gyro.

For example:

```
s.HG9900_Raw_Data_Frame_Raptor_Counts_gyro1_checksum = ...
    zeros(N,1,'uint8');
```

The comments explicitly identify these as **placeholder checksums** that may cause checksum validation to fail. Pasted text

So currently:

```
gyroChecksum = 0
```

for every synthetic frame.

This is important because the actual checksum test data still needs to be updated if we want every synthetic frame to intentionally contain a valid checksum.

---

## 7. Run the Legacy Decoder

The original synthetic structure `s` represents the **old HG9900 format**.

The test sends it into the legacy decoder:

```
mux_old = ...
    MinAtrTmAnalysis.HG9900.decodeHg9900RawMux_LEGACY( ...
        s, ...
        "IsChecksumEnabled",true);
```

The test then stores several of the processed status outputs:

```
oldStatus.gyro_status
oldStatus.readout
oldStatus.current
oldStatus.dither
oldStatus.plc
```

Pasted text

Conceptually:

```
OLD packed input
      |
      v
Legacy Decoder
      |
      v
Processed mux/status output
```

---

## 8. Build an Equivalent New-Format Input

The next major part of the test creates:

```
s_new = s;
```

So initially the new test structure is a copy of the old one.

The test then removes the legacy packed fields:

```
gyro1_status
gyro2_status
gyro3_status
```

This ensures that the **new decoder cannot accidentally use the old packed status byte**. Pasted text

---

## 9. Split the Old Status Information into New Fields

The same logical information represented by:

```
A8 A9 AA AB ...
```

is now supplied independently.

The test creates:

```
statusStatus     = 2;
readoutBitStatus = 1;
currentBitStatus = 0;
ditherBitStatus  = 1;
plcBitStatus     = 0;
```

and:

```
statusIdCounter = [
    0
    1
    2
    3
    0
    1
    2
    3
];
```

Pasted text

So instead of:

```
OLD:

10101000
```

the new input contains:

```
Status       = 10
Readout      = 1
Current      = 0
Dither       = 1
PLC          = 0
ID Counter   = 00
```

The information is the same; only its representation has changed.

---

## 10. Assign the New Fields to All Three Gyros

Each gyro receives the new split status fields.

For example, Gyro 1 gets:

```
gyro1_status_status
gyro1_status_readout_bit
gyro1_status_current_bit
gyro1_status_dither_bit
gyro1_status_plc_bit
gyro1_status_id_counter
```

Pasted text

The same structure is then repeated for Gyro 2 and Gyro 3. Pasted text

---

## 11. Run the New Decoder

The new-format structure is then passed into the updated decoder:

```
mux_new = ...
    MinAtrTmAnalysis.HG9900.decodeHg9900RawMux( ...
        s_new, ...
        "IsChecksumEnabled",true);
```

Pasted text

Conceptually:

```
OLD FORMAT                         NEW FORMAT

Packed status byte                Split status fields
       |                                 |
       v                                 v
Legacy decoder                     New decoder
       |                                 |
       +--------------+------------------+
                      |
                      v
             Outputs should match
```

---

## 12. Compare the Processed Status Outputs

The test checks these outputs:

```
gyro_status
bit_readout_flg
bit_curr_ctrl_flg
bit_dither_flg
bit_plc_flg
```

For every gyro:

```
for g = 1:3
```

it retrieves:

```
oldValue
newValue
```

and checks:

```
assert(isequaln(oldValue, newValue))
```

Pasted text

So the test is asking:

> Does the new split-field decoder produce exactly the same processed status values as the old packed-byte decoder?

If yes:

```
PASS: gyro_status
PASS: bit_readout_flg
PASS: bit_curr_ctrl_flg
PASS: bit_dither_flg
PASS: bit_plc_flg
```

---

## 13. Compare the Overall Decoder Structures

The final section begins looking at the complete output structures from:

```
mux_old
mux_new
```

For each gyro it obtains:

```
oldFields = fieldnames(mux_old.gyro(g));
newFields = fieldnames(mux_new.gyro(g));
```

and prints fields that exist:

```
Only in OLD
Only in NEW
```

Pasted text

The underlying goal is:

> Because only the source of the status/counter information changed, the final processed mux structure should otherwise remain equivalent.

---

## Overall Test Flow

```
                 CREATE 8 SYNTHETIC FRAMES
                           |
                           v
              Build OLD packed-status input
                           |
                           v
                    Legacy Decoder
                           |
                           v
                       mux_old


                 Same logical test data
                           |
                           v
              Remove old packed status byte
                           |
                           v
           Create NEW individual status fields
                           |
                           v
                      New Decoder
                           |
                           v
                       mux_new


              mux_old       vs       mux_new
                    \                 /
                     \               /
                      v             v
                   Compare outputs
                          |
                          v
                       PASS/FAIL
```

---


---

## Current State of the Test

At this point the test verifies the central behavior from Steps 1–4:

```
OLD packed status
        |
        v
Legacy Decoder

        should equal

NEW split status
        |
        v
New Decoder
```

The main unfinished portion for **Step 5** is the synthetic checksum data.

The current test still contains:

```
checksum = 0
```

for every frame, explicitly as a placeholder. Pasted text

Now that the new decoder reconstructs the legacy `gyroStatusByte` for the checksum calculation, the next useful test modification is to **calculate the correct synthetic checksum for each of these 8 frames instead of filling it with zeros**. Then all eight frames should intentionally pass checksum validation, and we can corrupt one checksum afterward to prove the rejection logic also works.