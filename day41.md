# what i learned today

#Day3

`-o` ist immer das etwas ausgegeben wird

## 1. Bewertung der Assembly-Qualitaet

`micromamba activate 00_anvio`

`cd` $WORK

- Es wird ein Verzeichnis im Ordner WORK erstellt welcher day3 heisst
`mkdir -p` day3

- Es wird in das Verzeichnis day3 gewechselt, welches sich im Ordner WORK befindet
`cd` $WORK/day3

- Es werden Ordner erstellt die die Ergebnisse abspeichern sollen, diese befidnen sich im WORK-day3-Ordner
`mkdir` -p metaquast_out

# WICHTIG: es muss dem System gesagt werden, dass es zum WORK-Ordner geht und wo lang es dann gehen soll

- Bewertung der Assembly-Qualitaet
- Vorgegebene Struktur: quast ? -o ? -t 6 -m 1000
`quast` ../day2/assembly_out/final.contigs.fa -o ../day3/metaquast_out/ -t 6 -m 1000

### Fragen zu Aufgabe 1.

**What is your N50 value? Why is this value relevant?** = 3014
**How many contigs are assembled?** = 55836
**What is the total length of the contigs?** = 142642586


## 2. Neuformatierung der Contigs
- Vorgegebenes Schema: anvi-script-reformat-fasta ? -o ? --min-len 1000 --simplify-names --report-file ? 
- Um Sequenz-IDs zu vereinfachen und kurze Contigs herauszufiltern
- ../ damit wieder sagen es soll zu Work gehen und dann den Pfad beschreiben
`anvi-script-reformat-fasta` ../day2/assembly_out/final.contigs.fa -o ../day3/metaquast_out/contigs.anvio.fa --min-len 1000 --simplify-names --report-file ../day3/metaquast_out/aufgabe2.1.txt

## 3. Indexierung der Contigs
- wenn Contigs organisiert und indexiert sind, wird das Mapping viel schneller ausgefuehrt -- deshalb erstellung von Index-Datein
- Vorgegebene Struktur: bowtie2-build ? ?
`bowtie2-build` ../day3/metaquast_out/contigs.anvio.fa ../day3/metaquast_out/contigs.anvio.fa.index

## 4. Zuordnung von Reads zu Contigs
- Wir nutzen jetzt die Hauptnummer bowtie2Programm fuer das eigentliche Mapping. Die Ausgabe von Read Mapping wird als AMS-Sequence-A-Lignment-M-App-Datei bezeichnet und endet mit *.sam.
-  `bowtie2` Programm fuer das eigentliche Mapping
- `*.sam` read mapping heisst Sequence Alignment Map file
- Vorgegebene Struktur: bowtie2 -1 ? -2 ? -S ? -x ? --very-fast 

- erstellen eines neuen Ornders zum speichern der Ergebnisse
`cd` $WORK/day3
`mkdir` -p sam_out

- es muessen die Dateinen aus day2/fastp_out genommen werden

`bowtie2` -1 ../day2/fastp_out/BGR_130305_mapped_R2_R1_clean.fastq.gz -2 ../day2/fastp_out/BGR_130305_mapped_R2_R2_clean.fastq.gz -x ../day3/metaquast_out/contigs.anvio.fa.index -S ../day3/sam_out/BGR_130305.sam --very-fast
`bowtie2` -1 ../day2/fastp_out/BGR_130527_mapped_R2_R1_clean.fastq.gz -2 ../day2/fastp_out/BGR_130527_mapped_R2_R2_clean.fastq.gz -x ../day3/metaquast_out/contigs.anvio.fa.index -S ../day3/sam_out/BGR_130527.sam --very-fast
`bowtie2` -1 ../day2/fastp_out/BGR_130708_mapped_R2_R1_clean.fastq.gz -2 ../day2/fastp_out/BGR_130708_mapped_R2_R2_clean.fastq.gz -x ../day3/metaquast_out/contigs.anvio.fa.index -S ../day3/sam_out/BGR_130708.sam --very-fast

## 5.  Uebertragen Sequence Alignment Map (.sam) to Binary Alignment Map (.bam)
- Vorgegebene Struktur: samtools view -Sb ? > ?

- erstellen eines neuen Ornders zum speichern der Ergebnisse
`cd` $WORK/day3
`mkdir` -p bam_out

- fuer jede Datei muss das einzeln gemacht werden
`samtools view` -Sb ../day3/sam_out/BGR_130305.sam > ../day3/bam_out/BGR_130305.bam
`samtools view` -Sb ../day3/sam_out/BGR_130527.sam > ../day3/bam_out/BGR_130527.bam
`samtools view` -Sb ../day3/sam_out/BGR_130708.sam > ../day3/bam_out/BGR_130708.bam

