# Journal
## 2025
📅 **April 20th**
Morning: Added a few more tweaks to my keyboard layout, and got treesitter incremental selection working, let's see how it goes... But main plan today is to add the little indicator pixel and then start on the Twitch plugin.
* Stream went down at 13:25
Evening: Got the little blue indicator merged. Feeling a bit better with the keyboard but there's a bug where the treesitter incremental select sometimes crashes 😭. Started working on the Twitch Tattoy plugin, but nothing of note yet. Did find out that Twitch has an API endpoint for emotes, must check it out. Noticed some bugs in Tattoy from the dogfooding, it needs a PTY output change after a resize in order to actually render the resize. Also doesn't seem to parse a CTRL-SPACE.

📅 **April 19th**
Morning: First thing is see if I can get undercurls working. Also want to make sure that Tattoy is handling all key combos, like CTRL-SPACE for example. Then maybe make a start on the Twitch plugin, I was wondering if there's a convenient way to get the image data for all known Twitch emotes?
Evening: Finally got undercurls working, but it involved adding code to wezterm. I'm quite surprised that termwiz doesn't already support undercurls, so maybe I've added code for no reason, just because I couldn't find the right code? There is also some strangeness with the $TERM variable, or at least how neovim determines the capabilities of the user's terminal. So I have to set TERM=tmux-256color to get undercurls working in neovim - they work fine in tattoy just on the CLI. Finished off the day nearly completing the indicator for the top right, because now that I have cursor shape and undercurls there's no excuse for me not to dogfood tattoy! But I do it without any shaders so I don't even know if tattoy is running, hence the need for a little indicator. keyboard layout is still kicking my arse.
* The colour chosen by YourInty for the indicator is: #0035a1

📅 **April 18th**  
Morning: Some great feedback from Fern and Zebcode about Tattoy installation:
  * Zeb tried in WSL, it was a bit tricky getting Rust going, and even then once it was working it just filled the screen with ANSI codes. Was the same in Windows Terminal and VSCode.
  * Fern's screenshotter seems to use another form of compression that causes the palette parsing to fail. I think the takeaway from that is that we just need to better communicate to the user about how to verify that their terminal is true colour and that their manual screenshot isn't using compression. In theory it should actually be possible to handle PNG's reduced palette compression, but that will get complicated because will need some kind of fancy code to calculate the size of palatte squares.
  * Still don't have a reliable way of indicating to the user that their terminal is not true colour.
* Wifi went bad at exactly 13:26 and 17:30.
Evening: Got cursor shape working and fixed a bug, the one where the cursor isn't hidden in alternate screen. But got stuck trying to figure out why underline curlies aren't working. I think it's something to do with how Wezterm probes the user's terminal's capabilities and for some reason thinks that my terminal doesn't support undercurls. I think a good next step could be to use my local Wezterm repo and force its detection of capabilities. Been really hard with the new keyboard layout but I persevere.

📅 **April 16th**  
Morning: Just want to get the smokey cursor port done. Then maybe do a plugin for Twitch??
* Wifi went bad at exactly 13:26
Evening: Merged the plugin code 🥳

📅 **April 15th**  
Morning: So write a test for the new plugin, tidy up the code and port the smokey cursor. Then maybe think about a quick Twitch bot integration??
* There was just a short moment of buffering at 13:27
* Dropped frames at 13:45
* IP at 13:57: 	181.117.11.23
* Nothing happened at 17:27 😞 But the stream DID go down at 17:30!!!!!!
Evening: Got the plugin input messages done and pushed and green 🎉 Started working on the port of smokey cursor to the new plugin architecture, but proving a bit more involved than I thought. But good productive day.

📅 **April 14th**  
Morning: Even more modal keys configured today, let's see if I can keep my fingers on the home row! Already found missing keys like ESC for example. And so for the main coding of the day just carry on with the plugin stuff, think I'll actually write the e2e test for the input plugin in Rust.
* YourInty came up with the idea of an anonymous pool of arrival sounds for people that don't have arrival sounds yet.
* Scott recomends the Ferris Sweep split keyboard.
* Wifi went bad at exactly 13:27
* inmate302 recommends the band Opeth:
  * To bid you farewell and Will o' the wisp and burden are notable tracks
* Wifi went bad at exactly 17:27!!!
* Set alarm tomorrow for 13:26 and 17:26. Also setup `mtr`.
Evening: Got a nice working Rust plugin that flips the terminal contents, it's nice and succinct, and can be easily used for e2e testing. Just need to tidy up all the code, write the test and port the smokey cursor plugin, then the plugins are done for now.

