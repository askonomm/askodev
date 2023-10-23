---
title: Siena 3
slug: siena-3
date: 2023-10-23
status: published
---

I'm happy to announce the immediate availability of [Siena 3](https://github.com/askonomm/siena), my data provider agnostic ORM for Rust. This release includes a number of bug fixes and improvements. Some of the changes are breaking changes.

## YAML and FrontMatter data parsing improvements

YAML parsing has much improved. When previously YAML parsing only supported `HashMap<String, String>`, as in root level key-value only and nothing else, then now the YAML parser supports a variety of new data structures such as `String`, `usize`, `bool`, `Vec<RecordData>` and `HashMap<RecordData>` where `RecordData` is:

```rust
pub enum RecordData {
    Str(String),
    Num(usize),
    Bool(bool),
    Map(HashMap<String, RecordData>),
    Vec(Vec<RecordData>),
}
```

Meaning that yes, you can now have recursive data structures in YAML (and the YAML section in FrontMatter files).

## Name changes

- `when_isnt` method is now `when_is_not`
- `when_hasnt` method is now `when_has_not`
- `RecordSortOrder::Custom` is now `RecordSortOrder::CustomStr`

## `usize` custom sorting

I renamed the `RecordSortOrder::Custom` to `CustomStr` because I also introduced a new sorter called `RecordSortOrder::CustomNum` which sorts `usize` values in the same way the `CustomStr` sorts strings.

---

I dog food Siena myself for this very website actually, in [Vau](https://github.com/askonomm/vau), my in-progress static site generator, and the above improvements are a direct result of me wanting to use Siena for my own purposes and finding it lacking. Dogfooding is great!



