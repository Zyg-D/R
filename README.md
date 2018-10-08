# R

# Returning a sorted data frame on specified column  
    Dataset[order(-Dataset$Age),]  #desc  
Dataset[order(Dataset$Age),]   #asc  
  
# creating a matrix from another, taking only numeric cols  
numDataset = Dataset[,sapply(Dataset, is.numeric)]  
  
# creating a data frame obj  
fakeddf = data.frame(col1 = c("c1", "c2"),  
                     col2 = c(1, 2),  
                     col3 = c("c21", "c22"),  
                     col4 = c("c31", "c32"),  
                     col5 = c(2, 3))  

