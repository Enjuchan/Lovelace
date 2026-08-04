# Changelog

All notable changes to this theme are documented here.

Versioning follows [Semantic Versioning](https://semver.org):

- **PATCH** (x.y.Z) - fixes only, e.g. selectors repaired after a Discord update.
- **MINOR** (x.Y.0) - new styled areas or options, nothing existing breaks.
- **MAJOR** (X.0.0) - the look changes substantially, or variables are renamed
  and users have to adjust their customisations.

---

## 3.0.1

### Fixed

- A custom background image set through LovelaceSettings was tiled at 160×160
  instead of filling the screen.

  `background-size` applies per layer. The default gradient has several layers
  and carried a matching list of sizes, the first of which was meant for the
  noise. A single image URL is one layer and inherited that first value.

  The noise now sits on its own layer, so the image layer is always `cover`. It
  stays active with a custom image as well; set `--bg-noise: none` to remove it.

---

## 3.0.0

Dark edged glass. The look changes substantially, hence the major version.

### Changed

- **Corners are gone.** `--panel-radius` is 0. On dark glass the cut edge is
  what makes it read as material; rounding turns the same surfaces into
  buttons. Set it back to `10px` for the old look.

- **The columns are darker and staggered by depth.** Server rail furthest back
  and most blurred, channel and member lists one layer forward, the message
  area at the front. They used to share one fill and ran into each other.

  Separation comes from brightness and a hairline of light at each edge, not
  from gaps or borders. Other themes solve this with detached cards, which is
  paper logic; glass separates by depth.

- **Category headings are centred**, framed by two lines that fade towards the
  text. Channel names deliberately stay left-aligned: scanning a list needs a
  fixed left edge, while a heading is read on its own.

- **The user panel blends in** instead of sitting there as a bordered card. It
  carries the same fill as the list above it and a single light edge, like
  every other transition in the picture.

- Surfaces are thinner overall. Glass should let through; the separating work
  is done by blur and light edges, not by opacity.

### Added

- **The default background is a computed gradient**, not a photo. Nothing to
  load, no dead link when a host disappears, sharp at any resolution.

  More importantly it is built from the glow colours: change them in the plugin
  and the background follows. Picture and interface can no longer drift apart.

  A fine noise layer sits on top. Gradients across a full screen show banding -
  visible steps between colour stops - and noise breaks those edges up without
  reading as grain.

### Notes

Three things about the glow ring, in case anyone is tempted to change it back.

Both stages must point the same way. With the resting state inset and the hover
state outset, hovering did not change the strength but the kind of effect, and
the outer shadow was clipped at the edge.

Inset was tried on the theory that an outer ring follows the corners of a
square surface and reads as a drawn border. In practice the outer glow lifts
the surface off the background while the inset one muddied it.

And centring a heading needs the heading itself to span the full width. Three
attempts adjusted its children; the element was 114px wide inside a 247px
container, so there was never anything to centre.

---

## 2.7.0

### Changed

- **The glow means something now.** It used to sit on every server icon, every
  button and every panel at once, faintly and permanently. When everything
  glows, glowing is the resting state and says nothing.

  Nothing glows at rest any more. In exchange the remaining states are stronger
  than before, because each one hits a single element and is therefore worth
  looking at: what you hover, where you are, what is unread, who is speaking.

  The one exception is your own avatar in the bottom left. It is not a signal
  but the anchor of the interface, and the collapsed panel consists of it.

- The message bar no longer glows at rest. It carried the effect permanently
  and was thus the brightest thing on screen without ever having anything to
  report. It lights up on focus instead.

### Added

- **Unread servers get a glowing dot.** Discord marks them with a small grey
  indicator that has to be looked for; it now lights up in the theme colour.
  The open server keeps Discord's elongated shape but takes the second colour,
  so "I am here" stays distinguishable from "something is new".

  The server icon itself deliberately stays dark. Dot and icon together turn
  one message into two.

- **Mention badges are hearts.** The red circle with the count becomes a heart
  in the theme colour, using the same outline as the avatars. The number stays
  centred where the shape is widest.

### Fixed

- Two rules still referenced `item_caf372`, a hash that no longer exists. They
  had been doing nothing for a while.

### Notes

The unread state has no class with "unread" in it, which a first attempt
assumed. Discord simply shows the indicator by adding `visible` to it, and that
is the anchor to hang things on.

---

## 2.6.0

### Changed

- **The bottom-left panel opens and closes as one motion.** Previously the
  frame snapped to size while only Stream and Voice glided, and all of them at
  once. Everything is staggered now, and closing runs the same sequence
  backwards and slightly faster.

  The frame still snaps rather than animating, and that stays deliberate:
  animating `width` forces a layout recalculation of the whole sidebar every
  frame, and at a fixed size the mouse would sit over nothing in the gaps
  between cards, dropping `:hover` mid-motion. What changed is that the
  collapse is now *delayed* - the children are gone before the frame folds.

- Sidebar spacing is now handled per view. Discord already shortens the server
  and channel lists itself; only the DM list needed the full clearance, and
  applying it everywhere cost visible space.

### Notes

Most of the work went into values that cannot be interpolated - `box-shadow:
none`, `width: auto`, `margin: 0 auto`. Each discards the transition along with
its delay and jumps immediately, which looks like a timing problem but is not.

---

## 2.5.0

### Fixed

- **The bottom entries of every sidebar list were unreachable.** The lists
  stopped 34px above the bottom edge while the collapsed panel is 56px tall -
  the remaining 22px sat underneath it. Most noticeable in DMs, where the list
  is long and the last conversation is often the one you want.

  `--panel-reserve` is now derived from `--panel-collapsed` instead of being a
  separate hard-coded number. The two always belonged together; nothing said so,
  so they drifted apart.

### Added

- While the panel is open, the lists fade out further up. The panel is frosted
  glass, so entries underneath showed through it - restless, especially in the
  gaps between cards.

  Shortening the list would have been the wrong fix: that changes its layout,
  so it would jump on every mouse movement and carry the scroll position with
  it. The mask costs no layout at all. Everything stays reachable - move the
  mouse away and the entries are back.

### Notes

The distinction that matters here is duration. What is only covered while you
look at it gets masked; what stands in the way permanently gets real space.

Permanently-open mode therefore reserves 210px and does *not* mask. Both at
once removed 440px and swallowed the lowest servers entirely, even scrolled all
the way down - two fixes for the same problem, each fine alone, ruinous
together.

That 210px is an estimate and has to be: CSS cannot read a sibling's height. It
covers the account card plus a voice panel; with a screen share running it can
get tight again. Override `--panel-reserve` in Custom CSS to taste.

---

## 2.4.0

### Changed

- **The bottom-left panel opens and closes as one motion.** Previously the
  frame snapped to size while only Stream and Voice glided, and all of them at
  once - the account card, its name and its buttons appeared instantly. The
  break between the two was what made it feel abrupt.

  Everything is staggered now. Opening: the card's surface fades in, then the
  name, then the buttons, with Voice and Stream rising behind them. Closing
  runs the same sequence backwards and slightly faster - opening is something
  you want to see, closing is something you want over with.

  The frame still snaps rather than animating, and that stays deliberate:
  animating `width` forces a layout recalculation of the whole sidebar every
  frame, and at a fixed size the mouse would sit over nothing in the gaps
  between cards, dropping `:hover` mid-motion. What changed is that the
  collapse is now *delayed* - `transition: width 0s 0.28s` waits, then jumps in
  one step, so the children are gone before the frame folds.

### Fixed

- With "Collapsing panel" switched off, the name and buttons stayed invisible
  and the glow was missing entirely. The permanently-open rules restored
  `display` but not `opacity` and `transform`, and the collapsed-ring rule hung
  on `:not(:hover)` - which is always true when nothing ever hovers.

### Notes

Most of the work here went into values that cannot be interpolated, and every
one of them broke the same way: the transition is discarded, the delay with it,
and the property jumps immediately.

- `box-shadow: none` → a transparent shadow instead, so there is something to
  blend to.
- `width: auto` on the account card → a concrete length, otherwise the card
  snapped left while the panels above were still fading.
- `margin: 0 auto` on the avatar → kept constant in both states. An auto margin
  in a flex row absorbs free space, so the avatar re-centred the instant its
  siblings left the layout.

That last one needed one more step: name and buttons now fade out early but
keep occupying space until the card actually shrinks. Visibility and layout are
deliberately decoupled, otherwise the avatar gets a window in which it is alone
in a still-wide card.

Known and left open: the DM list still runs visibly behind the panel, unlike
the server and channel lists. Its scroll area carries the margin (verified in
the DOM), so the selector is not at fault. Cause unclear, impact minor.

---

## 2.3.0

Needs **LovelaceSettings 1.2.0** for the glow toggle to work completely. Older
plugin versions still switch off the breathing, just not the resting glow.

### Added

- **Message list** (new section 10). The client frame was styled throughout
  while the message list - where you actually look - still showed Discord's own
  appearance.
  - Mentions in theme colours instead of Discord's yellow: a gradient fading
    out to the right rather than a solid block, so the text stays readable, and
    the bar on the left recoloured. Inline `@name` mentions get a tinted
    background that brightens on hover.
  - Message hover as a faint sheen with a light edge on the left, replacing the
    grey box. Deliberately without `backdrop-filter` - this fires on every
    mouse move, and blur is the most expensive effect in the theme.
  - Embeds pick up the frosted glass. The coloured stripe on the left stays: it
    is set per embed by Discord, often in the colour of the linked site or bot,
    and overwriting it would remove information rather than add any.

### Changed

- **The glow is a system now**, not a collection of one-off rules. Four
  variables define it: `--lv-glow-shape` and `-hover` for anything clipped
  (uses `drop-shadow`, follows the heart outline), `--lv-glow-box` and `-hover`
  for boxes. Every rule draws from those, so adjusting one value moves the
  whole client instead of leaving individual spots sticking out.
- The message bar no longer glows permanently. It carried the generic panel
  border and belonged to no system, which is why it looked stuck on. It now
  lights up on focus - the effect appears where attention already is.
- Mention colours are mixed from `--glow-pink` and `--glow-blue` via
  `color-mix()` instead of being hard-coded. They follow the colour set in the
  plugin; previously they stayed pink while everything else changed.

### Fixed

- **Server icons had no glow at all.** The rule hung on `.wrapper__6e9f8`, a
  hash that no longer exists in current Discord builds. Now anchored to
  `[data-list-id="guildsnav"]`, which survives hash changes.
- **The glow toggle only stopped the breathing.** Three rules carried a static
  glow with no variable behind it, so switching glow off left them lit. All
  glow rules now hang on `html:not(.lv-no-glow)`, which the plugin sets.
- **The account avatar had a rectangular glow.** It was never the avatar: the
  collapsed panel's `::after` ring is `inset: 0` plus `box-shadow`, and while
  collapsed the panel is exactly avatar-sized. Collapsed, only the avatar glows
  now - heart-shaped, via a blurred shape behind it. The ring returns when the
  panel expands, where it actually encloses an area.

### Notes

Three things about the avatar glow that cost time and are worth recording.

`drop-shadow` cannot produce a heart here at all. Above the `foreignObject` it
only sees that layer rasterised - a rectangle. Below it, at the image,
`clip-path` cuts the shadow away, because clipping happens *after* filtering.
Both sides of that boundary give a rectangle, so the glow is now a blurred
heart-shaped surface drawn behind the avatar instead of a shadow derived from
it.

Animating `filter: blur()` caused visible stuttering - it is recomputed every
frame and does not run on the GPU. The blur is fixed now; only `opacity`
changes, which the compositor handles for free.

The collapsed ring is hidden with a *transparent* `box-shadow`, not `none` and
not `opacity: 0`. There is no interpolation from `none`, so the browser waits
out the transition and jumps at the end, which reads as lag. And `opacity` is
animated by `glow-breathe` itself - a running animation overrides plain
declarations, so that value was overruled every frame. Expanding also happens
without a transition, matching the panel, which grows instantly.

---

## 2.2.3

### Fixed

- The online/idle/DND status dot disappeared from avatars. Introduced in 2.2.1
  and present in 2.2.2 - update if you are on either.

  `[class*="avatar_"]` matched more than avatars: some of those classes sit on
  layout containers rather than images. `div.avatar__20a53` is a 32×32 flex box
  holding a 40×40 SVG, and that overhang is deliberate - the status dot lives
  there, outside the avatar. A clip path on the container cut it away entirely.

  The rule now reads `img[class*="avatar_"]` and only ever touches images. The
  status dot sits in the same SVG as a sibling of the image and is left alone.

  Worth recording why 2.2.1 looked correct: it changed two things at once, the
  selector *and* `border-radius: 0`. The border radius was what actually fixed
  the round avatars; widening the selector achieved nothing except breaking the
  status dot. One confirmation cannot tell two simultaneous changes apart.

---

## 2.2.2

### Changed

- Server icons are now addressed by where they sit in the tree, not only by
  their hashed class. `[data-list-id="guildsnav"]` identifies the server rail
  through an attribute tied to function rather than generated CSS, so it
  survives Discord updates that `.svg_cc5dd2` would not. Inside that context an
  icon SVG is unambiguously a server icon, so a loose class match is safe there.

  Note for anyone tempted by the obvious alternative: the rail's `aria-label`
  is translated ("Server-Seitenleiste", "Servers sidebar", …), so anchoring to
  it would silently do nothing for anyone running Discord in another language.

  The old hashed classes stay in place. They work today and catch anything the
  context selector might miss; they only go dormant once Discord rotates its
  hashes, at which point the context rule takes over.

  Verified: server icons, the add button and the compass. Not verified: the
  unread badge - no server was unread at the time. It should be unaffected, and
  the file notes what to change if it is not.

---

## 2.2.1

### Fixed

- The heart shape only reached part of the avatars. Server icons were hearts
  while user avatars - message list, member list, DM list, the account panel
  and profile cards - stayed round. Two separate causes, both of which had to
  go:

  **Only two of seven classes were listed.** Discord gives avatars a different
  hashed class depending on where they sit; the DOM holds at least
  `avatar__44b0c`, `avatar_c19a55`, `avatar__20a53`, `avatar__91a9d`,
  `avatar_f75fb0`, `avatar_fc8177` and `avatar__37e49`. The rule named the
  first two. It now matches `[class*="avatar_"]`, which covers all of them and
  survives the next hash change.

  The underscore in that selector matters: Discord always attaches the hash
  with `_` or `__`, so `avatar_` hits avatars only. `avatarDecoration_`,
  `avatarStack_` and `avatarWithText_` carry a capital letter there and drop
  out by themselves - deliberately, because clipping the Nitro decoration
  would fray it.

  **`border-radius` was fighting the clip path.** Both apply at once and only
  the intersection shows, so the tip and the two arcs were cut off against the
  circle and what remained looked round with a hint of a heart. Avatars now get
  `border-radius: 0`. Server icons never had the problem - they carry no
  radius, which is why they looked right the whole time.

---

## 2.2.0

### Changed

- The heart shape is round now. It had twelve points, and `polygon()` draws a
  straight line between any two of them - with the two upper arcs getting three
  segments each, they read as corners rather than curves, even at 32 px.

  The new outline has 48 points and is not hand-placed: it is sampled from the
  classic heart curve (`x = 16sin³t`, `y = 13cos t − 5cos 2t − 2cos 3t − cos
  4t`), normalised to 0-100% with the y-axis flipped for CSS. Above 48 points
  nothing visible changes at the sizes Discord actually renders avatars.

  Two side effects worth knowing: the dip at the top sits slightly lower than
  before, and the flanks run a little straighter into the tip. Both come from
  the curve itself. `--heart-clip` is still a single variable, so any other
  outline can replace it.

---

## 2.1.1

### Fixed

- With the merged title bar on, a BetterDiscord notice banner swallowed the
  window controls. The minimise, maximise and close buttons disappeared behind
  the banner, and clicking the banner's own close button hit the window's X
  instead.

  BetterDiscord inserts its banners as a grid row above the app, so everything
  below shifts down - except the window control bar, which is `position: fixed`
  and stays pinned to the top edge. It ended up sitting invisibly on top of the
  banner, and since its `z-index` outranks everything, it caught the clicks
  meant for it.

  The bar now shifts down by the height of the banners. CSS cannot count, so
  three staggered rules cover one, two and three banners; beyond that the bar
  merely sits too high again rather than breaking.

---

## 2.1.0

### Fixed

- Server rail, channel list and DM list ran on behind the bottom-left panel,
  so entries disappeared underneath the account avatar while scrolling. The
  scroll areas now end above the panel.

  Padding alone was not enough: it only adds room at the end of a list, so
  anyone with enough servers to scroll still saw icons pass behind the avatar
  mid-list. A margin shortens the scroll area itself, which holds at any
  scroll position.

- The lists now fade out at the bottom instead of being cut off mid-word.
  Adjustable through `--panel-reserve` and `--panel-fade`.

---

## 2.0.0

### Added

- Seven features can now be switched off individually: heart shapes, glow, the
  collapsing panel, the merged title bar, the list fills, full timestamps and
  the enlarged picker. Everything else stays fixed.

  Four of them work through `--lv-*` variables. The rules read their value from
  a variable with a default, so leaving it alone keeps the feature on.

  The other three - hearts, title bar and the collapsing panel - use classes on
  the `<html>` element instead. A variable can change a value but cannot switch
  a rule off, and these three need exactly that: Discord's server icon shapes
  come from a `mask` attribute rather than CSS, the collapsed panel state spans
  a dozen declarations, and the window controls are sized by several rules at
  once. Setting a variable to `revert` fell back to the browser default in each
  case, which is not the same as Discord's.

- Companion plugin
  [LovelaceSettings](https://github.com/Enjuchan/LovelaceSettings) sets all of
  this through an interface, so nothing has to be edited by hand.

### Notes

Nothing changes visually after updating - every switch defaults to the previous
behaviour.

---

## 1.0.0

First release.

### Added

- Frosted glass treatment across chat, context menus, tooltips, modals, the
  settings window, the call view, pinned messages and profile popouts
- Heart-shaped avatars and server icons via a scalable polygon
- Collapsible panel in the bottom left holding stream and voice controls
- Breathing glow on panels, selected server and active DM; stronger pulse while
  speaking in voice
- Enlarged emoji and GIF picker, stretched to the full window height
- The separate title bar row is merged into the header below it. Minimise,
  maximise and close sit next to the search field, the header takes over as
  the window drag area, and the space they need is reserved in both the server
  view and the friends list. Adjustable through `--titlebar-height` and
  `--winbuttons-space`.
- Light fill behind the server, channel, DM and member lists to set them apart
  from the background, without an extra backdrop blur
- Shop, Nitro and Quests hidden from the DM list; gift button removed from
  the message bar
- Full timestamps instead of Discord's short form
- Gold gradient on progress bars
- Background image, swappable through a single `--bg-image` variable, with
  `--brightness-bg-image` and `--bg-blur` for dimming and softening
- Optional styling for the SpotifyControls plugin: album art as a full-bleed
  blurred backdrop, controls layered over it, artwork sharpening on hover
- Blur is tiered: `--blur-other` for persistent surfaces, `--blur-popup` for
  transient ones. Setting both to `0px` disables the most expensive effect.

### Fixed

- The theme and DynamicBackgrounds fought over the same layer. Both attach
  their background to Discord's base layer, and the theme's sat on top, so the
  plugin's transitions and ambient effects were invisible and its blur and
  dimming sliders had no effect. The theme now steps aside entirely whenever
  the plugin is present.
- Removing that conflict also removed the need for the plugin's "Override theme
  CSS variable" setting, which should be left off.
- `position: relative` on Discord's background layer collapsed the plugin's
  absolutely positioned container to zero size, hiding every image.

### Notes

Selectors were verified against Discord's live DOM rather than guessed. Where a
name provably matched exactly one class it uses `[class*="name_"]`, which
survives Discord changing its generated hashes. Ambiguous names such as
`wrapper_` (14 classes) or `container_` (17) deliberately keep their hash.