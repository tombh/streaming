# Current Tasks
_Each one of these should be usefully descriptive of what I'm working on during a stream_

_Ordered by most-likely-to-be-working-on-now first_    

CLI Tools LSP
  * [ ] Reply to Alex about LSP-tools PRs and his big lsprotocol PR
  * [ ] Publish CLITools module on Pip, lowkey hush hush
  * [ ] Docs
    * [ ] Configs for various editors
    * [ ] Help user discover LSP's `language_id` for any given CLI tool
    * [ ] `language_id`/filetype configuration is unique to every editor
  * [ ] Get default config from YAML file
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

Misc
  * [ ] Find stick/thing to stop table/webcam wobbling so much

# Journal

📆 **October 13th**    
Basically just wanna make a lowkey publish of the CLI tools LSP module on Pip and start pretending like it's live, in order to find the rough edges of installation, usage, docs, etc.


📆 **October 12th**    
I realised that I can kill 2 birds with one stone, I can make the CLI Tools LSP both a CLI Tools LSP _and_ an easy starting template for Pygls in general. Considering that all the heavy lifting is done by the CLI tools (`jq`, `markdownlint`, etc) the actual LS-specific code should be minimal and, more importantly, easily refactorable such that a bare-bones LS can be created to start your own dedicated LS. As such it would be beneficial to both goals of the LS (CLI tools and Pygls template) to support _all_ the featues of LSP in general.

CLI Tools LSP
  * [x] Refactored CLITool-specific code into `src/lsp/custom`
  * [x] First end to end tests

Pretty much ready for MVP beta release!

📆 **October 7th**    
I only had an hour or so. Figured out what was going on in the world of LSP config in terms of specifying the language ID. The upshot is that it is the _client's_ responsibility to specify the language ID. There isn't a clever way for a language server to tell the client (think editor) which file types it should send. I'm not 100% sure that it is the user's tasks, with the help of our docs, to manually specify the language IDs/filetypes that they want their editor to connect to the language server for.

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
