## Source

This version of Taxonomic Literature 2 is based on XML-tagged clean OCR products presented by [Smithsonian Libraries](https://library.si.edu/) and International Association for Plant Taxonomy ([IAPT](https://www.iaptglobal.org/)). The source files are available [here](https://www.sil.si.edu/DigitalCollections/tl-2/downloads.cfm), in two formats, XML and TXT, with separate author index in CSV format. Visual checks were made against PDFs obtained [from BHL](https://doi.org/10.5962/bhl.title.48631).

## License

Similarly to the Smithsonian's version, database of *tl2.io* is licensed under [CC BY-NC 3.0](https://creativecommons.org/licenses/by-nc/3.0/).

## Text preparation

#### Pre-processing

CRLF line terminators were replaced with LF terminators. Names of sections (after `<Section>` tags) were checked for typos and normalized where appropriate; misplaced `</Section>` tags fixed. 
___
#### Clearing of line breaks

##### Per-section modifications

Original text naturally contained two types of line breaks, "soft" – for line wraps enforced by the width of printed area on a page, and "hard" – for intentional paragraph returns. In the OCR products, `<br/>` tags often signified hard breaks, while absence of a tag marked soft ones. Unfortunately, given the dense and complex layout of the printed TL-2, it was not always a rule: different sections of the text showed contrasting tendencies in `<br/>`-tagging. To distinguish line breaks and re-tag the XML, per-section regular expressions (.NET flavor) were developed in [regex101.com](https://regex101.com/) and executed in [dnGREP](https://dngrep.github.io) in the following order.
___
**Section Name**
All `<br/>` deleted.
Search for:
```regex
(?s)((?:\A|(?<=</Name>)).+?(?:\z|(?=<Name>)))|<br/>(?=.*?())
```
Replace with: `$1$2`
___
**Small sections**
`<br/>` protected/inserted: after `</Section>`, short lines, and in Ref. blocks.
Search for:
```regex
(?s)((?:(?:\A|</Sections>).+?(?:<Sections>\n|\z))|(?:<Section>(?:<[^>A-Z]+?>)*?(?:HERBARIUM|BIBLIOGRAPHY|BIOFILE|COMPOSITE|NOTE).+?</Section>\n)|(?:\n(?:<[^>A-Z]+?>)+?\bRef\b.+?</Section>)|(?:</Section>\n))|(?:\n(?:<br/>)?(?=(?:<[^>A-Z]+?>)*?\bRef\b)(?=.*?(\n<br/>)))|(?:(?<=(?-s:^.{0,79}$))\n(?:<br/>)?(?=.*?(\n<br/>)))|<br/>(?=.*?())
```
Replace with: `$1$2$3$4`
___
**1st paragraph of section BIBLIOGRAPHY... in Vols. 1-3**
All `<br/>` deleted.
Executed on files: `TL_2_Vol_[1-3].*`
Search for:
```regex
(?s)((?:\A|(?:\.\)?\n(?:<[^>A-Z]+?>)*?\p{Lu}\p{Ll})|</Section>).+?(?=\z|<Section>(?:<[^>A-Z]+?>)*?BIBLIOGRAPHY))|<br/>(?=.*?())
```
Replace with: `$1$2`
___
**Section BIBLIOGRAPHY...  in the rest of volumes**
All `<br/>` deleted.
Executed on files: `(?:TL_2_Vol_[^1-3].*)|(?:TL_2_Suppl_.*)`
Search for:
```regex
(?s)((?:\A|</Section>).+?(?=\z|<Section>(?:<[^>A-Z]+?>)*?BIBLIOGRAPHY))|<br/>(?=.*?())
```
Replace with: `$1$2`
___
**Section HERBARIUM...**
`<br/>` protected/inserted: Ref. blocks, after short lines, before 1|a-list lines.
Search for:
```regex
(?s)((?:\A|(?<=</Section>)).+?(?:\z|(?=<Section>(?:<[^>A-Z]+?>)*?HERBARIUM))|(?:\n(?:<[^>A-Z]+?>)+?\bRef\b.+?</Section>))|(\n<br/>\((?:(?:[1-9]|[1-2][0-9])|[a-z])\))|(?:(?<=(?-s:^.{0,79}$))\n(?:<br/>)?(?=.*?(\n<br/>)))|<br/>(?=.*?())
```
Replace with: `$1$2$3$4`
___
**Section COMPOSITE WORKS**
`<br/>` protected/inserted: after short lines, before 1|a)|.-list lines.
Search for:
```regex
(?s)((?:\A|(?<=</Section>)).+?(?:\z|(?=<Section>(?:<[^>A-Z]+?>)*?COMPOSITE)))|(?:\n(?:<br/>)?(?=\(?(?:(?:[1-9]|[1-2][0-9])|[a-z])(?:\)|\.))(?=.*?(\n<br/>)))|(?:(?<=(?-s:^.{0,79}$))\n(?:<br/>)?(?=.*?(\n<br/>)))|<br/>(?=.*?())
```
Replace with: `$1$2$3$4`
___
**Section NOTE**
`<br/>` protected/inserted: after short lines, before 1|a)|.-list lines.
Search for:
```regex
(?s)((?:\A|(?<=</Section>)).+?(?:\z|(?=<Section>(?:<[^>A-Z]+?>)*?NOTE)))|\n(?:<br/>)?(?=\(?(?:(?:[1-9]|[1-2][0-9])|[a-z])(?:\)|\.))(?=.*?(\n<br/>))|(?:(?<=(?-s:^.{0,79}$))\n(?:<br/>)?(?=.*?(\n<br/>)))|<br/>(?=.*?())
```
Replace with: `$1$2$3$4`
___
**Section Full_Title**
`<br/>` protected/inserted: after short lines.
Search for:
```regex
(?s)((?:\A|(?<=</Full_Title>)).+?(?:\z|(?=<Full_Title>)))|(?:(?<=(?-s:^.{0,70}$))\n(?:<br/>)?(?=.*?(\n<br/>)))|<br/>(?=.*?())
```
Replace with: `$1$2$3`
___
**Section Short_Title**
All `<br/>` deleted.
Search for:
```regex
(?s)((?:\A|(?<=</Short_Title>)).+?(?:\z|(?=<Short_Title>)))|<br/>(?=.*?())
```
Replace with: `$1$2`
___
**Section Notes (in Titles)**
`<br/>` protected/inserted: Ref blocks, after .:)" , short lines, before (1|a)-list lines.
Search for:
```regex
(?s)((?:(?:\A|\n</Title>).+?(?:\z|(?=<Title>)))|(?:<Title>.+?(?:\z|<Notes>))|(?:(?:(?:\G(?:<[^>A-Z]+?>)?)|(?:\n(?:<[^>A-Z]+?>)+?))\bRef\b.*?</Notes>))|(?:(?<=[\.:""](?:</?(?:e|s)m>)*)(?:\n<br/>|\n(?=<(?:e|s)m>)|\n(?=\((?:(?:[1-9]|[1-2][0-9])|[a-z])\)))(?=.*?(\n<br/>)))|(?:(?<=(?-s:^.{0,72}$))\n(?=.*?(\n<br/>)))|<br/>(?=.*?())
```
Replace with: `$1$2$3$4`
___
**All Ref. blocks**
`<br/>` protected/inserted: in `.\\n<br/>Aa|A. ?A.`, after short lines, and before Ref.
Search for:
```regex
(?s)((?:\A|(?<=</Notes>|</Section>|<Indexes>.*)).+?(?:\z|(?=^(?:<[^>]+?>)+?\bRef\b)))|(?:(?:^(?:<br/>)?)(?=(?:<[^>A-Z]+?>)?\bRef\b)(?=.*?(<br/>)))|(?:(?<=\.\)?)(?:\n(?:<br/>)?)(?=(?:<[^>A-Z]+?>)*?(?:(?:\p{Lu}\p{Ll})|(?:\p{Lu}\. ?\p{Lu}\.)))(?=.*?(\n<br/>)))|(?:(?<=(?-s:^.{0,70}$))(?:\n(?:<br/>)?)(?=.*?(\n<br/>)))|<br/>(?=.*?())
```
Replace with: `$1$2$3$4$5`
___
##### Dehyphenation

