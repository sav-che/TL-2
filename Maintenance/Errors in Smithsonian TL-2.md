Some technical errors in XML files of the Smithsonian's TL-2 version (https://www.sil.si.edu/DigitalCollections/tl-2/downloads.cfm) 

##### Batch 1, 2023-10-30
___

`<AlternateAbbrev>` and `</AlternateAbbrev>` are missing from XML schema in `taxonomic-literature.xsd` 

___

Inconsistency in index naming: tags `<Index Type="Index_To_Titles">` and `<Index Type="Index_To_Names">` sometimes spelled with extra underscore: `<Index Type="_Index_To_Titles">` and `<Index Type="_Index_To_Names">`. Some meaning behind it?

___

In the file `TL_2_Vol_5.xml` index of titles is interrupted with a block of rogue duplicate tags: 
```
</Entries>
</Index>
<Index Type="Index_To_Titles">
<Entries>
```

___

Complete pages missing from both XML and TXT:  Volume I, pp. 896-897. 
As a result, the authors on these pages are not present in separate author index `tl2-authors.csv`

---
A few authors are fused and tags are missing:

`TL_2_Vol_3.xml`: Nicholson, Henry Alleyne AND Nicholson, William Alexander
Attention to line 51946.

`TL_2_Vol_5.xml`: Sandahl, Oskar Theodor AND Sandberg, John Herman
Attention to line 1912.

Same `TL_2_Vol_5.xml`: Sauerbeck, Friedrich Wilhelm AND Saunders, Charles Francis. Attention to line 4624.

As a result, the fused authors are not present in separate author index `tl2-authors.csv`

___

Tables were not included in XML except two:

`TL_2_Vol_1.xml` on lines 4563-4589
`TL_2_Vol_1.xml` on lines 54620-54621, just a header

Should be deleted from XML? They are present in TXT.
