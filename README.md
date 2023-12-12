# TL-2 vault for Obsidian

This is a repository for digital version of **Taxonomic literature : a selective guide to botanical publications and collections with dates, commentaries and types, ed. 2** (usually abbreviated **TL-2**). It is presented as a folder-based markdown-formatted "vault" for [Obsidian](https://obsidian.md/) and can be either viewed online (https://tl2.io) or used offline from your own Obsidian installation. See [this page](./Front%20matter/Documentation.md#publishing) to learn how TL-2 vault and website are maintained and populated.

## Offline setup 

1. Install Obsidian: https://obsidian.md/download.
2. Download TL-2 vault (any of TL-2_\* archives in https://github.com/sav-che/TL-2/releases/latest) and extract it to a preferred location.
3. Open Obsidian. On the first launch it will ask for action – select "Open folder as vault" and navigate to the folder you specified above. If you already have some vault opened, click on small "Open another vault" button in the bottom left corner, as in the screenshot below. After that the setup is done.

**NB:** if you want to install a newer version, do not try to overwrite a folder of an older version with it. Select a different name/location for the new version, or delete the old folder first. Also note that the first launch will be necessarily slow, because Obsidian will index the entire vault.

![](/Maintenance/misc_images/instructions_open_folder.jpg)

## Tips

- To quickly search through page names (= author names), first type `file:` in the search bar. To do so for publication numbers and titles, type `section:`.

- To use regular expressions in the search, put your query between slashes, e.g. `/Charko[vw]/`

- Other useful search options are explained here: https://help.obsidian.md/Plugins/Search

- TL-2 vault integrates well with [obsidian-wikidata-plugin](https://github.com/echinopscis/obsidian-wikidata-plugin). To set it up, open the plugin settings, type `wikidata_id` in "Frontmatter wikidata entity ID property name" field, and close the settings. Do not worry if this field will look unchanged. You may need to restart Obsidian after that. 

- Author pages in the version ≥1.1 include a by-default invisible "properties" block that contains author metadata from IPNI. To see properties, click on the ⓘ button in the upper right corner (screenshot below), or enable "Source mode" when in Editing view. To make properties always visible in the text, go to Options → Editor → Properties in document → set to Visible. 

![](/Maintenance/misc_images/instructions_open_properties.jpg)

- Note that Obsidian have two modes: editing and viewing, toggled with a button on upper right. Usually you would want to keep it in reading view.

![](/Maintenance/misc_images/instructions_change_view.jpg)

- If you want to edit the content frequently, you might want to switch default mode to "editing" in Settings → Editor → Default view for new tabs.

![](/Maintenance/misc_images/instructions_change_default_view.jpg)

- All official help pages: https://help.obsidian.md/.

## Fonts

The fonts cannot be included in the vault. If you want to see the same fonts as in *tl2.io*, please install them on your system:

- https://fonts.google.com/specimen/Source+Serif+4

- https://fonts.google.com/specimen/Source+Code+Pro 

## Contact

Anton Savchenko savchenko.anton.kh@gmail.com 