The resulting files with `<br/>` tags in (mostly) right places were then dehyphenated. First, all hyphens in the end of lines were mechanically deleted unless surrounded with punctuation marks, spaces, capital letters, and roman numerals (smaller than d):
Search for:
```regex
(?s)(?:(?<=[\p{Nd}\p{Lu}\p{P}-[-]]|(?:\b[clxvi]+\b))(-)\n(?=.*?()))|(?:(-)\n(?=[\p{Nd}\p{Lu}\p{P}-[-]]|(?:\b[clxvi]+\b))(?=.*?()))|(?:( -)\n(?=.*?( )))|-\n(?=.*?())
```
Replace with: `$1$2$3$4$5$6$7`

Afterwards, each deletion/retention case was visually verified and fixed if needed by comparing hyphenated and dehyphenated version with a diff tool in Visual Studio Code.
___
In the cleaned and dehyphenated XML files, line breaks without `</br>` tag were collapsed, while line breaks with `<br/>` just lost the tag:
Search for:
```regex
(?s)(?:(\n)(?=(?:<[^>]*?[A-Z][^>]*?>)|(?:<br/>)|%%))|(?:(?<=\S-)\n(?=.*?()))|(?:\n(?=.*?( )))|(?:<br/>(?=.*?()))
```
Replace with: `$1$2$3$4`
___
**Misc.**
Additional manual check was made for lines ending without basic punctuation:
```regex
[^.,:;'"\n*\[\]()|?!†]\n(?!\n|\|)
```
___

