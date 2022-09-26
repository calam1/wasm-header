wasm filter from proxy-wasm project - i just changed logging from trace to warn so I did not have to enable trace logging, and I changed the endpoint from /hello to /home
https://github.com/proxy-wasm/proxy-wasm-rust-sdk/blob/master/examples/http_headers.rs


Basically all this does is add headers to an endpoint with a path of /home and using a GET


To build this for development purposes we will use wasm-pack.
after you run the wasm-pack build command there will be 2 new directoris, pkg and target

```
❯❯❯ cargo install wasm-pack

❯❯❯ wasm-pack build                                                                                                                                                                                                                                                                   on branch: main
[INFO]: 🎯  Checking for the Wasm target...
[INFO]: 🌀  Compiling to Wasm...
   Compiling log v0.4.17
   Compiling proc-macro2 v1.0.43
   Compiling unicode-ident v1.0.4
   Compiling quote v1.0.21
   Compiling wasm-bindgen-shared v0.2.83
   Compiling syn v1.0.100
   Compiling cfg-if v1.0.0
   Compiling version_check v0.9.4
   Compiling bumpalo v3.11.0
   Compiling once_cell v1.15.0
   Compiling proxy-wasm v0.2.0
   Compiling wasm-bindgen v0.2.83
   Compiling ahash v0.7.6
   Compiling hashbrown v0.12.3
   Compiling wasm-bindgen-backend v0.2.83
   Compiling wasm-bindgen-macro-support v0.2.83
   Compiling wasm-bindgen-macro v0.2.83
   Compiling wasm-header v0.1.0 (/Users/xcxl200/git/development/wasm-header)
    Finished release [optimized] target(s) in 11.67s
[INFO]: ⬇️  Installing wasm-bindgen...
[INFO]: Optimizing wasm binaries with `wasm-opt`...
[INFO]: Optional fields missing from Cargo.toml: 'description', 'repository', and 'license'. These are not necessary, but recommended
[INFO]: ✨   Done in 12.31s
[INFO]: 📦   Your wasm pkg is ready to publish at /Users/xcxl200/git/development/wasm-header/pkg.
[WARN]: ⚠️   There's a newer version of wasm-pack available, the new version is: 0.10.3, you are using: 0.10.0. To update, navigate to: https://rustwasm.github.io/wasm-pack/installer/

```

look into the pkg dir, you are interested in the wasm_header_bg.wasm file
```
❯❯❯ ls -l pkg
total 256
-rw-r--r--  1 xcxl200  71667744     350 Sep 26 10:52 README.md
-rw-r--r--  1 xcxl200  71667744     249 Sep 26 10:56 package.json
-rw-r--r--  1 xcxl200  71667744      42 Sep 26 10:56 wasm_header.d.ts
-rw-r--r--  1 xcxl200  71667744      83 Sep 26 10:56 wasm_header.js
-rw-r--r--  1 xcxl200  71667744      48 Sep 26 10:56 wasm_header_bg.js
-rw-r--r--  1 xcxl200  71667744  104741 Sep 26 10:56 wasm_header_bg.wasm
-rw-r--r--  1 xcxl200  71667744    1985 Sep 26 10:56 wasm_header_bg.wasm.d.ts
```

for development purposed we will use ConfigMap to deploy the binary
```
# this command is ran inside the pkg directory, but doesn't have to be, once ran you will see a configmap deployed named new-filter in your namespace demo
❯❯❯ kubectl create configmap -n demo  new-filter --from-file=new-filter.wasm=wasm_header_bg.wasm
```
