# Current Tasks
_Each one of these should be usefully descriptive of what I'm working on during a stream_

_Ordered by most-likely-to-be-working-on-now first_    

CLI Tools LSP
  * [ ] Explore wrapping as much of the server as possible int try/except
  * [ ] Write the tests for the default configs currently supported
  * [ ] Get formatters working
  * [ ] Apoyar language_id wildcard con "\*"
  * [ ] Add a changelog file. Automate?
  * [ ] Docs
    * [ ] Help user discover LSP's `language_id` for any given CLI tool
    * [ ] `language_id`/filetype configuration is unique to every editor
  * [ ] Hook into LSP progress/status to give feedback about bad config
  * [ ] Validate format token strings, eg; have to contain `line`, `col`, `msg`, ordered by priority
  * [ ] Add more tests
  * [ ] Automatic CI releases to Pypi on version changes. Github releases?
  * [ ] Rename to "Super GLS"
  * [ ] Support diagnostic severity (info, warning, error, etc)
  * [ ] Support multiple `language_id` values
  * [ ] Remove `triggerCharacters` option from JSON example in Pygls README

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
  * [ ] Make a noise or notification when I get a new follower
    * Quick solution: make a noise, or highlight latest follower for 15 mins
    * Best solution: look into OBS overlay

# Journal

📆 **October 19th**    
Main thing is to get foramtters working, I wonder how long that will take? Could be quick! Really I need to get to all the grunt work of adding loooooooooooooads of defaults.

Damn, ok, actually got to write some tests and check out wrapping server in try/except

📆 **October 18th**    
I want to remove the sleeps from the `jq` test. Learn more about LSP diagnostic notifications and maybe even custom notifications/logs. Maybe even look into the LSP spec for status updates.

* [x] Configs for various editors

Had a good responding to lots of issues on Pygls around the new breaking change release.

📆 **October 17th**    
Hopefully get yesterday's commit committed soon. Then something...

* Supporting more editors; Vim, Emacs and VSCode
* More features; formatting, code actions, hover, workspace symbols, find references ...
* Fill it up with more default configs

* [x] Get default config from YAML file

📆 **October 16th**    
So mainly just want to tidy things up a bit; docs and tests mostly. Maybe start more configs.

Almost got a big commit in that made the user config must nicer. Basically introducing YAML.

* [x] Update to latest lsp-devtools. Test init from test body

📆 **October 15th**    
Got a minimal working version of completion lists. It's pretty impressive how with such minimal config you can get completion lists popping up in your editor. The possibilities are endless!

  * [x] Reinstaler Mypy! 
  * [x] Add another classic LS feature, I think completion list?

📆 **October 14th**    
Been thinking. What if I called CLI Tools LSP "Super GLS" to contrast with "Pygls". Then would be great to create a pygls-org namespace in Github so we have th 2 projects; `pygls-org/pygls` and `pygls-org/super-gls`. That way we have the low-level Pygls for fine-grained control over custom LS behaviour. And then Super GLS for a quicker, more user-friendly wrapper over Pygls. I think it's important because it really helps place the unique usecase of each language server.

Started adding notable/illustrative params fields. Felt good to be helping devs out with these pointers to deeper inside the data types. That data is there but you have drill down through multiple files to find it. So placing some clues up front on the API surface feels nice.

Writing an example completion feature made me learn a fair amount of new things about LSP and Pygls. I realise that the CLITools/Super LSP isn't going to so easily support every minute detail of LSP, but it does fell like it's pretty simple to support the headline features. And for more fine-grained control devs can just drop down into Pygls, which is the whole point anyway.

📆 **October 13th**    
Basically just wanna make a lowkey publish of the CLI tools LSP module on Pip and start pretending like it's live, in order to find the rough edges of installation, usage, docs, etc.

Got a working MVP released on Pip!

* [x] Reply to Alex about LSP-tools PRs and his big lsprotocol PR
* [x] Publish CLITools module on Pip, lowkey hush hush
* [x] Soft publish, and use like a normal LSP 😏

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
