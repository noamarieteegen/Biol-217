# what i learned today

# Day6 



## Allgemein Theorie:
**`CheckM`**
**`CheckM2`**
**`L50/N50`**
**`gbk`**
**`gfa`**

- andere micromamba aktivierungen beachten
- ordnere so benennen - einfacher fuer die Pfade 

## 1. Quality Control der Short Reads
- neuen Output-Ordner fuer qc erstellen

`mkdir -p` $WORK/genomics/1_short_reads_qc/1_fastqc_raw

- auf die Pfade hier achtenn! wir sind schon in work deswegen ../ nicht vorher

`for i in` genomics/0_raw_reads/short_reads/*.gz
do 
  `fastqc` $i -o genomics/1_short_reads_qc/1_fastqc_raw -t 8
done

## 2. fastp der short reads durchfuehren
- 2_cleaned_reads = fastp_out
- Das vorgegebene Schema: fastp -i ? -I ? -o ? -O ? -t 6 -q 20 -h ? -R ?
- erst muss wieder angegeben werden in welchen Ordner wir wollen 

`mkdir -p` $WORK/genomics/1_short_reads_qc/fastp_out
`fastp -i` genomics/0_raw_reads/short_reads/241155E_R1.fastq.gz -I genomics/0_raw_reads/short_reads/241155E_R2.fastq.gz -o genomics/1_short_reads_qc/fastp_out/241155E_R2_R1_clean.fastq.gz -O genomics/1_short_reads_qc/fastp_out/241155E_R2_R2_clean.fastq.gz -t 6 -q 20 -h sample1.html -R "Sample1 Fastp Report"

## 3. fastqc cleaned ueberpruefen
- eigentlich genau so wie in schritt nummer 1.

`mkdir -p` $WORK/genomics/1_short_reads_qc/2_fastqc_raw

`for i in` genomics/1_short_reads_qc/fastp_out/*.gz
do 
  `fastqc` $i -o genomics/1_short_reads_qc/2_fastqc_raw -t 16
done


## 4. Fragen zu schritt 1-3.

**How Good is the read quality?**
  - ist gut - liegt zwischen 20 und 30

**How many reads before trimming and how many do you have now?**
  - vorher: 3.279098 M
  - nachher:	3.226784 M

**Did the quality of the reads improve after trimming?**
  - bei R1 und R2 ist die Qualitaet nach Trmming besser als vorher (siehe Screenshots day6_FastQC(cleaned))

- Fastp-Report: file:///run/user/745871/gvfs/sftp:host=caucluster.rz.uni-kiel.de/work_beegfs/sunam234/genomics/1_short_reads_qc/fastp_out/sample1.html

## 5. Long Reads Nanoplot
- erst dran denken anderes Micromamba zu aktivieren
`micromamba activate .micromamba/envs/02_long_reads_qc`

- wir gehen in das verzeichnis long_reads
`cd` $WORK/genomics/0_raw_reads/long_reads/

- neuen ordner fuer nanoplot ergebnisse in Long_reads erstellen
`mkdir -p` $WORK/genomics/2_long_reads_qc/1_nanoplot_raw

- Command fuer den Nanoplot an sich durchfuehren
`NanoPlot` --fastq $WORK/genomics/0_raw_reads/long_reads/*.gz \
 -o $WORK/genomics/2_long_reads_qc/1_nanoplot_raw -t 16 \
 --maxlength 40000 --minlength 1000 --plots kde --format png \
 --N50 --dpi 300 --store --raw --tsv_stats --info_in_report

 ## 6. Filtlong Command durchfuehren
 - erst wieder Ordner erstellen fuer die Ergebnisse
`mkdir -p` $WORK/genomics/2_long_reads_qc/2_cleaned_reads

- Filtlong command ist vorgefertigt alsp einfach Pfad einsetzen und durchfuehren
`filtlong` --min_length 1000 --keep_percent 90 $WORK/genomics/0_raw_reads/long_reads/*.gz | gzip > $WORK/genomics/2_long_reads_qc/2_cleaned_reads/241155E_cleaned_filtlong.fastq.gz

## 7. Nanoplot der gecleanten Long reads durchfuehren
- wir gehenn zu den long reads mit den clean reads ordner
`cd` $WORK/genomics/2_long_reads_qc/2_cleaned_reads

- wir erstellen einen Ordner fuer die gecleanten Nanoplot Ergebnisse
`mkdir` -p $WORK/genomics/2_long_reads_qc/3_nanoplot_cleaned

- durchfuehren des Nanoplots Commands gleiches Schema wie bei Aufgabe 5. 
`NanoPlot` --fastq $WORK/genomics/2_long_reads_qc/2_cleaned_reads/*.gz \
 -o $WORK/genomics/2_long_reads_qc/3_nanoplot_cleaned -t 16 \
 --maxlength 40000 --minlength 1000 --plots kde --format png \
 --N50 --dpi 300 --store --raw --tsv_stats --info_in_report

 ## 8. Fragen zu Aufgabe 5-7.

**How Good is the long reads quality?'**
  - median_read_length 	4477.0
  - n50 	22747.0
  - NanoPlot Report zu beantwortung der Frage: file:///run/user/745871/gvfs/sftp:host=caucluster.rz.uni-kiel.de/work_beegfs/sunam234/genomics/2_long_reads_qc/3_nanoplot_cleaned/NanoPlot-report.html

**How many reads before trimming and how many do you have now?**
  - NanoPlot vor Trimming (nicht gecleanet): file:///run/user/745871/gvfs/sftp:host=caucluster.rz.uni-kiel.de/work_beegfs/sunam234/genomics/2_long_reads_qc/1_nanoplot_raw/NanoPlot-report.html
  - Number of reads:  	15963

  - NanoPlot Danach: file:///run/user/745871/gvfs/sftp:host=caucluster.rz.uni-kiel.de/work_beegfs/sunam234/genomics/2_long_reads_qc/3_nanoplot_cleaned/NanoPlot-report.html
  - Number of reads:  	12446


## 9. Assembly des Genoms mit Unicycler
- wieder drauf achten das ein anderer Micromamba benutzt wird
`eval` "$(micromamba shell hook --shell=bash)"
`cd` $WORK
`micromamba activate .micromamba/envs/03_unicycler`

`cd` $WORK/genomics

- erstelle einen neuen Ordner im Ordner Genomics fuer die Ergebnisse des Hybrid Assemblys
`mkdir -p` $WORK/genomics/3_hybrid_assembly

- Unicycler Command ist quasi vorgegeben: wir nehmen die beiden gecleanten short reads aus dem short read folder - fastp_out und das gecleante Filtlong der Longreads und fuehren das assembly durch
- `-o` ist dabei wieder die Ausgabe: das Assembly wird hier ausgegeben und zwar in dem oben erstellten Ordner 3_hybrid_assembly
- 
`unicycler` -1 $WORK/genomics/1_short_reads_qc/fastp_out/241155E_R2_R1_clean.fastq.gz -2 $WORK/genomics/1_short_reads_qc/fastp_out/241155E_R2_R2_clean.fastq.gz -l $WORK/genomics/2_long_reads_qc/2_cleaned_reads/241155E_cleaned_filtlong.fastq.gz -o $WORK/genomics/3_hybrid_assembly/ -t 8
- `-1` und `-2` = `Paired-End-Sequenzierung`: ist quasi wie eine aufzaehlung, sie geben die beiden verschiedenen Output dateien der Short Reads an. Bei den Long reads brauchen wir das nicht weil wir nur eine Datei haben
- `-l` = `Single-End-Sequenzierung` : gibt an das es ein long read ist

## 10. Ueberpruefung der Assembly Qualitaet mit Quast
- auch hier wieder Micromamba umstellen
- gehe zum ordner mit den assembly ergebnissen
`cd` $WORK/genomics/3_hybrid_assembly

- erstelle einen neuen Ordner fuer die Quast Assembly Ergebnisse
`mkdir -p` $WORK/genomics/3_hybrid_assembly/quast_assembly

- bei dem Command ist alles vorgegeben ausser das assembly.fasta und der Ordner in den es asugegeben werden soll mit `-o` das ist in dem Fall der Ordner quast_assembly
`quast.py` $WORK/genomics/3_hybrid_assembly/assembly.fasta --circos -L --conserved-genes-finding --rna-finding \
 --glimmer --use-all-alignments --report-all-metrics -o $WORK/genomics/3_hybrid_assembly/quast_assembly -t 8
micromamba deactivate

- Link fuer den Report des Quast Assembly: file:///run/user/745871/gvfs/sftp:host=caucluster.rz.uni-kiel.de/work_beegfs/sunam234/genomics/3_hybrid_assembly/quast_assembly/report.html

## 11. Uberpruefung der Assembly Qualitaet mit CheckM
- wieder Micromamba aendern in checkM
- erst muessen wir auf die Daten die auf Work legen zugreifen koennen
`checkm data setRoot $WORK/databases/checkm`

- erstellen eines Output Ordners 
`mkdir -p` $WORK/genomics/3_hybrid_assembly/checkm_out
- erst wiedre zum Hauptordner Work gehen
`cd` $WORK

- durchfuehren des CheckM auf das Assembly
`checkm lineage_wf` genomics/3_hybrid_assembly/ genomics/3_hybrid_assembly/checkm_out -x fasta --tab_table --file genomics/3_hybrid_assembly/checkm_out/checkm_results -r -t 8
  
- Run CheckM QA for this assembly
`checkm tree_qa` genomics/3_hybrid_assembly/checkm_out
`checkm qa` genomics/3_hybrid_assembly/checkm_ou/lineage.ms genomics/3_hybrid_assembly/checkm_out -o 1 > genomics/3_hybrid_assembly/checkm_out/final_table_01.csv
`checkm qa` genomics/3_hybrid_assembly/checkm_out/lineage.ms genomics/3_hybrid_assembly/checkm_out -o 2 > genomics/3_hybrid_assembly/checkm_out/final_table_checkm.csv

- Completness = 98.88            Contamination = 0.19 

## 12. Uberpruefung der Assembly Qualitaet mit CheckM2
- Erstellen eines Output Ordners fuer checkM2
- hier ist mein Genom gib mir alle informationen dazu
`mkdir -p` $WORK/genomics/3_hybrid_assembly/checkm2_out

- durchfuehren des CheckM2 Commands - input ist das fasta file des assmeblys und output soll hierbei in den neu erstellten Ordner fuer checkm2 ergebnisse
`checkm2 predict` --threads 1 --input $WORK/genomics/3_hybrid_assembly/assembly.fasta --output-directory $WORK/genomics/3_hybrid_assembly/checkm2_out

## 13. Kommentieren der Genome mit PROKKA
- wir wechseln zu dem Verzeichnis 3_hybrid_assembly
`cd` $WORK/genomics/3_hybrid_assembly

- Prokka wird durchgefuehrt - WICHTIG Prokka erstellt eigenen Ergebnisordner muss vorher also kein mkdir machen
`prokka` $WORK/genomics/3_hybrid_assembly/assembly.fasta --outdir $WORK/genomics/4_annotated_genome --kingdom Bacteria --addgenes --cpus 32

## 14. Klassifizieren der Genome mit GTDBTK
- erstellen eines neuen Ordners fuer die Speicherung der ergebnisse
- kopieren der fna datei in den Ordner
`mkdir -p` $WORK/genomics/5_gtdb_classification

- in den Ordner 4 gehen
`cd` $WORK/genomics/4_annotated_genome

- durchfuehren des gtdbtk commands
`gtdbtk classify_wf` --cpus 1 --genome_dir $WORK/genomics/4_annotated_genome/ --out_dir $WORK/genomics/5_gtdb_classification --extension .fna --skip_ani_screen

## 15. MultiQC fuer die Verbindung aller Reporte
- kombiniert alle qualitiys controls in einem 

`multiqc -d` $WORK/genomics/ -o $WORK/genomics/6_multiqc

- Multiqc: file:///run/user/745871/gvfs/sftp:host=caucluster.rz.uni-kiel.de/work_beegfs/sunam234/genomics/6_multiqc/multiqc_report.html


## 16. Allgemein Fragen:

**How good is the quality of genome?**
   - bewertet durch N50 wert (hoeher = bessere Zusammenhaenge)

**Why did we use Hybrid assembler?**
  - kombiniert short und long reads
  - long reads geben bessere zusammenhaenge (groessere scaffords)
  - short reads geben hoehere genauigkeit 

**What is the difference between short and long reads?**
  - short reads: bsp illumina, sehr genau, 150-300 bp
  - long reads: bsp. Nanopore, sehr lang bis zu 100000 bp, fehleranfaellig, gut fuer die erkennung der struktur des Genoms

**Did we use Single or Paired end reads? Why?**
  - Paired ends: lesen beide enden eines dna-fragments was eine bessere abdeckung und ein genaueres assembly ermoeglicht - helfen bei dem zusammenfuehren von contigs zu scaffords

**Which classification was assigned to the genome. Is it trust worthy and why?**
  - 