# getpapers
## basic queries
**************************************
getpapers -q 'TITLE:"cell migration”' -o cellmig -n -> 1867
wc -l cellmig/eupmc_fulltext_html_urls.txt
head cellmig/eupmc_fulltext_html_urls.txt
head cellmig/eupmc_results.json
**************************************
This returns a list of metadata and urls.


getpapers -q 'TITLE:"cell migration”' -o cellmig -p
getpapers -q 'TITLE:"cell migration”' -o cellmig -x
getpapers -q 'TITLE:"cell migration”' -o cellmig -s
***************************************************
**Comments**:
THE -p FLAG DOWNLOADS PDF, WHERE AVAILABLE.
THE -x FLAG DOWNLOADS XML-RESULTS.
THE -s FLAG DOWNLOADS SUPPLEMENTARY INFORMATION, WHERE AVAILABLE.
***************************************************
To visualize the structure of the resulting CProject

*******************************************
## more advanced queries

getpapers -q '(LICENSE:"cc by" OR LICENSE:"cc-by") AND TITLE:"cell migration"' -o cellmig_license -n

getpapers -q '(PUB_YEAR:[2000 TO 2016]) AND TITLE:"cell migration"' -o cell_mig -n -> 1742
getpapers -q '(PUB_YEAR:[1995 TO 2000]) AND TITLE:"cell migration"' -o cell_mig -n -> 66

getpapers -q '(PUB_YEAR:[2000 TO 2016]) AND METHODS:"ORIS" AND TITLE:"cell migration"' -o cell_mig -n -> 17

getpapers -q '(PUB_YEAR:[2000 TO 2016]) AND INTRO:"wound healing" AND TITLE:"cell migration"' -o cell_mig -n -> 198
getpapers -q '(PUB_YEAR:[2000 TO 2016]) AND INTRO:"cancer invasion" AND TITLE:"cell migration"' -o cell_mig -n -> 124


getpapers -q '(PUB_YEAR:[2000 TO 2016]) AND INTRO:"cancer invasion" AND RESULTS:"in vitro" AND TITLE:"cell migration"' -o cell_mig -n -> 62

**For now, I decided to work with the following query:**
getpapers -q '(PUB_YEAR:[2000 TO 2016]) AND RESULTS:"in vitro" AND TITLE:"cell migration"' -o cell_mig -n
677 papers are retrieved.
*****************************************************************************************************************
getpapers -q '(PUB_YEAR:[2000 TO 2016]) AND RESULTS:"in vitro" AND TITLE:"cell migration"' -o cell_mig -x
	searching using eupmc API (by default)
	download XML results

FULL EUPMC result metadata saved to eupmc_fulltext_html_urls.txt
PMC4948956 and PMC3300687 were not OA (671 downloaded out of 673 requested)


├── PMC4981866
│   ├── eupmc_result.json
│   └── fulltext.xml


**Use norma to transform the full text xml files into s.html (scholarly-html):**
norma --project cell_mig -i fulltext.xml -o scholarly.html --transform nlm2html
	!UNKNOWN: page-range
	!UNKNOWN: prefix
	!UNKNOWN: alt-text: Supplemental Table S1
	!UNKNOWN: alt-text: Supplemental Table S2

tree cell_mig
└── PMC524493
    ├── eupmc_result.json
    ├── fulltext.xml
    └── scholarly.html

**Use the ami2-word plugin to extract word frequencies from the downloaded texts:**
ami2-word
ami2-word --project cell_mig/ -i scholarly.html --w.words wordFrequencies

[main] WARN  org.xmlcml.ami2.plugins.word.WordCollectionFactory  - No scholarlyHtml or PDFTXT → no words found to extract (there is indeed no scholarlyHtml, so the normalization did not work out – why? Because there was no full text available!? – strange, because I did not find 2 OA full texts, but this step fails for more than 2 folders, namely for 12).

cell_mig/PMC2064507
cell_mig/PMC2150733
cell_mig/PMC2150849
cell_mig/PMC2175152
cell_mig/PMC2175259
cell_mig/PMC2190593
cell_mig/PMC2193325
cell_mig/PMC2196419
cell_mig/PMC2199198
cell_mig/PMC2199208
cell_mig/PMC3300687
cell_mig/PMC4948956

Some stopwords files are available at: https://github.com/ContentMine/ami/tree/master/src/main/resources/org/xmlcml/ami2/plugins/word/stop-words-collection-2014-02-24/stop-words

I created my own stopword file ("stopwords_eng.txt")
ami2-word --project cell_mig --w.words wordFrequencies --w.stopwords stopwords_eng.txt

Here something seems to go wrong... the stopword stream cannot be parsed:
0    [main] DEBUG org.xmlcml.ami2.wordutil.WordSetWrapper  - symbol expands to: /org/xmlcml/ami2/wordutil/stopwords_eng.txt
2    [main] WARN  org.xmlcml.ami2.wordutil.WordSetWrapper  - Cannot read stopword stream: /org/xmlcml/ami2/wordutil/stopwords_eng.txt
