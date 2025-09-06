# Journal
## 2025

📅 **September 6th**     
Afternoon: 2 days streaming in a row, been a while since I did that. It's been tough being stuck on this same bit of code for so long. But I think this is why I do my own projects, so that I have the time to really be with the parts that challenge me?

Evening: I'm glad I persisted with getting an intutive DEM projection of the final_viewshed test viewshed, because once I did, I found an off-by-one bug elsewhere. And that seems to have change my benchmark Cardiff viewshed yet again. So there's just some minor lints to fix before fiiiinally getting this commit committed.

📅 **September 5th**     
Afternoon: I think I managed to get that text issue sorted from last stream, just some subtle coordinate/projection stuff. So I can I finally get this commit committed today?

Evening: Nearly got all the tests passing. Just the `final_viewshed` one is failing. We already have 3 coordinate conversion functions, and I think I need one more, but just for this last test. `dem_coord_to_latlon()`. Also, even then, there are some notable differences in the extent coordinates for the final viewshed, so best to double check that that is actually expected.

📅 **September 4th** No stream        

📅 **September 3rd**    
Afternoon: Haven't been streaming much at all recently. Still been doing a bit of viewshed stuff though. I think I've got the viewshed reconstruction commit functionally ready. Just need to clean it up. I fixed that skew that I mentioned in the last stream (August 29th), it was to do with using the top-left coordinates of the DEM for the AEQD anchor when reconstruting viewsheds, but using the centre of the DEM for interpolating all the elevation points to a 100m grid. But then once that was fixed it revealed the bug that Ryan had mentioned before, that the band deltas containted a lot of zeroes. Not only did I see that, but I also found that longer band deltas weren't even starting at their real beginnings, so most bands started off with distance deltas well above zero, obviously leading to quite incorrect viewsheds. I'm still not quite sure how to pragmatically verify our viewsheds, there are so many factors involved, height of the observer, construction of bands, curvature of the earth, refraction etc. But at least for now I think we've got a decent basis to start exploring from.

Evening: Converted most coordinates into new types to try to prevent say a lat/lon coordinate being used when a AEQD coordinate is expected. Then started fixing the viewshed tests, but the reprojection step makes the results unintuitive so seeing if there is a way to project back to DEM-relative coords, but got stuck.

📅 **August 30th - September 2nd** No streams.     

📅 **August 29th**     
Afternoon: Offline I managed to get viewshed construction into a really good place. Just a little issue where there's some skew, there's a couple of islands in the Bristol Channel that show up in viewsheds from Cardiff, but the little visibility boxes for the islands are off by about 200m. So first thing is to try to generate the same viewshed with `gdal_viewshed`, that will show us if the problem is in the original DEM data or not. Then just want to get on with all the tidying up that needs doing for this big viewshed reconstruction PR.

Eveninb: Didn't get anywhere with the skewed viewsheds. But I did realise/remember that metric projections cannot be used to calculate true distances, which feels very relevant to the offsets I'm seeing. I just can't figure out where the distance calculations are coming in?? Is it because I add the ring sector coordinates to 0,0 rather than the coordinates of the PoV point?

📅 **August 26-28**  No streams.  

📅 **August 25th**   
Afternoon: Got a bit of stuff done off stream, like adding `fjall` a key-value store for the ring data. Would be great if I can get viewshed reconstruction completely done today, that would mean displaying viewsheds in their correct geolocation on a map.
* Newcomers to programming at an advantage learning Rust: https://youtu.be/meEXag1XCFw?si=z7lx4D36GUf7ANUi

Evening: Seem to have gotten most of the reprojection code working, just the small issue of it displaying off the african cost rather than Bristol, but hopefully that's just a matter of double checking all the projection defintion strings.

📅 **August 24th** No stream.    

📅 **August 23rd**     
Afternoon: Me and Ryan are going to do a bit of pair programming on viewsheds today.

Evening: Great session, geeking out, got deep into GPU stuff.

📅 **August 22nd**     
Afternoon: Managed to get the maths side of viewshed reconstruction done on my day off yesterday. And so then started to see if I could reconstruct a viewshed from my "small" Bristol tile. But the RAM/disk usage is a bit prohibitive, 17GB of ring sector data, just for the Bristol tile!! I think that means that Everest sized tiles will produce about 17TB 😬. So the first thing to do today is to do the reconstruction from saved data (after a full GPU crunch) sector-by-sector. Then after that I need to start looking into how to project all the geo points back into coordinates relative to those defined by the original DEM tile.

Evening: Got my first actual viewsheds showing! So just cleaning up all the code now. But gotten a bit confused about the orientation of the y-axis for the trig calculations, should y be flipped or not??

📅 **August 21st** No stream          

📅 **August 20th**     
Afternoon: Well let's see if we can get these viewsheds displaying.

Evening: Got a sinking feeling today when I realised today that the outer spokes of the reconstructed viewsheds have huge gaps in them. And so the worst case for say Everest would be to increase the number of sectors by 1000! I can't believe I overlooked this. There must be something in the original paper about this?? Another solution could be to just make fatter bands of sight. So things to think about. But at least I have made some progress with reconstructing viewsheds. There's just this weird bug where the rings sector coordinates aren't appearing square on the GEOJSON viewer.

📅 **August 19th**     
Afternoon: Probably just a short stream. Looks like Ryan is able to run Everest then. So I think I just need to get with resconstructing viewsheds so we can actually verify the correctness of our results.

Evening: Ryan's been running the new code and got it down ~8 seconds per sector (we need ~1s per tile to crunch the whole world in 2 days). He's also got some CUDA profiling and we'll be doing some pairing on Saturday. I managed to get some "working" code to reconstruct a viewshed, but it currently looks like a mess. One big question is how to union all the ring polygons so that we remove all overlapping intersections. Hopefully something already exists in `geo` to do this, I just haven't found it yet. Or just my polygon constructor code just isn't right. I should start off tomorrow with some unit tests for the funcion.

📅 **August 18th**     
Afternoon: Looked into the `.bt` DEM format last night, it's a simple format that I used in the old C++ version of total viewsheds. I imagined that in the 8 years that have passed since I last worked on it there'd be something better, well I mean there is GeoTiff, but the `geotiff` Rust crate is not well written and errors trying to load the Everest file. So I feel settled on `.bt` now. Also today I want to fix the GPU batch size finder to actually be balanced. And I want to explore simplifying the cached calculations, I think I'm OOMing on the sort steps for the Everest file?
* Uploaded the Everest DEM to: https://x0.at/njLa.zip
* Dharma Dom's Insta: https://www.instagram.com/domrocks__/

Evening: Managed to get everything into a pretty good place for crunching Everest. Got the `.bt` v1.3 format fully supported. Got the dispatch size finder actually finding balanced dispateches. Sadly it seems my laptop is pushed to its limit crunching the Everest DEM. I think it's mostly just memory. But I have got some sectors to run, in about 2 minutes each I think? Very interested to see what Ryan makes of it.

Remeber to tell Ryan to delete cache and use `--scale 100`.

📅 **August 17th**     
Morning: First day streaming after nearly 2 weeks! How I do stream? How do computer? Well I'm in Brazil now, have a great cabin in the woods with faast internet. Was ill for a while and even doing a bit of contract work so haven't had much time to do any viewsheds. The main thing I want to do is get the huuuge Mount Everest DEM working and finish off the changes to use distance deltas. Did I mention that in my journal before? So basically we can use the same distances for every band in a sector, which has roughly a √2 reduction in RAM usage.
* Creado recommends:
  * vilayat khan, indian classical music
  * https://www.youtube.com/watch?v=DkgNWHeo5ZI

