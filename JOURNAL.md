# Current Tasks
_Each one of these should be usefully descriptive of what I'm working on during a stream_

_Ordered by most-likely-to-be-working-on-now first_    

Super Glass LSP
  * [ ] Think about flakey tests
  * [ ] Write tests for GotoDefiniton
  * [ ] Hook into LSP progress/status to give feedback about subshell progress and bad config
  * [ ] Allow apps to be initiated from LSP `initializationOptions`
  * [ ] Test on windows
  * [ ] Add a changelog file. Automate?
  * [ ] Docs
    * [ ] Help user discover LSP's `language_id` for any given CLI tool
    * [ ] `language_id`/filetype configuration is unique to every editor
  * [ ] Validate format token strings, eg; have to contain `line`, `col`, `msg`, ordered by priority
  * [ ] Automatic CI releases to Pypi on version changes. Github releases?
  * [ ] Grep for "SKIPPED" in CI and fail
  * [ ] Support multiple `language_id` values

Pygls
  * [ ] Refactor LSP test client timeouts
  * [ ] Remove `triggerCharacters` option from JSON example in Pygls README
  * [ ] `apply_edit()` doesn't `apply_changes()`

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
  * [ ] Make a noise or notification when I get a new follower
    * Best solution: look into OBS overlay

# Journal
📅 **June 25th**    
TODO:
* [x] Create new issue(s) regarding support for LSP 3.17
* [x] Respond to issue about dedicated `pygls-community` Github Org for keeping community servers in
* [ ] See if I can make a quick PR demoing replacing Tox with Poetry
* [ ] Quick PR to demo moving Implementations.md file to bottom of README

LEFTOFF: Crazy spewing test output

📅 **June 10th**    
Gonna do some Pygls and Super Glass hopefully ¿
LEFT OFF: `if self.text_doc_uri is None` in `shell()` fails ¿

📆 **June 8th**    
6 months since my last stream 😲 How do I do this again??

* Reviewed Alex Carney's awesome TEXT_DOCUMENT_DID_SAVE PR.
* Dug even deeper into the whole UTF16 position bug.
* [X] REMEMBER HOW TO DO STREAMING AGAIN

📆 **December 13th**    
Had another cool idea for using LSP in the email app. Use diagnostics to show new emails.

But first wanna see if I can get LSP progress working.

LEFT OFF: `WorkDoneProgress.__init__()`: trying to know if a config has a `progress` field

📆 **December 7th**    
Mostly just worked on getting error feedback working. LSP progress support next time
* [x] Test for at least one CLI tool error
* [x] Think about how to give feedback in the editor about day-to-day errors (eg unexpected diagnostic line formats)

📆 **December 6th**    
Start: Haven't thought much, just want to fix that regex word-finder bug thing...

End:
  Got first MVP vesion of Email client working! Inbox, viewing, replying, sending, archiving, deleting.
  Just need to refactor this recent work.
  Would be really good to do some progresss visualisation and feedback next.
  Explore abilty to use CodeActions instead of GotoDefinition.
  Then make one more app, the recipe traceker thing?

* [x] Apoyar language_id wildcard con "\*"
* [x] Support workspaceEdit? Start to prototype in-editor email client

📆 **December 5th**    
Start:
  Weren't any major problems since the Pygls v1 release, but lots of comments on the repo to respond to.
  Then maybe do some tidying up on the Super Glass email app.
End:
  Think I'm pretty much there with everything that's needed for the Email app. Only thing is the bug where
  you can't "click" on the left hand side of the "Archive" emoji button


📆 **December 3rd**    
Exciting start: released Pygls v1! @foobully and @alcarney popped on stream ❤️

Then went through all the Pygls issues to see which ones have changed now that v1 is live. Quite a few as it turns out.

Then made some good progress on the Super Glass email client. Left it trying to pipe an email into  `himalaya send`

📆 **November 29th**    
Made a test email account, so gonna try to get an actual emial inbox working!

Inbox working! 🥳

Drafted GotoDefinition feature, but my Neovim jsut won't send textDocument/definition requests in the inbox.md file, and I have no clue why 🤔

📆 **November 28th**    
What about following Potluck's recipe demo idea? So that:
```
"Cook for 2 mins" # automatically gets updated to:
"Cook for __⏰2:00▶️__" # and some LSP code actions let you start, stop, reset that timer
```