📅 **April 13th**  
Morning: Experimenting with a modal keyboard layout to help keep my fingers on the home row and prepare me for a nice little travelling split keyboard. So can a get I commit for the plugins in today?
Evening: Yaaay got an actual commit of the basic plugin code into a PR and made some good progress on the plugin input side of things.
* Everyday for the last 3 days, at around 17:37 (or was it 17:27!!!!!!!!), the stream struggles???

📅 **April 12th**  
Morning: Whilst playing with the plugin/STDOUT stuff last night, I found that using a `std::thread` solved the application close deadlock issue 🤔 Good but I don't quite understand how, might write it up on a Rust forum somewhere. So let's see if I can get the STDIN protocol working for the plugins today...  
* Notes installing Umpriel's Bot:
  * The default `.env.local.example` doesn't contain all the required ENV vars
  * It's not clear what the Twitch Bot's redirect URL should be
  * https://twitchapps.com/tmi/ is deprecated and the new site that it links to has a lot of options and isn't clear how to use.
Evening: Spent waaaay too much time fixing rocks.nvim, only to copy everything from my personal account and have it work straight away. But anyway, good day, got Telescope showing syntax colours again. Made a good start on setting up config for the Tattoy plugins and am half way through writing the first e2e test for the plugins.

📅 **April 11th**    
Morning: Wanna have a look at a `cargo-gpu` PR. Then find a solid reason for not being able to use the serde stream reader with malformed JSON.
Evening: Realised, with the help of Lord again, that I had misinterpreted the error logs of the stream parser. The repeated errors weren't from the parser restarting from the beginning, they were from the parser failing at each bad character in the stream, so basically restarting the parser was always working. But now there's a problem where the child process won't exit because the child's STDOUT is still being read, leading to deadlock.

* Informal "how are you?" in Bengali: কেমন আছেন?

📅 **April 10th**  
No stream.

📅 **April 9th**  
Morning: Just keep on keeping on, want to get the pluging stufff working. Actually, I was thinking it'd be pretty easy to make Tattoy plugin for Twitch, so we could have a set of commands following the convential log levels, `!tt-error $arg`, `!tt-warn $arg`, `!tt-info $arg`, `!tt-debug $arg`, `!tt-trace $arg`, each command would take a string of text as an argument, that string of text would then be used to match text in my terminal, and depending on the "level" of the command would play a short but fun animation anchored to the first occurence of that text in my terminal. So for example chatters could say that `!tt-error fn foo()` and that function in my editor would flash red or something.
* Hyperland also has a good plugin architecture, says Fern.
* "Yo": https://translate.google.com/?sl=auto&tl=en&text=%D8%B9%D8%A7%D9%84%D8%B9%D8%A7%D9%81%D9%8A%D9%87&op=translate
* "What's up?": https://translate.google.com/?sl=auto&tl=en&text=%D8%B4%D9%88%20%D8%A7%D9%84%D9%88%D8%B6%D8%B9&op=translate
* Umpriel's Twitch Contrib bot: https://github.com/Umpriel/twitchContrib
Evening: got stuck in trying to support automatic restart of the stream deserializer when malformed JSON is encountered, didn't get very far, but still have hope.

📅 **April 8th**  
Afternoon: late start, had an idea last night, use serde_json'd streaming thing to automatically parse messages from plugins, that way I think it might be possible to avoid any kind of delimeter.
* Stefan recommends this series about Serbian politics: "Muzika iz serije bela ladja"
Evening: Great collab with Lord getting the plugin protocol working. Really happy to have gotten serde stream deserialization working, no need for delimeteres. Got a basic "plugin" working, that `tail`s a file of JSON messages, verrrry happy with it. So next step will be to send messages to the plugin over STDIN.

📅 **April 7th**  
Morning: Just in the grind, just gotta keep going with Tattoy issues, I think I'll adding plugin support today...
* One should only be able to `!unlurk` after `!lurk`ing.
* Think about how to communicate config file changes 🤔
* Put all default keybindings in README/docs.
* My name in hebrew! טומס
Evening: Mostly chatting today. But did get a verrrrry basic plugin setup, just rendering the output of `ls`. Need to know think about the protocol a bit more, mainly what do I use as a delimeter for JSON messages? Newline? Or prefixing the length of the message??

