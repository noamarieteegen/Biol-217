# what i learned today

ssh -X sunam234@caucluster.rz.uni-kiel.de 

#Day9

## 1. Lernziele heute:

**`Read pre-processing and assembly (either virus-specific assembly or standard metagenomic assembly)`**
**`Viral identification`**
**`Virus-specific quality control`**
**`Viral clustering`**
**`Viral binning`**
**`Abundance estimation`**
**`Virus-specific annotation`**
**`Host prediction`**

**`Viral taxonomy prediction`**
**`Viral Lifecycle prediction`**
**`Virus-specific metabolic analyses`**

**`MVP`** = Modular Viromics Pipeline
**`iPHoP`** = integrated Phage-Host Prediction


## 2. Virus identification and quality control

### 2a. How many free viruses are in the BGR_140717 sample?

`cd` $WORK/viromics
- erst in das richtige Verzeichnis wechseln
`grep` ">" -c 01_GENOMAD/BGR_140717/BGR_140717_Viruses_Genomad_Output/BGR_140717_modified_summary/BGR_140717_modified_virus.fna
- gebe Anzahl aus

**846**


### 2b. How many proviruses are in the BGR_140717 sample?

`grep` ">" -c 01_GENOMAD/BGR_140717/BGR_140717_Proviruses_Genomad_Output/proviruses_summary/proviruses_virus.fna
- gebe Anzahl aus der .fna Datei aus

**11**


### 2c. How many Caudoviricetes viruses are in all samples together? (Use the filtered version)

`grep` -c "Caudoviricetes" 02_CHECK_V/BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv

**10764**


### 2d. How many Unclassified viruses are in all samples together? (Use the filtered version)

`grep` -c "Unclassified" 02_CHECK_V//BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv

**145**


### 2e. What other taxonomies are there across all files? Below, paste a table with (at least) taxonomy, Genome type and Host type.

- -v = invert-match, Invert the sense of matching, to select non-matching lines.

`grep` -c "Unclassified" 02_CHECK_V//BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -v "Unclassified" | grep -v "Sample"

MVP_02_BGR_130305_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_130305	BGR_130305_NODE_2265_length_4061_cov_5.941338	4061	No	3	1	0	Low-quality	Genome-fragment	2.89	AAI-based (medium-confidence)	1.0		11	0.9643	04.4395	**Viruses;Varidnaviria;Bamfordvirae;Nucleocytoviricota;Megaviricetes;Imitervirales;Mimiviridae	dsDNA	Eukaryote**

MVP_02_BGR_130527_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_130527	BGR_130527_NODE_3972_length_2853_cov_3.214796	2853	No	3	1	0	Low-quality	Genome-fragment	4.0	HMM-based (lower-bound)	1.0		11	0.8411	0	3.4351	**Viruses;Varidnaviria;Bamfordvirae;Nucleocytoviricota;Megaviricetes;Algavirales;Phycodnaviridae	dsDNA	Eukaryote**

MVP_02_BGR_130708_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_130708	BGR_130708_NODE_5125_length_2609_cov_6.036805	2609	No	3	1	0	Low-quality	Genome-fragment	3.65	HMM-based (lower-bound)	1.0		11	0.7483	0	3.4351	**Viruses;Varidnaviria;Bamfordvirae;Nucleocytoviricota;Megaviricetes;Algavirales;Phycodnaviridae	dsDNA	Eukaryote**

MVP_02_BGR_131021_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_131021	BGR_131021_NODE_3917_length_3744_cov_3.725400	3744	No	4	1	0	Low-quality	Genome-fragment	1.08	AAI-based (medium-confidence)	1.0		11	0.9654	12.6771	**Viruses	Unknown	Unknown**

/MVP_02_BGR_131021_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_131021	BGR_131021_k141_151878_length_2551_cov_25.4033	2551	No	5	1	0	Low-quality	Genome-fragment	6.07	HMM-based (lower-bound)	1.0		11	0.9284	0	1.7183	**Viruses;Varidnaviria;Bamfordvirae;Preplasmiviricota;Tectiliviricetes;Autolykiviridae	dsDNA	Prokaryote**