#### Content adjustments, pre-splitting

Undocumented small modification were done at different stages, e.g. "ctd" lines deleted, etc.
___
For better readability of summaries, (first parts of) short titles were parsed from text and placed immediately after publication numbers (`<Number>` tags).
Search for:
```regex
(?s)(?:(?<=<Number>)(\d.*?)(?=\.?</Number>\n<Full_Title>.*?<em>(?-s:(.+?)|(?:(.+?)\n(.+?)))</em>))
```
Replace with: `$1. $2$3 $4`; unneeded spaces deleted.

Numbers themselves were then preceded with "n.", otherwise disappearing in "on this page" summaries.
___
Page number was added before each author, later converted into a callout with a link to BHL (undocumented).
Search for:
```regex
(?s)(?<=(<Page Number=\"\d+\">).*?)(<Author>)
```
Replace with: `$1$2`
___
Section names were converted to sentence case (boost regex flavor):
Search for: 
```regex
(?-s)(?<=^#### )(.+)$
```
Replace with: `\L\u$1`
___
Tables were retrieved from TXT files and converted into markdown (with undocumented regex), then checked against PDFs, fixed where needed, and placed in the main text manually.
___

#### Conversion of XML into markdown 

Resulting files were converted into markdown using Python Script plugin for Notepad++ with the following code:

```python
# encoding=utf-8
from Npp import editor

## Multiple replacement for conversion of TL-2 xml version into markdown.

editor.rereplace('\*', '\\\*')

editor.rereplace('#', '\\\#')

def replacement(m):  return '\\' + (m.group(1))
editor.rereplace('(\[|\])', replacement)

editor.rereplace('<Author>', '\n\n%%Author%%')

editor.rereplace('</Author>', '%%/Author%%\n')

editor.rereplace('<Name>', '### Author\n\n%%Name%%\n')

editor.rereplace('</Name>', '\n%%/Name%%\n')

# Tag not in XSD scheme
editor.rereplace('<AlternateAbbrev>', '\*\*;Alternate abbreviation:\*\* ')
# Tag not in XSD scheme
editor.rereplace('</AlternateAbbrev>', '\n%%/AlternateAbbrev%%\n')

editor.rereplace('<Sections>', '%%Sections%%')

editor.rereplace('</Sections>', '%%/Sections%%')

editor.rereplace('<Section></Section>', '')

def replacement(m):  return '%%Section%%\n#### ' + (m.group(1)) + '\n\n'
editor.rereplace('<Section>(.*?): ?', replacement)

editor.rereplace('</?Section>', '\n%%/Section%%\n')

editor.rereplace('<Titles>', '### Publications\n')

editor.rereplace('</Titles>', '%%/Titles%%')

def replacement(m):  return '%%' + (m.group(1)) + '%%'
editor.rereplace('<(/?Title)>', replacement)

editor.rereplace('<Number>', '##### n\.')

editor.rereplace('</Number>', '')

def replacement(m):  return 'Title>\n\n**' + (m.group(1)) + '**\n'
editor.rereplace('Title>\n<(Notes)>', replacement)

editor.rereplace('<Full_Title>', '\n\*\*Title\*\*\n%%FullTitle%%\n')

editor.rereplace('</Full_Title>', '\n%%/FullTitle%%')

editor.rereplace('<Short_Title>', '\*\*Abbreviated title\*\*\: ')

editor.rereplace('</Short_Title>', '\n%%/ShortTitle%%')

editor.rereplace('<Short_Title>', '%%ShortTitle%%\n')

editor.rereplace('</Notes>', '\n%%/Notes%%')

editor.rereplace('</?sm>', '')

editor.rereplace('<br/>', '')

editor.rereplace('&amp;', '&')

editor.rereplace('~', '\\\~')

editor.rereplace('_', '\\\_')

editor.rereplace('\t', '')

# Convert space-padded hyphens into en-dash:
editor.rereplace(' - ', ' – ')

editor.rereplace('</?em>', '\*')

editor.rereplace('XYZZY', '==Name in the standard form is not specified==')

editor.rereplace('<Table/>', '\n\> \[!warning\] This is a \(temporary\) placeholder for a table\.\n');
```

All `%%`-style Obsidian comments produced by this script were later deleted because their rendering load in Obsidian was too high.
___
#### File splitting