📅 **April 6th**    
Morning: Feeling okay today, thought I might be ill, but seems I just need rest. So just get on with a few more keybindings today I reckon.
Evening:
  * Got all the basic keybindings working! And learnt something important abot `.collect()`, it can wrap the whole collection in a `Result`, was tough learning, but got there.
  * 20% progress to public release.

📅 **April 5th**  
Morning: Had a little look at ntfy.sh, I don't even need to sign up to send a notification to a topic?? That's easy then, but I don't see an easy Twitch endpoint to get immediate channel-gone-live.
* [ ] Send a blank surface when toggling Tattoy renderer off.
* [ ] E2e test running Tattoy with an empty config file.
* LordMZTE's RSS: https://mzte.de/feed.rss
Evening: Feeling a bit under the weather but got the first green keybindings PR in, niiice. Gonna rest.

📅 **April 4th**  
Morning: Back to the streaming life. Main plan is to try to get keybindings done today. Maybe setup raid alerts?
* [ ] Add logging the config file at the trace level
* [ ] Suggest GPU log filters in README
* Checkout Gary Clark Jr
* https://ntfy.sh and setup an alert to it from my bot when stream starts
Evening: Thanks to help from chat, LordMZTE in particular, I finally got type-safe keybindings setup. Just a tiny bit of tidying before I commit. Then I can go and add all the basic user-configurable events in another commit.

📅 **April 3rd**
No stream.

📅 **April 2nd**  
Morning: Had a bit of a bad night's sleep, funny tummy, so might be bit slow today. Plan for today is to basically have a look at adding Tattoy keybindings.

📅 **April 1st**  
Morning: No April Fools yet... (woaah Fern sent me the daily bandle that had a Rickroll 😏)
  * Wondering about the `!osd` command, basically want a way to prevent newcomers from spamming it. Thinking about stuff like only allowing it once a user has been here for a week, or something liket that?
  * Just get on with some Tattoy stuff, like the UP/DOWN arrow keys bug...
* ScottB recommends: https://www.youtube.com/shawnp0wers
Evening:
  * Got the `htop`/`less` UP/DOWN arrow keys bug fixed 🥳 Thougth it was going to take longer, but no. So another productive day.
Evening: Got the basics of keybindings working, left it all in a mess, but should be easy to pick up tomorrow, well Friday, seeing as tomorrow is my day off.

📅 **March 31st**   
Morning: Changed my layout again, using Twitch's own Stream Manager in my second "monitor", and shifted my workspaces around, so will take a bit of getting used to again. I good streamrrr. What's the plan today?
  * Well 5 hours in and I've already got nicely distracted. Tried adding a OBS browser source, don't think it even works on Fedora, let alone Asahi, but would love to know either way. Tried Flatpak but couldn't add the OBS repo even. Also looked at OBS-CEF as a Chromium based solution but no packages for that either.
  * So finally just wrote OSD functionality for the bot! So chat can now do `!osd sdfsdf` commands and we should also see a OSD popup for new followerws 🤞
  * Installed: `webkit2gtk4.0-devel-2.46.5-1.fc41 webkit2gtk4.0-2.46.5-1.fc41`
Evening:
  * Got Twitch Follower notifictions working!!!! Nyxisprime's advice again very helpful, he showed us the  `twitch-cli` mocj websocker server: https://dev.twitch.tv/docs/cli/websocket-event-command
  * Got a couple of new chirps

📅 **March 30th**  
Morning:
* Dear Journal, got a second "monitor" setup, it's my phone VNC'd to a local virtual desktop. So streaming is a bit unfamiliar, but I'm sure I'll get used to it in no time. It'd be nice to see my emails and who else is streaming etc, but not important right now. And it'd be nice to have a keybinding to change focus from the live desktop to the second desktop, cos moving the mouse into a desktop doesn't actually automatically change the focus, you have to click something, which often confusing.
* So I think the main thing for today is just try to do some of the Tattoy issues.
* Maybe play some Geoguessr later?
Evening: Got the lossy palette parsing bug in!! Really felt like it was going to be a marathon this morning, but turned out to be fairly straight forward and I'm happy with the code.

📅 **March 29th**  
Morning: Was just looking at the Wayfire repo before stream and saw that, via wlroots, Wayfire has support for virtual monitors! So this evening after stream, I'll try to set up my phone as a second monitor. Things I wanna do today:
  * [ ] Look stelee's gist and icon things in the weather app
  * [ ] Reply to the latest issues and PRs on `cargo-gpu`
  * [ ] Tattoy issues, palette parser bug etc.

