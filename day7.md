# what i learned today

#Day7



 ## Allgemein Theorie:
**`Pangenomcs`**       
**`repititve Element`**

- werden wir die Genome von 52 Vibrio Jasicida-Sorten mit Anvio vergleichen
- Wir werde ein Pangenom erstellen und visualisieren es

## 1. Erforderliche Module laden und benoetigte Daten herunterladen
**Program or Database** 	**Function**
anvi'o 	                    Wrapper for genome comparissons
DIAMOND 	                creates high-throughput protein alignments
pyANI 	                    calculates genome similarities based on average nucleotide identity
KEGG 	                    Kyoto Encyclopaedia of Genes and Genomes (Database)
NCBI COG 	                Clusters of Orthologous Genes (Database)

- die verwendeten Datewn werden ueber das Terminal geladen

## 2. Contigs.dbs aus den fasta files erstellen
- command ist vorgegeben, es wird in den neu erstellten Ordner jascida gewechselt und dort aus den fasta files eine text datei erstellt
`cd` $WORK/V_jascida_genomes
`ls *fasta` | awk 'BEGIN{FS="_"}{print $1}' > genomes.txt

- entferne alle Contigs die kleiner sind als 2500 nt
- fuer alle ... in dem textfile genomes fuehre das anvi-script-reformat fasta durch und gebe aus also erstelle eine neue datei mit der endung 2.5K
`for g in cat genomes.txt`
`do`
    `echo`
    `echo` "Working on $g ..."
    `echo`
    `anvi-script-reformat-fasta` ${g}_scaffolds.fasta \
                               --min-len 2500 \
                               --simplify-names \
                               -o ${g}_scaffolds_2.5K.fasta
`done`

- generiere eine Contigs-Datenbank
- fuer alle ... in der textdatei genomes fuehre den command anvi -gen-contigs-database aus und gebe jeweils eine datei aus mit dem schema V_jascida... .db
`for g in cat genomes.txt`
`do`
    `echo`
    `echo` "Working on $g ..."
    `echo`
    `anvi-gen-contigs-database` -f ${g}_scaffolds_2.5K.fasta \
                              -o V_jascida_${g}.db \
                              --num-threads 4 \
                              -n V_jascida_${g}
`done`

- annotiere die Contigs.db

`for g in *.db`
`do`
    `anvi-run-hmms` -c $g --num-threads 4
    `anvi-run-ncbi-cogs` -c $g --num-threads 4
    `anvi-scan-trnas` -c $g --num-threads 4
    `anvi-run-scg-taxonomy` -c $g --num-threads 4
`done`

