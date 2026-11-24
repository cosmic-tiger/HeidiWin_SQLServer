This README block created by google gemini
======================= README BLOCK GOOGLE GEMINI START ============================================================
# Building HeidiSQL in RAD Studio 13 Community Edition

## 1. Patch & Install SynEdit
1. Open `components\synedit\Source\SynEdit.inc` and add the Delphi 13 (`VER370`) mapping above `{$IFDEF VER360}`:
   ```pascal
   {\$IFDEF VER370}
     {\(DEFINE SYN_COMPILER_29} {\)DEFINE SYN_DELPHI}
     {\(DEFINE SYN_DELPHI_12} {\)ENDIF}
   ```
2. Open the SynEdit package group in `components\synedit\Packages\`.
3. Ensure **Windows 32-bit** is the active (bold) target platform for both packages.
4. Right-click `SynEdit_R` (Runtime) -> **Build**.
5. Right-click `SynEdit_D` (Design-time) -> **Install**.
6. Add `components\synedit\Source` to **Tools > Options > Language > Delphi > Library > Library Path** (for Windows 32-bit, and Windows 64-bit if building 64-bit).

## 2. Build & Install VirtualTreeView
1. Open the VirtualTreeView package group in `components\virtualtreeview\packages\`.
2. Ensure **Windows 32-bit** is active for both packages.
3. Right-click `VirtualTreesR` (Runtime) -> **Build**.
4. Right-click `VirtualTreesD` (Design-time) -> **Install**.
5. Add `components\virtualtreeview\source` to **Tools > Options > Language > Delphi > Library > Library Path**.

## 3. Install madExcept
1. Close RAD Studio 13.
2. Run the **madCollection** installer and select **madExcept 5** (required for Delphi 13 support) along with its default dependencies.

## 4. Compile `.rc` Resource Files
1. Locate `rc.exe` inside the Windows SDK (`C:\Program Files (x86)\Windows Kits\10\bin\<version>\x86\`).
2. Copy `rc.exe` (do **not** overwrite the existing `rcdll.dll`) into `C:\Program Files (x86)\Embarcadero\Studio\37.0\bin\`.
3. Open a Command Prompt in the `HeidiSQL` root folder and run:
   ```bat
   call "C:\Program Files (x86)\Embarcadero\Studio\37.0\bin\rsvars.bat"
   build-res.bat
   ```
4. Confirm `.res` files (including `icon.res` and `styles.res`) are generated in `res\` and `source\vcl-styles-utils\`.

## 5. Build & Run HeidiSQL
1. Open `packages\Delphi12.3\heidisql.dproj` in RAD Studio 13.
2. Press **Shift + F9** (**Project > Build heidisql**) to compile `heidisql.exe`.
3. Press **F9** to run.
4. *(Optional)* Copy database client DLLs (`libmariadb.dll`, `libmysql.dll`, `libpq.dll`, etc.) from an official HeidiSQL folder into `out\` to connect to live databases.


======================= README BLOCK GOOGLE GEMINI END = ============================================================


=====================================================================================================================

using RAD 13 / Delpghi 13 community edition. 32 BIT.

First, install madexcept. nagivate to madshi.net, install the latest madcollection (5?) which supports delphi 13

secondly, clone the solution

thirdly, using RAD, build Synedit. You need to make a slight change due to RAD studio 13 as the original was
built on 12.




Need to update the synedit.inc file

Why You Needed That Block

• What it does: VER370 is RAD Studio 13's internal compiler symbol.
       Because HeidiSQL's bundled copy of SynEdit.inc stopped at VER360 (Delphi 12),
       adding this block tells SynEdit to treat Delphi 13 (VER370) with the exact same
       compiler capabilities (SYN_COMPILER_29 and SYN_DELPHI_12) as Delphi 12.
• Note for your README: If you ever run git reset or pull updates to the synedit
        submodule later, SynEdit.inc might revert and need that 5-line block re-added before rebuilding SynEdit.

Find file ...
HeidiSQL\components\synedit\Source\SynEdit.inc

Add a block mapping to cater for latest version ..
Find the block of code beginning with {$IFDEF VER360}
copy that block.
Paste the block above that block.
On the new block, change VER360 to VER370, so you should see as below.
the lines inside (compiler 29, delphi 12) can stay the same


{------------------------------------------------------------------------------}
{ VERXXX to SYN_COMPILERX, SYN_DELPHIX and SYN_CPPBX mappings                  }
{------------------------------------------------------------------------------}


(nb this is new to add above the VER360 block - nb dont type this nb line )
{$IFDEF VER370}
  {$DEFINE SYN_COMPILER_29}
  {$DEFINE SYN_DELPHI}
  {$DEFINE SYN_DELPHI_12}
{$ENDIF}

(nb this already exists - add VER370 above this - nb dont type this nb line)

{$IFDEF VER360}
  {$DEFINE SYN_COMPILER_29}
  {$DEFINE SYN_DELPHI}
  {$DEFINE SYN_DELPHI_12}
{$ENDIF}

{$IFDEF VER350}
  {$DEFINE SYN_COMPILER_28}
  {$DEFINE SYN_DELPHI}
  {$DEFINE SYN_DELPHI_11}
{$ENDIF}

etc



=========================================================
Those two numbers inside the block (SYN_COMPILER_29 and SYN_DELPHI_12) are internal flags defined by SynEdit.inc itself,
not built-in Delphi symbols.

Why Keeping 29 and 12 Works

• VER370 is built into the compiler: RAD Studio 13 automatically defines VER370 when it compiles any file.
• SYN_COMPILER_29 and SYN_DELPHI_12 are custom flags: Further down in SynEdit.inc, dozens of feature switches—such as
default parameters (DefParam), Unicode strings, and generics—are turned on by checking {$IFDEF SYN_COMPILER_29} and {$IFDEF SYN_DELPHI_12}.
• No 37 or 13 exists further down: Because HeidiSQL's copy of SynEdit.inc was written before Delphi 13 existed,
the rest of the file has no rules for SYN_COMPILER_37 or SYN_DELPHI_13. If you changed those numbers to 37 and 13 without also rewriting the cascading rules lower down in SynEdit.inc, all of those modern compiler features would stay turned off, bringing back the exact same Not enough parameters error.
Mapping VER370 directly to SYN_COMPILER_29 and SYN_DELPHI_12 acts as an alias that tells SynEdit to treat Delphi 13 identically to Delphi 12.
=========================================================

=== compile the resources

-- install the windows 11 sdk (think its 10)  - should be 32 bit, into programs 86.

navigate to the sdk bin folder, the ones with largest 10.* ...
C:\Program Files (x86)\Windows Kits\10\bin\10.0.28000.0\x86
copy rc.exe.
paste into rad studio bin ...
C:\Program Files (x86)\Embarcadero\Studio\37.0\bin
the dll rcdll.dll should already be there. if it doesnt, get rcdll.dll from the sdk bin folder.
next, open rad studio (no solution or project selected), tools options, language, delphi, library.
Add two paths, to the source folders of the Synedit and virtualtree .
next, open the main heidi solution (under package , delph 12 , heudi group proj,
select "show in explorer"
navigate up to C:\Dev\HeidiWin_SQLServer
do a command line here.
compile all the .rc files by running build.res.bat - this will create a load of .res files for each .rc file.
NB this requires rc.exe