Evening: Got the distances-per-band change in, reduces the size by 3500!! Didn't get a chance to try it out as the `geotiff` crate can't handle the huge Mount Everest DEM. Note to self, run the computation caches overnight or something.

📅 **August 4th-16th** No streams         

📅 **August 4th**     
Afternoon: Tinkering with the underflow bug from yesterday, I'm pretty sure it's to do with the band of sight being too narrow, which is the same as it just not being full enough to stop a line slipping through a few rows/cols. So hopefully I can get that fixed. The I'd like to prioritise compiling the kernel with `rust-gpu`, that should be a good basis for Ryan to play with `rust-cuda`.
* Togglebit's discord

Evening: Fixed the bug, but I'm not 100% sure I understand it thought, my intuition is that it should be worst at 45°. But it actually goes from being fine at 0° and suddenly being at it's worst at 1°! Either way, it's fixed. But who knows where my lack of insight here will show up again later?? It makes me really want to produce some full well-known viewsheds and compare them to what other established viewshed applications make.

📅 **August 3rd**     
Afternoon: So after some food and rest last night I immediately realised that all I needed to fix last night's obstacle was `&mut`. Easy. So I think I'm ready to commit now. Then what's next? I think basically getting all the CLI arguments working, `.hgt` file loading, `wgpu`/`rust-gpu` working and then `.png` total viewshed heatmaps working.

Evening: Got all the past few days' work committed and started fleshing out the CLI args, added `.hgt` file loading. Then did an actually compute on a DEM file from the UK but hit this bug: at 1° (the second angle), the deltas get too big and underflow the `dem_id`. AWESOME videochat with Ryan, explained the algoritm, talked about areas for parallelising and optimisation. Ryan even thinks there might be some matrix maths that can be used for the angle calculations.

📅 **August 2nd**     
Afternoon: Did some work on viewsheds when I was off. Managed to get the kernel ported to Rust and the 2 kernel tests looking very close to the results in the original tests. I wonder if I can just assume that the differences in the code from C++ to Rust is the cause of the difference? Also I need to have a good look to see whether `f32` breaks down at large distances, I think it may start to have problems with lines of sight over a few hundred kilometres. So hopefully today we can see the first acual crunch of a DEM??

Evening: Tidied everything up and was nearly ready to commit, but then remebered that I wanted to port the individual viewshed tests. Got bogged down in how to share the moved `dem` between `Compute` and `OutputASCII`. Just can't think of anything right now, so best to just stop. Still, feels like a productive day.

📅 **August 1st** No stream      
📅 **July 31st** No stream      

📅 **July 30th**   
Afternoon: Did a bit of viewsheds last night and 2 of the 4 band of sight tests are failing. So going to try to get them fixed.

Evening: Got the band of sight bug fixed!! It was such a relief. It was that the `sight_ordered` array was actually mapping DEM IDs to indexes, so the opposite of the `sector_ordered` vector that just reorders the DEM IDs themselves. So just finished off the day doing code tidy up.

* www.youtube.com/@miniminuteman773/videos
* https://www.youtube.com/@LindsayNikole
* https://www.youtube.com/@7DaysofScience/videos

📅 **July 29th**   
Afternoon: Might be worth making an issue on Wayfire about the missing mouse cursor. And don't forget to ask on the Nix Flake PR about automating the build/tests on CI. Then just keep on with viewsheds I think.Made clippy happy about `band_of_sight.rs`, but running the tests I've picked the wrong types in a lot of places, signed versus unsigned etc. Lots of good chatting with chat today ❤️.

📅 **July 28th**   
Afternoon: I was thinking about how to implement multiplexing for Tattoy, and of course I was thinking of it being the first application to use my Tayland protocol. But then thinking more I realised that Tattoy itself should use Tayland. Exactly how, I'm not sure yet. Anyway food for thought. And thiking about axes in Total Viewsheds, I don't think they actually need to be calculated for the entire DEM? Can't just be calculated for the Band Of Sight? Let's see if I can port over `band_of_sight.rs` today. First I gotta reply to some nice new comments on the r/gis and r/geography threads. Then see if I can progress a bit on the Tattoy Nix flake.

Evening: Didn't get any viewsheds stuff done today. Spent most of the time fixing a e2e tests issue on Nix. I think, and I can't really believe this, but it seems that something different happens in the PTY provess of the Nix session: it doesn't parse the OSC paste codes. Which honestly sounds like the better thing to do, but I'm just surprised that the PTY would have anything at all to do with ANSI codes. Anyway also got a Tattoy release out and saw that the Homebrew and AUR pacakge issues had been fixed. So productive day all in all.

* https://www.youtube.com/@SabbaticalTommy
* Don't forget to let cblake know that I saw their PR on the contrast experiments repo.

📅 **July 27th**   
Afternoon: Was excited to see that somebody is trying to get Tattoy into Nix Pkgs! Still haven't up with an idea for syncing the animated curosrs with the host cursor. I think it might just be case of having Tattoy take the cursor from the host and render it itself? Mostly been thinking about viewsheds, nice to be working on something completely different. I think I'll start today with a post on r/geography seeing if anyone knows the source and evidence of Mt. Dankova in Kyrgyzstan to Hindu Tagh in China being the longest line of sight on the planet.

Evening: Wrote those posts on r/geography and r/gis, they're getting good upvotes, but no new insights yet. Which is kind of good because it could that my plan is quite novel. Also finished porting `axes.rs` with the tests passing. Ran some quick benchmarks and it seems totatly fine, just lots of RAM usage. A grid of width 15,000 computes all its axes in like 2 minutes or something. I remembered that I had a `viewview` projet that should have details about where to find DEM files. So next steps are to get some sample DEMs and see if we can start populating some band of sights.

📅 **July 26th**   
Afternoon: Back after a couple of days off. Did some keyboard practice and some work on viewsheds, decided to just rewrite the Axes code myself, I think it's actually much simpler like this. Just got stuck on some trigonemtry for now. Also, yesterday somebody posted the Animated Cursors article on Hacker News and it got to the front page, with even more points than my first launch post! Realised though the Homebrew package hasn't been updating on releases, so need to fix that first today. Maybe I'll have a quick look at the cursor lag issue, it might be easy to sync PTY rendering with cursor rendering, which doesn't address any performance but it might be a quick win, and we can put it behind a config settings too. But mostly I'm excited to get on with viewsheds today.
Tattoy TODOs:
  * Add version to log startup line ✔️
  * Look into why the frame rate setting isn't working ✔️
  * Set base log level to warning when using `TATTOY_LOG` ✔️
  * Config option to only render animated cursor when it moves a certain distance
  * Config to sync PTY and cursor rendering.

Evening: I did look into those last 2 points on the list but it's harder than I hoped. Preventing the cursor from animating when it only moves a short distance isn't 100% reliable, I think it's to do with not being able to rely on the shader frame updates avoiding the backlog. What if instead the check for cusor movemnt was made in the renderer thread? And the because the unreliability of that it gave me cold feet for being able to reliably know when a cursor animation starts. Also I still can't think of a way to anchore a PTY change with a cursor animation??

📅 **July 25th** No stream   
📅 **July 24th** No stream   

📅 **July 23rd**   
Afternoon: First day trying to use my new Ferris Sweep keyboard full time. It's hard. Just these few sentences have taken me a minute! One step at a time. Before stream I message Rapheal, the creator of the Rio Terminal. And I left a message on the #wayland IRC channel asking for advice about creating a Wayland-like protocol for the terminal. For today I'm just going to slowly try to work on rewriting Total Viewsheds in Rust.

