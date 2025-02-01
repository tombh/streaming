# A Place For All My Live Streaming Things

I stream on Twitch: [https://twitch.tv/tom__bh](https://twitch.tv/tom__bh)

See the [journal](/JOURNAL.md) for up to date details on what I'm working on.

I have some ideas for a bot and commmunity interaction things. Watch this space.

## Current Tasks
_Each one of these should be usefully descriptive of what I'm working on during a stream_

_Ordered by most-likely-to-be-working-on-now first_    

Tattoy:
  * [x] Resizing isn't detected.
  * [x] Look into performance, especially scrolling in nvim.
  * [ ] Implement scrollback/history.
  * [ ] Explore a method to get any terminal's pallette colours in true colours.
  * [ ] Double width characters aren't passed through, eg "🦀".
  * [ ] `tmux` mouse events cause runaway behaviour in `htop`.
  * [ ] Look at projects like Ratatui to see how to do integration tests.
  * [ ] `CTRL-D` doesn't fully return to terminal, needs extra `CTRL-C`.
  * [ ] Doesn't work on Nushell. Just freezes.

`cargo-gpu`: https://github.com/Rust-GPU/cargo-gpu

`rust-gpu`: https://github.com/rust-gpu/rust-gpu
  * [ ] Start discussion about updating the docs
  * [ ] What's the word on Rust `struct` layouts, is Crevice ok? Or is `#[repr(c)]` is enough?

Wrach: https://github.com/tombh/wrach
  * [ ] Finish Bevy rewrite
  * [ ] Implement PIC/FLIP using `rust-gpu`
  * [ ] Experiment with the `capability = []` config to allow `enum`s in shaders.

Pygls: https://github.com/openlawlibrary/pygls
  * [ ] Start adding types.
  * [ ] Refactor LSP test client timeouts
  * [ ] Remove `triggerCharacters` option from JSON example in Pygls README
  * [ ] `apply_edit()` doesn't `apply_changes()`

Browsh: https://github.com/browsh-org/browsh
  * [ ] Browsh: Merge Vim Keybindings PR
  * [ ] Rewrite everything in Rust 😏

Misc
  * [ ] Make a noise or notification when I get a new follower
    * Best solution: look into OBS overlay
