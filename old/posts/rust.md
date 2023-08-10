---
title: Some Rust Things
date: 2020-09-09
summary: The Rust type system and language features
slug: rust
---

## Getting familiar with Rust

Type Systems in Rust
- <a href="https://www.rust-lang.org/learn">Rust Official Learn Docs</a>
- "the book" of Rust, for a solid <em>reference</em> to the language and all its principles
- <a href="https://play.rust-lang.org/">An Online Rust Playground</a>

### Why Rust?
- Safer systems programming
- Painless and <a href="https://blog.rust-lang.org/2015/04/10/Fearless-Concurrency.html">Fearless concurrency</a>
- Interface with C/C++

### Data Types in Rust
- Integer Types: unsigned and signed <pre><code>i32 u8</code></pre>
- Tuples: groups values into compund type <pre><code>(3, 4.5, 1)</code></pre>

## Getting Into some Concepts

### Mutability
<pre><code data-line-numbers data-trim data-noescape>
  fn not_mutable() {
    let vec = Vec::new();
    vec.push(110);
    vec.push(120);
    vec.push(130);
    println!("{}", sum_val);
  }
</code></pre>

### Move Semantics
Ownership, moving and lifetimes
<pre><code data-line-numbers data-trim data-noescape>
  fn move_it() {
    let vec = vec![1, 2, 3, 4];
    let sum_val = sum(vec);
    // accessing vec after this point is a static error
    println!("{}", sum_val);
  }
</code></pre>
<pre><code data-line-numbers data-trim data-noescape>
  fn sum(v: Vec&lt;i32&gt;) -> i32 {
    let mut acc = 0;
    for i in v.iter() { acc += *i; }
    acc // no need to expilicitly mention return
  }
  // v is destroyed here
</code></pre>

### Copy Semantics
<pre><code data-line-numbers data-trim data-noescape>
  let i = 10;
  let j = i + 10;
</code></pre>
We can also explicitly define or derive the <code>copy</code> Trait for some structs

### Sharing is Important! The &
<pre><code data-line-numbers data-trim data-noescape>
  fn borrow_it() {
    let s = String::from("borrow this");
    let len = calculate_len(&s);
    print_it(&s);
  }

  fn calculate_len(s: &String) -> usize {
   s.len()
  }

  fn print_it(s: &String) {
   println!("{}", s);
  }
</code></pre>
- References
- Borrow Checking
<pre><code data-line-numbers data-trim data-noescape>
  fn borrow_it() {
    let s = String::from("borrow this");
    let ref_s = &s;
    print_it(s);

    let len = calculate_len(ref_s);
    println!("{}", len);
  }

  fn print_it(s: String) {
    println!("{}", s);
  }

  fn calculate_len(s: &String) -> usize {
    s.len()
  }
</code></pre>
### Sharing is Important, But so is Modifying &mut
- A mutable reference allows for modifyinng a borrow value.
- The constraint however, is that there can be only one mutable reference to a variable.
- This allows to completely eliminate race conditions and is a key feature of rust.
<pre><code data-line-numbers data-trim data-noescape>
  fn share_it() {
    let mut s = String::from("share this");
    let mut_ref = &mut s;
    change(s);
    println!("{}", s);
  }

  fn change(s: &mut String) {
    s.push_str(", please!");
  }
</code></pre>

### This tutorial still needs to be finished 