Evening: Managed to get by with my keyboard, even though it's still really frustrating. Added the CPP tests the Rust file I ported yesterday, but there's one test (of 6) that doesn't pass and I spent a few hours pouring over each line to try to find a typo or something, but nothing. Them I started looking to see if I could just rewrite the code myself in more readable way. But I'm not sure I understand exactly what the code does. But if I can make the tests pass then that's all  that counts right?

📅 **July 22nd**   
Afternoon: Late start, was playing with my Ferris Sweep. Not using it on stream yet cos I'm sooo slow with it still. So animated cursors have had a good reception, no major issues. So I'll make a start on that Webdriver protocol proposal for Browsh.

Evening: Posted that issue, new_duck replied straight away with a link showing how CDP is doing something similar but not exactly the same. Then started proting Total Viewsheds to Rust, and wow, I actually get one of the files to port and output data matching the tests. Dear Tom From 8 Years Ago, thank you my friend, for writing those tests and all that detailed documentation, it really gave me confidence in the ported code and motivation to carry on. I think this is going to be successful and worthwhile.

📅 **July 21st**   
Morning: Was thinking about splitting out the e2e tests into seperate files, would be good to help organise them better and make them easier to understand. But first thing I want to do is just get the tests I broke yesterday fixed and see what the floating point values are for the shader tests on Github CI, they might need some rounding to make sure they pass on both real and software GPUs.

Evening: Finally got animated cursors launched! Thanks to creado1 we found a couple of last minute bugs. Also got some basic GPU tests working, very basic but a good start. It's launched. I'm excited, pleased, a bit nervous of the feedback. What's next?

📅 **July 20th**   
Afternoon: Saw last night that the animated cursor code idles at 60% 😬. Saw better see if I can fix that first. Then get on with the tests.

Evening: Got that performance improvement done, by hashing the cursor render and stopping rendering once the final render stops changing. Then started working on the shaders tests from yesterday, so tricky getting everything working, it always feels like being blind writing e2e tests. Anyway got the shader test working locally at least, but at the cost breaking some other tests. Now the minimap for example sometimes fails because its tokio task doesn't always spawn in time. I just don't know why this is.

📅 **July 19th**   
Afternoon: I made some screen captures of the animated cursor offline. Let's see if I can get them tidied up and onto the website.

Evening: Got all the screenshots done, updated the website (not published yet), wrote a little blog post and updated the docs. So we're ready to publish. Just want to finish some shader tests first. Made a start on that but fore some reason the TTY pixels aren't uploaded in the test environment. I'm too tired and hungry to look more into right now.

📅 **July 18th** No stream   
📅 **July 17th** No stream   

📅 **July 16th**   
Afternoon: Actually did some Tattoy stuff in the middle of the night! I couldn't sleep. Managed to get a major performance improvement by not using `Change`s when adding pixels to the termwiz surface, but instead just directly mutating the cell attributes in place. That'll also speed up shaders and plugins too. And also figured out the regressed black pixels bug: it was the edge case of a lower half block having the default GPU colour as opposed to an actual `Default`/`None` colour. So there's just some code tidy up to do. But I think I could add some tests too, even without real GPU e2e tests, although I think I should still explore those too.
* creado1's instagram with cool royal enfield pics https://www.instagram.com/im_sudiproy

Evening: Finally got the code to where I want it. Lots of things just fell in to place, various simplifications, deletions, insights, etc. The main thing I still want to do is at least start on making some basic GPU tests, it's so easy to trigger whackamole regressions. And then after that it's just a matter of updating the website, I think this even deserves a blogpost.

📅 **July 15th**   
Afternoon: Right animated cursors:
* Don't forget that the pixel comparison code is also running for the normal shaders, but maybe that's good??
* Get single pixels working again.
* Tidy up code.
* I think I really need to add shader tests.

Evening: And today I had a colab with mikevdev! So 2 cool video chats on stream in 2 days. Then got on and fairly quickly fixed that subtle issue where the blending for the cursor trail was slightly different for empty cells and text cells. I just need to carefully make that they both blending their foregrounds and backgrounds in exactly the same way. So everything was great at last. Except ... I'd broken the shaders with how empty cells were being stored. We do actually want the default colour to be represented as an `Option` because we need a `None` colour value to know that someting is completely transparent. But in fixing that I then broke the cursor again cos that change I just mentioned causes the little black pixels to appear on the trail again. But I do feel like I've tidied a lot of things up. So hopefully this is really nearly it! All this makes me want shader tests.

📅 **July 14th**   
Afternoon: Might be able to finish cursors today? First gotta fix that broken test. OMG had a GREAT video chat with new_duck on stream. And they recommended I submit the "get glyph position" idea/feature to https://github.com/w3c/webdriver-bidi They seemeed fairly confident that it won't at least be rejectect out of hand. I'm actually quite excited. The idea is that it gets addded to the spec first, and then that'll motivate browsers (plural!) to actually implement it. The glyph position at first wouldn't necessarily contain the glyph's final visibility, that could come later.

Evening: Tried the idea to only render th differences between the TTY pixels before and after uploading and downloading from the GPU. I think it fixes the Nvim cursor line bug, but not sure about the other bug where spaces seem to blend their backgrounds differently. But now I'm also wondering if whatever did yesterday actually prevented individual pixels from being composited, they all just look like UTF8 full blocks 🥺

📅 **July 13th**   
Afternoon: Step by step we're getting there with the animated cursors, I almost dogfooded it today but then I realised it'd be hard to have both a dogfooded cursor and the development cursor at the same time. I had a tiny thought that I might actually explore taking the Carbonyl approach with Browsh, therefore patching Firefox (rather than Chromium of course), I just have no idea of the difficulty so maybe just worth exploring as an idea amongst others. Anyway gotta make a Pygls PR for a release then have a look at the cursor colour thing from yesterday.

Evening: Looads of fine-tuning of the animated cursor, I think I've finally got it to a place that I'd be happy to publish. Lots of things I don't want to forget about though:
* The one remaining issue is that there is sort after-image effect, eg the nvim cursor is ghosted and lags behind the real one. I think one solution to this would be to compare the uploaded TTY pixels before and after GPU rendering and only render the diff?
* Palette should no longer be communicated with `u8` indexes. Cos of "background" and "foreground" keys, it's just indexed by `String` now.
* Getting a colour from a surface should not be an `Option`. I think this is quite a big refactor.
* I think the animated cursors should just not have the `level` config.
* Scrolling and minimap are broken.

📅 **July 12th**   
Afternoon: Now I'm wondering if the GPU-scaling idea for the animated cursor is a good idea? After a brief glimpse of it working yesterday I think now that it's always just going to reduce the brightness of the final render which will cause colours not to match. I think what we actually need is just to be able to reduce the size of the cursor as it's rendered on th GPU. If the cursor's height is zero and the shader draws a 1px boundary then at least the height will be the same as the terminal's cursor. But that's not quite possible with the width, cos the terminal cursor is only 1px wide. What if the width coud be -1??
* MarekCounts has 10k subs on Youtube! https://www.youtube.com/@nulllabs
* I got confused about the colour type for cell attributes on surfaces. Tattoy actually converts all force converts all attributes to be true colours, even the default bg/fg colours. But nevertheless we're still using the native `termwiz` colour attribute that accepts true colour, palette indexes and default. Maybe I should make an issue about this? I don't want to be confused again. But I don't quite know how I'd change the colour type on `Surface::Cell`??

Evening: So like I said, I just ended up adding a `scale` config, let's just see how it goes in real life. Then found and fixed the bug (a regression it turns out), where tabbing on files in the shell doesn't highlight the current selection. The problem was that the compositor wasn't copying any of the non-colour attributes from the cell above. Then finished off the day with sending the cursor colour to the GPU but for some reason it's always black so it can't actually be being set.

