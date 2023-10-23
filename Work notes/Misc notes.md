### Blocks to recognize and interpret

#### Specific blocks
```xml
Structure:
OPENING_TAG_EXAMPLE
MEANING

<Volume Number="5" supplemental_volume="no">
Obvs. Incorporate into pages as links to BHL?

<Pages>
Binds all authors, sep from indexes

<Page Number="1">
Page N, only occurs before each <Authors>!

<Authors>
Binds 1 author. =<Author>?

<Author>
Binds 1 author. =<Authors>?

<Name>
Name and short bio. filename?

(1839-1905)
Life years. Parseable?

(<em>Saldanha</em>).</Name>
Standard name, linkable?

<Sections>
Binds HERB., BIBL., BIOF., EPON. 

<Section><sm>HERBARIUM</sm> and <sm>TYPES</sm>:
Obvs. Freetext with herb acronyms. Parseable?

<Section><sm>NOTE</sm>
Obvs.

<Section><sm>BIBLIOGRAPHY</sm> and <sm>BIOGRAPHY</sm>:
Obvs. Includes also standard short refs! Parseable?

<Section><sm>BIOFILE</sm>:
Bio references. Parseable? Not in all volumes.

<Section><sm>COMPOSITE WORKS</sm>
?

<Section><sm>HANDWRITING</sm>
Obvs

<Section><sm>EPONYMY</sm>:
Freetext with eponymies. Parseable.

<Titles>
Binds publications

<Title>
A single publication

<Number>
TL-2 pub ID

<Full_Title>
Obvs.

<Short_Title>
Not always a short title, can be random info. Drop?

<Notes>
Obvs. Not in all volumes.

<em>Publ</em>.:
Part of <Notes>


<em>Copies</em>: OR <em>Copy</em>:
Part of <Notes>


<em>Plates</em>:
Part of <Notes>


<em>Ref</em>.
Part of <Notes>


<Indexes>
Binds all indexes


<Index Type="Index_To_Titles">
Binds particular index


<Entries>
Binds particular index; = <Index ___>


<Entry>
Obvs
```

### Roadmap

#### Basic preparations

- [x] Replace \r\n with \n for easier regex101 testing. 
- [x] Fix broken Sections.
	- [x] Fix Name closing tags after Herb & Types. 
- [x] Fix typos in sections.
- [x] Delete technical ctd. (continued) lines.

#### Additional data recovery

- [ ] Tables
	- [x] Get tables from txt files or OCR them.
	- [ ] Insert tables in main text.
	- [x] Mind header-less tables in XML, some tabbed. 
- [x] Get images (look for Illustration annot. in BHL pages).
- [ ] Incorporate additions (e.g. see before Indexes in V5).

#### Line breaks (LBs)

- [ ] Distinguish soft and hard line breaks, fix the first ones.
	- [x] #name delete br/no-br LBs.
	- [x] #sections different approaches:
		- [x] #BIB-BIO delete br/no-br LBs **except in V1-3**.
		- [x] #BIB-BIO sort out in **V1-3**:			
			- [x] Detect first paragraphs with abbrev. refs. and delete LBs there (length-dependent?).
			- [x] Trust br in the rest - **no replacements needed**.
			- [ ] Postfix exceptions (subsections) by looking for `<br/><em>` in xmls.
		- [x] #BIOFILE trust br/no-br - **no replacements needed**.
			- [x] But check when #BIOFILE (or any other section) continues **from indent** on the next page - br can be broken here.
		- [x] #COMP-WORKS delete br/no-br LBs except:
			- [x] Trust br in lists.
				- [x] Common lists are formatted like `(1)` and `(a)`.
				- [ ] Manually find and protect/fix false positives.			
		- [x] #HERB-TYPES delete br/no-br LBs except:
			- [x] Trust br in lists.
			- [x] Trust br/no-br in `Ref.:` in #HERB-TYPES except too short lines.
		- [x] #HANDWRITING delete br/no-br LBs
			- [ ] Postfix exceptions, a few lists.
		- [x] #NOTE delete br/no-br LBs except:
			- [ ] Numerous exceptions!
			- [x] Common lists formatted like `(1)` and `(a)`.
		- [x] Rest of #sections : delete br/no-br except after very short lines and maybe lists.
		- [x] #Full_Title , #Short_Title delete br/no-br LBs.
			- [ ] Check long entries (>3 lines?) for paragraph LBs.
		- [x] #Title_Note trust br/no-br **except**:
			- [ ] Multiple list formats.
			- [ ] Subsections: `<em>Publ</em>.:`, `<em>Ref</em>.`, `<em>Cop(?:ies|y)</em>` , `<em>Note</em>`, `<em>Authorship</em>`
			- [x] Inside `<em>Ref</em>.`
		- [ ] Protect or postfix LBs before `<sm>`NOTE etc.
