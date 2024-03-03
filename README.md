# BIOL0044

df['HG37.Formatted'] = df.apply(lambda row: f"chr{row['CHR']}:{row['HG37.Location']}-{row['HG37.Location']}", axis=1)
output_file_path_hg37 = '/mnt/data/snp_chromosome_start_end_hg37_formatted.csv'
df[['SNP', 'CHR', 'HG37.Location', 'HG38.Location', 'HG37.Formatted']].to_csv(output_file_path_hg37, index=False)
output_file_path_hg37

Lift Genome Annotations
Output hg location










1. Extract 90 signal from Nall _et al_(2019) paper._
70,711 genes from **ENSEMBL**
3,099,706,404 bp
1 gene every 43836.2688125 bp

**Genecards**: 3,099,706,404/466,053
1 gene every 6650.97403943 bp

4. Convert coordinates  from the GRCh37 site to the corresponding GRCh38 location.
5. 