* Stelea's Emoji Picker updates: https://gist.github.com/ste1ee-dot/94b483dff30f7074f60656947529e51d
* Nyxisprime found this doc for letting the bot automate getting its own access tokens using the CLIENT_ID and CLIENT_SECRET:https://dev.twitch.tv/docs/authentication/getting-tokens-oauth/#client-credentials-grant-flow
* Bot randomness improvement: `let daily_user_rng_seed = format!("{}{}{}{}", ENV["SECRET], user_id, date, reset_counter)`. Then have a new command, `!mydailyrng`, which responds in chat with your daily RNG probablility, but _also_ resets to a new seed without revealing that new one. With 1 hour cooldown on the reset.
Evening: Refactored the state machine, it's definitely reading better now, but there's one `if` condition that doesn't read very well and feel like I spenmt at least 30mins trying figure out. Anywayyy made a start on the IndieJonez palette parsing bug and all ready I'm seeing better parsing, but not quite. Next step is to use the new `self.pixel_certainty` accumulator along with the exact type of palette being parsed.

```rust
// From @FernOfSigma. Learn proper Rust Errors!
#[macro_export]
macro_rules! oops {
    ($kind:ident, $message:expr $(,)?) => {
        Err(std::io::Error::new(std::io::ErrorKind::$kind, $message))
    };
}
```

📅 **March 28th**  
Afternoon: Not totally sure what I'm going to do, tidy up the frame stuttering code I've been playing with the last few days. And then look into getting bracketed paste working for large pastes?
* [ ] Fix for bot when wifi disconnects: start a Tokio task that pings in a continuous loop and restarts the entire bot when the pings stop.
* Checkout 3:29 in the stream for audio choppiness
Evening: Nice being back after a bit of a break. Got the FPS stuttering commit nice and tidy and merged into main 💪 Aaaaand I fixed the large pasting bug! 🎉 Feeling good about dogfooding now. So going to move on to some other issues, thinking: the palette parsing bug, then the UP/DOWN keys bug then starting on the keybinding feature.

📅 **March 27th**  
No stream

📅 **March 26th**  
Morning: Did a little maintenance on the bot, hopefully it can self-restart now 🤞 Last night I realised that the problem with my keybindings in Tattoy is todo with `tmux` not being able to know when it's in Neovim, there should a way for the `is_vim` command to recursively look into child processes for the Neovim process. Also want to do one more little tweak to the render prioritiser, I want it to drain the entire frame queue, both PTY frames and Tattoy frames, for each render. This may result in some PTY frame drops, but will prevent Tattoy frame stuttering. Let's try. Then ... I don't know! Look at the list of issues and continue the slow march to the first public releaseeeeee.
* Jonas4236 Youtube: https://youtu.be/h8T0DD2XnQQ?si=76DLEDZ9YUCkNOsU
Evening: More dogfooding today, went okay. But not quite happy with using Tattoy fulltime, it is definitely usable, which is crazy, but things like cursor flicker, frame stuttering, pasting of large text blocks not working, all makes it feel a bit subpar. Spent a fair bit of time making a easing function to smoothly reduce the frame rate to ease channel backpressure. Without much success, but just at the end of stream, Nyxisprime (No 1 in bot DB), got me to think through the renderer and I realised that it is the _renderer_ itself that needs to dictate the frame rate. Because when the renderer sees a backlog of frames it will just render them all as quickly as it can 🤦.

📅 **March 25th**  
Morning: Last night I played with performance a bit. One thing I noticed is that the `FrameUpdate` channel can get backlogged with say shader updates, and so a PTY update might not get rendered immeditely if the channel is full of shader frames, so I'm experimenting with dropping shader frames if they get backlogged. But I'm wondering if I could split the channel into 2, one for tattoys (lower priority) and one for PTY updates (higher priority). Also want to see if I can disable some unnescary screen/scrollback update handling in the shader tattoy code, shaders currently only need the cursor position, not actual cell information, well at least soon, I will want to send the screen contents to the GPU, not the scrollback.

Also, on the dogfooding theme, I want to get all my keybindings working I need for working in Neovim.

* Crab shader! https://www.shadertoy.com/view/MdBGDy

Evening: Got some really nice performance improvements done! Drop tattoy frames instead of backlog them. And aggregate PTY outputs within a tiny time window (0.1ms).
Next things I want for dogffoding:
* Keybindings for working in Neovim: SHIFT+L/R-ARROWS for selecting, PAGE-UP, ALT+D
* Flickering cursor.
* Don't just dump dropped frames, try to gracefully reduce the frame rate.

Bot broke again 😭 Websocket disconnected, so need autoreconnect.

