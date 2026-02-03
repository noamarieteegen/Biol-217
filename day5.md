# what i learned today

#Day5

## 1. Taxanomic Assignment
- You will now add taxonomic annotations to your MAG.
- Verknuepft die Single-Copy-Core-Gene in Ihrer Contigs-Datenbank mit taxonomischen Informationen.

`cd` $WORK
`anvi-run-scg-taxonomy` -c day3/contigsdb_out/contigs.db -T 20 -P 2

`-p` <profile>: Die Fusion PROFILE.db.
`-c` <contigs>: Die contigs Datenbankdatei.
`-C` <collection>: Geben Sie einen Namen fuer die Ausgabesammlung von Behaeltern.

- Dieses Programm erstellt schnelle taxonomische Schaetzungen fuer Genome, Metagenome oder Bins, die in Ihrer Contigs-Datenbank gespeichert sind, unter Verwendung von Single-Copy-Core-Genen.
- hier wird fuer jede Datei eine Text-Datein erstellt 

`mkdir` -p day5
`cd` $WORK/day5

`anvi-estimate-scg-taxonomy` -c day3/contigsdb_out/contigs.db -p day3/profile_out/BGR_130305_profile/PROFILE.db --metagenome-mode --compute-scg-coverages --update-profile-db-with-taxonomy > temp130305.txt
`anvi-estimate-scg-taxonomy` -c day3/contigsdb_out/contigs.db -p day3/profile_out/BGR_130527_profile/PROFILE.db --metagenome-mode --compute-scg-coverages --update-profile-db-with-taxonomy > temp130527.txt
`anvi-estimate-scg-taxonomy` -c day3/contigsdb_out/contigs.db -p day3/profile_out/BGR_130708_profile/PROFILE.db --metagenome-mode --compute-scg-coverages --update-profile-db-with-taxonomy > temp130708.txt

- Eine abschliessende Zusammenfassung, um umfassende Informationen ueber Ihre METABAT2-Behaelter zu erhalten:

`anvi-summarize` -p day3/profile_out/merged_profiles/PROFILE.db -c day3/contigsdb_out/contigs.db -o day5/SUMMARY_METABAT2_FINAL -C METABAT2


## 2. Genome Dereplication

`anvi-dereplicate-genomes` -i /PATH/TO/file.txt --program fastANI --similarity-threshold 0.95 -o ANI --log-file log_ANI -T 10


## 3. Fragen zu Aufgabe 1

**Haben Sie eine Artenzuordnung zu den zuvor identifizierten Archaea erhalten?** 
    - ja siehe Bild day5_taxonomy

**Does the HIGH-QUALITY assignment of the bin need revision?**


**How many species do you have in the dataset?**
    - 3

**Try to dereplicate again at 90% identity then at 80%identity. In your own words, explain the differences between the different %identities.**
    - 95% = standard fuer Speziesgrenze - sehr aehnliche Genome werden zusammengefasst
    - 90% = mehr Genome werden zusammengefasst - weniger Cluster, auch leicht unterschiedliche Spezies werden gut gruppiert
    - 80% = noch mehr Zusammenfassung - noch weniger Cluster, auch unterschiedliche Spezies/Gattungen koennten gruppiert werden