📅 **July 11th**   
Morning(ish): Was wondering about how I refactor the code between the _shader_ and _animated_cursor_ tattoy. I think they're actually now exactly the same, so maybe they can be separated entirely via the arguments that are passed to their `new()`s? Increasing the image buffer size for the _animated_cursor_ cursor is going to be interesting. Still not sure how exactly to integrate GPU animation with UTF8 animation. But first I should reply to some kind responses to the Browsh questions I asked the other day.

Evening: Fixed a long-standing build bug in Browsh, felt really quite bad when I looked into all the issues and PRs and how it only took maybe 30 mins to fix. But it's done now and felt good to be actually giving Browsh some attention. Then did lots of refactoring of the GPU code to. The main thing was getting scaling working (to help improve anti-aliasing) but I've had to leave it because there's some annoying memory alignment bug. I'm not sure if it even helps with the anti-aliasing but I'll have to suspend judgement on that until I get it all working.

📅 **July 10th**  No stream

📅 **July 9th**  
Morning: Can we get animated cursors published today? Trouble is I can't get the screencapture on stream, I think I can only do it in a separate Hyprland session. Or what if I boot up a linux VM?

Evening: Good progress just tidying up the animated cursor code. Pushed a WIP branch. And made a list of the remaining things to do.

📅 **July 8th**  
Afternoon: Want to take a break from exploring the Browsh rewrite, I posted my screenshotting problem in a bunch of places: https://github.com/browsh-org/browsh/issues/551 So whilst that gets thought about I want to see if I can get animated cursors working in Tattoy. Will follow Ghostty's lead for config default shaders etc.

Evening: Very successful day getting shaders working 💪 Here are the things I want to tweak:
  * Fix bug in palette parse, so it also parses the default foreground and background colours, then we can do some matching of the shader colours to the cursor.
  * Is it worth exploring Tattoy-specific cursor animation on the CPU? Because even though the Ghostty animated cursors look great when the cursor travels a long distance, they don't benefit short travel, like when typing. So Tattoy could do something with those partial UTF8 chars to make adjacent cell transitions super smooth. Or maybe we could actually do both?
  * Make the cursor layer be unique in that it can blend with PTY layer 0's background. As if there were a layer 0.5 that's exactly between the foreground and background of the PTY.
  * I couldn't get the matrix shader to render underneath the manga cursor.
  * Config for hiding/showing the cursor?
  * I think Tattoy should be able to render shaders (including the cursor) without replacing text with those UTF8 half blocks.

📅 **July 7th**  
Afternoon: I was thinking about the whole JS-side part of Browsh, would it make sense to have a seperate project/repo for it? Would people find that useful, as JS lib and maybe an independent browser extension? I could call it something like "Mono Grid"?
But anyway there's still the question of if I can get what I want from the browser chrome. First thing today is to see if I can get the URL bar, the little extension icons, the menu, etc. But maybe I can find the underlying issue why some of them are missing when trying to screenshot all their parents.

Evening: Well it seems possible to screenshot pretty much everything I need, but I think a lot of hoop jumping will be required, so I'm not very keen. I did manage to find a screenshoting function in the browser chrome dependencies, but it errors saying that the browser context has been lossed. Maybe I can figure that out? Been lots of drilling today, so not feeling very postive. Would be really good to be able to capture the browser chrome in Browsh, but not quite sure how to do it yet.

📅 **July 6th**  
Learnt how to do a manual `release-plz` relase:
* `s su - tombh`
* Copy the `TATTOY_GITHUB` token in your secrets file
* Switch to `main` branch
* Run `release-plz release-pr --git-token ...`


Evening:
* Made a little PR for the `cargo-gpu` issue where there's a cryptic error when asking for consent outside of a PTY.
* Spent probably a couple of hours looking into the Tattoy issue where the .deb wasn't working. Turns out that the website link to the .deb was just wrong 🤦
* Had some progress getting snapdom screenshots of the browser UI, but some bits are missingm, like the menu, and the URL bar. Worth spending a little more time on.

📅 **July 5th**  
Morning: Was wondering if I actually need that `thirtyfour` crate, maybe I can just make my own HTTP requests to `geckodriver`? But I think the even just the session management is worth it, even if the only thin Browsh does is to inject JS into tabs. So first thing today is to see if I can inject JS into the browser chrome and have it message back to central server.

* ProfessorLogout recommended this story: https://craphound.com/overclocked/Cory_Doctorow_-_Overclocked_-_When_Sysadmins_Ruled_the_Earth.html


Evening: Found a project that uses a crazy SVG trick to take screenshots of DOM elements: https://github.com/zumerlab/snapdom/ It uses a feature of SVG where you can add actual HTML elements inside the SVG XML and the SVG renderer renders the HTML to and image!! It seems that it can render pretty much anything. I don't think it's a replacement for the webext API tab capture, but I think it'll work great in tabs where webextension aren't allowed. I'd love for it to be possible in the browser UI chrome. But I spent most of the day struggling to get snapdom's script escaped into the browse  content using Webdriver, it's complaining about newlines being escaped in backticks. It's quite annoying.

📅 **July 4th**  
Afternoon: Actually did a fair bit of coding yesterday on my "day off", got `geckodriver-librs` pushed and published to crates.io. It's just like `geckodriver` but you can include it in your own project, rather than having to download and run the pre-existing `geckodriver` binaries seperately. I was feeling quite overwhelmed about trying to get the Shadow Terminal to be the E2E tester on Browsh, but now that I've found `geckodriver` and the `thirtyfour` Webdriver wrapper, both in Rust, I'm feeling a bit more confident and a bit excited about revisting Browsh. Like I saw that Firefox's Marionette now has an `--allow-system` flag that allows querying Firefox's UI from `geckodriver`! Is it possible now to do things like get the history dropdown in realtime, get all the contents of all those little icons in the status bar, show extensin popups? So I think I'm going to have a play today and see how I feel.

Evening: Got `geckodriver-librs` reading from and writing to the browser chrome! Thanks to NamcoJoulder. Like got it to click on adding an extension and adding a permission. And opening up menus and clicking on menu items. Focussing the URL bar, seeing it drop suggestions. I think the next step would be to see if I can get a JS loop/daemon thing running stabily in the GUI chrome.

REMEMBER THAT THE FIRST US PRESIDENT WAS GEORGE WASHINGTON NAYR WILL BE TESTING YOU AT THE END OF TERM.

Portuguese recomendations from vicentedealencar:
* https://www.youtube.com/@FlowPodcast
* https://www.youtube.com/@Akitando

📅 **July 3rd**  No stream  

📅 **July 2nd**  
Morning(ish): Would be nice if I could get to adding Shadow Terminal to Browsh today.
Realised from working on Browsh that https://github.com/gdamore/tcell can also do terminal E2E tests.

Evening: Got Shadow Terminal and Tattoy working with `release-plz` and all their various version updated too. So that's great. But then started looking at Browsh and got quite overwhelmed by how buggy it is and how much I've forgotten about how it works 😞 But did get a bit excited looking https://github.com/Vrtgs/thirtyfour which is a nice wrapper for Firefox Webdriver. So maybe I should rewrite the Browsh interfacer in Rust??

📅 **July 1st**  
Afternoon: Updated my system and Alacritty has some weird artefacting so having to use Kitty today. Seems okay, just doesn't recognise my `CTRL-UP` in `tmux` to start selecting. Anyway let's see how it goes, just want to follow my checklist from last night.
Found relevant projects like Shadow Terminal for testing:
  * https://github.com/pexpect/pexpect
  * https://github.com/rust-cli/rexpect