📅 **March 24th**  
Morning: I don't know what I'm going to do today. I think the main thing I want to guide me is dogfooding Tattoy:
  * CLI config overrides
Evening: Easy day, got some nice improvements to the config and logging. I can easily launch dedicated Tattoys, so fewer excuses for not dogfooding. Although, now I see that there's pretty serious performance issues on larger terminals. At least reducing the frame rate should help, but would be good to have a bit more of an indepth look. Also RAM usage is very high, like 20% of my machine's RAM high 😬

📅 **March 23rd**  
Morning: Really glad to have gotten the Windows PR in yesterday. So maybe relax a bit today. Would like to make a start on the ANSI `^[6n` cursor position response. Also:
  * Have a look at the `cargo-gpu` issue where a shader has to have ALLL the settings in its `Cargo.toml`
  * Update my bot to use the client secret to auto update from a refresh token.
  * Update my Neovim plugins and push my latest dotfiles
  * Add more bird chirps
Useful:
  * Installing Windows on Asahi: https://pastebin.com/73bTFdfR
* Special Purpose Arabic: https://www.eimj.org/uplode/images/photo/%D8%A7%D9%84%D9%84%D8%BA%D8%A9_%D8%A7%D9%84%D8%B9%D8%B1%D8%A8%D9%8A%D8%A9_%D9%84%D8%A3%D8%BA%D8%B1%D8%A7%D8%B6_%D8%AE%D8%A7%D8%B5%D8%A9-_%D8%A7%D9%84%D9%84%D8%BA%D8%A9_%D8%A7%D9%84%D8%B9%D8%B1%D8%A8%D9%8A%D8%A9_%D9%84%D9%84%D8%B9%D9%85%D8%A7%D9%84_%D8%A7%D9%84%D9%88%D8%A7%D9%81%D8%AF%D9%8A%D9%86..pdf
Evening: Got `^[6n` merged! No excuses for dogfooding now.

📅 **March 22nd**  
Morning: Thinking about the Powershell stuff from yesterday, I think basically we have to disable all the e2e tests on windows. I do think it'll be possible to get them working, I will at least try again after getting cursor position responses working.
Evening: Finally got a basic e2e-ish test running on CI, it get's us 70% of what's relevant to Windows testsing, so pretty happy about that. Also did some more testing in the remote Windows VPS (slow and painful), and with the help of Umpriel we figured out what's working and what isn't. It's at least compiling, and even renders the minimap and responds to the mouse hover in the VSCode terminal. So I feel confident that Tattoy doesn't have any fundamental issues with running on Windows, it's probably just a matter of settings, conditional code, etc. The main painpoint is that I don't have a local Windows environment with GPU access to test modern terminals.

📅 **March 21st**  
Didn't stream yesterday, but did boot up a windows VM on Vultr to play with Windows support for Tattoy and realised a few things. Some basic things like on Windows you have to check for `\r\n` when parsing user responses (relevant for the palette consent prompt). `bash` isn't considered a normal program in Windows, because it depends on such a large amount of Linux. So I started investigating Powershell (which, BTW, has some notable similarites to Nushell). Turns out that Tattoy already works well in the default Windows Terminal and Powershell, it's CI that I need to consider. And the big blocker there is simply that my method of simulating ANSI input doesn't work, neither on the Windows VM nor on CI, what happens is that the ANSI codes just get written to output without being parsed, I'm not sure who's responsible? 

So main plan for today is just get the Windows CI build working, just disabling any of the tests that send ANSI input.

Evening: Discovered something very interesting about Powershell, on starting up it sends a request to get the cursor position, the ANSO code `^[6n`, and hangs until it gets a response. This was a interesting thing to discover, because one of the reasons that I'm not dogfooding Tattoy yet is that Atuin won't start because... it can't find the cursor! So this is definitely something that I need to fix.

📅 **March 20th**  
No stream.

📅 **March 19th**  
Morning: Don't process `!arrived` commands for people that don't have a sound yet. Record all messages in the channel in the bot. Then work on the Windows CI failing issue. Then dogfooding Tattoy.

📅 **March 18th**  
Morning: Want to continue a bit more with the bot, like adding some spammable sounds. Also want to add Tattoy to my emoji and application picker terminal popups, got start dogfooding.
* "Tom" in Thai! ""ทอม"
* szaroo's arrival translation: "oh fuck, a beaver! what a monster!"
Evening: Got the `!chirp` command working with a random chance of RUBBER CHICKEN SCREAMING ahha. And recording when anybody gets more than one RUBBER CHICKEN SCREAM in a row (because when you get a scream you also get put back in the random pool to proc another scream immediately after). Nice.

