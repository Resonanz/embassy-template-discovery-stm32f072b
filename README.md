### Running Embassy's Blinky code on STs STM32F072 Discovery evaluation board.

Notes:
- Embassy repository: https://github.com/embassy-rs/embassy
- The microcontroller on the evaluation board is an STM32F072RBT6
- Schematic: https://www.st.com/en/evaluation-tools/32f072bdiscovery.html#cad-resources

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

In VSCode open ```src/blinky.rs``` and modify the LED ports as follows:

```
#![no_std]
#![no_main]

use defmt::*;
use embassy_executor::Spawner;
use embassy_stm32::gpio::{Level, Output, Speed};
use embassy_time::Timer;
use {defmt_rtt as _, panic_probe as _};

// main is itself an async function.
#[embassy_executor::main]
async fn main(_spawner: Spawner) {
    let p = embassy_stm32::init(Default::default());
    info!("Hello World!");
    //PA5 is the onboard LED on the Nucleo F091RC                 // Change LED ports as follows
    let mut led_u = Output::new(p.PC6, Level::High, Speed::Low);  // UP LED
    let mut led_d = Output::new(p.PC7, Level::High, Speed::Low);  // DOWN LED
    let mut led_l = Output::new(p.PC8, Level::High, Speed::Low);  // LEFT LED
    let mut led_r = Output::new(p.PC9, Level::High, Speed::Low);  // RIGHT LED

    loop {
        info!("high");
        led_u.set_high();
        Timer::after_millis(300).await;

        info!("low");
        led_u.set_low();
        Timer::after_millis(300).await;
        
        info!("high");
        led_d.set_high();
        Timer::after_millis(300).await;

        info!("low");
        led_d.set_low();
        Timer::after_millis(300).await;
        
        info!("high");
        led_l.set_high();
        Timer::after_millis(300).await;

        info!("low");
        led_l.set_low();
        Timer::after_millis(300).await;
        
        info!("high");
        led_r.set_high();
        Timer::after_millis(300).await;

        info!("low");
        led_r.set_low();
        Timer::after_millis(300).await;
    }
}
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