## 6. Sortieren der Mapped reads
- Vorgegebene Struktur: anvi-init-bam ? -o ?
- Die Sortierung von Kartierungslesungen beschleunigt nicht nur die Datenverarbeitung, sondern ermoeglicht Ihnen auch nachgelagerte Analysen wie Visualisierung und Variantenanrufe 
- Der Befehl wird automatisch generiert *.bam.bai Dateien. Es sind die Indexdateien. Beruehre sie nicht. nur mit .bam weiterarbeiten

`anvi-init-bam` ../day3/bam_out/BGR_130305.bam -o ../day3/bam_out/BGR_130305_sorted.bam
`anvi-init-bam` ../day3/bam_out/BGR_130527.bam -o ../day3/bam_out/BGR_130527_sorted.bam
`anvi-init-bam` ../day3/bam_out/BGR_130708.bam -o ../day3/bam_out/BGR_130708_sorted.bam

## 6. Binning reads: Erstellen der Contigs Database
- `fasta` Das Format der Contigs ist begrenzt und kann keine komplexeren Arten von Informationen speichern
- deswegen verwendung von `anvi-gen-contigs-database`
- Vorgegebene Struktur: anvi-gen-contigs-database -f ? -o ? -n ?

- erstellen eines neuen Ornders zum speichern der Ergebnisse
`cd` $WORK/day3
`mkdir` -p contigsdb_out

`anvi-gen-contigs-database` -f ../day3/metaquast_out/contigs.anvio.fa  -o ../day3/contigsdb_out/contigs.db -n binningreads_db

`-f` <fasta> : tut re-formatted fasta file darein
`-o` <contigs> : gibt contigs database file aus
`-n` <name> : Name des work project

Ausgabe: CONTIGS DB CREATE REPORT