- [ ] De-hyphenate soft line breaks, at least in #name and #Full_Title .

#### Rearrangements and linking

- [x] Put short publication name after every publication ID
	- [ ] Prepare indexes for header enrichment (delete shorter duplicates).
	- [ ] Find-replace with library method.
- [x] #linking Put page info in front of every author.
	- [x] Enrich page tags with volume info.
- [ ] #linking Make entries and links for herbaria.
- [ ] #linking Make entries and links for years?
- [ ] #linking Make entries and links for major lit. sources?
- [ ] #linking Consider internal links, e.g. "see TL-2/4: 978-979"
- [x] Separate by author.
- [ ] Fuse author entries from main and supplementary volumes?
- [x] Converse XML into markdown.
	- [x] Tag purge/transformation into md formatting.

#### Post-processing

- [ ] Delete multi-spaces.
- [x] Fix `(<em>XYZZY.</em>)` in places of missing standard author names.
- [x] Fix 66666... in V4.
- [ ] Fix unusual sections (lookup xmls).

### Exceptions

#### Large entries with multiple wrong LBs
- Thunberg, Carl Peter
- Linnaeus
- Weigel, Johann Adam Valentin

#### Sections
- `<sm>BOTANISCHE</sm> <sm>WANDTAFELN</sm>` under "Peter, [Gustav] Albert" - should be a title, but lacks a number. 
- `CONTEXT AND BASES OF THE BESSEYAN DICTA` and `CHARLES E. BESSEY PAPERS` under Bessey, Charles Edwin are potentially titles.
- `Note` under "Meurer, Pfarrer" and "Thurn, Everard Ferdinand Im".
- 2 tabbed tables in XML:
```
  C:\Users\cereb\Desktop\TL-2\tl2-data-v1.2\010 sections, typos fixed\TL_2_Vol_1.xml (1 hit)
	Line 4565: <br/>-----------------------------------------------------------------------
  C:\Users\cereb\Desktop\TL-2\tl2-data-v1.2\010 sections, typos fixed\TL_2_Vol_6.xml (1 hit)
	Line 54617: <br/>--------------------------------------------------------------------------
```

### Misc notes

- #sections The most common sections are, with variations in order and exact names: 
	- HERBARIUM and TYPES aka #HERB-TYPES	
	- BIBLIOGRAPHY and BIOGRAPHY (interleaved in V1-3, LBs poorly tagged; after V3 - wrapped block of text.) aka #BIB-BIO
	- BIOFILE (appears after V3, assumes interleaved structure of B&B above, decently tagged.) aka #BIOFILE
	- HANDWRITING aka #HANDWRITING 
	- COMPOSITE WORKS aka #COMP-WORKS 
	- EPONYMY aka #EPONYMY
	- NOTE aka #NOTE