📅 **March 17th**  
Afternoon: Late start, was online but not streaming trying to fix the Twitch bot auth, turned out to just be a whitespace issue 🤦 So plan is to just carry on with the bot today and maybe do a bit of Windows CI debugging in parallel.
Evening: Managed to get the `!arrived` command working so users can choose they're on arrival sound that they can play just once a day. Using good ol' sqlite and sqlx to persist state.

📅 **March 16th**  
* https://github.com/ErikMcClure/bad-licenses/blob/master/dbad-license.md
Evening: Got MacOS CI green! And, most importantly, made a start on the tbhbot 🥳 Just got a bit stuck on the end of stream saving the access token and reading it back.

📅 **March 15th**  
Morning: Want to have a little look at yesterday's bug. And write to both the Zellij and Wezterm repos asking for advice about my use of termiz input parsing. Really I want to make a good start on the bot.
* New goal for default shader! https://www.shadertoy.com/view/cdlyWr
Evening: Fixed the bug from yesterday! Turned out to be how I was filling a fixed-size buffer from a buffer slice. Just need to detect the first zero of the fixed-size buffer and pass that on as a whole thing, rather than sending each byte piecemeal until the first zero. But now CI is failing for Windows and OSX for unknown reasons 😞.
* QEMU for Windows
* OCI for OSX: https://christitus.com/docker-macos/

📅 **March 14th**  
Morning: I want to finish the Windows bug, and see if I can get Windows and OSX tests running on CI. Then I want to start on `tbhbot`!
* Play Geoguesser on stream some time!
* I have a welcome in Krusevac, Serbia
* Lofi generator https://lofigenerator.com/
* REMEMBER: Set "Require approval for all external contributors" in CI as soon as we start using credentials in Actions, eg when we automate publishing.
* Had a idea about the palette parsing prompt: print out some true colour sample and say, "if you don't see this pretty rainbow then you don't have true color enabled, goto this link that explains how to enable trye colour".
Evening: Tough day, fixing input bugs. At first I thought that the input parser had broken because the new `pty.master.take_writer` was handling bytes differently, spent a few hours on that. But of course I'd actually forgotten how my app works 🤦 Bytes are actually only sent to the PTY if they _don't_ match a user's configured keybinding. Once I'd actually started focussing in the relevant area of the code I slowly began to realise that everything was really pretty solid in the live app, the issue was simply that the e2e tests really struggle in sending realistic input. Looks I have keyboard input sorted, with a little sleep, but mouse input seems to be corrupted at the first byte 🥺. Having to leave it ther for now. Hopefully I think of the solution in my sleep 🤞.

📅 **March 13th**    
No stream. Still did a bit of work on the Windows compatibility bug.   

📅 **March 12th**  
Morning: What do I want to do today?
* Look at the windows bug
* Look at the input splurge bug
* Run cross-platform tests for Tattoy on Github Actions
* Dogfooding Tattoy:
  * Adding a nice little shader effect to my terminal popups https://github.com/tombh/tattoy/discussions/27
  * Fixing the atuin bug will help me use Tattoy more in daily life
* Start on `tbhbot`: https://dev.twitch.tv/docs/chat/irc-migration/
  * Handy CLI arg to quickly override config file settings.
  * Just reacting to individual chat messages would be a good start
  * Start on the daily sound, something like `!arrived`
* Convert my Twitch BASH scripts to print out any channel's logo nice and big in the terminal.
* Use my Gym Ball Bursting clip as a raid animation: https://clips.twitch.tv/TriangularAgreeableYogurtSmoocherZ-4Po_zRXgTbfuKF1y
* YourInty recommended this YT vid https://www.youtube.com/watch?v=3e-nauaCkgo
Evening: Realised that the short term goal is to dogfood Tattoy. But feeling like after I've fixed the main bugs for the beta release, I'll do a bit of tbhbot stuff for a break.

📅 **March 11th**  
Morning: Main goal to make a beta release of Tattoy 😀
* Make a Matrix room for the Twitch channel!!!
* https://www.sesame.com/research/crossing_the_uncanny_valley_of_voice#demo
Evening: Got a beta release of Tattoy out! And had 2 reports of it successfully working! (and a couple of bug reports).

