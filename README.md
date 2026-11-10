# V2 pack - auto-update list

This is the update list for our modded Minecraft pack (NeoForge 1.21.1). There are no mod files in here, only small
pointer files that tell the updater where each mod comes from. It is for Prism Launcher. CurseForge app users keep
importing the zip.

## You already imported the V2 Prism zip - switch it to auto-update

1. Download `packwiz-installer-bootstrap.jar` from
   https://github.com/packwiz/packwiz-installer-bootstrap/releases (the newest release, one small file).
2. In Prism, right-click the V2 instance - Folder. Open the `minecraft` folder inside it and put the jar there.
3. Right-click the instance - Edit - Settings - Custom commands. Tick the box and paste this into
   **Pre-launch command**:

   ```
   "$INST_JAVA" -jar packwiz-installer-bootstrap.jar https://unknowncaz.github.io/ff-v2-pack/pack.toml
   ```

4. Close the window and press Launch. A small updater window shows first, then the game starts. From now on it checks
   for updates every time you launch.

Good to know: the first run sets the pack's config files back to the pack's versions, and puts the pack's server list in.

## Starting from nothing

Make a NeoForge 1.21.1 instance (NeoForge 21.1.265) in Prism, then do steps 1 to 4 above.

Six mods can't be downloaded automatically because their authors turned that off. The first launch shows a window
called "Failed file downloads" listing them:

1. Click **Open missing mods**. Six CurseForge pages open.
2. Download the file on each page.
3. Move the six files from your Downloads folder into the instance's `minecraft\mods` folder. Don't rename them.
4. Click **Continue**.

Do NOT click "Ignore update" - that starts the game with no mods. You only do this once. It comes back only if one of
those six mods gets a new version.

## Can't get in?

The server has a whitelist. Ping Caz with your Minecraft name.