Got things in place ready to actually write the Himalaya BASH script to populate a markdown inbox. Exciting

Spent, what felt like a few hours trying to fix a bug, that turned out to be just an errant slash on a `file:///` 🤦 I don't think that would happen so easily in Rust.

📆 **November 26th**    
Nothing to write 🤔

Fiiiiiiiiinally got the WorkspaceEdit test working. Underlying problem was that Pygls expects to _receive_ workspace edits rather than request them from the client.

📆 **November 25th**
Wrote the WorkspaceEdit class. Now trying to get the first test passing. Stuck on killing the daemon to get the test to finish

📆 **November 24th**    
Umm. Just gonna get on with it...

Commited the email client config draft sketch, with a test for loading app configs.

What's next? I think `workspace_edit` support

📆 **November 21st**    
Woah 11 days without a stream. Did have some thoughts about Super Glass over those days, but forgotten them now. Basically: less thoughts more coding!

Finished a reasonable design draft for the app config idea. Now starting to implement it. Left off needing to write an e2e test to trigger the server.initialize that in turn triggers the new app config loader.

📆 **November 10th**     
Surpised how interesting it was to sketch out the email client app. It was good to see how it _could_ work, even if it's not close to the final design. Also good to see how many of the pieces and precedents are already there. Feels like I'm really on to something. So just gonna carry with a bit more relaxed sketching

Fleshed even more of the email client configs. Really, really interesting thinking through it all. Still seems refreshngly simple and doable

📆 **November 9th**    
Some action in Pygls with fixes coming in, need to review and merge. Then not exactly sure what to do on Super Glass, but that's nice cos it means there's no big stress to get at least a basic state of usability.

* [x] Stream: rescale resolution to 1080p

Made a start on the Himalaya email app. Was really interesting to see how the design started to come into focus as I sketched out the YAML. It really feels like it's doable!

📆 **November 7th**    
First day back after a few days off, forgot how to do this streaming thing a bit. Gonna catch up on some Pygls messages. Then see what I geel like doing on Super Glass.

* [x] Fix formatter when buffer's number of lines differs from what's on disk

Released v0.5.1 of Super Glass, hopefully that can be dogfooded from now on.

📆 **November 4th**    
Sent a reply to Alex Carney on the pytest-lsp thread, we're just exploring the lay of the land at the moment. It's really great the scope vision he has for it.

* [x] Test defered debounced diagnostics
* [x] I feel like there's a bit too much implementation-specific code in the unit tests. Eg; needing unique config_ids to bust the debounce between async tests

Woohoo! Completed the big refactor of `Debounce` and `Feature`, code a lot tidier, readable and intuitive I reckon. But gotta keep an eye on those new flakey e2e tests 🫤

📆 **November 3rd**    
Realised how to fix deferred debounce issue. Need to responde to v1 pygls thread.

It would be great that I get Super Glass into a announcable state aroung the same time that pygls v1 is ready. Then annoucing all the things at once. Maybe even have a Twitch stream release party thing.

Gotta go for a videocall. But had important insight about how debounced _do_ need the same key as caches, because individual tools are allowed to set their own debounce periods.

Did a big refactor to only allow Features to have one config during their lifetimes. Simplifies the code a ot and gets rid of a lot of ugly guards. Also refactored Debounce to be an isolated class with all related behviour inside it. Just need to get the tests to pass now. You were here:
`poetry run python -m pytest -vvv super_glass_lsp/lsp/custom/tests/units/test_formatter.py::test_formatter_debounce --log-cli-level=DEBUG`

📆 **November 2nd**    
Need to publish some new versions:
  * Pygls v1.0.0a2 ✅
  * Pygls v0.12.5 ✅
  * Super glass 0.6.0

David G recommended me the new Ink and Switch article about "gradually enriched" text documents. It's almost exactly what I've been thinking about with the use of Super Glass for making applications, like the email client idea. https://www.inkandswitch.com/potluck

* [x] Defer debounced diagnostics to ensure final doc change still gets diagnosed
* [x] Release 0.13.0 to PyPI!
* [x] Replace Python 3.11-dev in Pygls CI
* [x] Namespace diagnostics and merge them, so that empty diagnostics for one tool don't delete the diagnostics for another tool