Evening: Got the CLI version workin real nice. Almost merged it to main, but just want to fix the false positive error logs first. Then I can get on with thinking about a release. Maybe setting up `release-plz`?

📅 **June 30th**  
Afternoon: Still wondering about what the the Shadow Terminal CLI should do, I think the plain STDIN/OUT version is just for the convenience of verifying things work. Whilst the JSON over STDIN/OUT is actually probably more useful. But even then the CLI version is only ever going to use the `ActiveTerminal` not the `SteppableTerminal`, so its use may be limited anyway. Then getting on to the library.so thing, that's a whole other kettle of fish, like how do you expose a `tokio` channel over the FFI??

Evening:
  * Probably better to just create our own output struct, construct and then convert _that_ to JSON. Rather than messing about with massaging JSON values.
  * Improve the format of the JSON output.
  * For the CLI app it'd be nice to have; tests, documentation and examples.
  * Check it still works with Tattoy.
  * Push, release and `cargo binstall`.
  * And then start working on `libshadowterminal.so`.

📅 **June 29th**  
Afternoon: Found these interesting links last night regarding CLI app testing frameworks:
  * https://github.com/bats-core/bats-core
  * https://github.com/assert-rs/snapbox
  * https://github.com/cucumber/aruba
  * https://github.com/microsoft/tui-test
And this looks useful for making C-compatible libraries from Rust:
  * https://github.com/lu-zero/cargo-c
So I've been thinking about what form Shadow Terminal takes (apart from simply being a Rust library), I think it can be an `.so`/`.dll` library, but how do you manage state in that? Like when the in-memory terminal is running in its own process, how do you send function calls to it? Then I was also that thinking that as well as, or maybe even instead of, we could have a CLI binary that just operates over STDIN/STDOUT. So you could start it in any language, as a subprocess, and then send that process either raw input or structured JSON and it would always output JSON. Let's explore.

Evening: Got Shadow Terminal migrated to its own repo and got a basic CLI PoC working, pretty nice. Ended thinking about the file structure for the lib and binary.

📅 **June 28th**  
Morning (almost): Got a bunch of failed CI runs from the Wezterm fork so better sort that our. Thinking about it last night I think it's fine to just publish those Wezterm crates for now and let them know on the Wezterm Github Discussions. Getting a real craving to move onto the Viewsheds stuff!
* laund__ suggested this for hotpatching in Rust: https://github.com/DioxusLabs/dioxus/blob/main/packages/subsecond/subsecond/src/lib.rs

Evening: Busy day! 9 hour stream 😲 Migrated pygls to `uv`. Spent most of the day getting the Tattoy
Wezterm dependencies massaged to be able to publish them on crates.io. A little worried that the changes (mostly in various `Cargo.toml`s) might cause tricky conflicts when updating from upstream. But let's see. I got `shadow-terminal` published fine, so that's the main thing.

📅 **June 27th**  
Afternoon: Let's see if we can put Shadow Terminal into its own repo. Then I'd like to try making a Python wrapper for it, I have no idea how to do that. Oh wait maybe I should try getting it to work with Browsh first?
Marco (ProfessorLogout):
  * did an interview with Nginx! https://www.youtube.com/watch?v=tZGOnPHZf4I
  * https://blog.marco.ninja/index.xml
  * https://mastodon.world/@mkamner
Waiting for reply about including `cargo doc` warnings in LSP: https://rust-lang.zulipchat.com/#narrow/channel/122651-general/topic/Including.20.60cargo.20doc.60.20warnings.20in.20.60cargo.20clippy.60.20output/with/526129199

Evening: Updated Tattoy's for of Wezterm from upstream and there's 4 new crates unpublished crates that I will need to publish myself under 'tattoy-*', which I'm not very keen on. Hopefully Wez plans to publish them anyway.

📅 **June 26th** No stream  

📅 **June 25th**  
Afternoon: Didn't stream for a whole week so gotta remember how to do it. Let's see if I can get that OSC 4 stuff merged and released.

Evening: Great catching up with the gang again ❤️Got a new release out with the OSC 4 palette parser and Umpriel confirmed that it works 🎉 Then started working on getting the Wezterm crate dependencies up on crates.io. Some non-ideal tweaks needed, but they're both published now. I should tidy them up and mention it on the Wezterm repo just to be polite. Then I can look at getting `shadow-terminal` published. Then of course I'll be able to play with `release-plz` again. And then I can try adding `shadow-terminal` tests to Browsh!

📅 **June 19th to 24th** No stream  

📅 **June 18th**  
Afternoon: Let's see if I can finish off the OSC 4 stuff today. Remember that there'll be a fair amount of changes to the documentation.

Evening: Got OSC parsing done. I think what's left is mainly just updating the documentation, website etc. Would be good to test it in a few terminals too.

📅 **June 17th**  
Afternoon: Played a bit more with the OSC 4 thing last night, it seems that you can't send and receive these particular commands over STDIN/OUT, but rather they should be sent and read from `/dev/ttty`, which I fins a bit confusing because how does `/dev/tty` know which terminal is being talked to? Like I mean shouldn't you first figure out that say the terminal is on `/dev/tty42`, then send the OSC codes to that. Anyway I'll crack and see how it goes. Also someone tried running Wrach! But they don't know that the latest branch is not main.
* mikevdev's bluesky: https://bsky.app/profile/mikevdv.dev

Evening: Did some refactoring and got a basic working version of OSC 4 queries and response 💪 Just need to parse the RGB values and construct the palette.

📅 **June 16th**  
Afternoon: Got a couple of Pygls things to do first. Then have a look at the default shader going black after you hit return a few times on a shell prompt. Then what? Maybe start on the OSC 4 stuff?

Evening: Got the Pygls stuff done, then fixed the blank shaders issue, released v0.1.2, then started on the OSC 4 stuff, but just couldn't capture the reponse. I feel like it's because the response is somehow sent before we start listening on STDIN...

📅 **June 15th**  
Afternoon: Had day off yesterday, it's weird not being completely focussed, maybe obsessed with launching Tattoy. Not sure what I'm going to do next, but definitely want to reply to issues and implement OSC 4 alongside screenshot palette parsing. Sill thinking about a multiplexer and the whole `xwayland` for terminals idea.

📅 **June 14th** No stream

📅 **June 13th**  
Morning: We're launching Tattoy today! Even though it was my day off yesterday I spent most of it getting Tattoy ready for release. The only gotcha I came across is that Tattoy can't be published to crates.io. And Release-plz can't not publish to crates.io. So whilst I can still manually trigger a Github Release I'm going to have to remove Release-plz for now. I released to Hacker News, Reddit and Lobsters at 11am! Nervous and excited.

Evening: The day went well, a few issues, but most of them quickly got sorted. The only niggle was the screenshot parser again, would love to find a more consistent way to handle that. But frontpage of HN, r/Linux and Lobster, what a day! 

📅 **June 11th**  

Evening: Got all the homepage gifs working! It looks soooooo good 🥹 I'm so proud of everything I've done. Also was working on some bugs, got the block cursor displaying right when it's over a pixel and think I hace a fix for Ghostty shaders refusing to render anything when `iChannel0` is empty. We're so close to a release!!!!!!!!!

