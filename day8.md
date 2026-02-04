# what i learned today



#Day8

## 1. Datensatz downloaded der analysiert werden soll
- `READemption`komplettes Skript, um eine RNA-Seq-Analyse mit READemption vorgegeben

- `InSPI2` beschreibt ein saures phosphatlimitierendes minimales Medium, das die Salmonellen-Phathogenitaetsinsel (SPI) 2 induziert. Dies wird gemacht, um einen Umweltschock zu verursachen, der die Hochregulierung von spezifische Gen-Sets zur folge hat.

- `LSP`(spaete stationaere Phase). Dies wird durch das Wachstum in Lennox Brother Medium erreicht.

- `NCBI-Accession Number von Prasse et al. 2017`= The RNA-seq data discussed in this publication have been deposited in NCBI's Gene Expression OmnibusCitation66 and are accessible through GEO Series accession number GSE85456 (https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE85456). 

- SRR Nummer finden wir hier: https://www.ncbi.nlm.nih.gov/sra?term=SRP081251
- Suche da nach den SRR Nummern: SRR4018514, SRR4018515, SRR4018516, SRR4018517

- verwende micromamba to activate grabseq
module load micromamba/1.4.2
eval "$(micromamba shell hook --shell=bash)"
`micromamba activate $WORK/.micromamba/envs/10_grabseqs`

- erstelle ein new directory fuer die Speicherung der Daten im neuen Ordner day8/fastq_raw

`mkdir` $WORK/day8/fastq_raw
`cd` $WORK/day8/fastq_raw

- hier werden die sequence data gedownloaded from various repositories including SRA
- die commands werden in den Terminal direkt geladen 
- umbennung erfolgt dann einfach so 
`grabseqs` sra -t 4 -m ./metadata.csv SRR4018514
`grabseqs` sra -t 4 -m ./metadata.csv SRR4018515
`grabseqs` sra -t 4 -m ./metadata.csv SRR4018516
`grabseqs` sra -t 4 -m ./metadata.csv SRR4018517

- Rename each SRR file according to the sample name. For example, SRR4018514 to wt_R1.fastq.gz, SRR4018515 to wt_R2.fastq.gz, SRR4018516 to mut_R1.fastq.gz, and SRR4018517 to mut_R2.fastq.gz.

### wenn man was downloaden moechte muss man den `export` command ausfuehren
### `wget` nimmt die Informationen raus
### mit `sed` wird das genom gerenamed
### download the reals RNA-Sequenz 

## 2. Create Reademtion
- Activate the reademption environment and create the folder structure for the analysis
- go to the directory you want to work in
`cd` $WORK/rnaseq

- create folders
`reademption create` --project_path READemption_analysis_2 --species methanosarcina="Methanosarcina mazei Gö1"

## 3. Download die referenzgenome und annotiere die files
- download reference genome wird mit dem command wget ausgefuehrt
`wget` -O READemption_analysis_2/input/methanosarcina_reference_sequences/GCF_000007065.1_ASM706v1_genomic.fna.gz https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/007/065/GCF_000007065.1_ASM706v1/GCF_000007065.1_ASM706v1_genomic.fna.gz

- download annotation wird mit dem command wget ausgefuehrt
`wget` -O READemption_analysis_2/input/methanosarcina_annotations/GCF_000007065.1_ASM706v1_genomic.gff.gz https://ftp.ncbi.nlm.nih.gov/genomes/all/GCF/000/007/065/GCF_000007065.1_ASM706v1/GCF_000007065.1_ASM706v1_genomic.gff.gz

- die datein muessen jetzt noch geunzip werden
`gunzip` READemption_analysis_2/input/methanosarcina_reference_sequences/GCF_000007065.1_ASM706v1_genomic.fna.gz
`gunzip` READemption_analysis_2/input/methanosarcina_annotations/GCF_000007065.1_ASM706v1_genomic.gff.gz

## 4. Run REDemption
- wechsel in das Verzeichnis wo alle daten drin sind und wo die daten hin sollen
`cd` $WORK/rnaseq

- zuordnen der reads zu den reference mit folgendem Command
- -- fastq muss angegeben werden um die zu sagen welche art von dateien das ist
`reademption align` -p 4 --poly_a_clipping --project_path READemption_analysis_2 --fastq

- berechne die Read coverage mit folgendem Command
`reademption coverage` -p 4 --project_path READemption_analysis_2

- quantify gene expression
`reademption gene_quanti` -p 4 --features CDS,tRNA,rRNA --project_path READemption_analysis_2

- berechene verschiedne Exprpressionen mit DESeq2
`reademption deseq` -l mut_R1.fastq.gz,mut_R2.fastq.gz,wt_R1.fastq.gz,wt_R2.fastq.gz -c mut,mut,wt,wt -r 1,2,1,2 --libs_by_species methanosarcina=mut_R1,mut_R2,wt_R1,wt_R2 --project_path READemption_analysis_2

- visualisierung der Ergebnisse mit Hilfe folgender Commmands
`reademption viz_align` --project_path READemption_analysis_2
`reademption viz_gene_quanti` --project_path READemption_analysis_2
`reademption viz_deseq` --project_path READemption_analysis_2