- #name Unknown parts of names denoted by ...
- #name Inferred parts of names denoted by [] (sometimes orth. alt., sometimes dropped parts) or, more rarely ()
- #name Honorable titles denoted by []
- #dates Unknown life years denoted by -x or x- or -? or [unknown] or "or later" or "(0000-0000)".
- #dates Uncertain years denoted by / or ± or ?: 1891/92 , ± 1700-1769 , 1839?
- #dates fl (often in italics) with years means flourished: (<em>fl</em>. 1849)
- #dates #names #exceptions "(1735, 1736 (baptized) - c. 1803)" , "Fries family. (0000-0000)" , "Gmelin family. (0000-0000)"
- #dates Find dates, 9939 hits, few mismatches: `(?s)<Name>.+?\(.+?\)`. With comma after fetches 9915: `(?s)<Name>.+?(\(.+?\),)` .
- #std_name Unknown standard names are spelled like this: `(<em>XYZZY.</em>)`
- #titles Short Title sensu xml = abbreviated title sensu TL-2. Short title sensu TL-2 is italicized part in Full_Title. 
- #titles Mind sub-headers in Titles, e.g. `<br/><sm>REPRINTS</sm> and <sm>PREPRINTS</sm>:`.
-  #titles #LB Line breaks in Titles (Notes only?): 
```
- Preserve LB before ^<br/>, <em>, <sm>, specifically in [\.:\)"](?:<.?\wm>)*\r\n(?:<br/>|<\wm>)		Few or none exceptions? Can overfetch because of " 
- Preserve LB before ^<br/>, <em>, <sm>, specifically in [\.:\)")](?:<.?\wm>)?\r\n<br/><\wm>		Currently no exceptions. Not needed?
- Preserve LB before ^<em>, specifically in [\.:\)"](?:<.?\wm>)*\r\n<em>		Slight overfetch
- Delete LB without br.		Except when they precede list entries, section tags
```

- #LB Find markers of paragraph end: `.` `".` `.)` `.]` : what else?
- #LB Make an exception, do not paste space instead of br if the line ends with dash `.-$` but not `. -$`. Note: `. - $` does not exist.
- #LB Preserve LB if the first line ends with stop or colon (semicolon?) and he second starts with uppercase letter?
- #LB Do not delete line break if the next line starts with italics or list number, e.g. (1). Like this?
`\.\r\n<br/><em>` and `<br/>\([a-z0-9]{1,2}\)`. Check if `<br/>\([a-z0-9]{1,3}\)` exists in TL-2 - it doesn't.
- #LB Ref before `</Title>` should, but not always italicized; also `\.\r\n` within it should be (partially?) protected.
- #LB Identify lists that follow dot/colon+LB?
Variants: `(1) `, `(22) `, `(3a) `, `(a) `, `(bb) `, `[<em>2</em>]`
In #COMP-WORKS: `(1) `, `(a) `, `[<em>2</em>]` (rare), `Ad (b)` (rare), `2. `.

```
(?-i)<br/>\([0-9]{1,2}\)   Ok

(?-i)<br/>[0-9]{1,2}\.		Bad overfetch; use only in title notes / composite works?

(?-i)<br/>\([a-z]\)		Slight overfetch

Also not br'ed: 
.(\r\n\([0-9]{1,2}\)) - ok
.\r\n[0-9]{1,2} - desperate, see list in Linnaeus 4829

```
- Postfix `^\d\D` lists in non-Titles. 
- #LB There seems to be no way to distinguish paragraps and regular line breaks, except the first can be shorter. E.g. see "HERBARIUM and TYPES" in Linnaeus. Shortest not broken is ~74-79, depending on section.
- #page txt files, unlike xml, have all page numbers.
- #LB After de-br'ing look for "-and" and fix for likes of "high- and low-tech".
- Postfix orphan lines, something like `(?-s)<br/>.{1,10}`
- Some subsection in title-notes: `(?s)(\n(?:<[^>A-Z]+?>)*?(?:\bRef\b|\bCop(?:ies|y)\b|\bPubl\b|\bNote\b|\bEd\b|\bReprint\b|\bRe-issue\b|\bSupplement\b|\bOrig\b|\bOrig\b|\bAtlas\b|\bText\b))`


