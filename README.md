# NAI Image Tool

A browser-based workspace for generating NovelAI images and managing the prompts, references, tags, and reusable assets behind them.

[Open the live demo](https://yuli-art.github.io/nai-image-tool-site/)

## User problem

Repeated image-generation work becomes difficult to manage when prompts, reference images, character definitions, artist styles, and generation settings are spread across separate files and tools.

As the image library grows, browsing and reusing earlier work can also become slow and cumbersome. The goal was to make this workflow easier to organise, test, and repeat from one interface.

## What the tool does

The tool provides:

- NovelAI image generation with configurable model and generation settings
- Reusable character, artist/style, prompt-note, and image libraries
- Search, tags, filters, and custom library views
- Image-to-image, Vibe Transfer, and Precise Reference workflows
- Batch generation and A/B comparison
- Local browser storage with import/export tools
- Optional Google Drive backup and synchronisation
- Danbooru tag search and suggestions

## My role

I led the product definition, workflow design, hands-on testing, issue identification, and iteration of the tool.

I translated problems from repeated real-world use into requirements, tested proposed fixes, checked regressions across related workflows, and deployed working versions through GitHub Pages.

I used AI coding assistance extensively to generate, debug, and revise the implementation. This repository demonstrates my product thinking, QA work, and ability to direct an AI-assisted prototype; it is not intended to imply that I personally hand-wrote every line of code.

## How I tested it

I used manual exploratory and regression testing based on realistic workflows rather than testing isolated controls only. This included:

- Repeated and extended sessions with a growing image library
- Searching, filtering, navigating, and reusing stored assets
- Moving reference images between the gallery, character library, and generation workflow
- Checking persistence and restoration of saved data
- Testing supported and unsupported Precise Reference configurations
- Repeating related workflows after fixes to check for regressions
- Verifying the deployed GitHub Pages build, not only a local copy

## Issues identified and resolved

| Issue observed | How it was found | Resolution and validation |
| --- | --- | --- |
| Loading the entire image library caused noticeable lag during extended use. | The problem emerged as the stored library grew and normal browsing became slower. | I specified and tested pagination that loaded 20 images at a time. The current implementation generalises that fix with paginated gallery, character, artist, and notes views, selectable page sizes, and a current default of 30 items. |
| Precise Reference requests were unreliable when source images and request data did not match the API's expected format. | I reproduced failures through the local-upload and saved-gallery reference workflows and narrowed the problem to image normalisation and request construction. | The fix normalises references to supported canvas sizes with black padding, validates model and feature compatibility, constructs the required director-reference fields, and adds clearer request tracking and error handling. |
| Failures were difficult to diagnose from generic network errors. | Error-path testing showed that users lacked enough information to distinguish invalid configurations, rate limits, and server failures. | The request flow now provides more specific validation messages, correlation IDs, timeout handling, and limited retry behaviour for rate-limited requests. |

Relevant implementation history:

- [Library pagination implementation](https://github.com/Yuli-Art/nai-image-tool-site/commit/949b212f70153491cb64e47a1d9dc64456375056)
- [Precise Reference API fix](https://github.com/Yuli-Art/nai-image-tool-site/commit/f9760001001619429466dc529b6785f23efdbe2b)

## Security and privacy

The live demo requires users to provide their own NovelAI token before generating images. The token is stored in the browser's local storage and sent directly from the browser to NovelAI's API.

Do not enter a token on a shared or untrusted computer. Remove it using the application's settings when finished.

Google Drive synchronisation is optional and requires the user to configure their own OAuth client ID.

## Project status

This is an independently developed, AI-assisted prototype and portfolio case study. It is not affiliated with or endorsed by NovelAI or Danbooru.