With shell scripts, volume-sized markdown files were split into author-pages which were then renamed on a basis of a tab-delimited dictionary file (TBA), in turn derived from TL-2 Author Data table (see it [here](https://www.sil.si.edu/DigitalCollections/tl-2/downloads.cfm) ).

```sh
# Split files at [!cite] callouts that precede each author

for input in *.md;
do
	csplit --prefix="${input%.*}_" --elide-empty-files --suffix-format=%04d.md "${input}" "/> \[!cite\]/" {*}
done

# Rename files based on a tab-delimited dictionary file

rename_input="<TSV_RENAMING_DICTIONARY_FILE>"

sed 's/^/mv -vi "/;s/\t/" "/;s/$/";/' < "${rename_input}" | bash -
```
___
#### Content adjustments, post-splitting

##### Links between Volume I and supplements

Authors in the Vol. I often have additional pages in Supplements. Cross-links between such pairs of pages were made with `R` code below, kindly provided by Vladimir Mikryukov:

```R
library(data.table)
library(plyr)

## Opereation mode
MODE <- "copy"       # copy modified file into './mod'
# MODE <- "inplace"  # add 2nd line inplace

## Get the table with filenames
fls <- fread (input = "./tl2-authors_for_suppl-main_entries_linking.tsv")

## Remove singletons (for them, no changes are required)
non_singlets <- fls[ , .(N = .N), by = "author_name_abbrev" ][ N > 1]$author_name_abbrev
fls <-
  fls[author_name_abbrev %in% non_singlets, .(after_split_name,
                                               page_title_short,
                                               page_title_long,
                                               page_title_longer_max80,
                                               author_name_abbrev)]

## Find out which string should be written to the second line
fls <- split(fls, f = fls$author_name_abbrev)

fls <- llply(.data = fls, .fun = function(x){
  ## If there are only a pair of files  
  if(nrow(x) == 2){ x[ , ExtraFile := rev(page_title_longer_max80) ] }
  ## TO DO - If there are multiple files
  # ... paste + exclude self-match
})

fls <- rbindlist(fls)

## Prepare full string
s1 <- "> [!example] See also [["
s2 <- gsub(pattern = "\\.md", replacement = "", x = fls$ExtraFile)
s3 <- ifelse(grepl("(Suppl.)", fls$page_title_short, fixed = TRUE) == TRUE, "|first", "|second")
s4 <- " entry]] for this author"

fls <- fls[ , StringToAdd := paste0(s1, s2, s3, s4) ]

## Prepare bash command to execute
if(MODE %in% "inplace") {
  # sed -i '1 a \\\\nStringToAdd' filename.md
  
  cmd1 <- "sed -i '1 a "
  newline <- "\\\\n"
  cmd2 <- "' '"
  filetype <- ".md"
  cmd3 <- "'"
  fls[, SED := paste0(cmd1,
                      newline,
                      StringToAdd,
                      cmd2,
                      page_title_longer_max80,
                      filetype,
                      cmd3)]
}

if(MODE %in% "copy") {
  # sed '1 a \\\\nStringToAdd' filename.md > mod/filename.md
  
  cmd1 <- "sed '1 a "
  newline <- "\\\\n"
  cmd2 <- "' '"
  cmd3 <- "' > mod/'"
  filetype <- ".md"
  cmd4 <- "'"
  fls[, SED := paste0(
    cmd1,
    newline,
    StringToAdd,
    cmd2,
    page_title_longer_max80,
    filetype,
    cmd3,
    page_title_longer_max80,
    filetype,
    cmd4
  )]
}


## Execute command from R (in bash)
# dir.create(path = "mod", showWarnings = F)
# system2(command = fls$SED[1])  # E.g., for the first file

## Export commands
fwrite(x = fls[, .(SED)], file = "cmd.txt",
       quote = F, row.names = F, col.names = F, eol = "\n")
```

Resulting `cmd.txt` was executed from the folder with renamed files with the shell commands:

```sh
mkdir -p mod
parallel -j1 -a cmd.txt "{}"
```
___
##### Herbarium markup

Herbaria (aka "collections") were first marked up in the respective sections as links pointing to non-existing collection pages:
Search for:
```regex
(?s)((?:(?:\A|(?<=^###)).+?(?:\z|(?:#### Herbarium.*?\n))|(?:\n\*Ref\*.+?\n###))|(?:\bIH\b|\bICBN\b|\bWW\b|\bI+\b|TL-(?:1|2)|(?:\n|\. | [––] )\bA |\bIDC\b|\bUSSR\b)|(?-s:(?<!\n|\bat |\bin |\b[A-Z]+(?:, | and ))\b[A-Z]\..))|(?<!\.|\b[Ff]ide |\b[Ss]ee |-)(\b[A-Z]+\b)(?!')
```
Replace with: `$1[[Collection $2|$2]]`

Resulting artifacts were removed, and all affected collection codes were verified visually and fixed where needed. 

List of collections was pulled from [Index Herbariorum (IH) API](https://github.com/nybgvh/IH-API) with the [request](http://sweetgum.nybg.org/science/api/v1/institutions) provided in their wiki. Resulting JSON file was converted to tab-separated table (TSV) in Python with [pandas](https://pandas.pydata.org/) library as follows:

```python
import json

import pandas as pd

data = json.load(open('<PATH_TO_INPUT_FILE>'))

df = pd.DataFrame(data["data"])

df.to_csv('<PATH_TO_OUTPUT_FILE>', sep="\t", encoding='utf-8', index=False)
```

Lists of herbaria from IH present in TL-2 and of those missing a page in IH were prepared with SQLite queries. Herbarium codes not occurring in TL-2 were filtered out from TSV, which was then half-automatically converted to a two-column long format. Example:

```
irn	126836
organization	University of Tartu
code	TU
division	Natural History Museum and Botanical Garden
```

This file was converted into markdown with the Python script executed in Notepad++:

```python
# encoding=utf-8
from Npp import editor

## Multiple replacement for conversion of Index Herbariorum long table into markdown collection pages.

# Character protection for markdown
editor.rereplace('\*', '\\\*')
editor.rereplace('#', '\\\#')
editor.rereplace('~', '\\\~')
editor.rereplace('_', '\\\_')

# Convert irn into IH link and put it after code.
def replacement(m): return (m.group(2)) + '[' + (m.group(3)) + '](https://sweetgum.nybg.org/science/ih/herbarium-details/?irn=' + (m.group(1)) + ')'
editor.rereplace('(?s)^irn\t(\d+)\r\n(.+?\r\ncode\t)(.+?)$', replacement) 

# Rearrange main fields in a better order
def replacement(m): return (m.group(2)) + (m.group(1)) + (m.group(3)) + (m.group(4)) + (m.group(6)) + (m.group(5))
editor.rereplace('(^organization.+\r\n)(^code.+\r\n)(^division.+\r\n)(^department.+\r\n)((?s:.+?))(^address.+\r\n)', replacement) 

# Get simplified locality data
def replacement(m): return (m.group(1)) + (m.group(2)) + ', ' + (m.group(3)) + ', ' + (m.group(4))  
editor.rereplace('^(address\t).+(?:\'|\"\")physicalCity(?:\'|\"\"): (?:\'|\"\")(.*?)(?:\'|\"\"), (?:\'|\"\")physicalState(?:\'|\"\"): (?:\'|\"\")(.*?)(?:\'|\"\"),.+?(?:\'|\"\")physicalCountry(?:\'|\"\"): (?:\'|\"\")(.*?)(?:\'|\"\"),.+?$', replacement) 

# remove artifact in empty physicalState
editor.rereplace('(?:, )+', ', ')

# Remove csv string delimiters (")
editor.rereplace('\t\"', '\t')
editor.rereplace('\"$', '')

# Remove peripheral JSON list markup
editor.rereplace('\t\[(?:\'|\"\")', '\t')
editor.rereplace('(?:\'|\"\")\]$', '')
editor.rereplace('\t\[\]$', '\t')

# Remove internal JSON list markup 
# potentially problematic!
editor.rereplace('(?:\'|\"\"), (?:\'|\"\")', ', ')

# Protect the rest of [ and ], except in the link after code 
def replacement(m):  return '\\' + (m.group(1))
editor.rereplace('(?<!^code\t)(\[|\])(?!\()', replacement)

# Fix multi-" 
editor.rereplace('\"+', '\"')

# Remove empty fields or those contining "0"
editor.rereplace('^.+?\t0?\r\n', '')

# Remove not needed fields
editor.rereplace('^(?:cites|contact|location|collectionsSummary)\t.*?\r\n', '')

# Make **bold** field names
editor.rereplace('^(?!\n)', '\*\*')
editor.rereplace('\t', '\*\*:\t')

# Replace tabs with spaces
editor.rereplace('\t', ' ')

# Insert closing callout
def replacement(m): return (m.group(1)) + '\n\n> [!cite] This page is automatically generated\n• Data on this page were obtained from [Index Herbariorum](https://sweetgum.nybg.org/science/ih/) on 2023-08-23.\n• Meaning of acronyms in TL-2 is not always the same as in IH. Consult list of original [[Abbreviations|abbreviations]] if you suspect acronym mismatch.\n'  
editor.rereplace('^(\*\*dateModified.+?$)', replacement) 

# Rename fields, prettify
editor.rereplace('\*\*code', '\*\*Code')
editor.rereplace('\*\*address', '\*\*Location')
editor.rereplace('\*\*organization', '\*\*Organization')
editor.rereplace('\*\*division', '\*\*Division')
editor.rereplace('\*\*department', '\*\*Department')
editor.rereplace('\*\*specimenTotal', '\*\*Total number of specimens')
editor.rereplace('\*\*currentStatus', '\*\*Current status')
editor.rereplace('\*\*dateFounded', '\*\*Date founded')
editor.rereplace('\*\*taxonomicCoverage', '\*\*Taxonomic coverage')
editor.rereplace('\*\*geography', '\*\*Geographic coverage')
editor.rereplace('\*\*incorporatedHerbaria', '\*\*Incorporated herbaria')
editor.rereplace('\*\*importantCollectors', '\*\*Important collectors')
editor.rereplace('\*\*notes', '\*\*Notes')
editor.rereplace('\*\*dateModified', '\*\*Date modified in Index Herbariorum')

# Line breaks conversion
editor.rereplace('<br/>', '\n')
editor.rereplace('\r\n', '\n')

# Pad with empty lines
editor.rereplace('^\*\*(?!Code\*\*)', '\n\*\*')
```

A two-column dictionary table for renaming split files (see next step) was prepared from the filtered and elongated TSV as follows:

```python
# encoding=utf-8
from Npp import editor

## Preparation of a renaming table for after-split collection pages.

editor.rereplace('\r\n', '\n')

# Delete everything except code lines
editor.rereplace('(?!^code\t)^.*?(?:\n|\Z)', '')
editor.rereplace('\n\Z', '')

# Create output file names and provide extensions
editor.rereplace('\t', '\.md\tCollection ')
editor.rereplace('$', '\.md')

# Inser sequential numbers to finalize input file names
def custom_replace_func(m):
    new_text = "Collection_{}".format(custom_replace_func.counter)
    custom_replace_func.counter += custom_replace_func.increment
    return new_text

custom_replace_func.counter = 0  # starting value
custom_replace_func.increment = 1
editor.rereplace('^code', custom_replace_func)
```

Example of two-column tab-separated output:

```
Collection_0.md	Collection MVM.md
Collection_1.md	Collection BAK.md
Collection_2.md	Collection BHMG.md
```

The aggregate markdown herbaria page was split and renamed with the following shell script:

```sh
# Split files at [!warning\] callouts that precede each herbarium

split_input="<MD_FILE>"

csplit --prefix="Collection_" --elide-empty-files --suffix-format=%01d.md "${split_input}" "/> \[!warning\]/" {*}

# Rename files based on a tab-delimited dictionary file

rename_input="<TSV_RENAMING_DICTIONARY_FILE>"

sed 's/^/mv -vi "/;s/\t/" "/;s/$/";/' < "${rename_input}" | bash -

```

Collection codes recognized as such, but without a match in current online Index Herbariorum were checked manually, first for OCR errors, and then for typographic errors against printed versions of "Index Herbariorum. Part II. Collectors" [at BHL](https://www.biodiversitylibrary.org/bibliography/200637). For summary of this work see [[List of collections]] and extended list with more annotated errors [at GitHub](https://github.com/sav-che/TL-2/tree/main/Maintenance/raw_files) (`tl2-collections`).
___

##### Misc.

A single figure present in author records was downloaded from BHL and placed in the main text manually.
___
Links to supporting pages were added to page-opening callouts.
___
Text missing from Smithsonian's version was recovered (Vol. I, pp. 869-897)  
___
Fused authors were separated (`<Name>` tag issues? Vol. III, p. 742; Vol. V, p. 27, 64).
___

### TODO

- Fix publication titles in headers.
- Mark up lists.
- Establish internal links to authors and publications ("See TL-2/x ...")
- Establish author links to IPNI, resolve non-standard standard abbreviations.
- Find taxa names and establish links from them.

![[TL-2 logo alt transparent.png|30]]