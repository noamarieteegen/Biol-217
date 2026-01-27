# what i learned today

#Day2

1. **Allgemeines**
    * `Warum Mikrobiologie` = verstehen der mikrobiuellen Welt - kleine Erzeuger, grosse Wirkung. Neue Biotechnologie.
    * `Warum Metagenomics` = Teilgebiet der Mikrobiologie - gesamte genetische Material aller Gene
    *  `16S-rRNA` = universell vorhanden, variable Bereiche die sich artspezifisch unterscheiden,

2. **Background**
    * `Amplicon Sequenzing` = es wird ein bestimmtes Gen sequenziert (nachteil: keine Funktionsgene, keine Viren, ...)
    * `Shotgun Sequenzing` = gesamte DNA der Probe wird sequenziert

    * Sample - Extrakt DNA - Libary Praeperation - Sequenzing - Bioinformatik Prozessing - Downstream Analyse

    * `MAG` = Metagenome Assembled Genomes (wir sequenzieren eine community)
    * Genomes - Shot Sequenzing - Shot Reads - De-Novo-Assembly - Metagenomic Binding - MAG

    * `FASTQ-Format` = bestehend aus vier Linien: SequenceID, Raw Sequence/Barcode, +, Phred Quality

    3. **Workflow-Aufgaben**

    4. **Aufgabe 1**

    # WICHTIG: Micromamba muss aktiviert werden
    `micromamba activate 00_anvio`

    # Zum Verzeichnis Work wechslen - immer bei Cau-Cluster
    `cd` $WORK
    # Es wird ein Verzeichnis im Ordner WORK erstellt welcher day2 heisst
    `mkdir -p` day2
    # Es wird in das Verzeichnis day2 gewechselt, welches sich im Ordner WORK befindet
    `cd` $WORK/day2


    # Es werden Ordner erstellt die die Ergebnisse abspeichern sollen, diese befidnen sich im WORK-day2-Ordner
    `mkdir -p` fastqc_out
    `mkdir -p` fastp_out

    5. **Aufgabe 2**
    # WICHTIG: es muss dem System gesagt werden, dass es zum WORK-Ordner geht und wo lang es dann gehen soll
    ## hier wird die Qualitaet der Daten evaluiert
    `fastqc` ../metagenomics/0_raw_reads/*.fastq.gz -o fastqc_out

    6. **Aufgabe 3**
    # Das vorgegebene Schema: fastp -i ? -I ? -o ? -O ? -t 6 -q 20 -h ? -R ?
    # erst muss wieder angegeben werden in welchen Ordner wir wollen
    `cd` $WORK/day2
    `fastp` `-i` ../metagenomics/0_raw_reads/BGR_130305_mapped_R1.fastq.gz `-I` ../metagenomics/0_raw_reads/BGR_130305_mapped_R2.fastq.gz `-o` fastp_out/BGR_130305_mapped_R2_R1_clean.fastq.gz `-O` fastp_out/BGR_130305_mapped_R2_R2_clean.fastq.gz ``-t 6 -q 20 -h` sample1.html -R "Sample1 Fastp Report"
    `fastp` `-i` ../metagenomics/0_raw_reads/BGR_130527_mapped_R1.fastq.gz `-I` ../metagenomics/0_raw_reads/BGR_130527_mapped_R2.fastq.gz `-o` fastp_out/BGR_130527_mapped_R2_R1_clean.fastq.gz `-O` fastp_out/BGR_130527_mapped_R2_R2_clean.fastq.gz `-t 6 -q 20` -h sample2.html -R "Sample2 Fastp Report"
    `fastp -i` ../metagenomics/0_raw_reads/BGR_130708_mapped_R1.fastq.gz -I ../metagenomics/0_raw_reads/BGR_130708_mapped_R2.fastq.gz -o fastp_out/BGR_130708_mapped_R2_R1_clean.fastq.gz -O fastp_out/BGR_130708_mapped_R2_R2_clean.fastq.gz -t 6 -q 20 -h sample3.html -R "Sample3 Fastp Report"

    7. **Assembly**
    # megahit -1 ? -1 ? -1 ? -2 ? -2 ? -2 ? -o ? --min-contig-len 1000 --presets meta-large -m 0.85 -t 12     
 
    `megahit` -1 fastp_out/BGR_130305_mapped_R2_R1_clean.fastq.gz -1 fastp_out/BGR_130527_mapped_R2_R1_clean.fastq.gz -1 fastp_out/BGR_130708_mapped_R2_R1_clean.fastq.gz -2 fastp_out/BGR_130305_mapped_R2_R2_clean.fastq.gz -2 fastp_out/BGR_130527_mapped_R2_R2_clean.fastq.gz -2 fastp_out/BGR_130708_mapped_R2_R2_clean.fastq.gz -o assembly --min-contig-len 1000 --presets meta-large -m 0.85 -t 12 

 