📅 **March 10th**  
Afternoon: Late start, wanna get the GPU code nice and tidy. Maybe make a beta release today??
* oklab colour space for human perception
* @inmate302 came up with: "Cross-platform Terminal Compositor". But they don't want the credit.
Evening: Got raided by Teej, over 1000 raiders, very exciting. Got the shader work committed.

📅 **March 9th**  
Morning: Really excited about the shaders working, wanna make lots of progress on that today.
* rbergler:matrix.org
Evening:
 * [x] Got blending tests done, and made some more improvements to the blending code.
 * [x] Got opacity easily working for the shader layer.
 * [x] Started and nearly finished a refactor to isolate the GPU code.
 * [x] Live-reloading config for shader path. Added the point lights shader to the binary for a default shader.

📅 **March 8th**  
I think I fogot to write my journal this day?

📅 **March 7th**  
Afternoon: It's time to play with getting Shadertoy running in Tattoy!
* Shaders that I want to get working:
  * Raindrops: https://www.shadertoy.com/view/DdKyR1
  * Interactive fish: https://www.shadertoy.com/view/DdKyR1
  * Some sort of fluid, but internet is playing up right now 🫤
Evening: OMG I got basic shader toys rendering in the tattoy 😲 Still sooooo much to do, but it's an excellent proof of concept 💪

📅 **March 6th**   
Morning: After thinking about it yesterday, I still think it'd be good to aim to make a beta release once I've made a basic version with shader support. Which I think I can even make a start on today. First I just want to write a little test for the minimap.
Afternoon: Spent 4 hours trying to find a bug that the minimap test surfaced. Turned out to be a very important bug to do with caching the size of the TTY in the base Tattoyer code. So in the end was really glad I found it, even though it got quite dark at times 🥺. Basic test is working now, just want to have check the minimap colours too. Then that's a wrap on this particular version of the minimap, I'll work on it again after the beta release.

📅 **March 5th**   
* It's weird that the Shadow Terminal is strict about only sending primary screen updates on the Scrollback output type and alternate screen updates on the Screen output type, whilst Tattoy forwards all screen updates on the Screen output type. I think this is because I don't want the Shadow Terminal crate to be too prescriptive about how it sends output. Whilst with tattoy plugins I know what the domain is, and now that it's actually useful to overload the Screen output type with all possible output.
* Evening: Almost got the minimap animation commit ready. Still want to write a test. But there's a bug where the screen doesn't automatically "scroll down" when old content goes off the top of the screen. Also some lag issues in the minimap updating, eg when scrolling in Neovim.

📅 **March 4th**  
Morning: Still the same goal as yesterday, make the minimap show live updates of the alternate screen, but also do the fancy slide in/out animation when the mouse hovers over the right-mouse column.
* ONLY EVER USE ALPHA ON TEXT CELLS NOT PIXELS! It doesn't work, you get weird staggered line.
Evening: Got minimap working reeeeeeeal nice 😏. Slide animation using a state machine, hot-reloading config and transparent background for empty minimap area. Some lag issues, which can wait for another time, so just want to add the scollback behind the alternate view and add a little test.

📅 **March 3rd**  
Morning: Monday morning, start of the week. Still main aim is to get the minimap showing live updates of the alternate screen. But probably a winding road before I get to that.
Evening: Completed the refactor to make the existing core tattoys use an architecture more like how the plugins will be. A couple of performance issues though.

📅 **March 2nd**    
Morning: My goal for today is to try to get the minimap live-updating an alternate screen app like Neovim.
Evening: @nayrberger and I (mostly nayr), looked into using an async fn pointer in a vec to avoid a minor amount of boilerplate, much was learned about futures, trait objects and lifetimes changing types. Conclusion is that a macro would probably be best to avoid the tattoy loading boilerplate. Nevertheless, managed to convert the scrollbar tattoy to the new design that more closely mirrors the plans I have for plugins.

📅 **March 1st**    
Morning: Going to see if I can get that `cargo-gpu` PR finished, fix conflicts and review notes.
Afternoon: Yay got all my `cargo-gpu` things done.

📅 **February 28th**  
Morning: Last night off stream I got the palette true colour converter refactor done. So there's only one bug left, before I could commit, and that's resizing vertically. Then I'd like to look into some weird latency issues.
Evening: Finally got the diffing refactor done, committed and green on CI 🥳
* Scott B watched Red Dwarf.

📅 **February 27th**    
Morning: Submitted 2 PRs to `wezterm`, one to add the really useful `Clone` to `termwiz::surface::Surface`, there's looooooads of open PRs, so it may take a while. But I can always use my fork until then. Only have a couple of hours to stream today.