Split Length .................................: 20,000
K-mer size ...................................: 4
Skip gene calling? ...........................: False
External gene calls provided? ................: False
Ignoring internal stop codons? ...............: False
Splitting pays attention to gene calls? ......: True
Contigs with at least one gene call ..........: 55803 of 55836 (99.9%)
Contigs database .............................: A new database, ../day3/contigsdb_out/contigs.db, has been created.
Number of contigs ............................: 55,836
Number of splits .............................: 56,157
Total number of nucleotides ..................: 142,642,586
Gene calling step skipped ....................: False
Splits broke genes (non-mindful mode) ........: False
Desired split length (what the user wanted) ..: 20,000
Average split length (what anvi'o gave back) .: 22,700

## 7. Annotation von ORF
-  Idee, nach moeglichen biologischen Funktionen zu suchen, die die vorhergesagten ORFs haben koennen.
- Vorgegebene Struktur: anvi-run-hmms -c ? --num-threads 4
-  eine HMM-Suche gegen mehrere Sammlungen von Genen durchfuehren, um zu sehen, ob die ORFs vorhergesagt werden

`anvi-run-hmms` -c ../day3/contigsdb_out/contigs.db --num-threads 4

`-c` <contigs>: Eingabe der Contigs-Datenbank.
`--num-threads 4`: die Anzahl der CPU-Threads, die fuer die Berechnung verwendet werden.

## 8. Visualizing the contigs database
- super compliziert 
`anvi-display-contigs-stats` day3/contigsdb_out/contigs.db
- command wird im Terminal ausgefuehrt

## 9. Erstelle ein anvio Profil
- anvi'o Profil ist wie ein Upgrade anvi'o Datenbank, die auch Lese-Mapping-Ergebnisse und detaillierte Pro-Nukleotid-Informationen speichern kann
- Vorgegebene Struktur: anvi-profile -i ? -c ? --output-dir ?
- `anvi-profile` Will:
    - Verarbeiten Sie jede Contig, die laenger ist als 2.500 Nukleotide standardmaessig.
    - Denken Sie daran, dass die minimale Kontiglaenge lang genug sein sollte, damit Tetra-Nukleotid-Frequenzen genug Signal haben. Gehen Sie nie unter 1.000 Nts.
    - Berechnen Sie die mittlere Abdeckung, die Standardabweichung der Abdeckung und die durchschnittliche Abdeckung fuer die inneren Quartile (Q1 und Q3) fuer jeden Contig.
    - Charakterisieren Sie Einzelnukleotidvarianten (SNVs) fuer jede Nukleotidposition.
    - wird der Profiler auf keine Nukleotidposition mit einer Abdeckung von weniger als 10X achten.
- `anvi-profile` Will nicht:
    - Nicht Cluster-Contigs standardmaessig weil einzelne Profile selten fuer Genom-Binning oder Visualisierung verwendet werden
        und weil der Clustering-Schritt die Profiling-Laufzeit ohne guten Grund erhoeht.

- erstellen eines neuen Ornders zum speichern der Ergebnisse
`cd` $WORK/day3
`mkdir` -p profile_out

`anvi-profile` -i ../day3/bam_out/BGR_130305_sorted.bam -c ../day3/contigsdb_out/contigs.db -o ../day3/profile_out/BGR_130305_profile/
`anvi-profile` -i ../day3/bam_out/BGR_130527_sorted.bam -c ../day3/contigsdb_out/contigs.db -o ../day3/profile_out/BGR_130527_profile/
`anvi-profile` -i ../day3/bam_out/BGR_130708_sorted.bam -c ../day3/contigsdb_out/contigs.db -o ../day3/profile_out/BGR_130708_profile/

`RUNLOG.txt`: detailliertes Protokoll fuer den Profilerstellungslauf.
`PROFILE.db`: Die gewuenschten anvioProfildatenbank, die wichtige Informationen ueber die Zuordnung von kurzen Lesungen aus mehreren Proben auf die zusammengefuegten Contigs enthaelt.

## 10. Fusionieren der anvi'o Profile aus allen Proben
- Um alle Proben zu analysieren und zu vergleichen, muessen die Profile, die aus den verschiedenen Proben stammen, in ein Profil zusammengefuehrt
- Vorgegebene Struktur: anvi-merge ? ? ? -o ? -c ? --enforce-hierarchical-clustering

`anvi-merge` ../day3/profile_out/BGR_130305_profile/PROFILE.db ../day3/profile_out/BGR_130527_profile/PROFILE.db ../day3/profile_out/BGR_130708_profile/PROFILE.db -o ../day3/profile_out/merged_profiles/ -c ../day3/contigsdb_out/contigs.db --enforce-hierarchical-clustering

-anvi-merge wird ausgefuehrt um die einzelnen Profil-Datenbanken der drei BGR Proben in ein Profil Verzeichnis zu fuehren, dabei wird Clustering angewendet

`-o` = gibt wieder eine Ausgabe an
`-c` =  <contigs> Contigs Datenbankdatei. 
`enforce-hierarchical-clustering` = konstruieren Sie einen phylogenetischen Baum, der die Beziehungen zwischen den Contigs zeigt

## 11. Binning Contigs zu Genomen
- das ist die Durchfuehrung des Binnings
- Vorgegebene Struktur: anvi-cluster-contigs -p ? -c ? -C METABAT2 --driver metabat2 --log-file ? --just-do-it
- Vorgegebene Struktur: anvi-summarize -p ? -c ? -o ? -C METABAT

- einmal der Command mit MetaBat **warum gibt es da zwei verschiedene**
`cd` $WORK/day3
`mkdir` -p binning3.6_out
`anvi-cluster-contigs` -p ../day3/profile_out/merged_profiles/PROFILE.db -c ../day3/contigsdb_out/contigs.db -C METABAT2 --driver metabat2 --just-do-it --log-file ../day3/binning3.6_out/metabat2.log
`anvi-summarize` -p ../day3/profile_out/merged_profiles/PROFILE.db -c ../day3/contigsdb_out/contigs.db -o ../day3/binning3.6_out/SUMMARY_METABAT2 -C METABAT2

- einmal der Command mit MaxBin **warum gibt es da zwei verschiedene**
`anvi-cluster-contigs` -p ../day3/profile_out/merged_profiles/PROFILE.db -c ../day3/contigsdb_out/contigs.db -C MAXBIN2 --driver maxbin2 --just-do-it --log-file ../day3/binning3.6_out/maxbin2.log
`anvi-summarize` -p ../day3/profile_out/merged_profiles/PROFILE.db -c ../day3/contigsdb_out/contigs.db -o ../day3/binning3.6_out/SUMMARY_MAXBIN2 -C MAXBIN2

`-p` <profile>: Die Fusion PROFILE.db.
`-c` <contigs>: Die contigs Datenbankdatei.
`-C` <collection>: Geben Sie einen Namen fuer die Ausgabesammlung von Behaeltern.
`--driver` <binner>: Waehlen Sie das Binning-Programm.
`--log-file` <file>: Ausgabeprotokolldatei, um alles aufzuzeichnen, was waehrend dieses Befehls passiert.
`--just-do-it`: Sie muessen diese Option aktivieren, weil anvi-cluster-contigsist ein experimenteller Workflow, der sich noch in der Entwicklung befindet und unerwartete Fehler verursachen kann.
`-o` <dir>: Ausgabeverzeichnis zum Speichern der Zusammenfassung.


## Frage zu Aufgaben 6-11

**How many Archae bins did you get from MetaBAT2?** = 3
**How many Archae bins did you get from Maxbin2?** = 1

## 12. Evaluating MAGs Quality
- ist alles so  vorgegeben
`anvi-estimate-genome-completeness` -c ../day3/contigsdb_out/contigs.db -p ../day3/profile_out/merged_profiles/PROFILE.db -C METABAT2
`anvi-interactive` -p day3/profile_out/merged_profiles/PROFILE.db -c  day3/contigsdb_out/contigs.db -C METABAT2

## 13. Examining bins manually

