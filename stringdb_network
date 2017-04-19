suppressMessages(suppressWarnings(library(STRINGdb)))
suppressMessages(suppressWarnings(library(data.table)))
args <- commandArgs(TRUE)

string_db <- suppressMessages(suppressWarnings(STRINGdb$new(version="10", species=9606,
                                                            score_threshold=0, input_directory=args[2])))

selected_protein <- suppressMessages(suppressWarnings(fread(args[1],sep=',',header=TRUE)))
map_name <- colnames(selected_protein)[1]

#判断是单个蛋白还是蛋白列表，并分别做不同处理
if(nrow(selected_protein) > 1){
  map_protein <- suppressWarnings(suppressMessages(string_db$map(selected_protein, map_name, removeUnmappedRows = TRUE)))
  net_int = map_protein$STRING_id
} else {
  str_id <- string_db$mp(selected_protein[1,1])
  net_int <- string_db$get_neighbors(str_id)[1:10]
  net_int[11] <- str_id
}

Prefix <- round(runif(1,0,100000000000))
Prefix_name <- paste0(Prefix,'.png')
Prefix_name <- paste0(args[2],Prefix_name)
png(Prefix_name)
string_db$plot_network(net_int,add_link=FALSE,add_summary=FALSE)
dev.off()

print(Prefix_name)
string_db$get_link(net_int)
