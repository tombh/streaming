# Journal
## 2025
📅 **February 18th**
Morning: Actually got transparent scrollbar working last night, after stream. So just want to do some tidying up of the code, then can commit. Then might have a look at using a screen capture to get the terminal palette's true colours.
Afternoon: Basically did everything I said I would from this morning. Stopped at an interesting place, where I'm bucketing all the colours in the screenshot that have continuous runs. I can see that the most common run length is 7, and I'm assuming that they represent all the colour squares printed out by the terminal. Feeling hopeful 🤞


📅 **February 17th**    
Afternoon: late start today. Got e2e scrolling test working on Github, turned out to be just `sh` weirdness, so using BASH everywhere now. So going to try to make a scrollbar at last 🎉
Evening: Got the scrollbar working 😏 Now in the middle of implementing opacity, getting bogged down in the performance of querying the character for the layer below.

📅 **February 16th**    
Morning: Just gotta get some test for scrolling done, then I can start on a Tattoy scrollbar!
Afternoon: Had some poor internet, interruptions started around 1pm and some big drops at 3.30pm 😞
Evening: Got the scrolling test working! Were some quite hairy moments with all of the things to consider around input. Ended up having to make the Steppable Terminal use OSC Paste to send more than one character to the Tattoy instance. But couldn't tests to pass on CI 😭 I _think_ it's because Github's `sh` gets confused about OSC codes, so tomorrow I'll see if I can get BASH working instead 🤞.
  
📅 **February 15th**    
Morning: Responded to some Github comments on `cargo-gpu` and `stc`. Going to get on with scrollback support in Tattoy.
Evening:
  * Finally finished basic scrollback! Just need to write some tests before commmiting. Then make a new Tattoy that shows a nice little scrollbar on the right.
  * Vacillated a bit on how to support shared state in the Tattoys loader thread. Eventually just bit the bullet and added the async-traits crate, and it just worked 💪 Which meant that I could change all the read/write locks in the shared state to async. Also I discovered `tokio::sync::watch` which could well be better than the current `Arc<SharedState<...tokio::sync::RwLock>>`, but let's optimise prematurely.

📅 **February 14th**    
* Made some off-stream progress yesterday, have the basics of scolling working 🎉 So main thing is to tidy that up and work on a default Tattoy that shows a scrollbar. Then maybe work on a minimap Tattoy??
* Dom Rocks youtube: https://www.youtube.com/channel/UCLSPUhzFZWnS741w7iJ9x-A
* Great progress on scrollback, some refactoring has simplified things. I'm leaving things a bit in limbo, need to decide wether the `Screen` surface update is just for the alternate screen, or is also for the view when scrolling? I feel the latter, but I'm bit nervous about keeping the scrollback in sync. Like, how does a tattoy that works on the scrollback get itself rendered. Consider a tattoy that "rots" old text in the scrollback 🤔

📅 **February 12th**    
Morning: Made some comments on `cargo-gpu`, felt a bit awkward 🙈 Gotta start now with the e2e test fail on CI, it is very weird, hopefully it takes me less than a week to fix 🤞
Afternoon: Yay \o/ I did 🥳 e2e tests passing on CI. And improved a few other little things whilst I was at it. Now onto scrollback.
Evcening: Beginning to implement alternate scree behaviour. A bit uncomfortable about basically just sending `IsAlternateScreen` messages everywhere. It works, but is there not another way?

📅 **February 11th**    
Morning: Didn't stream yesterday, weird. So just want to continue with the e2e tests today. Maybe get on to supporting scrollback?
Evening: Got all the e2e tests working locally with a `--retry 1` to get a couple of the flakey tests passing. But tattoy::e2e::resizing fails consistently on CI 😞
* YourInty shared the talk on the cool Amiga demo: https://www.youtube.com/watch?v=WDfrA7PE-G0

📅 **February 9th**    
* Dom Rocks recommended Rob Scallon learning Kora: https://www.youtube.com/watch?v=PTWpoZITraE
* Made good progress on writing actual e2e tests for Tattoy. Got quite stuck in the resize tests, but it's good because it's surfacing lots kinks in the Shadow Terminal code.

📅 **February 8th**    
Morning: feeling ready for the day, but worried that I'm not going to figure out this Tokio tests issue. Basically the best thing would be to come up with a minimal reproduction and show that to some people with more experience.
Afternoon: finally finished the shadow terminal refactor! 🥳 It's such a relief. Everything's working well, the tests, they're soooo fast, like unit tests even. And the original application itself still works of course haha, maybe even a bit faster too.
* [x] Refactor "shadow terminal" into its own crate and use it for dogfoodable testing.

📅 **February 7th**    
Morning: hoping to wrap up the shadow terminal, with both tests for the shadow terminal crate itself, and to use that new crate to test Tattoy.
Afternoon: got really stuck in the Tokio runtime not spawning tasks in tests. I've spent hours on it now and not figuired it out. I have a hunch that it's to do with opening a PTY, that it somehow interferes with the Tokio runtime. Feeling pretty demoralised 😞

📅 **February 6th**    
Morning: Carrying on with the shadoow terminal refactor
Afternoon:
Finally got the ShadowTerminal refactor working! 🥳 Still some minor bugs around complete shutdown. And need satisfy all the Clippy lints. But great progress, feeling good 😊.

