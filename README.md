<h1 align="center">ormx</h1>
<div align="center">
 <strong>
   lightweight macros for <a href="https://github.com/launchbadge/sqlx">sqlx</a>  
 </strong>
</div>

<br />

<div align="center">
  <!-- Version -->
  <a href="https://crates.io/crates/ormx">
    <img src="https://img.shields.io/crates/v/ormx.svg?style=flat-square"
    alt="Crates.io version" />
  </a>
  <!-- Discord -->
  <a href="https://discord.gg/mrZz4Wv8r2">
    <img src="https://img.shields.io/discord/665528275556106240?style=flat-square" alt="chat" />
  </a>
  <!-- Docs -->
  <a href="https://docs.rs/ormx">
    <img src="https://img.shields.io/badge/docs-latest-blue.svg?style=flat-square"
      alt="docs.rs docs" />
  </a>
  <!-- Downloads -->
  <a href="https://crates.io/crates/ormx">
    <img src="https://img.shields.io/crates/d/ormx.svg?style=flat-square"
      alt="Download" />
  </a>
</div>

## getting started
Add ormx and sqlx to your `Cargo.toml`
```toml
[dependencies.ormx]
version = "0.7"
features = ["mysql"]

[dependencies.sqlx]
version = "0.5"
default-features = false
features = ["macros", "mysql", "runtime-tokio-rustls"]
```
Right now, ormx supports mysql/mariadb and postgres.
## what does it do? 
ormx provides macros for generating commonly used sql queries at compile time.  
ormx is meant to be used together with sqlx. Everything it generates uses `sqlx::query!` under the hood, so every generated query will be checked against your database at compile time.  
## what doesn't it do?
ormx is not a full-fledged ORM nor a query builder. For everything except simple CRUD, you can always just use sqlx.  

it is required that every table contains an id column, which uniquely
identifies a row. probably, you would want to use an auto-incrementing integer for this.
this is a central requirement of ormx, and if your table does not fulfill this requirement, ormx
is not what you are looking for.

## help
if you encounter an issue or have questions, feel free to ask in [`#ormx` on the sqlx discord](https://discord.gg/mrZz4Wv8r2).  
The documentation currently is not what it should be, so don't be afraid to ask for help.

## [mysql example](https://github.com/NyxCode/ormx/tree/master/example-mysql/src/main.rs)
## [postgres example](https://github.com/NyxCode/ormx/tree/master/example-postgres/src/main.rs)
## features
- `mysql` -  enable support for mysql/mariadb  
- `postgres` - enable support for postgres  
## migration guide for 0.7
Since 0.7, id columns are not special anymore - if they are generated by the database, you must annotete them with `#[ormx(default)]`.
## a note on reborrowing
if you run into the issue that the compiler tells you that you can't re-use a `&mut Connection` because  
- `use of moved value` and
- `move occurs because 'con' has type '&mut Connection', which does not implement the 'Copy' trait`  

you'll have to manually reborrow the connection with `&mut *con`.