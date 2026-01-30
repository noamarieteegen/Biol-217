# what i learned today

#Day4

- **Interactive**: 
        - $exit immer hinterher so kommt man daraus
    1. Cau Cluster normal anmelden
    2. srun --pty --x11 --partition=interactive --nodes=1 --tasks-per-node=1 --cpus-per-task=1 --mem=10G --time=01:00:00 /bin/bash in Terminal eingeben
    3. module load gcc12-env/12.1.0
    module load micromamba 2> /dev/null
    micromamba activate $WORK/.micromamba/envs/00_anvio/
    cd $WORK (anvio starten)
    4. command den man sonst in bash eingibt in den terminal eingeben
    5. ssh -L localhost:8080:localhost:8080 sunamNNN@caucluster.rz.uni-kiel.de das jetzt im neuen Terminal eingeben
    6. ssh -L localhost:8080:localhost:8080 nNNN das auch in den 2. Terminal eingeben - dabei darauf achten auf den  hostname und welche Nummer NNN ist





- ganz unbedingt nochmal durchgehen - verstehe hier das Prinzip nicht

## 1. Fragen zu MAGs Qualitaet von day3

**Which binning strategy gives you the best quality for the A R C H A E A bins?**
**How many A R C H A E A bins do you get that are of High quality?** = 1
**How many B A C T E R I A bins do you get that are of High quality?** = 4

## 2. Detecting Chimaeras in MAGs
### wir arbeiten nur mit den Archaae Bins weiter

- zuerst muss micromamba gunc aktiviert werden
`micromamba activate 00_gunc`

- wieder wechsel in das Hauptverzeichnis Work
`cd` $WORK

- Es wird ein Verzeichnis im Ordner WORK erstellt welcher day4 heisst
`mkdir` -p day4

`cd` $WORK/day4

- Es wird ein Verzeichnis im Ordner WORK/day4 erstellt welcher refine_out heisst
`mkdir` -p refine_out

`cd` $WORK/day4/refine_out

- Es wird ein Verzeichnis im Ordner WORK/day4/refine_out/METABAT__13 erstellt welcher gunc_out heisst
`cd` $WORK/day4/refine_out/METABAT__13
`mkdir -p` gunc_out

`cd` $WORK

- check for chimeras and potential contamination
`gunc` run -i day4/refine_out/METABAT__13/METABAT__13-contigs.fa -r $WORK/databases/gunc/gunc_db_progenomes2.1.dmnd  --out_dir day4/refine_out/METABAT__13/gunc_out --detailed_output --threads 12

## 3. Erstellen von interaktiven Dots der Chimaeren
- Vorgegebene Struktur: gunc plot -d ? -g ? --out_dir ?

`cd` $WORK/day4/METABAT__13/gunc_out
`mkdir` -p diamond_output

- After running chimera detection, you can visualize the results in a plot.
`gunc plot` -d day4/refine_out/METABAT__13/gunc_out/diamond_output/*.diamond.progenomes_2.1.out -g day4/refine_out/METABAT__13/gunc_out/gene_calls/gene_counts.json --out_dir day4/refine_out/METABAT__13/gunc_out

## 4. Fragen zu Aufgabe 3.

**Do you get A R C H A E A bins that are chimeric?** =
**In your own words, briefly explain what a chimeric bin is.** = 


genome	n_genes_called	n_genes_mapped	n_contigs	taxonomic_level	proportion_genes_retained_in_major_clades	genes_retained_index	clade_separation_score	contamination_portion	n_effective_surplus_clades	mean_hit_identity	reference_representation_score	pass.GUNC
METABAT__13-contigs	1970	1893	261	kingdom	0.99	0.95	0.0	0.0	0.0	0.83	0.79	True
METABAT__13-contigs	1970	1893	261	phylum	0.98	0.94	0.0	0.0	0.0	0.83	0.79	True
METABAT__13-contigs	1970	1893	261	class	0.98	0.94	0.0	0.0	0.0	0.83	0.79	True
METABAT__13-contigs	1970	1893	261	order	0.97	0.93	0.0	0.0	0.0	0.84	0.78	True
METABAT__13-contigs	1970	1893	261	family	0.96	0.92	0.0	0.0	0.0	0.84	0.78	True
METABAT__13-contigs	1970	1893	261	genus	0.95	0.91	0.0	0.0	0.0	0.84	0.77	True
METABAT__13-contigs	1970	1893	261	species	0.89	0.85	0.1	0.37	1.04	0.85	0.72	True


## 5. Manual bin refinement
- Da grosse Metagenombaugruppen zu Hunderten von Behaeltern fuehren koennen, waehlen Sie einige der besseren fuer die manuelle Verfeinerung vor, z. B. > 70% Vollstaendigkeit.

`cp` day3/profile_out/merged_profiles/PROFILE.db day4/refine_out/PROFILE_refined.db

`anvi-refine` -c day3/contigsdb_out/contigs.db -p day4/refine_out/PROFILE_refined.db --bin-id METABAT__13 -C METABAT2 

## 6. Frage zu Aufgabe 5.

**How much could you improve the quality of your A R C H A E A ?** = sehr gute Qualitaet (98.6%/2,6%)
        **Compare the completeness and redundance of the bin before and after refining.**

## 7. Visualizing Coverage
- How abundant are the Archaea MAGs, actually?
`anvi-interactive` -p day3/profile_out/merged_profiles/PROFILE.db -c day3/contigsdb_out/contigs.db -C METABAT2

## 8. Frage zu Aufgabe 7.h
**How abundant (relatively) are the A r c h a e a bins in the 3 samples?**
       bins	 BGR_130305_sorted	BGR_130527_sorted	BGR_130708_sorted
    - METABAT__13 	1.76 	1.12 	0.57
    - METABAT__33 	0.83 	0.01 	0.39
    - METABAT__34	0.79	0.28	0.39

        