MVP_02_BGR_131021_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_131021	BGR_131021_NODE_12517_length_1678_cov_37.389402	1678	No	3	1	0	Low-quality	Genome-fragment	16.44	HMM-based (lower-bound)	1.0		11	0.9323	1	1.7183	**Viruses;Riboviria;Pararnavirae;Artverviricota;Revtraviricetes;Ortervirales;Retroviridae	ssRNA-RT	Eukaryote**

MVP_02_BGR_140221_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_140221	BGR_140221_NODE_2074_length_4089_cov_7.783837_1_1_3696	3696	Yes	4	1	0	Low-quality	Genome-fragment	3.02	AAI-based (medium-confidence)	1.0	1-3696	11	0.9072	0	1.7183	**Viruses;Varidnaviria;Bamfordvirae;Nucleocytoviricota;Megaviricetes;Algavirales;Phycodnaviridae	dsDNA	Eukaryote**

MVP_02_BGR_140605_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_140605	BGR_140605_NODE_5969_length_1911_cov_2.155172	1911	No	5	3	0	Low-quality	Genome-fragment	39.82	AAI-based (medium-confidence)	1.0		11	0.97	14.5511	**Viruses;Monodnaviria;Sangervirae;Phixviricota;Malgrandaviricetes;Petitvirales;Microviridae	ssDNA	Prokaryote**

MVP_02_BGR_140919_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_140919	BGR_140919_NODE_1812_length_5087_cov_5.712838	5087	No	10	1	1	Low-quality	Genome-fragment	0.41	HMM-based (lower-bound)	1.0		11	0.9463	0	4.0154	**Viruses	Unknown	Unknown**

MVP_02_BGR_140919_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:BGR_140919	BGR_140919_k141_2996_length_1161_cov_5.0000	1161	No	2	1	0	Low-quality	Genome-fragment	17.74	HMM-based (lower-bound)	1.0		11	0.959	1	1.5528	**Viruses;Monodnaviria;Sangervirae;Phixviricota;Malgrandaviricetes;Petitvirales;Microviridae	ssDNA	Prokaryote**


### 2f. How many High-quality and Complete viruses are in all samples together? (Use the filtered version and focus on the CheckV quality)

- nach verwendung von cut wird -f geschrieben um to cut a colum from a file 
- grep -e wird fuer jeden Keyword einzeln verwendet und wc -l fuehrt dann die Aufzaehlung durch

`cut -f` 8 02_CHECK_V/BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | `grep -e` "High-quality" `-e` "Complete" | `wc -l` 

**5**


### 2g. Create a table based on the CheckV quality with the columns Sample, Low-quality, Medium-quality, High-quality, Complete. Fill it with the amount of viruses for each of the categories. Tip: If you are tight on time, just save the output and make it pretty later!

`for x in` BGR_130305  BGR_130527  BGR_130708  BGR_130829  BGR_130925  BGR_131021  BGR_131118  BGR_140106  BGR_140121  BGR_140221  BGR_140320  BGR_140423  BGR_140605  BGR_140717  BGR_140821  BGR_140919  BGR_141022  BGR_150108; do echo "| $x | "; cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "Low-quality" ; cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "Medium-quality"; cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "High-quality";  cut -f 8 02_CHECK_V/"$x"/MVP_02_"$x"_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv | grep -c "Complete"; `done`

