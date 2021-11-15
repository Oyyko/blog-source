---
title: Hello World
date: 2020-12-27
tag: Rust
category: Rust
mathjax: false
---

A rust way to say hello world!

<!--more-->

## Code in Rust
```rust
use ferris_says::say;

use std::io::{stdout, BufWriter};

fn main() 
{
    let stdout=stdout();
    let message = String::from("Hello fellows!\nHello again.\n");
    let width =  message.chars().count();

    let mut writer = BufWriter::new(stdout.lock());

    say(message.as_bytes(),width,&mut writer).unwrap();
    
}
```

## Output

```
 ________________
/ Hello fellows! \
\ Hello again.   /
 ----------------
        \
         \
            _~^~^~_
        \) /  o o  \ (/
          '_   -   _'
          / '-----' \
```
