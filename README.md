# SSVMRPC
A Remote Procedure Call (RPC) implementation which facilitates both code-deployment and code-execution interactions with SecondState's stateless Virtual Machine (SSVM)

## Design overview
![ssvmrpc diagram](https://github.com/second-state/SSVMRPC/blob/master/architecture.jpg)

## HTTP POST specification
The [http_post_specification](https://github.com/second-state/SSVMRPC/blob/master/http_post_specification.md) file provides detailed specification for calling SSVMRPC from the web.

## State specification
The [ssvmrpc_application_state_specification.md](https://github.com/second-state/SSVMRPC/blob/master/ssvmrpc_application_state_specification.md) file provides a detailed specification for storing application state.

## Service specification
The [ssvmrpc_service_specification.md](https://github.com/second-state/SSVMRPC/blob/master/ssvmrpc_service_specification.md) file provides a detailed specification for service objects (function to call, arguments to use etc).

## Installing SSVMRPC
The following instructions will guide you through setting up your SSVMRPC server, which can receive HTTP POST requests from the web and communicate those to the rest of the SSVM ecosystem.

System preparation
```
sudo apt-get update
sudo apt-get -y upgrade
```
Rust installation
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
```
Rust configuration and housekeeping
```
rustup override set nightly
rustup update && cargo update
```
Add rocket dependency to Cargo.toml
```
[dependencies]
rocket = "0.4.2"
serde_json = "1.0"
```
Create the SSVMRPC project
```
cd ~
cargo new ssvmrpc
cd ssvmrpc
```
Create/open the ~/ssvmrpc/src/main.rs file and fill with the following contents
```
#![feature(proc_macro_hygiene, decl_macro)]
use std::str;
use rocket::Data;
use serde_json::{Value};
use rocket::response::content;
#[macro_use] extern crate rocket;


#[post("/deploy", data = "<bytes_vec>")]
fn deploy(bytes_vec: Data) -> content::Json<&'static str>{
    if bytes_vec.peek_complete() {
        let string_text = str::from_utf8(&bytes_vec.peek()).unwrap();
        let v: Value = serde_json::from_str(string_text).unwrap();
        println!("Application: {:?}", v["application"]);
    }
    content::Json("{'response':'success'}")
}


#[post("/destroy", data = "<bytes_vec>")]
fn destroy(bytes_vec: Data) -> content::Json<&'static str>{
    if bytes_vec.peek_complete() {
        let string_text = str::from_utf8(&bytes_vec.peek()).unwrap();
        let v: Value = serde_json::from_str(string_text).unwrap();
        println!("Application: {:?}", v["application"]);
    }
    content::Json("{'response':'success'}")
}


#[post("/execute", data = "<bytes_vec>")]
fn execute(bytes_vec: Data) -> content::Json<&'static str>{
    if bytes_vec.peek_complete() {
        let string_text = str::from_utf8(&bytes_vec.peek()).unwrap();
        let v: Value = serde_json::from_str(string_text).unwrap();
        println!("Application: {:?}", v["application"]);
    }
    content::Json("{'response':'success'}")
}

fn main() {
    rocket::ignite().mount("/", routes![deploy, destroy, execute]).launch();
}
```
Build the ssvmrpc application
```
cd ~
cd ssvmrpc
cargo build --release
```
Start the ssvmrpc server
```
./target/release/ssvmrpc
```
