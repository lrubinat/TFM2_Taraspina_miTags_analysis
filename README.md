# Taraspina miTags analysis


Original dataset:
	--> "otu_table99.txt" [93771 seq.]
	annotation through "miTags_workflow.sh"


## 1) Select SRF samples:

	#substitute "-" for "_":
	$ sed "s/-/_/g" otu_table99.txt > otu_table99_nohyphen.txt


	#select SUR columns and remove rows with no occurrence in any sample:
	$ R select_SRF_stations.R
	--> "otu_table99_nohyphen_SUR.txt" [38772 OTUs]

	#add the suffix 'TARA_' in Tara stations name (header):
	$ sed 's/\"X/TARA_/g' otu_table99_nohyphen_SUR.txt > otu_table99_nohyphen_SUR_header1.txt

	#remove all '"':
	$ sed 's/\"//g' otu_table99_nohyphen_SUR_header1.txt > otu_table99_nohyphen_SUR_header2.txt

	#add manually 1rst column in header "OTUId", so that we have 56 columns in all rows:
	--> otu_table99_nohyphen_SUR_header3.txt [38772 OTUs]


## 2) Remove mitochondria & metazoa:

	$ grep -v -E "mitochondria|Mitochondria|metazoa|Metazoa" otu_table99_nohyphen_SUR_header3.txt > otu_table99_nohyphen_SUR_header3_nomit_nomet.txt
	--> otu_table99_nohyphen_SUR_header3_nomit_nomet.txt [38007 OTUs]


## 3) Split 16S & 18S into separated tables:

	[ 18S ]
	#select SILVA 18S sequences:
	$ grep -E "SILVA.v123|OTUId" otu_table99_nohyphen_SUR_header3_nomit_nomet.txt | grep -E "Eukaryota|OTUId" > otu_table99_SUR_miTags_18S.txt
	--> otu_table99_SUR_miTags_18S.txt [9039 OTUs]

	[ 16S ]
	#select SILVA, CyanoDB, and Phytoref bacteria sequences:
	$ grep -E "Bacteria|OTUId" otu_table99_nohyphen_SUR_header3_nomit_nomet.txt > otu_table99_SUR_miTags_16S.txt

	#add Phytoref plastidial sequences:
	$ grep "PhytoREF" otu_table99_nohyphen_SUR_header3_nomit_nomet.txt >> otu_table99_SUR_miTags_16S.txt
	--> otu_table99_SUR_miTags_16S_v2.txt [27898 OTUs]


## 4) Standarize classification:


	[ 18S ]
	$ python3 A_add_OTUtb_classif_18S.py
	--> otu_table99_SUR_miTags_18S_classif.txt [8643 seq.]

		5669 NA
		1500 Dinophyceae
		 393 Bacillariophyceae
		 252 Chrysophyceae
		 179 Prymnesiophyceae
		 117 Mamiellophyceae
		 107 Cryptophyceae
		  60 Trebouxiophyceae
		  53 Dictyochophyceae
		  52 Chlorophyceae
		  38 Prasinophyceae_clade-VII
		  27 Eustigmatophyceae
		  26 other_Prasinophyceae
		  26 Pelagophyceae
		  25 Bolidophyceae
		  21 Chlorarachniophyceae
		  20 Pyramimonadaceae
		  17 Prasinophyceae_clade-IX
		  15 Rhodophyceae
		  14 Pavlovophyceae
		  12 Xanthophyceae
		   7 Nephroselmidophyceae
		   5 Ulvophyceae
		   4 Phaeophyceae
		   2 IncertaeSedis_Archaeplastida
		   1 classif
		   1 Phaeothamniophyceae


	 [ 16S ]
	 $ python3 B_add_OTUtb_classif_16S.py
	 --> otu_table99_SUR_miTags_16S_classif.txt [28295 seq.]

		27500 other_bacteria
		 188 other_cyanobacteria
		 173 Bacillariophyceae
		 107 Synechococcus
		 102 Prochlorococcus
		  50 Prymnesiophyceae
		  32 other_plastids
		  32 Cryptophyceae
		  29 other_Prasinophyceae
		  15 Mamiellophyceae
		  10 Pelagophyceae
		   8 Dinophyceae
		   7 Pyramimonadaceae
		   7 Dictyochophyceae
		   7 Bolidophyceae
		   5 Chlorarachniophyceae
		   4 Trebouxiophyceae
		   4 Rappemonads
		   4 Chlorodendrophyceae
		   3 Phaeophyceae
		   2 Eustigmatophyceae
		   2 Chlorophyceae
		   1 class_B
		   1 Pinguiophyceae
		   1 Pavlovophyceae
		   1 Nephroselmidophyceae



