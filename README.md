# nvim-spider
Use the `w`, `e`, `b` motions like a spider. Move by subwords and skip insignificant punctuation.

Lua implementation of CamelCaseMotion. Works in normal, visual, and operator-pending mode. Works with counts.

<!--toc:start-->
- [Features](#features)
	- [Subword Motion](#subword-motion)
	- [Skipping Insignificant Punctuation](#skipping-insignificant-punctuation)
	- [Text Object](#text-object)
- [Installation](#installation)
- [Limitations](#limitations)
- [Credits](#credits)
<!--toc:end-->

## Features
The `w`, `e`, `b` (and `ge`) motions work the same as the default ones by vim, except for two differences:

### Subword Motion
The movements happen by subwords, meaning it stops at the sub-parts of a CamelCase (or SCREAMING_SNAKE_CASE or kebab-case) variable.

```lua
-- positions vim's `w` will move to
local myVariableName = FOO_BAR_BAZ
--    ^              ^ ^

-- positions spider's `w` will move to
local myVariableName = FOO_BAR_BAZ
--    ^ ^       ^    ^ ^   ^   ^
```

### Skipping Insignificant Punctuation
A sequence of one or more punctuation characters is considered significant if it is surrounded by whitespace and does not include any non-punctuation characters.

```lua
foo == bar .. "baz"
--  ^      ^    significant punctuation

foo:find("a")
-- ^    ^  ^  insignificant punctuation
```

This speeds up the movement across the line by reducing the number of mostly unnecessary stops.

```lua
-- positions vim's `w` will move to
if foo:find("%d") and foo == bar then print("[foo] has" .. bar) end
-- ^  ^^   ^  ^^  ^   ^   ^  ^   ^    ^    ^  ^  ^ ^  ^ ^  ^  ^ ^  -> 21

-- positions spider's `w` will move to
if foo:find("%d") and foo == bar then print("[foo] has" .. bar) end
-- ^   ^      ^   ^   ^   ^  ^   ^    ^       ^    ^    ^  ^    ^  -> 14
```

If you prefer to use this plugin only for subword motion, you can disable this feature by setting `skipInsignificantPunctuation = false` in the `.setup()` call.

> __Note__  
> vim's `iskeyword` option is ignored by this plugin.

### Text Object
For an alternative `iw` text object that considers CamelCase word parts, check out the "subword" text object of [nvim-various-textobjs](https://github.com/chrisgrieser/nvim-various-textobjs).

## Installation

```lua
-- packer
use { "chrisgrieser/nvim-spider" }

-- lazy.nvim
{ "chrisgrieser/nvim-spider", lazy = true },
```

No keybindings are created by default. Below are the mappings to replace the default `w`, `e`, and `b` motions with this plugin's version of them.
Note that for dot-repeat to work properly, you have to use a `<cmd>` mapping that calls the plugin function, **not** a lua function calling it!

```lua
-- Keymaps
vim.keymap.set({"n", "o", "x"}, "w", "<cmd>lua require('spider').motion('w')<CR>", { desc = "Spider-w" })
vim.keymap.set({"n", "o", "x"}, "e", "<cmd>lua require('spider').motion('e')<CR>", { desc = "Spider-w" })
vim.keymap.set({"n", "o", "x"}, "b", "<cmd>lua require('spider').motion('b')<CR>", { desc = "Spider-w" })
vim.keymap.set({"n", "o", "x"}, "ge", "<cmd>lua require('spider').motion('ge')<CR>", { desc = "Spider-w" })
```

## Configuration

The `.setup()` call is optional. Currently, its only option is to disable the skipping of insignificant punctuation:

```lua
-- default values
require("spider").setup({
	skipInsignificantPunctuation = true
})
```

## Credits
<!-- vale Google.FirstPerson = NO -->
__About Me__  
In my day job, I am a sociologist studying the social mechanisms underlying the digital economy. For my PhD project, I investigate the governance of the app economy and how software ecosystems manage the tension between innovation and compatibility. If you are interested in this subject, feel free to get in touch.

__Profiles__  
- [Discord](https://discordapp.com/users/462774483044794368/)
- [Academic Website](https://chris-grieser.de/)
- [GitHub](https://github.com/chrisgrieser/)
- [Twitter](https://twitter.com/pseudo_meta)
- [ResearchGate](https://www.researchgate.net/profile/Christopher-Grieser)
- [LinkedIn](https://www.linkedin.com/in/christopher-grieser-ba693b17a/)

__Buy Me a Coffee__  
<br>
<a href='https://ko-fi.com/Y8Y86SQ91' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi1.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