📆 **November 1st**    
Updated the Pygls team with my October news, lots to share!

Just lots of little things to do today I think.

* [x] Make an issue on Pygls exploring whether Pygls itself should have support for debounce and cache
* [x] I don't think formatters need debounce?
* [x] Check diagnostic lines off by 1?
* [x] Async debounce

📆 **October 31st**    
First things is that I want to prototype the pygls-org organisation in Github. I'll make it private for now, but just want to show the other Pygls folks my ideas ✅

* Merge that improved error handling PR ✅
* Fix the unit test mock issue I was having with the debounce feature ✅

Gym ball popped at 3:45! https://clips.twitch.tv/TriangularAgreeableYogurtSmoocherZ-4Po_zRXgTbfuKF1y

Are diagnostics lines off by 1 again??

📆 **October 29th**    
Spent a good while this morning trying to figure out some OBS pain points. Turns out I had set a emulated video format for the webcam which was causing lot of CPU usage even when not streaming! Feel like I almost have my perfect streaming set up; OBS on a humble 15% CPU usage, losless encoding, nice travelling mic, "second display" on my little phone 😊 BTW let me know if my audio ever goes out of sync.

Been thinking a lot more about the in-editor email client and the potential of Super Glass to be a bit of a game changer in the space. Also realised I could quite easily support the major snippet collections and formats, see https://github.com/rafamadriz/friendly-snippets

It's getting near the end of the month so I need to report back to the Pygls team about my month.

✅ Added flake8 and various other fixes and tidying up

Got stuck with mocks trying to unit test debounce function.

📆 **October 28th**    
Early morning stream for a couple of hours. Had more thoughts about how I'd make a email client using Super Glass, basically it depends on the workspaceEdit notification coming from the server: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#workspaceEdit I'd like to take some time soon to experiment with that.

But for now I just want to fix those things from yesterday and I think I'll try to do the whole rename thing to Super Glass.

  * [x] Rename to "Super GLS"
  * [x] Get formatters working

2 weird things I noticed with Black:
  1. It can add extra chars when wide (eg; 🏰) chars are in the doc
  2. It takes a few seconds to warm up?? Until then it errors with exit code 123 🤔

📆 **October 27th**    
Been thinking about how to make a terminal email client, using `himalaya` and Super Glass, will explain my idea another tim.

So want to figure out a good solution to `jq`, `stdout: ignore` and actually get Mypy working in my editor using Super Glass.

Then maybe look into starting work on formatters.

Published new version with "working", almost, version of Mypy! Relatively easy bugs to fix next time.

Pretty dissapointed that that `single-source` Python package version getter, doesn't get the version when the package is published 🤦

  * [x] Support diagnostic severity (info, warning, error, etc)


📆 **October 26th**    
Made a couple of updates to the Pygls "Improve error handling" PR. Had some nice feedback.

Just want to try to fix the config merging spaghetti. And hopefully get Mypy support done!

Check 1:49 for the choppy audio thing.

Got Mypy test working! And tidied up lots of other stuff that should have been tidied before. Might even have `severity` parsing working. But the Mypy LSP config not actually working in my editor.

Need some sort of config like `stdout: ignore` to fix `jq` test

📆 **October 25th**    
Had another idea to dogfood Super Glass and use it to display my Mypy diagnostics. 
  * [x] Think about the LSP notification error handling problem

Got a PR sent for Pygls improved error handling

Had to give up adding Mypy support because I hadn't figured out the whole thing with merging user and default config.

📆 **October 23rd**
First time back from being ill, catching up with everything. Couple of Pygls issues to respond to, and new idea for CLI Tools: self-contained apps, eg; auto `jqi`-like outputing of `jq` queries, could also apply to SQL, etc.
  * [x] Find stick/thing to stop table/webcam wobbling so much
  * [x] Write the tests for the default configs currently supported
  * [x] Pytest timeouts

📆 **October 19th**    
* [x] Quick solution: make a noise, or highlight latest follower for 15 mins

Main thing is to get foramtters working, I wonder how long that will take? Could be quick! Really I need to get to all the grunt work of adding loooooooooooooads of defaults.

Damn, ok, actually need to write some tests and check out wrapping server in try/except.
* [x] Explore wrapping as much of the server as possible in try/except

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
