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
library(biomaRt)
library(openxlsx)

# Load SNP information with new coordinates
snp_info <- read.csv("path/to/BP conversion.csv")  # Update this path to your CSV file location

# Initialize an empty data frame to collect all results
all_results <- data.frame()

# Function to query BioMart and append results for a single SNP
queryAndAppendForSNP <- function(snp, chr, bp, results_df) {
  start <- bp - 25000
  end <- bp + 25000
  mart <- useMart("ensembl", dataset="hsapiens_gene_ensembl")
  
  filters <- list(
    chromosome_name = as.character(chr),
    start = as.character(start),
    end = as.character(end)
  )
  
  attributes <- c(
    "ensembl_gene_id", "external_gene_name", "phenotype_description", 
    "chromosome_name", "band", "strand", "start_position", "end_position"
  )
  
  # Execute query
  results <- getBM(attributes=attributes, filters=filters, mart=mart)
  
  if(nrow(results) > 0) {
    results$SNP <- snp  # Add SNP column to results
    results_df <- rbind(results_df, results)  # Append to the accumulating data frame
  }
  
  return(results_df)
}

# Iterate over SNPs and perform queries, appending results
for (i in 1:nrow(snp_info)) {
  all_results <- with(snp_info[i, ], queryAndAppendForSNP(SNP, CHR, HG38.Location, all_results))
}

# Save all results in a single Excel file
write.xlsx(all_results, file = "All_SNP_Results.xlsx")
