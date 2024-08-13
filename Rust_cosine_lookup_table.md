Benchmark for cosine table in rust
also checks performance of arrays and lists

output:
```
Cosine lookup table benchmark
Table size: 10000
Number of lookups: 10000000
List duration: 13.175ms --> -536.6215
Array duration: 13.1453ms --> -536.6215
Cos duration: 93.8803ms --> -537.1735
```

Code:

```rust
use std::time::Instant;

const TABLE_SIZE: usize = 10000;
const COUNT : usize = 10000000;

fn main() {

    println!("Cosine lookup table benchmark");
    println!("Table size: {}", TABLE_SIZE);
    println!("Number of lookups: {}", COUNT);

    let mut list: Vec<f32> = Vec::new();
    let mut array: [f32; TABLE_SIZE] = [0.0; TABLE_SIZE];
    

    // Populate the list and array with cosine values
    for i in 0..TABLE_SIZE {
        let angle = (i as f32 / TABLE_SIZE as f32) * std::f32::consts::PI * 2.0;
        list.push(angle.cos());
        array[i] = angle.cos();
    }

    // make an array of COUNT random values between -100 and 100
    let mut random_values = Vec::new();
    for _ in 0..COUNT {
        random_values.push(rand::random::<f32>() * std::f32::consts::PI * 2.0);
    }

    // Benchmark the list
    let list_start = Instant::now();

    let mut v_index = 0.;

    for i in 0..COUNT {
        let val = random_values[i];
        let index = (val * TABLE_SIZE as f32 / (std::f32::consts::PI * 2.0)) as usize;
        v_index += list[index];
        
    }
    let list_duration = list_start.elapsed();


    // Benchmark the array
    let array_start = Instant::now();

    let mut v_array = 0.;
    for i in 0..COUNT {
        let val = random_values[i];
        let index = (val * TABLE_SIZE as f32 / (std::f32::consts::PI * 2.0)) as usize;
        v_array += array[index];
        
    }
    let array_duration = array_start.elapsed();

    // Benchmark pure cosine calculation
    let array_start = Instant::now();

    let mut v_cos = 0.;
    for i in 0..COUNT {
        let val = random_values[i] as f32;
        v_cos += val.cos();
        }

    let cos_duration = array_start.elapsed();


    println!("List duration: {:?} --> {:?}", list_duration, v_index);
    println!("Array duration: {:?} --> {:?}", array_duration, v_array);
    println!("Cos duration: {:?} --> {:?}", cos_duration, v_cos);
}
```