## 5. Analyse der Ergebnisse

**`all_species_viz_align`**
Das Arten-Boxplot zeigt, wie viele Reads auf welche Arten gemappt wurden (oder gar nicht gemappt wurden). In unserem Fall wurden alle gemappten Reads auf unser einziges Referenzgenom von Methanosarcina mazei Gö1 gemappt. 
- screenshot: stacked spezies

**`methanosarcina_viz_align`**
Das Boxplot-Diagramm fuer die alignierten Reads zeigt die Anzahl der alignierten Reads und wie sie aligniert wurden. Eindeutig ausgerichtete Reads wurden nur einer Stelle im Genom zugeordnet, waehrend mehrfach ausgerichtete Reads mehreren Stellen zugeordnet wurden. Hier sehen wir, dass die meisten Reads eindeutig ausgerichtet sind. Geteilte ausgerichtete Reads sind Reads, die sich ueber mehrere separate Regionen des Genoms erstrecken, was in eukaryotischen Genomen mit Introns vorkommen kann, in Prokaryoten jedoch seltener ist. Kreuzausgerichtete Reads werden mehreren Referenzgenomen zugeordnet, was hier jedoch nicht relevant ist, da wir nur eines haben. 
- screenshot: methanosarcina_aligned_reads

**`methanosarcina_viz_deseq`**
    **`MA-Plot`** = visualisieren die Differentialexpressionsanalyse. Jeder Punkt steht fuer ein Gen, wobei die x-Achse das durchschnittliche Expressionsniveau und die y-Achse die log2-Fold-Change zwischen den Bedingungen anzeigt. Gene, die signifikant unterschiedlich exprimiert werden, werden als rote Punkte dargestellt, nicht signifikante als schwarze Punkte. Eine log2-Fold-Change von ueber 0 bedeutet, dass das Gen in der ersten Bedingung im Vergleich zur zweiten hochreguliert ist und umgekehrt. Im Allgemeinen erwarten wir eine weitgehend symmetrische Verteilung um den log2-Fold-Change-Wert = 0, wobei einige Gene eine signifikante Hoch- oder Herunterregulierung aufweisen.
    **`Volcano-Plot`** = visualisieren auch die Differentialexpressionsanalyse. Es gibt Diagramme sowohl fuer die rohen p-Werte als auch fuer die angepassten p-Werte. Der angepasste p-Wert ist konservativer als der rohe p-Wert und zeigt daher weniger falsch-positive Ergebnisse. In beiden Faellen zeigt das Diagramm die log2-Fold-Changes auf der x-Achse, wobei positive Werte eine Hochregulation in der ersten Bedingung im Vergleich zur zweiten und umgekehrt anzeigen. Die y-Achse zeigt den -log10 des p-Wertes, d. h. je hoeher ein Punkt liegt, desto niedriger ist sein p-Wert. Die gruenen gepunkteten Linien zeigen die Schwellenwerte fuer die Signifikanz (p-Wert < 0,05) und die log2-Fold-Change (> 1 oder < -1) an. 
- Screenshot: Ma-Plot, volano plot


**`methanosarcina_viz_gene_quanti`**
Die Expressionsstreuungsdiagramme vergleichen die Expression jedes Gens unter verschiedenen Bedingungen. Im Allgemeinen erwarten wir, dass die meisten Gene unter allen Bedingungen eine aehnliche Expression aufweisen. In diesem Fall sollten die meisten Punkte entlang der diagonalen Linie liegen und der R-Wert sollte nahe bei 1 liegen.
Die RNA-Klassen-Boxplots zeigen die Anzahl der Read-Counts fuer verschiedene RNA-Klassen (CDS, tRNA, rRNA) in jeder Probe. Dies kann dabei helfen, Verzerrungen in den Daten zu identifizieren, beispielsweise wenn eine RNA-Klasse in einer Probe ueberrepraesentiert ist. 
- Screenshot: rna_class_sizes und expression_scatter_plots


**`read_lengths_viz_align`**
Die Diagramme zur Leselaengenverteilung vergleichen die Unterschiede in den Leselaengen vor und nach dem Trimmen sowie zwischen den Proben. Die Leselaenge wird durch die verwendete Sequenzierungstechnologie bestimmt. 
- Screenshot: input_reads_length_distribution


## 6. Unsere Ergebnisse:
### stacked spezies: 
- most of aller reads geht zu Mazei Gö1, wir haben keine Cross aligned Reads

### methanosarcina_alligned_reads: 
- multiple means
- only have alignment reads

### MA-Plot
- links: 0 heisst kein change , 1 heisst double expression, -1 heisst half expression
- can see the specification - how hard are they expressed
- desto hoeher / niedriger mehr diffrents
- rot ist signifikant hoch und runter

### volcano plots
- andere art als bei MA-Plot
- alles was oben ist ist hoch significant

### expression - scatcherplot
- wenn inhalt aehnlich ist, desto dichter an der linie dran
- quasi eine art von Qualitaetcontrol

### Ma-Class

### dreseq_with anootation
- lange Tabelle im deseq output folder
- p-value so low as possilble