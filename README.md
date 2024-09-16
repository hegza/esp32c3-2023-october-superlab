# esp32c3-rtic-tau

Repository for `esp32c3-rtic-tau` demonstration and assignments.

- `esp32c3`, code for the target.
- `host`, code for the host.
- `shared`, library for shared data structures and communication between the host and target.

## Software requirements

Software components needed for running the superlab examples (**Already satisfied on course VMs**):

- `PuTTY` for monitoring the serial on the examples that use it
- `probe-rs-tools` for flashing and debugging the target
- A Rust toolchain from the `stable` channel (<https://rustup.rs/>).

We flash these examples using `cargo embed`, cargo-subcommand. Obtain the tools by running the following commands according to advice at [probe-rs](https://probe.rs/) site (**Already done on the course VM**):

1. `curl --proto '=https' --tlsv1.2 -LsSf https://github.com/probe-rs/probe-rs/releases/latest/download/probe-rs-tools-installer.sh | sh`
2. Setup udev rules for probe-rs: <https://probe.rs/docs/getting-started/probe-setup/>
3. Refresh udev rules `sudo udevadm control --reload-rules && sudo udevadm trigger`

## Running the examples

ESP32-C3 programs can be run on the target device as follows.

- Change to target directory:
  - `cd esp32c3`
- Use `cargo embed` to build & run an example, e.g.,
  - `cargo embed --example blinky`

## Using FTDI to connect serial to USB

You cannot put serial wires into a USB port and expect it to work. Therefore we must use a small FTDI2232HL board to
fill in the gaps.

The FTDI's pins should be configured the following way:

### Internal (Loopback) connections

| **Start** | **End** | **Description** |
| :-:       | :-:     | :-:             |
| CN3-1     | CN3-3   | Connect the USB power to the VCC of the FTDI |
| CN2-3     | CN2-11  | Connect the VCC of the FTDI to the VCC IO of the FTDI |

### External connections to the ESP32-C3

| FT2232H Mini  | ESP32-C3 |
| :-:           | :-:      |
| CN2-7 (TX)    | IO20/RX  |
| CN2-10 (RX)   | IO21/TX  |

### Useful links

- [https://ftdichip.com/wp-content/uploads/2020/07/DS_FT2232H_Mini_Module.pdf](https://ftdichip.com/wp-content/uploads/2020/07/DS_FT2232H_Mini_Module.pdf)
- [https://ftdichip.com/wp-content/uploads/2020/07/DS_FT2232H.pdf](https://ftdichip.com/wp-content/uploads/2020/07/DS_FT2232H.pdf)

## Connecting to the FTDI

In order to connect to the FTDI and interact with the ESP32-C3 via serial, we will use PuTTY. For the course VM, just run `sudo putty` and select the `FTDI Serial` profile (**For those using the couse VM**). Click open and now, if you have connected the FTDI to the VM, you should see the data the ESP32-C3 sends over the serial!
