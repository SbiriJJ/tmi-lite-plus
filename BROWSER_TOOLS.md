# Browser Tools in TMI Lite+

Document version 2.7

TMI Lite+ uses Playwright MCP in extension mode on Windows. Browser Tools is
optional and off by default. TMI does not install Node, Playwright, or a browser
automatically.

## One-time setup

1. Install Node.js 20 or newer and make `node.exe` available on PATH before
   starting TMI Host.
2. From the directory containing `TMIHost.exe`, run:

   ```powershell
   npm.cmd install --prefix .\third_party\playwright --no-audit --no-fund --ignore-scripts --save-exact @playwright/mcp@0.0.86
   ```

3. In the browser profile you want to share, install the [Microsoft Playwright
   Extension](https://chromewebstore.google.com/detail/playwright-extension/mmlmfjhmonkocbjadbfplnigmagldckm).
4. For a portable browser or a non-default profile, optionally create
   `third_party\playwright\browser.ini`, next to the installed package:

   ```ini
   [Browser]
   Executable=C:\path\to\chromium\chrome.exe
   ProfileDirectory=Default
   ```

   Without an executable, Playwright looks for installed Chrome. The profile
   directory is the final folder shown by `chrome://version` under **Profile Path**.

## Use

Open the configured browser and profile first. Enable **Browser Tools** on the
idle thread. On the first browser operation, approve Playwright's connection in
the browser and select only the tabs you want to share. Playwright can interact
with an approved tab; the connection is not read-only. TMI does not use an
extension token to skip this approval.

Turning Browser Tools off reconnects only this thread, without starting
Playwright or OpenAI's Node browser runtime. TMI stops the old thread's process
group; an independently opened browser and its tabs remain open. This does not
disable native TMI Desktop tools or web search.
