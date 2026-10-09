# TMI Lite+ Third-Party Notices

Document version 2.0  
Last updated: 9 October 2026

TMI Lite+ interoperates with or is built using third-party products. Those
products remain governed by their own licenses and terms.

## OpenAI Codex

Codex CLI and OpenAI services are installed, licensed, authenticated, updated,
and operated separately from TMI Lite+.

OpenAI, ChatGPT, Codex, and associated names and marks belong to OpenAI or its
affiliates. TMI Lite+ is not affiliated with or endorsed by OpenAI.

Official information:

- [Codex documentation](https://developers.openai.com/codex/)
- [OpenAI terms](https://openai.com/policies/terms-of-use/)
- [OpenAI service terms](https://openai.com/policies/service-terms/)
- [OpenAI design and brand guidance](https://openai.com/brand/)

## Development and runtime components

TMI Lite+ is built with Embarcadero Delphi and uses Raize Components VCL.
Distributed builds may contain compiled runtime portions covered by the
applicable Embarcadero and Raize Software licenses. This notice does not grant
rights beyond those licenses.

## Microsoft Windows

TMI Lite+ uses Windows APIs and is distributed only as a 64-bit Windows
application. Windows and related marks belong to Microsoft Corporation.

## Visualize viewer dependencies

The viewer uses Microsoft Edge WebView2. The 64-bit WebView2Loader.dll is a
Microsoft SDK component (1.0.3065.39) supplied with the Delphi redistribution files; the
Microsoft Edge WebView2 Runtime is installed and licensed separately. Microsoft
terms apply; TMI does not download or install the runtime implicitly.
The matching SDK license is reproduced in WebView2-LICENSE.txt.
See [WebView2 information](https://developer.microsoft.com/microsoft-edge/webview2/).

Lucide 0.468.0 is bundled for visualization icons. Its ISC license and embedded
Feather MIT notice are reproduced in visualize-lucide-LICENSE.txt beside the
executable. See [Lucide](https://github.com/lucide-icons/lucide).

visualize-host.css and visualize-host.js are TMI's local host implementation.
OpenAI's Visualize skill runtime/assets are not redistributed. A fragment may
load approved external libraries on request; their licenses and terms are
separate from TMI and from these notices.

## User-installed tools

MicroTeX binaries, fonts and resource files are not distributed with TMI Lite+.
The optional local formula renderer must be installed separately, with its
upstream license notices retained. The separate adapter source archive contains
the adapter/build recipe, not bundled third-party source. See
[MicroTeX Setup](MICROTEX_SETUP.md) before building or installing it.

MCP servers, command-line programs, package managers, browsers, compilers,
scripts, and other tools invoked through Codex CLI are not included merely
because TMI Lite+ can display or approve their use. Their licenses and notices
must be reviewed separately by the User.
