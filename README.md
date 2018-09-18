# tabular: plain text tables in Rust

Builds plain, automatically-aligned tables of monospaced text.
This is basically what you want if you are implementing `ls`.

## Example

```rust
use tabular::{Table, Row};

fn ls(dir: &::std::path::Path) -> ::std::io::Result<()> {
    let mut table = Table::new("{:>}  {:<}{:<}  {:<}");
    for entry_result in ::std::fs::read_dir(dir)? {
        let entry    = entry_result?;
        let metadata = entry.metadata()?;

        table.add_row(Row::new()
             .with_cell(metadata.len())
             .with_cell(if metadata.permissions().readonly() {"r"} else {""})
             .with_cell(if metadata.is_dir() {"d"} else {""})
             .with_cell(entry.path().display()));
    }

    print!("{}", table);

    Ok(())
}

ls(Path::new(&"target")).unwrap();
```

produces something like

```
 1198     target/.rustc_info.json
 1120  d  target/doc
  672  d  target/debug
```

## Usage

It's on [crates.io](https://crates.io/crates/tabular), so you can add

```toml
[dependencies]
tabular = "0.1.0"
```

to your `Cargo.toml`.

## See also

You may also want:

  - https://crates.io/crates/text-tables – This is more automatic 
    than tabular. You give it an array of arrays, it renders a nice
    table with borders. Tabular doesn't do borders.  
  
  - https://crates.io/crates/prettytable-rs — This has an API more
    similar to tabular’s in terms of building a table, but it does
    a lot more, including, color, borders, and CSV import.