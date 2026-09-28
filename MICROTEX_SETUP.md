# Optional local formula rendering — Prompt Lite+ 1.6

Unicode formula display and the PNG/JPEG/BMP/GIF image viewer work without
MicroTeX. Graphical formula viewing is optional and entirely local: no AI or
remote rendering service is used.

Prompt Lite+ does **not** distribute MicroTeX binaries, fonts or resources.
The application includes only its Delphi loading interface. MicroTeX and its
dependencies remain separate components with their own upstream licenses.

## Install an optional renderer

The supported interface is `PromptLiteMath.dll` (Windows x64), exporting the
`RenderFormula` C function implemented by the small adapter below. A random
MicroTeX DLL is not interchangeable: upstream MicroTeX exposes a C++ API.

1. Build the adapter from [MicroTeXAdapter-source.zip](MicroTeXAdapter-source.zip).
   This archive contains only the adapter/build recipe, not third-party code.
2. Place its output and the upstream resource directory beside the application:

   ```text
   PromptLitePlus.exe
   MicroTeX/
     PromptLiteMath.dll
     res/
       .clatexmath-res_root
       fonts/
       greek/
       cyrillic/
       ...remaining upstream resource files...
   ```

3. Keep the resource tree intact, including its upstream license files.
4. Restart Prompt Lite+. Formula blocks now offer **View formula**. The module
   is not loaded at startup; it is loaded only when that link is clicked.

Without these files there is no formula-view link. Unsupported TeX remains
visible as its original text. Installing MicroTeX does not change AI behavior,
token counts, or the existing account limits.

## Build the adapter

Prerequisites: CMake and a **64-bit MinGW-w64** toolchain providing `g++` and
`gmake` on PATH. Tested with GCC 13.2.0. Run `Build.ps1` from the extracted
adapter sources. It downloads:

- [MicroTeX](https://github.com/NanoMichael/MicroTeX), revision
  `0e3707f6dafebb121d98b53c64364d16fefe481d`;
- [tinyxml2](https://github.com/leethomason/tinyxml2), version 10.0.0.

Review their license and font-license files before any redistribution of a
renderer you build. The script builds only the optional DLL and prints the
paths to the DLL/resources; it does not modify a Prompt Lite+ installation.
The CMake recipe scopes upstream GDI definitions to their namespace to avoid
a MinGW COM `Font` naming conflict. No browser, WebView or TeX installation is
required at runtime.

## Viewer behavior

The non-modal viewer offers Fit, 100%, zoom in and zoom out. It owns its image
and closing it does not refresh the conversation. Image data is read only on
click; network images are downloaded only then, with a 64 MB encoded-file
limit. Other image formats still use the Windows default application.

Formula conversion is a supported TeX subset, not a complete TeX engine. A
renderer error is shown in the viewer without changing the source message.
