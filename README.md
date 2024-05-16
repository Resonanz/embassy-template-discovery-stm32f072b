### Running Embassy's Blinky code on STs STM32F072 Discovery evaluation board.

Notes:
- Note: Embassy repository: https://github.com/embassy-rs/embassy
- Note: The microcontroller on the evaluation board is an STM32F072RBT6.

First, clone the Embassy repository:

```git clone https://github.com/embassy-rs/embassy```

cd into embassy and type ```code .``` to open VSCode with the current folder as the root folder.

In VSCode go into the (hidden) .cargo folder ```examples/stm32f0/.cargo``` and edit ```config.toml``` to update the chip type:

```
[target.thumbv6m-none-eabi]
runner = 'probe-rs run --chip STM32F072RBTx'

[build]
target = "thumbv6m-none-eabi"

[env]
DEFMT_LOG = "trace"
```

In VSCode find cargo.toml in the root folder and update the microcontroller type  to ```stm32f072rb``` under:

 ```
 [dependencies]
 embassy-stm32 = ...
 ```

Connect the DISCOVERY board USB to the computer. From the terminal cd into ```examples/stm32f0``` then compile and run blinky using:

```cargo run --bin blinky```

This should eventually result in:

```
Compiling embassy-template-stm32l0 v0.1.0 (/home/Github/embassy/examples/stm32f0)
  Finished dev [unoptimized + debuginfo] target(s) in 0.16s
    Running `probe-rs run --chip STM32F072RBT6 target/thumbv6m-none-eabi/debug/blinky`
      Erasing ✔ [00:00:02] [#################################] 63.00 KiB/63.00 KiB @ 23.58 KiB/s (eta 0s )
  Programming ✔ [00:00:03] [#################################] 63.00 KiB/63.00 KiB @ 18.10 KiB/s (eta 0s )    Finished in 6.172s
```

The Blinky program code has compiled and uploaded into the microcontroller on the eval board. The green LED on the eval board should be blinking on and off.

To change the LED flashing rate, edit the delay times in ```main.rs``` then save and rerun.