|BGR_130305|BGR_130527|BGR_130708|BGR_130829|BGR_130925|BGR_131021|BGR_121118|BGR_140106|BGR_140121|BGR_140221|BGR_140320|BGR_140423|BGR_140605|BGR_140717|BGR_140821|BGR_140919|BGR_141022|BGR_150108|
|   625    |   693    |   860    |   964    |   856    |   1091   |   613    |   372    |   578    |    724   |   471    |   362    |   531    |   564    |   338    |   485    |    409   |   348    | 
|    0     |    1     |    1     |    3     |    3     |    3     |    3     |    0     |    0     |     1    |    1     |    0     |    2     |    3     |    1     |    1     |     0    |    1     |
|    0     |    0     |    0     |    0     |    0     |    0     |    0     |    0     |    0     |     1    |    0     |    0     |    0     |    0     |    1     |    0     |     0    |    0     |
|    0     |    0     |    0     |    0     |    0     |    1     |    0     |    0     |    1     |     0    |    0     |    0     |    0     |    1     |    0     |    0     |     0    |    0     |



### 2h. For the Complete viruses from all samples, extract all the lines (from the same output file you just used to answer previous questions) so we can take a closer look. Also add a header so we know what each column contain

`grep` "Complete" 02_CHECK_V/BGR_*/MVP_02_BGR_*_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv

|Sample|virus_id|virus_length|provirus|gene_count|viral_genes|host_genes|checkv_qual.|miuvig_qual.|completeness|completeness_method|kmer_freq|coordinates|genetic_code|virus_score|n_hallmarks|marker_en.|taxonomy|Genome type|Host type|

02_CHECK_V/BGR_131021/MVP_02_BGR_131021_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:**BGR_131021	BGR_131021_NODE_96_length_46113_cov_32.412567	46113	No	53	12	2	Complete	High-quality	100.0	DTR (high-confidence)	1.0		11	0.9741	6	26.0838	Viruses;Duplodnaviria;Heunggongvirae;Uroviricota;Caudoviricetes	dsDNA	Prokaryote**

02_CHECK_V/BGR_140121/MVP_02_BGR_140121_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:**BGR_140121	BGR_140121_NODE_54_length_34619_cov_66.823718	34619	No	47	24	0	Complete	High-quality	100.0	DTR (high-confidence)	1.0		11	0.9812	10	41.8446	Viruses;Duplodnaviria;Heunggongvirae;Uroviricota;Caudoviricetes	dsDNA	Prokaryote**

02_CHECK_V/BGR_140717/MVP_02_BGR_140717_Filtered_Relaxed_Merged_Genomad_CheckV_Virus_Proviruses_Quality_Summary.tsv:**BGR_140717	BGR_140717_NODE_168_length_31258_cov_37.020094	31258	No	44	21	0	Complete	High-quality	100.0	DTR (medium-confidence)	1.0		11	0.9818	10	40.2292	Viruses;Duplodnaviria;Heunggongvirae;Uroviricota;Caudoviricetes	dsDNA	Prokaryote**


### 2i. Answer the following questions based on the data from 8:

    - In what samples were the complete viruses found? : BGR_131021, BGR_140121, BGR_140717
    - Are they integrated proviruses? : no, no, no
    - How long are they? : 46113, 34619, 31258
    - How many viral hallmark genes do they have? : 6, 10. 10
    - What percentage of the viral genes are viral hallmark genes? You may round to full numbers. : 6 von 12(50%), 10 von 24 (42%), 10 von 21 (48%), 
    - Thinking for yourself: Why are there more genes (gene_count) than viral genes and host genes combined? Tip: What does the column gene_count tell us? : Gene koennten ueberlappen also sowohl als virale als auch als Wirtsgene klassifiziert werden aufgrund von Homologien, gene_count gibt auch nicht klassifizierte gene an waehrend virale und host nur klassifiziert vorliegen, ganz vielleicht Kontamination


## 3. Clustering and Abundance

### 3a. Understanding the output: Find the clustering output. What is the difference between the three tsv files? Tip: Just look at the file names.

- wechsel in das Clustering verzeichnis

`cd` $WORK/viromics/03_CLUSTERING

- Datei 1: nur Cluster-Repraesentanetn gefiltert (kleinste, qualitative beste Auswahl)
- Datei 2: alle Viren gefiltert (mittlere Groesse, Qualitaet gesichert)
- Datrei 3: alle Viren ungefiltert (groesste aller Daten ohne Qualitaetsfilter)

