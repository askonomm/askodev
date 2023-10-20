---
title: Friends don't let friends unwrap
slug: friends-dont-let-friends-unwrap
date: 2023-10-20
status: published
---

I'm pretty new to Rust, so I should say that not too long ago I thought `unwrap`-ing everything was 
the norm. Of course, I would check for things with `is_some`, `is_none`, `is_ok`, `is_err` and then `unwrap`, because 
I'm not a psychopath, but in the end of the day, I would still `unwrap` everything. 

Then I discovered that I can just use `?` instead, and capture all the errors in one big generic enum. 
This is made especially easy using `thiserror` crate, with what my code can now look like this:

```rust
#[derive(Error, Debug)]
pub enum Error {
    #[error("JSON error: {0}")]
    Json(#[from] serde_json::Error),
    #[error("HTTP error: {0}")]
    Http(#[from] reqwest::Error),
    #[error("URL error: {0}")]
    Url(#[from] url::ParseError),
}

fn main() -> Result<(), Error> {
    let url = Url::parse("https://example.com")?;
    let response = reqwest::get(url)?;
    let body = response.text()?;
    let json: Value = serde_json::from_str(&body)?;
    println!("{}", json);
    Ok(())
}
```

Much nicer, right? I think so. Thanks to `thiserror` I can still have custom error messages, but also 
I get to use `?` instead of `unwrap`-ing everything, leaving my code a lot cleaner and nicer to read.