📅 **February 5th**    
Morning:
Continue with the refactor of the shadow terminal...
Evening:
Over 8 hours streaming! Just continued with the refactor of the shadow terminal. Feel like a good sketch of the design is in place, just pair programming with Clippy now.
Had great chat with tarb, he linked a paper from his team about CD at scale: https://www.usenix.org/conference/osdi23/presentation/grubic

📅 **February 4th**   
Afternoon:
Got a nice little refactor done of the tasks/threads, now everything is passed throguh `Err` and `?`, 
rather than using shared global state to keep the error.

Evening:
Decided to refactor out PTY and shadow_tty into ShadowTerminal. This will allow me to dogfood significant parts of Tatty for integration testing.

📅 **February 3rd**   
Afternoon:
* Finally got graceful shutdown working!
* Had a awesome chat with @new_duck about their Porffor project, their living the dream, making a living writing open source. Want to make sure that keep abreast of all developments https://bsky.app/profile/goose.icu


📅 **February 2nd**   
Went down a winding rabbit hole trying to figure out how to gracefully exit in all circumstances, in order that the user's terminal is always returned to "cooked" mode. The big problem is that I don't think there's a way to pass the `impl BufferedTerminal` terminal between tasks/threads 🤔 But I think it might be possible to actually make all exiting happen through the renderer, so that the `BufferedTerminal` doesn't need to be passed around 🤞
  * [x] `CTRL-D` doesn't fully return to terminal, needs extra `CTRL-C`.
  * [ ] Implement scrollback/history.

📅 **February 1st**    
  * [x] Look into performance, especially scrolling in nvim.
  * Started looking into history/scrollback. Made the sobering realisation that we have to intercept STDIN, parse the bytes for known events, and if we consume those bytes then they should also be culled, ie not forwarded. Whilst I think that's possible, I feel like it would be buggy 🤔
  
📅 **January 31st**    
  * I tried `tokio-console` but conficted with my existing `tokio-subscriber` ENV filter and file log. Also it made Rust re-compile EVERYTHING on every little code change. But it was interesting because it showed that Tokio creates a whole new blocking task for every single key press 🤔 I feel like figuring that out could lock a big performance boost.
  * Also want to try: https://github.com/KDAB/hotspot and https://github.com/flamegraph-rs/flamegraph and generally follow the advice from https://nnethercote.github.io/perf-book/profiling.html
  
📅 **January 30th**    
Tattoy:
  * [ ] Explore a method to get any terminal's pallette colours in true colours.
  * [ ] Up and down aren't detected in `less` or `htop`.
  * [ ] Double width characters aren't passed through, eg "🦀".

📅 **January 30th**    
Morning:
Just spent nearly 4 hours working on resizing. Very nearly there. Also wrote up some really helpful docs in the README about the 6, yes 6, different terminal-like things in Tattoy.

Evening: Finally got resizing done and started on performance improvements.

📅 **January 29th**    
* Look into using https://github.com/kas-gui/easy-cast instead of `as` conversions. Thanks @JustusFluegel.
Tattoy:
  * [x] Background colour of " " (space) isn't passed through.
  * [x] Bold doesn't get passed through properly, run `htop` to see.
  * [x] Cursor isn't transparent.

LEFTOFF:
Trying to get the PTY to recognise resize. Managed to get it resizing sideways but quite up and down.

📅 **January 28th**    
Wrach:
  * [ ] See if workgroup offer any speed improvements
  * Hit yet another showstopper 🤦 A `cargo` coredump, made a discussion on the Bevy repo to see if anybody has any ideas: https://github.com/bevyengine/bevy/discussions/17583

📅 **January 27th**    
  * [x] Submitted PR: https://github.com/Rust-GPU/cargo-gpu/pull/41
  * [x] Submitted PR: https://github.com/Rust-GPU/cargo-gpu/pull/42

📅 **January 26th**    
  * [x] Migrate Bevy from 0.14 to 0.15 

LEFTOFF:
  * Found a big bug in my `cargo-gpu` `support-ci-testing-old-rust-gpu-versions` branch, it doesn't support workspaces!
  
📅 **January 25th**    
LEFTOFF:
  * Thinking about using https://github.com/elastio/bon as a way of being strict with `#[non_exhaustive]` 🤔

Wrach:
  * At 0h18m I increased the bit rate from 2500kbos to 3000. Check what difference it made.
  * Found https://github.com/nicopap/bevy_mod_sysfail. I think it basically allows using Rust's idiomatic `?` operator in Bevy systems. I'd like to implement this one day.

📅 **January 23rd**    
Misc:
  * Debugged problems with the Wifi, or at least the internet in my current apartment. So ended up just on 4g. Made a new `tbx watch_claro_balance` command by scraping https://simple.claro.com.ar/inicio. Researched data packs, the best pack seems to be the 15Gb for 8000 pesos, so about 530 pesos per Gb. And rough calculations show that just streaming itself uses about 1Gb per hour.
  * See 2h55m in VoD to check the quality of the particle sim.

📅 **January 22nd**    
* [x] Respond to a PR review on `cargo-gpu` https://github.com/Rust-GPU/cargo-gpu/pull/34#pullrequestreview-2563118166

📅 **January 21st**    
* [x] Just say "hello" and remember how to do this streaming thing. Iruvinza, helping me out ❤️
* [x] Update my Twitch profile.
* [x] Make PR for doc update on `twitch-tui` repo: https://github.com/Xithrius/twitch-tui/issues/664

## 2024

Didn't even stream once 😞

## 2023

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