### 3b. How many cluster representatives are there?

- -c bedeutet count
`grep -c` "" MVP_03_All_Sample_Filtered_Relaxed_Merged_Genomad_CheckV_Representative_Virus_Proviruses_Quality_Summary.tsv

**5375**


### 3c. How many of the cluster representatives are proviruses?

- cut -f extrahiert die 5. Spalte aus der angebenen tsv datei
- | leitet die Aufgabe des ersten Befehls an den zweiten Befehl weiter
- grep -c such nach dem wort yes und durch das -c wird gezaehlt wie oft es vorkommt 

`cut -f` 5 MVP_03_All_Sample_Filtered_Relaxed_Merged_Genomad_CheckV_Representative_Virus_Proviruses_Quality_Summary.tsv | grep -c "Yes"

**91 der Cluster representieren Provirus**


### 3d. What clusters do the complete viruses from 8 belong to? How large are the clusters?

- grep sucht nach Mustern in den Textdatein und -e nimmt dann BGR_131021_NODE_96_length_46113_cov_32.412567, BGR_140121_NODE_54_length_34619_cov_66.823718 und BGR_140717_NODE_168_length_31258_cov_37.020094
- durchsuchung der TSV-Datei nach Zeilen die eine dieser drei spezificshen Virus-IDs enthalten

`grep -e` "BGR_131021_NODE_96_length_46113_cov_32.412567" -e "BGR_140121_NODE_54_length_34619_cov_66.823718" -e "BGR_140717_NODE_168_length_31258_cov_37.020094" 03_CLUSTERING/MVP_03_All_Sample_Filtered_Relaxed_Merged_Genomad_CheckV_Representative_Virus_Proviruses_Quality_Summary.tsv

|BGR_131021_NODE_96_length_46113_cov_32.412567|BGR_140121_NODE_54_length_34619_cov_66.823718|BGR_140717_NODE_168_length_31258_cov_37.020094|
|                   6                         |                     24                      |                    10                        | 


### 3e. Thinking for yourself: Now we want to look at abundances. The files can be found in 04_READ_MAPPING, split per sample. Open any of the CoverM files and ignore everything except the first two columns.

- Fuer diese Datei wurden die gesamten Samples einer Probe mit allen 5374 viralen Clustern abgeglichen. Spalte 2 enthaelt die ID des aktuell beschriebenen Clustervertreters. Aus dieser Datei koennen wir sehen, wie haeufig jedes der wiedergewonnenen Viren (vOTUs) in jeder der Proben vorkommt - jede Probe hat ihre eigene Datei. 


### 3f. Thinking for yourself: If you scroll through the CoverM file you selected, you will come across lines where all of the metrics (except length) are zero. How could this have happened? Is it a bug or a feature?!

- die CoverM-Datei enthaelt Mapping-Informationen zu allen bekannten Viren, aber immer nur fuer eine einzelne Probe. Nicht jede Probe enthaelt alle 5000 Viren. Deshalb gibt es Zeilen wo alle werte gleich 0 sind. 
- diese 0 Zeilen stehen fuer Viren die nicht in der Probe nachgewiesen wurden -- da sie bspw. nicht in der Probe vorhanden waren, vorhanden waren und keine passenden reads gefunden wurden oder die Reads beim Qualitaets/Reinigungscshritt rausgefiltert wurden sind.
- nicht alle reads werden zu contigs


### 3g. Right now, the output for this module is spread across multiple files, which is inconvenient and happens a lot in bioinformatics. Luckily, each file has a column where the sample information is listed anyways, so we can merge them. Task: Merge all CoverM files for all the samples (order doesn't matter) using e.g. the cat command (not cut). Remember to exclude the headers and keep only one. Your finished file should have 96733 lines total.

- zuerst wird in das Verzeichnis 04_READ_MAPPING gewechselt
`cd` $WORK/viromics/04_READ_MAPPING

