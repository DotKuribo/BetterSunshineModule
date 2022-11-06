# BetterSunshineModule
Template repository for making modules using Better Sunshine Engine

## Structure
`include/` This is where public header files go.

`lib/` This is where submodules and libraries go.

`src/` This is where source files and private header files go.

## Including SMS files
Super Mario Sunshine, Dolphin, JSystem, and BetterSunshineEngine headers can be included like so:

```
#include <SMS/...>
#include <Dolphin/...>
#include <JSystem/...>
#include <BetterSMS/...>
```

## Portability
You can port code to different regions using the macro `SMS_PORT_REGION(us, eu, jp, kr)`, which can be included using `#include <SMS/macros.h>`. This is commonly useful for porting addresses when patching.

## Patching

### Low Level
BetterSunshineEngine provides utilities to patch arbitary memory and code through Kuribo. The following macros are provided:

```
#include <BetterSMS/module.hxx>

SMS_PATCH_B(address, function)  // Branches to the specified function at the address
SMS_PATCH_BL(address, function)  // Calls the specified function at the address
SMS_WRITE_32(address, value)  // Writes the specified value at the address
```

These are the fundamental patching methods for low level module construction.

### High Level
BetterSunshineEngine provides utilities to hook into different points of the game generically. These are very useful for non-volatility and multi-module support. Examples shown below:

```
#include <Dolphin/OS.h>
#include <JSystem/J2D/J2DOrthoGraph.hxx>
#include <JSystem/J2D/J2DTextBox.hxx>

#include <BetterSMS/module.hxx>
#include <BetterSMS/game.hxx>
#include <BetterSMS/stage.hxx>

/*
/ Example that logs to the console and draws to the screen during gameplay
*/

void onGameInit(TApplication *app) {
	OSReport("Game initialized!\n");
}

void onGameBoot(TApplication *app) {
	OSReport("Game booted!\n");
}

static J2DTextBox *sOurTextBox = nullptr;

void onStageInit(TMarDirector *director) {
	sOurTextBox = new J2DTextBox(gpSystemFont->mFont, "Hello Screen!");
	OSReport("Initialization successful!\n");
}

void onStageUpdate(TMarDirector *director) {
	OSReport("Hello world!\n");
}

void onStageDraw2D(TMarDirector *director, const J2DOrthoGraph *ortho) {
	sOurTextBox->draw(180, 140);
}
```

The benefit of designing your module this way is generalization, ease of patching, and the ability to mix your module with other modules more easily!