📅 **February 26th**  
Morning: I had a thought last night, that the diffing approach is probably actually slower in a lot more cases than you'd imagine. Like certainly for say scrolling in Neovim, that dirties every line, so may as well just send a copy of the whole screen, than can just replace any existing copies in the various tasks, without having to iterate over diffs.
Afternoon: @chanddu16 said he started journalling everyday too! And @zebcode woke up anxious Buddhism Without Beliefs audiobook helped him relax.
Evening: Got the shadow terminal outputting both diffs and full screens, and fixed up scrolling. Left off with the true colour conversion, need to support both converting diffs and surfaces. Then need to get resizing fixed. Then I should be done with this commit. I think the next step will be looking into performance. Would be great to make a debug overlay of all all the timings and FPSs of the various parts.

📅 **February 25th**    
Morning: Just want to get this whole diffing performance improvement stuff done. Although I was thinking last night, and I realised that iterating over tattoy tick calls, and waiting for each one to return before calling the next, must surely have an impact on perceived rendering. Also, perhaps more importantly, that design doesn't mirror how plugins are going to work.
* Fern suggested visiting Santiniketan, the home of Tagore.
Evening: Diffing refactor is going pretty, got scrolling working again (although with new lagging regression??). Spent a lot of time trying understand why diff approach completely breaks down after a resize. Never got to the bottom of it, still curious, but I think it's best just to push and send entire surfaces on resize, we'll need that functionality anyway, for certain recovery situations.

📅 **February 24th**    
Morning: I was thinking it'd be good to just carry on with the minimap, want to make it behave exactly like the VSCode one. And also think I can improve performance.
Evening: Did the big diff-support refactor, but didn't quite make it. Realised that I can actuall pass Termwiz changeset over the protocol channels, should be exactly what we need.

📅 **February 23rd**     
Morning: Had a nice idea this morning, all pixel surfaces should actually be constructed using an `image` of pixels whose resolution is significantly bigger than the TTY screen, then we can use `image`'s resize functionality to allow placing pixels at something more like floating point resolution. For example, when a pixel is close to the borders of multiple TTY cells, its colour can bleed between the cells, giving the illusion that its moving a units of the TTY, but the higher units of the imatge. Aaaaanyway, still need to tidy up the minimap. I think there's even a pretty significant bug in the existing pixel placement code.
* @YourInty's blog:
  * https://www.jlatza.de/posts/index.xml
  * https://www.jlatza.de/en/posts/index.xml
* Let it be known, that on this date, Febraraury 23rd 2025, @zebcode came up with "Hackalong". From hence forth I shall be describing my streams using the aforementioned noun.

📅 **February 22nd**   
Had some really great chats today ❤️ I think because of the weekend, had a view more visitors than usual. So not so much coding, but was fun anyway. Replace `clipman` with `cliphist`, hopefully that solves my long-standing clipboard annoyance where copying would fail like 10 times per day 🥺 Also, got a MVP versio of a terminal minimap working 💪 Crude and slow, but wow, am I the first person to ever make a terminal minimap??

📅 **February 21st**    
Morning: Last night I managed to get programtic colour manipulation working, like saturation, lightening etc. So I was thinking that a nice thing to do today, would be to support changing those colour values dynamically in config.
Evening: Commited colour grading! Sooooooooooo cool, got config hot reloading changing the saturation, brightness etc of the final Tattoy render 🤯

📅 **February 20th**    
Morning: Started straight into some buffer bloat on the wifi, so switched to 4g 🫤 So just gonna crack on with the palette parsing stuff. Wondering about whether the first thing I do with true colour is make the minimap, it'd be so cool to see it live updating as you type, scroll and open nvim.
Afternoon: Pushed the palette parser commit! Was some trouble with lib*-dev packages on CI. But now trying to actually use the palette in the compositor.

📅 **February 19th**     
Morning: Using hotspot to proxy the apartment's 2.4Ghz wifi to 5Ghz, so hopefully then if I need to switch between wifi and 4g it'll be easier, like I won't have to close and reopen OBS. But basically hoping to get palette parsing done today. Will also require setting up config directories, because I'll need to save the palette colours somewhere. But would be really cool if I can do something with the true colours today, like a raindrop effect?
Evening: Got palette parsing done!! I am sooooo happy about this. It remains to be seen how it works in real world conditions, but it works damn well right now. Want to write a test for detecting false positive row starts. Then can get on with adding the found true colours to config. Also, should really ask for consent before taking a screenshot.

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
