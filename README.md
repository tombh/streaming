# A Place For All My Live Streaming Things

## What I Want/Did To Get Done
_Each one of these should be usefully descriptive of what I'm working on during a stream_

📆 **October 7th**    
Pygls
  * [ ] Derive required language IDs from the client LSP config (last thing for MVP! 🚀) 
  * [ ] Get default config from YAML file
  * [ ] Configs for various editors
  * [ ] Docs
  * [ ] Refactor, smaller functions etc
  * [ ] Validate format token strings, eg; have to contain `line`, `col`, `msg`, ordered by priority
  * [ ] Add tests
  * [ ] Soft publish, and use like a normal LSP 😏
  * [ ] `Poetry could not find a pyproject.toml`, not important when public. Just something to think about

Browsh
  * [ ] Browsh: Merge Vim Keybindings PR

Wrach
  * [ ] Start thinking about "Rustish-gpu"
    * [ ] Make the repo
    * [ ] Github action to compile the compiler
    * [ ] What's the word on Rust `struct` layouts, is Crevice ok? Or is `#[repr(c)]` is enough?
    * [ ] General quick guide to using rust-gpu
  * [ ] Downgrade wgpu and rust-gpu, separately if you have the patience


📆 **October 5th**
Pygls
  * [x] Handle output format variations, eg when col number is missing

📆 **October 4th**

Pygls
  * [x] Allow users to specify new LSP interactions through their editor's LSP config
  * [x] Handle multiple diagnostics per call

Misc
  * [x] Remove credit cards from "front page" of Bitwarden popup
  * [x] Use git diff tool setting from tombh

📆 **27nd September**

Pygls
  * [x] Release relaxed verion
  * [x] Choose a CLI tool, parse its output, and have that output influence actual diagnostics

📆 **26nd September**

Pygls
  * [x] Respond websocket issue/PR
  * [x] Update version bounds
  * [x] Make a bit of progress on cli-tools-lsp    

📆 **22nd September**
  * [x] Talk on rust-gpu discord about weird Wrach bug
  * [x] Pygls: lsprotocol migration PR
  * [x] Pygls: version issue