- drei verschiedene Commands werden benoetigt
- durchsucht werden alle CoverM.tsv Datein und anschliessend werden die Zeilen herausgefiltert, die "Sample" enthalten -> daraus wird dann eine tempraere Text Datei hergestellt
1. `grep` --no-filename -v "Sample" /BGR_*/*_CoverM.tsv >tmp_CoverM_output.tsv

- aehnlicher grep befehl, diesmal allerdings fuer eine spezifische Proben(BGR_150108), der Output der hier erzeugt wird soll ebenfalls in die oben erstellte temporaere Datei angeleht werden
2. `grep` "Sample" BGR_150108/BGR_150108_CoverM.tsv >tmp.tsv

- cat Befehl kombiniert die temporaere CoverM-Datei mit der tmp.tsv datei und schreibt alles in eine finale merged coverM output datei
3. `cat` tmp.tsv tmp_CoverM_output.tsv >merged_CoverM_output.tsv

- die temporaere Datei wird nun wieder entfernt, brauchen sie ja nicht mehr
4. `rm` tmp*


### 3h. Using the file you just created, how abundant are the complete viruses in different samples (RPKM)? Create a table.

`for x in` "BGR_131021_NODE_96_length_46113_cov_32.412567" "BGR_140121_NODE_54_length_34619_cov_66.823718"  "BGR_140717_NODE_168_length_31258_cov_37.020094" ; do grep "$x" 04_READ_MAPPING/merged_CoverM_output.tsv | cut -f1,2,11 ; done > 04_READ_MAPPING/merged_viruses_RPKM.tsv


|Sample    |                    abundant                  |    RPKM   |
|BGR_130708|BGR_131021_NODE_96_length_46113_cov_32.412567 |10.156808
|BGR_130829	BGR_131021_NODE_96_length_46113_cov_32.412567 |	201.0595
|BGR_130925	BGR_131021_NODE_96_length_46113_cov_32.412567 |	189.07722
|BGR_131021	BGR_131021_NODE_96_length_46113_cov_32.412567 |	154.71898
|BGR_131118	BGR_131021_NODE_96_length_46113_cov_32.412567 |	11.161729
|BGR_140106	BGR_131021_NODE_96_length_46113_cov_32.412567 |	31.22893
|BGR_140121	BGR_131021_NODE_96_length_46113_cov_32.412567 |	17.252619
|BGR_140221	BGR_131021_NODE_96_length_46113_cov_32.412567 |	0.60999864
|BGR_140423	BGR_131021_NODE_96_length_46113_cov_32.412567 |	13.359601
|BGR_140605	BGR_131021_NODE_96_length_46113_cov_32.412567 |	9.183679
|BGR_140717	BGR_131021_NODE_96_length_46113_cov_32.412567 |	0.12582351
|BGR_140821	BGR_131021_NODE_96_length_46113_cov_32.412567 |	0.099048875
|BGR_150108	BGR_131021_NODE_96_length_46113_cov_32.412567 |	2.3968215
|BGR_130305	BGR_140121_NODE_54_length_34619_cov_66.823718 |	530.0747
|BGR_130527	BGR_140121_NODE_54_length_34619_cov_66.823718 |	345.76505
|BGR_130708	BGR_140121_NODE_54_length_34619_cov_66.823718 |	199.11523
|BGR_130829	BGR_140121_NODE_54_length_34619_cov_66.823718 |	75.662476
|BGR_130925	BGR_140121_NODE_54_length_34619_cov_66.823718 |	88.48908
|BGR_131021	BGR_140121_NODE_54_length_34619_cov_66.823718 |	11.8905115
|BGR_131118	BGR_140121_NODE_54_length_34619_cov_66.823718 |	2.9122066
|BGR_140106	BGR_140121_NODE_54_length_34619_cov_66.823718 |	0.46738622
|BGR_140121	BGR_140121_NODE_54_length_34619_cov_66.823718 |	1329.697
|BGR_140221	BGR_140121_NODE_54_length_34619_cov_66.823718 |	210.0382  | 
|BGR_140320	BGR_140121_NODE_54_length_34619_cov_66.823718 |	472.9365  |
|BGR_140423	BGR_140121_NODE_54_length_34619_cov_66.823718 |	16.567926 |
|BGR_140605	BGR_140121_NODE_54_length_34619_cov_66.823718 |	3.1911626 | 
|BGR_140717	BGR_140121_NODE_54_length_34619_cov_66.823718 |	72.168    |
|BGR_140821	BGR_140121_NODE_54_length_34619_cov_66.823718 |	100.86392 |
|BGR_140919	BGR_140121_NODE_54_length_34619_cov_66.823718 |	152.32184 |
|BGR_141022	BGR_140121_NODE_54_length_34619_cov_66.823718 |	200.0772  |
|BGR_150108	BGR_140121_NODE_54_length_34619_cov_66.823718 |	267.48816 |
|BGR_130305	BGR_140717_NODE_168_length_31258_cov_37.020094|	197.46283 |
|BGR_130527	BGR_140717_NODE_168_length_31258_cov_37.020094|	161.75777 |
|BGR_130708	BGR_140717_NODE_168_length_31258_cov_37.020094|	113.52365 |
|BGR_130829	BGR_140717_NODE_168_length_31258_cov_37.020094|	43.803528 |
|BGR_130925	BGR_140717_NODE_168_length_31258_cov_37.020094|	46.017822 |
|BGR_131021	BGR_140717_NODE_168_length_31258_cov_37.020094|	6.9410787 |
|BGR_131118	BGR_140717_NODE_168_length_31258_cov_37.020094|	3.3950949 |
|BGR_140106	BGR_140717_NODE_168_length_31258_cov_37.020094|	3.1058502 |
|BGR_140121	BGR_140717_NODE_168_length_31258_cov_37.020094|	292.06485 |
|BGR_140221	BGR_140717_NODE_168_length_31258_cov_37.020094|	46.794453 |
|BGR_140320	BGR_140717_NODE_168_length_31258_cov_37.020094|	153.9964  |
|BGR_140423	BGR_140717_NODE_168_length_31258_cov_37.020094|	3.3980339 |
|BGR_140605	BGR_140717_NODE_168_length_31258_cov_37.020094|	2.0616696 |
|BGR_140717	BGR_140717_NODE_168_length_31258_cov_37.020094|	305.86407 |
|BGR_140821	BGR_140717_NODE_168_length_31258_cov_37.020094|	85.26142  |
|BGR_140919	BGR_140717_NODE_168_length_31258_cov_37.020094|	130.4371  |
|BGR_141022	BGR_140717_NODE_168_length_31258_cov_37.020094|	154.94527 |
|BGR_150108	BGR_140717_NODE_168_length_31258_cov_37.020094|	194.18689 |



## 4. Annotation

### 4a. Understanding the output: Find the annotation output from module 06. Based on the file naming alone, what is each of the files for and what is different between files with the same extension (file ending, like .tsv)?

- in module 06 wurden Datein zur Annotation von Viren erzeugt (also welche Gene/Proteine hat ein Virus und was machen sie vermutlich)
- `Tsv-Datein` enthalten Gen annotationen fuer ueber 5000 Virus Cluster also ueber die represantiven Viren dabei wurden drei verschiedene Datenbanken benutzt GENOMAD, PHROGS, PFAM --> hier steht also drin um welches Gen es sich ahndelt, zu welchem virus es gehoert und welche funktion es wahrscheinlkioch haben wird
- `Warum zwei tsv-Datein` es gibt eine Konservative Datei die sehr streng gefiltert ist und nur sehr sichere Annotationen besitzt wenig zeilen und eine Relaxte Datei die weniger streng ist und moegliche unsichere Treffer enthaelt ist also umfangreicher
- `.faa` Datein besitzen nur die Proteinsequenz an sich selber 

### 4b. We will focus on the file with the conservative results. Find the lines regarding the complete viruses.

    - How many genes does the complete virus have? : 53
    - What kind of genes does this virus have? Look at the PHROGS_Category. (This can also be solved by using commands): DNA, RNA and nucleotide metabolism
    
|BGR_131021_NODE_96_length_46113_cov_32.412567 |53|DNA, RNA and nucleotide metabolism, other, unknown                                        |
|BGR_140121_NODE_54_length_34619_cov_66.823718 |47|DNA, RNA and nucleotide metabolism und head and packaging, lysis, other, tail, unknown    |
|BGR_140717_NODE_168_length_31258_cov_37.020094|44|DNA, RNA and nucleotide metabolism und head and packaging, tail, connector, lysis, unknown|

### 4c. Reset the table so you can see all viruses again. Filter PHROGS_Category to moron, auxiliary metabolic gene and host takeover. If you look at the column PHROGS_GO_Annotation, you can see more general information related to the pathways.

    - What kind of metabolism are the viruses involved in?: siehe screenshot day9/aufgabe 21/22


### 4d. While still looking at the results filtered for moron, auxiliary metabolic gene and host takeover: Are there any toxin genes? Briefly look up the function of them (What do they do, where do they occur, what does it mean for this virus and its host?)

- siehe screenshot day9/aufgabe 21/22


## 5. Binning

### 5a. Understanding the output: Find this table and open it: 07_BINNING/07C_vBINS_READ_MAPPING/MVP_07_Merged_vRhyme_Outputs_Unfiltered_best_vBins_Memberships_geNomad_CheckV_Summary_read_mapping_information_RPKM_Table.tsv. What does each line represent (focus on column 1,2,5)? What is the purpose of this table?

- jede Zeile entspricht genau einem Virus oder genauer gesagt einem Mitglied eines viralen Bins (also ein Virus das zu einem bestimmten Bin gehoert). Da diese Binning auf vOTUs gemacht wurde sind diese Viren die Cluster-Repraesentanten aus einenm fruehren Modul
- `Spalte 1`: Name/ID des Virus (Cluster-Repraesentant)
- `Spalte 2`: zu welchem Bin dieses Virus gehoert
- `Spalte 5`: Membership


### 5b. How many High-quality "viruses" are there after binning?

**50**

### 5c. Thinking for yourself: Filter the results so you can only see vBin_16 (or just highlight the corresponding 5 lines). The metrics for all bin members are the same. But membership_provirus has different values. What does this tell you about the vBin/vMAG 16?

- `yes`: virus/contig ist ein Provirus - also in das genom eines Wirts eingbaut, lysogener zustand
- `no`: kein Provirus
- ein vBin ist ein Cluster aus meherren virus contigs die aehnliche sequenzen haben und vermutlich zum gleichen virus genom gehoeren, aber nicht jedes contig im vBin zeigen die gleichen biologischen Merkmalke, also einige contigs im vBin tragen keine signaturen eines provirus
- vBin_16 ist also wahrscheinlich ein virus, das als provirus existieren kann aber nicht alle teile des genoms sind proviral

### 5d. Thinking for yourself: Are your complete viruses part of a bin? Why does this result make sense? Tip: Use the search function or grep!

-  nein die vollstaendigen Viren sind nicht Teil eines Bins. Da Binning nur dazu dient fragmentierte Genome zu rekonstzruieren.

## 6. Host Prediction


### 6a. Understanding the output: Find this table and open it: 08_iPHoP/Host_prediction_to_genome_m90.csv. The input file to generate this table is 08_iPHoP/MVP_07_Filtered_conservative_Prokaryote_Host_Only_best_vBins_Representative_Unbinned_vOTUs_Sequences_iPHoP_Input_clean.fna. Based on the first column of the table and the name of the input file: What does each line represent? (Meaning: What kind of viruses are in this file?)

- jede Zeile entspricht einem Virus oder genbauer gesagt einemviralen Genom bzw einer viralen sequenz
- Input-datei: die besten vBins , also Virus Genome die zu einem Bin gehoeren und ungebinnte vOTU-Repraesentanten enthalten snd
- jede Zeile ein Virus, ein vollstaendig oder fast vollstaendiges Virusgenom (MAG-Mitglied) oder ein repraesentatives Virus einer vOTU
- fuer jedes Virus soll ein Host vorhergesagt werden
- viren ohne Host vorhersage tasuchen nicht auf

### 6b. What hosts were predicted for the complete viruses? From what habitat did the hosts come from?

Searching for: BGR_131021_NODE_96_length_46113_cov_32.412567
Searching for: BGR_140121_NODE_54_length_34619_cov_66.823718
Searching for: BGR_140717_NODE_168_length_31258_cov_37.020094
BGR_140717_NODE_168_length_31258_cov_37.020094,BGR_130829_bin.14.strict,d__Bacteria;p__Bacillota_A;c__Clostridia;o__Tissierellales;f__Peptoniphilaceae;g__;s__,blast,90.50,iPHoP-RF;78.00

### 6c. Find an example for a virus with more than one host prediction

    - With closely related hosts

BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_002584985.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A sp002584985	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_001884035.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A tropicus	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_001884105.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A luti	            iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_002551005.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A pseudomycoides_C	iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_002564555.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A cereus_K	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_002577405.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A cereus_AU	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_001884235.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A paramycoides	    iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_002567495.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A cereus_AQ	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_900112415.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A sp900112415	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_000383235.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A sp000383235	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_002200015.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A cereus_AZ	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_000832605.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A mycoides	        iPHoP-RF
BGR_130305_NODE_1097_length_6048_cov_14.323544	RS_GCF_006494425.1	d__Bacteria;p__Firmicutes;c__Bacilli;o__Bacillales;f__Bacillaceae_G;g__Bacillus_A;s__Bacillus_A sp006494425     	iPHoP-RF

    - With distantly related hosts

BGR_130527_NODE_365_length_14183_cov_15.647721	RS_GCF_004006535.1	d__Bacteria;p__Firmicutes_A;c__Clostridia;o__Tissierellales;f__Peptoniphilaceae;g__Anaerosphaera;s__Anaerosphaera multitolerans	blast
BGR_130527_NODE_365_length_14183_cov_15.647721	RS_GCF_000308275.2	d__Bacteria;p__Firmicutes_A;c__Clostridia;o__Tissierellales;f__Peptoniphilaceae;g__Levyella;s__Levyella massiliensis	        blast
BGR_130527_NODE_365_length_14183_cov_15.647721	BGR_140320_bin.8.strict	d__Bacteria;p__Cloacimonadota;c__Cloacimonadia;o__Cloacimonadales;f__Cloacimonadaceae;g__UBA3900;s__	                    blast
BGR_130527_NODE_365_length_14183_cov_15.647721	BGR_130527_bin.8.orig	d__Bacteria;p__Bacillota_A;c__Clostridia;o__Saccharofermentanales;f__DTU023;g__UBA4923;s__	                                blast


a. verwandte Host sind : rezeptor aehnlich und die Abwehrmechanismen sind aehnlich
   distanter Host: Virus ist evolutionaer flexibel oder horizontale Genuebertragung spielt eine Rolle

b. iPHoP basiert auf: Sequenzaehnlichkeit, gemiensame gene, k-mer profile, CRISPR-Spacern
   blast basiert auf: virale sequenzen werden mit MAgs verglichgen es sagt wie viele AS gleich sind und wie lang die alignment sind

c. Potential contamination of the host MAG: hoist Mag kann fremde DNA enthaltem, falsch gebinnt sein, virale sequenzen enthalten, dann sieht es so aus als haette das virus aehnlichkeit zu mehreren Hostst obwohl es ein artefakt ist




