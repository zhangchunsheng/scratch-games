# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Scratch to HTML converter - a tool that parses `.sb3` files (Scratch 3.0 project format) and generates standalone HTML games that can run in the browser without a server.

## Architecture

The project consists of two HTML-based converter tools:

### `scratch_to_html_converter.html`
- Modern UI with drag-and-drop support for `.sb3` files
- Uses **JSZip** (via CDN) to unpack `.sb3` archives
- Parses `project.json` from the SB3 bundle and extracts assets (costumes, sounds)
- Generates a self-contained HTML file with the game logic translated to JavaScript
- Supported blocks:
  - Motion: move, rotate, goto x/y, get position, bounce
  - Looks: say/think bubbles, switch costume, size, effects
  - Events: green flag, key press, broadcast
  - Control: loops, conditionals, wait, repeat until
  - Operators: math, comparisons, random
  - Variables: get/set
- Limitations:
  - Sound blocks not implemented (requires Web Audio API)
  - Clone support is basic
  - Video sensing and custom blocks not supported

### `htmlifier-offline.html`
- Fork of HTMLifier (SheepTester) for offline conversion
- Contains CSS by Mr. Cringe Kid
- More extensive block support (see the `convertBlock` function in the file)

## Generated Output Structure

Both converters produce HTML files containing:
- A `<game-container>` element with the stage and sprites
- CSS for Scratch-like rendering
- JavaScript that:
  - Initializes the game loop
  - Maps Scratch blocks to JS functions
  - Handles collision detection and boundary checks
  - Manages sprite state (position, direction, costume, variables)

## Development

This is a static HTML/JS project - no build system or dependencies beyond JSZip. To develop:

1. Open either converter HTML file in a browser
2. Test with `.sb3` files to verify conversion output
3. Check generated HTML runs correctly by opening in browser

## Testing

Manual testing workflow:
1. Upload a `.sb3` file via drag-drop or file picker
2. Download the generated HTML
3. Open the output HTML in a browser
4. Verify the game runs (green flag starts, controls respond)