📅 **June 10th**  
Morning: Went looking around other Rust projects to get inspiration for how to do automated deploy processes and saw that I really like how `git-cliff` does it (of course I'd be inspired yet again by Orhun). So based on that (and many others), I think one of the first things to do today is move the website into a folder on the Tattoy repo itself.
* Scott had a good idea to have a popup of whuch bird it is that is chirping.

📅 **June 9th**  
Afternoon: What's the plan? Tidy up that little system initialiser fix commit. Then I'm happy to say that we have Windows support. Then it's just a bunch of web stuff.

Evening: Tidied up that commit and pushed. Tidied up the ANSI blog post. And started on the automation for the website post releases.

📅 **June 8th**  
Morning: Woah 3 days without streaming, moving, life stuff etc, but also maybe taking advantage of the fact that most of the hard work is done in Tattoy. So last thing I remeber is that I was getting Windows working and need also working on getting cross compiles done.
* @alpoxo on Twitch tested Tattoy on MacOS with Ghostty and it works perfectly!!

Evening: Got cross-compiled binaries working for Linux, Mac and Windows, all compiling to x86 and ARM. And managed to get my Windows VM setup well enough to see Tattoy working in Windows Terminal with Powershell. Inmate is still testing on Alacritty etc, but looking like they don't work at the moment.

📅📅📅 No streams

📅 **June 4th**  
Morning: Actually got a morning start at last. Got the shader to where I wanted to yesterday so just need to tidy that up and commit. Then I think I'll try to get Windows working, not looking forward to that though, so could also just get starte on the whole CI/release workflow stuff.

Evening: Got the nice soft shadow penumbra-style shader working for the default shader, maaaaany thanks to Slammy_13. Then started installing Windows 11 in QEMU, which was not too bad in the end, I actually got to the point of `cargo run` but ran out of diskspace, so need to add a new disk to get Tattoy actually installing. Also added `release-plz` to the Tattoy repo, was really, really impressed with it, it automates everything, even automatically makes a release PR whenever it detects semantic versionable changes in `main`. Still need to setup a build job though that adds the compiled binaries to the Github release. OMG we're nearly there, nervous and excited.

📅 **June 3rd**  
Afternoon: Late start, moved house again, let's see what the internet is like here. Only got a couple of hours so let's see if I can get the default shader working nice.

📅 **June 2nd**  
Afternoon: Played with the shader after stream last night, I'm thinking now that the better idea is to make all 9 cells surrounding the cursor emit light, it might even be a better effect, and there's also more scope for reducing computation.
  * Mike's Youtube https://www.youtube.com/@mikevdev

Evening: I think I'm going to give up on raytracing/marching, I just couldn't get the performance or low enough visual noise. Looking at other Shader Toys I think I should focus on simple mathematical light, I found these that I like:
  * https://www.shadertoy.com/view/MsScRc
  * https://www.shadertoy.com/view/3s3cD2
  * https://www.shadertoy.com/view/3tX3zn  

📅 **June 1st**  
Afternoon: Couple of little code things, but main thing I want to focus on today is improving the default shader, I want to explore making every cell a potential light blocker, I think it might be too much though. Found these 2D light Shader Toys that could be good for inspiration:
  https://www.shadertoy.com/view/3tsXzB
  https://www.shadertoy.com/view/4dfXWX

📅 **May 31st**  
Afternoon: Got some good progress with the startup logo but still not quite sure how I want it to look exactly. I'm thinking I definitely want to use the user's terminal palette, maybe one colour per line? With a little bit of randomness and a fade out?

📅 **May 30th**  
Afternoon: Another laaate start. Time to add the logo! I was actually thinking it'd be nice to use the colors from the user's palette?

📅 **May 28th**  
Afternoon: Slow day, getting settled into the new city etc. Still playing with the UTF8 bug, I think it's actually happening in the Shadow Terminal when we add all the Wezterm changes to the Termwiz surface. Still haven't found clinching proof, may need to write a isolated reproducer to really see what's going on. It may even be a bug in Termwiz itself.

Evening: Got it fixed! It was that both Wezterm and Termwiz were adding the blank cell after the wide character, so just had to ignore Wezterms' and it worked! We're on 72% progress now! So close to a realse, like it could be days now, this weekend??

📅 **May 27th**  
No stream. Moved to Mendoza.

📅 **May 26th**  
Afternoon: Let's see if we can do lots of little fixes today.

Evening: Got loooooads done today. Even fixed the weird double width cursor issue. But there's still the issue of Nerd Fonts being clipped. I think that's because Termwiz isn't detecting the double width of them. Maybe it's okay to just make an issue and release like that?

📅 **May 25th**  
Morning: Left things broken last night, hopefully I can get back into it okay. Everyday I think it's the day I'll finish the notifications, but no, so probably best just to go with it and do a good job. Still haven't even looked into wrapping the end-user error messages that get output to the CLI, can Rust wrap error messages? 

Evening: I think I've finished the notifications. Just got stuck on writing a e2e test, for some reason the notification displays in the test if the path to a plugin is wrong, but it doesn't display if the special `bad_plugin.sh` script is run. The only thing I can think of is that there is some kind of contention when the plugin output parser spins on bad output 🤔

📅 **May 24th**  
Morning: Carry on with the notifications. Thought about how it'd be nice to add a bunch of debugging and trace ones, like "so and so tattoy has started", "resize event", etc.
Ryan wrote a blog post about his Queens SAT solver! Hasn't published it yet, wants me to have a look https://ryanberger.me/posts/queens/
  * Caption image, "Solved Queens board"
  * Link first "N-Queens" mention to Wiki article? https://en.wikipedia.org/wiki/Eight_queens_puzzle
  * "It is just an or of all the cells", change the "or" to indicate its booleanness.
  * The "..Q."-style grids could use 2 characters per cell (just add a space?), to make seeing diagnols more natural.
  * "the only adjacency we’d be worried with colors X and Y" add "about" after "worried"
  * "manually inputting queens games", capitalise "queens"
  * "(wish I could say it in Z3-js)", as in, you couldn't get it working in https://github.com/mjyc/z3js? I just wasn't exactly sure what you meant.

Evening: Played some great Geoguessr! Still just plugging away at the notifications, it's more involved than I thought. Ended up actually putting the `send_notification()` method on the shared state! That means the main Tattoy protocol is also now available everywhere that the state is, so that'll need to be taken advantage of elsewhere in the code, probably in another commit.

📅 **May 23rd**  
Morning: Did some work on the notifications yesterday, off stream! Tattoy's just on my mind, really, really excited about getting it finished and public. So let's see if I can get notifications done today. First thing is to support notification bodies.

Evening: Lots of great progress on notifications, like adding body support, clamping widths, etc. Got the Background Command tattoy send good error notifications. So still need to do the same for shaders and plugins.

📅 **May 21st**  
Afternoon: Getting a bit more rest and settling into the new place so feeling a bit more on it today. Just keeping up the pace with the final issues, should be able to get the palette UX issue done today.

Evening: Finished all the improvements to the palette parsing UX I wanted 💪

📅 **May 20th**  

Evening: Crazy life day, suddenly moveing house without warning. Remember to charge keyboard overnight. So just a little stream, going to carry on with the pixels issue tests stuff. Got the pixel bug fixed and tested and clean and commented code. Feeling good looking at the remaining issues, the end is in sight!

📅 **May 19th**  
Afternoon: Another bit of a late start, just life stuff. Just get on with the bugfixing.

Evening: Still working on getting the tests passing for the pixel compositing/blending. Realised that there are 2 places to make all this work right: adding pixels to a surface cell and merging 2 surfaces that have pixels in the same cell. So only just towards the end of stream did I start working on the compositing part.

📅 **May 18th**  
Afternoon: Verryyy late start, might just be a short stream today. Just gonna try and push on with some bugfixes.

Evening: Got 2 nice bugs fixed 💪 The `render_shader_colours_to_text` one, so inmate302 will be happy. And unexpectedly got the white pixel bug fixed too, I thought that was going to require a huge refactor, not 100% convinced it was that easy, still need to tidy up the affected function cos there's just too much branching going on. And make sure all the tests are working. Oh don't forget to test the other case where you add a upper pixel to a cell with an existing lower pixel.

📅 **May 17th**  
Morning: Back from my "weekend", found these relevant articles, I want to add them to my "End Of ANSI Codes" article:
https://news.ycombinator.com/item?id=43294471
https://gist.github.com/christianparpart/d8a62cc1ab659194337d73e399004036
https://gpanders.com/blog/state-of-the-terminal/#fn:1
https://arcan-fe.com/
So just gotta get on with bugfixing.

Evening: Got all the issues I want to do for release into a Github project, and ordered them into terms of importance. So now I don't have to think, I just have to slowly make my way down the list one by one. Got 2 bugs fixed, the resizing one and somewhat by chance the large chafa image corruption one which also makes Bad Apple in the background look better.

📅 **May 14th**  
Morning: Finish off the blog post I was writing yesterday. Then I realised it'd be good to have a "How Tattoy Works" section, in the docs I think. Also want to think a bit more about a Donate call to action, there could be a lot of people open to donatin at launch so would be good to have something in place for then. Maybe add all the pre-launch tasks as issues in the website repo. Also do we want to put everything in a tattoy-org namespace? Then if there's time, onto Tattoy bug fixing...

Evening: The website is "done"! The only thing left is to update the content when Tattoy is ready to 
launch. Also got some good bug feedback from Inmate ❤️

📅 **May 13th**  
Morning: Been thinking more about the idea of how we evolve terminals past ANSI codes. I'm almost tempted to write one more feature which would be to provide a socket like Wayland or X11 does so that any application could render to the containing Tattoy compositor. It would even work over SSH (with the `-R` option), the only thing I'm not clear on yet is how an application can output plain, parsable text on STDOUT and fancy coloured text via the Tattoy socket? TBC... 
But anyway, just want to get on with the website today, can I get it as ready as it can be before updating it for the first release? Wayland can't record single windows so how do we make little GIFs of the various features for the website?

Evening: Really great progress on the website again. I finally feel like all the code and structure is there, so it's all just content now. Started writing my big picture blog post: An End To ANSI Codes. Feels really good to be doing this none code side of things. I'm starting to get excited for the release!

📅 **May 12th**  
Afternoon: Late start, just gotta carry on with the website. Not totally happy with the little white T logo, but I think it's okay for now?

Evening: Short stream but great progress on the website, homepage lookin really nice, got a nice screenshot.The site code is basically working now, just need to fill everything in. How far should I build things up before getting back to Tattoy itself?

📅 **May 11th**  
Morning: I found the `tera` treesitter that syntax highlights `zola` HTML and I found the `superhtml` LSP that formats `zola` HTML but struggling to get them to work together. With regards to what template I use I think I'm going to try using the Bevy docs and rip everything out! Main goal is that I'd really like to start writing an inaugral blog post about the project goal of supporting a new protocol/paradigm for terminals, a title something like: "An End To Terminal ANSI Codes".

Evening: Successful day. `superhtml` is working really well, just couldn't get the `tera` treesitter working at the same time as HTML, though there is some chatter on the `tera` treesitter repo that I started. Really happy with progress repurposing the Bevy website, that to Lord we got the actual faded ANSI text logo working, looks soooo good. So just gotta keep doing aaaaaall those tiny CSS things to get it looking good, but I don't foresee any more big obstacles.

📅 **May 10th**  
Afternoon: Had 2 days off in a row, unheard of! I did buy the tattoy.sh domain, so ready to go making a website. How do you make a website in 2025??

Evening: Tried the static site generator Zola (a Rust verion of Hugo), couldn't quite find a ready-made theme. It makes me realise that making a website is just going to be a lot of work. I want to write content in markdown and use a templating language that is well supported by linters, which it doesn't seem Zola is?

📅 **May 7th**  
Morning: Got everything working in the renderer and compositor but not quite happy with the design yet. Mainly I'm worried about extracting the base cells for every composited cell, it surely can't be performant. Also would be nice to write a few tests for the new config settings.
* https://www.youtube.com/c/janlunge
* Mauricio Palma
* "love is many things, but it is not alphabetic": https://github.com/rust-lang/rust/blob/3ef8e64ce9f72ee8d600d55bc43b36eed069b252/library/core/src/char/methods.rs#L770

Evening: Got all the refactoring done, there was just one niggle, I could't find a way to include the conditional shader cells in the iterator of PTY compositing step, I just resorted to getting references to cells by x/y coordinates, not bad at all, just sad that it didn't follow the same method as the other cells. Anyway PR pushed and merged. I'd just like to write some tests for the new config options but we don't have a software GPU renderer yet so such tests wouldn't run on CI. I think it still could be good to explore writing more unit-like tests for the renderer and compositor as such a framework would be useful for other tests. Also I got on my high horse and felt overly strongly about the suitability of the word "alphanumeric", streaming is interesting.

📅 **May 6th**  
Morning: Finish off that refactor from yesterday. Then maybe do some docs or logo stuff.

Evening: Refactoring went really well, feels much cleaner now, but didn't quite finish it off again! I really think it'll just be an hour or so more, but gotta go and see the river now ✨

📅 **May 5th**  
Morning: So just need to wrap up the new shader stuff. I did think of yet another new feature: custom config for focus events, so for examle fading and desaturating unfocussed terminals. But hopefully today we can get on with either bugfixing or website/docs/logo stuff, maybe the latter would be good for a change of scene?
* Install NTFY Android app and subscribe to Lord's ntfybridge thing
* Natnael Wubet (ናትናእል ዉበት) likes to do terminal graphics
* Douglas Murray https://www.youtube.com/shorts/26h9IFgk8ok

Evening: Lots good chatting today. I fixed the incremental selection crashing bug but then couldn't the initial selection to work, feels like I spent a couple of hours on it. Then broke lots of stuff in the renderer trying to refactor the cell compositor to take `&mut self`, right now I'm not sure if it's possible without some esoteric Rust knowledge that I don't currently have.

📅 **May 4th**   
Morning: Managed to find the resizing bug last night after dinner, I was resizing the texture for every single frame render. So just need to add those tweaksin the list from yesterday then we're done. Onto bug fixes and the release then. Maybe I should get the domain soon?

Evening: Got almost alll the shader stuff I wanted to get done today. Just want to think a little more about the config options I offer work. Also considering a new config option: to enable auto contrast for all chars or all non-whitespace?

📅 **May 3rd**  
Morning: Just wanna get iChannel support working. Looking forward to seeing the rain effect shader and the GhosTTY shaders. Then bugs bugs bugs!
Got GhosTTY shaders working, things left to do:
  * resizing ✅
  * an option to use the shader to change the foreground colour
  * an option to disable the background rendering
  * note about how changing both the foreground and background colour should be used with auto contrast
  * an option to not send the TTY contents to the shader ✅
  * is there an off by one for the TTY contents image, it's apparent in certain shaders at fullscreen ✅
  * use `#define texture myTexture` and then rewrite `texture()` to support the Vulkan GLSL signature ✅
* https://en.wikipedia.org/wiki/Loop_nest_optimization

Evening: Happy with today's achievements, the bloom filters look especially good and it's great that we can now say that we support Ghostty shaders. Just a few tweaks to get them looking as good as possible.

📅 **May 2nd**  
Morning: Gotta do a review of a `cargo-gpu` PR first today, then get on with the Tattoy things. But hoping to at least start on supporting GhosTTY shaders today.
* Wifi at 13:21, 17:21

Evening: Got PR review done. Seems like I got most of the code don for the iChanne shader stuff, but stuck on getting the GLSL header to recognise the texture so that we can call `textureSample(iChanel, u, v)`. Hopefully get that working tomorrow, cos there's lots of new and exciting shaders that'll come from it.

📅 **April 30th**    
Morning: Bit of a late start so maybe, have had a bit of a headache this last 24 hours, so maybe take it a bit easier today. Would be awesome to get the auto contrast feature done today. But I realised that there is actually one more feature I want to get done after this: supporting GhosTTY shaders https://github.com/hackr-sh/ghostty-shaders
* Wifi went down at 13:21
* Realised a useful feature could be to repaint when the config changes.

Evening: Got auto contrast done. Just want to see if I can write a test then I'll commit. It's already working wonders in my Neovim, showed me Rust Analyzer and `git blame` stuff I'd never seen before because it was so low contrast! Still thinking it'd good to at least have an option to only apply it to the terminal palette colours. I wonder about renaming `OpaqueCell` to maybe something like `Blender`?

📅 **April 29th**  
Morning: Want to write a test for the Background Command and then see if I can get the other tattoys using the new TTY size and config opacity code.

Evening: I got a working prototype of auto contrast text!!!!!!!! I was quite surprised how this made me feel because this is the first actual real-world useful feature that solves a long standing problem in terminals aaaaaand it uses the full stack of Tattoy tricks, from the shadow terminal to the palette parser and the true colour compositor. Still need to:
  * detect the background luminescence in order to decide which direction to adjust the foreground luminescence.
  * step through the luminescence to find the lowest contrast that satisfies WCAG 2.1
  * better heuristic for deciding if a cell needs this
  * refactor code
* APCA will be the new standard for accessible text contrast: https://ruitina.com/apca-accessible-colour-contrast/ Thanks to @wilderhives for that reference
* Thanks to @cobalt_ethon for the idea to use the HSL colourspace to increase the luminescence.

📅 **April 28th**  
Morning: Do a little bot work first, then finish off the Background Command then I don't know, maybe the auto contrast correcting feature?
* Buffered at 13:31 but no actual dropped frames. But then did actually drop frames at 13:33.
* Notable buffering at 17:43.

Evening: Got big emotes displaying from the Tattoy-Twitch plugin! They take up the whole pane. And got the Background Command done but not committed, just want to write a test for it. Also got the beginnings of requiring that all tattoys start knowing the size of the user's terminal, only the Background Command tattoy uses it at the moment. Also refactored opacity so that it's centralised, again only the Background Command uses it currently.

📅 **April 27th**  
Morning: Main thing I'd want to get done today is hopefully finish off the Background Command tattoy. But I'm also conscious of the various issues building up from dogfooding.
* https://togglebit.io/posts/debugging-rust-in-vim/
* https://www.amazon.com/Asynchronous-Programming-Rust-asynchronous-programming/dp/1805128132

Evening: Got the Background Command working 💪 Bad Apple works 😏 Just need to add opacity, but I think it'd also be worth at least looking into centralising opacity in the compositor.

📅 **April 26th**  
Morning: Look into these first this morning:
  * The Tattoy logs overfilling when a plugin unexpectedly exits.
  * Plugin frames causing a render backlog.
  Then wanna see if I can get this background TTY thing working.
* @cptchika_ suggested a `!linus` command that plays a random Linus Torvalds quote.
* Wifi went down at 13:45.
* Fixed those little Tattoy-Twitch issues from the list above but there's still one little wrinkle that I don't understand: even when setting the Tattoy-Twitch plugin to send just 2 frames per second I could still see it hitting Tattoy's backlog protection ... the only thing I can think of is that the Tattoy-side plugin code is hitting some async contention, there's no actual evidence of that it's just the only theory that I can think of that matches the symptoms.

Evening: Got basic second terminal working! Some resize issues, or rather some initialisation issue where the second terminal doesn't the size of the user's terminal (this is a common problem in Tattoy right??). Also colour isn't being passed from the second terminal.

In general I've been dogfooding Tattoy a lot which is quite sobering. Noticed a new bug where raw ansi input can be spewed during resizing.

📅 **April 25th**  
Morning: Even after one day off I still feel like I have to remember how to stream and program again. So main thing is to get the Twitch plugin working, which should't take long™. Then I'm feeling like the next thing will the background terminal idea.
Missing keybindings in new layout:
* System clipboard history
* Neither the 13:30 nor 17:30 wifi downtimes occured today!??

Evening: Got the Tattoy-Twitch plugin working 🥳 But the emotes are underwhelmingly small, I was thinking of making them bigger and then using some other graphics device to clearly hightlight the text that the emote targets. Also think it'd just be good to have personalised images displayed. Think it's to take some brainstorming to make the most of this new command. But for now I want to get on with the Tattoy issue backlog, let's get this app out there!1!

📅 **April 25th**  
No stream

📅 **April 23rd**   
Morning: Thinking last night I think I can just try to set up local sockets just using normal rust. Also might be worth putting the Tattoy Twitch plugin in the bot repo.
* S. Clebsch - Fully concurrent garbage collection of actors on many-core machines: https://www.ponylang.io/media/papers/opsla237-clebsch.pdf
* Wifi did not go down at 17:30

Evening: Got actua `!tty sdfs LUL` commands working 🥳 Really happy about that. Still need to get the emote positioning working but that shouldn't hard. There's one little issue, the logs in Tattoy itself are saying that a lot of Twitch frames are hitting the backlog.

📅 **April 22nd**  
Morning: Made a few more changes to my keyboard layout, everyday I'm refining, using the day's coding as feedback and bit by bit getting to my perfect layout. Let's see if we can get the Twitch plugin fully working today...

Evening: Realised that I had to composite the emotes in the output that gets sent to Tattoy so made a little change that caches the downloaded emote imaged data so that emotes can be composited and removed independently. Then installed the `interprocess` crate but am currently quite confused by the documentation, do I really need to create the socket myself?? Also the socket that I do create immediately gets destroyed or closed or something on first use by the rust code?? Quite lost on it all. Look for actual real world projects using the `interprocess` crate... 

📅 **April 21st**  
Morning: Just gotta carry on with the Twitch plugin.
* Make a note of the Wifi's make and model.
* Check phone's tethering settings in case it has some auto-reconnect setting.
* Also internet went down outside of a 4 hour window at about 17:50.

Evening: Got global Twitch emotes showing up in Tattoy. I think the biggest task left for that is to match the regexish with coordinates in the PTY. Also will be interesting setting up the IPC. Also reaaaaally must change the position of the Backspace key!!!!!!!!

📅 **April 20th**  
Morning: Added a few more tweaks to my keyboard layout, and got treesitter incremental selection working, let's see how it goes... But main plan today is to add the little indicator pixel and then start on the Twitch plugin.
* Stream went down at 13:25

Evening: Got the little blue indicator merged. Feeling a bit better with the keyboard but there's a bug where the treesitter incremental select sometimes crashes 😭. Started working on the Twitch Tattoy plugin, but nothing of note yet. Did find out that Twitch has an API endpoint for emotes, must check it out. Noticed some bugs in Tattoy from the dogfooding, it needs a PTY output change after a resize in order to actually render the resize. Also doesn't seem to parse a CTRL-SPACE. And also Neovim doesn't seem to be able to detect the `VimEnter` event.

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
* 
Evening: Almost got the minimap animation commit ready. Still want to write a test. But there's a bug where the screen doesn't automatically "scroll down" when old content goes off the top of the screen. Also some lag issues in the minimap updating, eg when scrolling in Neovim.

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