## 3. Visualisierung der Contigs.db
- wurde interaktiv durchgefuehrt mit allen erstellten .db files aus aufgabe 2
- ergebnisse in den Screenshot von day7 zu sehen
`anvi-display-contigs-stats` $WORK/V_jascida_genomes/db_out/*db

## 4. Bilde eine external Genome file
- ich muss nur den weg zu den Datein angeben
- und den weg angeben wo die Datein ausgegeben werden sollen
`anvi-script-gen-genomes-file` --input-dir $WORK/V_jascida_genomes \
                             -o $WORK/V_jascida_genomes/external-genomes.txt

## 5. Kontaminationen untersuchen
- wird direket im Terminal ausgefuehrt
- wechsel erst in das Verzeichnis wo die benoetigte Datei liegt
`cd` V_jascida_genomes
`anvi-estimate-genome-completeness` -e external-genomes.txt

genome name   | domain   |   confidence |   % completion |   % redundancy |   num_splits |   total length |
+===============+==========+==============+================+================+==============+================+
| V_jascida_12  | BACTERIA |            1 |            100 |           5.63 |          282 |        5814827 |
+---------------+----------+--------------+----------------+----------------+--------------+----------------+
| V_jascida_13  | BACTERIA |            1 |            100 |           5.63 |          283 |        5812569 |
+---------------+----------+--------------+----------------+----------------+--------------+----------------+
| V_jascida_14  | BACTERIA |            1 |            100 |           5.63 |          290 |        5861661 |
+---------------+----------+--------------+----------------+----------------+--------------+----------------+
| V_jascida_47  | BACTERIA |            1 |            100 |           5.63 |          282 |        5821176 |
+---------------+----------+--------------+----------------+----------------+--------------+----------------+
| V_jascida_52  | BACTERIA |            1 |            100 |          21.13 |          362 |        6303185 |
+---------------+----------+--------------+----------------+----------------+--------------+----------------+
| V_jascida_53  | BACTERIA |            1 |            100 |           5.63 |          289 |        5853972 |
+---------------+----------+--------------+----------------+----------------+--------------+----------------+
| V_jascida_55  | BACTERIA |            1 |            100 |           5.63 |          289 |        5857980 |
+---------------+----------+--------------+----------------+----------------+--------------+----------------+
| V_jascida_ref | BACTERIA |            1 |            100 |           5.63 |          298 |        5989523 


## 6. Visualisierung von Contigs zur Verfeinerung (Refinement)

`anvi-profile` -c V_jascida_52.db \
             --sample-name V_jascida_52 \
             --output-dir V_jascida_52 \
             --blank

- wir gucken uns jetzt jascida 52 an da das das einzige ist das eine Redundancy von 21% hat
- das ganze wird wieder Interaktiv angeguckt also ist das ergebnis auf screenshot zu sehen

## 7. Aufspaltung des Genoms in die guten Bins

`anvi-split` -p V_jascida_52/PROFILE.db \
           -c V_jascida_52.db \
           -C default \
           -o V_jascida_52_SPLIT

- neue Textfile wird erstellt mit den guten Bins
`sed` 's/V_jascida_52.db/V_jascida_52_SPLIT\/V_jascida_52_CLEAN\/CONTIGS.db/g' external-genomes.txt > external-genomes-final.txt


## 8. Schaetzung der Vollstaendigkeit des Split vs. unsplit-Genoms
- erstelle eine uebersicht fuer die Vollstaendigkeit des spilt und unspilt genoms
- dabei soll das in einer txt datei ausgegeben werden

`anvi-estimate-genome-completeness` -e external-genomes.txt -o genome_completeness.txt

genome name	domain	confidence	% completion	% redundancy	num_splits	total length
V_jascida_12	BACTERIA	1.0	100.00	5.63	282	5814827
V_jascida_13	BACTERIA	1.0	100.00	5.63	283	5812569
V_jascida_14	BACTERIA	1.0	100.00	5.63	290	5861661
V_jascida_47	BACTERIA	1.0	100.00	5.63	282	5821176
V_jascida_52	BACTERIA	1.0	100.00	21.13	362	6303185
V_jascida_53	BACTERIA	1.0	100.00	5.63	289	5853972
V_jascida_55	BACTERIA	1.0	100.00	5.63	289	5857980
V_jascida_ref	BACTERIA	1.0	100.00	5.63	298	5989523

## 9. Berechne das Pangenom
- generate a genome storage database
- hierbei wird die Datei V_jascida-GENOMES.db in den Ordner V_jascida ausgegeben
`anvi-gen-genomes-storage` -e external-genomes.txt \
                         -o V_jascida-GENOMES.db

- calculate the pangenome indem V_jascida-GENOMES.db verwendet wird
anvi-pan-genome -g V_jascida-GENOMES.db \
                --project-name V_jascida \
                --num-threads 8   

- berechnung des ANI 
`anvi-compute-genome-similarity` -e external-genomes.txt \
                 -o ani \
                 -p V_jascida/V_jascida-PAN.db  \
                 -T 8                 

## 10. Display the Pangenom
- im interaktiven durchfuehren
- wichtig immer gucken wo man grade ist und dann da hin

cd $WORK/V_jascida_genomes
anvi-display-pan -p V_jascida/V_jascida-PAN.db \
                 -g V_jascida-GENOMES.db

ANI-Percentage
key	V_jascida_12	V_jascida_13	V_jascida_14	V_jascida_47	V_jascida_52	V_jascida_53	V_jascida_55	V_jascida_ref
V_jascida_12	1.0	0.99998142  	0.981554420 	0.9881690343	0.9809113056894888	0.9815684023896704	0.9815456033153432	0.9755564590609236
V_jascida_13	0.9999805463431664	1.0	0.9816644837587007	0.9882190399556049	0.9810653342375509	0.9816766943746376	0.9816607812802167	0.9755823670368206
V_jascida_14	0.9815676719984583	0.9815008384734002	1.0	0.9820643396226415	0.9835239405415641	0.9999347550631817	0.9999895638629284	0.9763137181234184
V_jascida_47	0.9878233941740413	0.9878163901179943	0.9818483310901749	1.0	0.9813492394039095	0.9818361603537781	0.9818319746202653	0.9754133833853356
V_jascida_52	0.9800775991569266	0.9800926236105788	0.9829853599698454	0.9810547135717032	1.0	0.983030850301659	0.9829575598266442	0.9762250369362364
V_jascida_53	0.9816221074937391	0.9815526873434791	0.9999783653679655	0.9822492066243022	0.9837172763457165	1.0	0.9999787911326636	0.9765505051687147
V_jascida_55	0.981733575699132	0.9816768492094099	0.9999894203149333	0.9821014406779662	0.9835872116843704	0.9999269210799585	1.0	0.9764966893865628
V_jascida_ref	0.9750974795799299	0.9750771903558235	0.9759950965853658	0.9752066354775829	0.9761173500588005	0.9759986346791496	0.975981523902439	1.0

- search function umstellen 

## 11. Fragen zu den Aufgaben

**Werden Gene aufgrund von Sequenzaehnlichkeit oder funktionaler Annotation gruppiert?**
    - beides
    - im pangenom zunaechst nach sequenzaehnlichkeit (clustering), anschliessend eine funktionale annotation um jeden cluster eine biologische funktion zuzuweisen zu koennen

**Wie erkennt man ein schlechtes Genom oder einen schlechten Bin in einem Genom?**
    - Completness >90%
    - Contamination <5% (redundancy als marker fuer zu viel fremdes dna material)

**Verwenden Sie die Suchfunktion, um alle Gencluster den folgenden Bins zuzuordnen: Kerngenom, akzessorisches Genom, Singletons und Single Copy Core Genes (SCGs). Fuegen Sie dem Protokoll einen Screenshot Ihres Pangenoms bei.**
    - Kerngenom: Gene die in allen Genomen vorkommen
    - Akzessorisches Genom: Gene die nur in einigen Genomen vorkommen
    - Singletons: Gene die nur in einem einzigen Genom vorkommen
    - SCGs: Gene die einmal pro Genom vorkommen (oft Referenzmarker)

**Wenn Sie dem Pangenom weitere Genome hinzufuegen, wie wuerde sich dies auf die Anzahl der Gencluster im Kerngenom und in den SCGs auswirken?**
    - Kerngenom: Anzahl der Cluster wueder sinken, da mehr Genome bedeuten, dass weniger Gene in allen Genomen glichzeitig vorkommen
    - SCGs: bleiben stabil - evolutionaer bedingt
    - Akzessorische und Singletons wuerden zunehmen

**Wuerden Sie aufgrund des ANI sagen, dass alle Genome zur selben Spezies gehoeren?**
    - ANI groesser oder gleich 95% - Genome gehoehren zur selben Spezies
    - ANI kleiner als 95% - es handelt sich um unterschiedliche Spezies


