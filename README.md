# LearningPython
The basics of python and regex are explored. The key concepts include using basic syntax to compute an arithmetic, using regex to isolate a part of string and using regex to find certain components of information in a file or dataset. 


**Outline**
+ Computing the Circumference Using Arithmetic Operators in Python
+ Extracting the Area Code from a Phone Number
+ Extracting Key Information Pertaining to a Gene from a Dataset Contained in a File

## Downloading the DataSet
The required data set can be downloaded from the following link: http://ftp.ensembl.org/pub/release-75/gtf/homo_sapiens 
Using wget and gunzip, the contents of the file can be saved. 

## Code

### **Code 1: Circumference**
The cicumference of a circle of radius 3 is calculated. The radius is defined within the code script
```
#!/usr/bin/python
pi = 3.14159
radius = 3
cir = ((2*pi)*3)
print "The circumference of a circle of radius 3 is ", cir
```
Explanation: The value of the constant pi and the variable radius are defined within the script. The circumference of a circle can be defined by the formula 2*pi*r. This operation is computed using '*' as the operator and brackets to first multiply the value of pi by 2 and the result of that calculation by the radius. 

### **Code 2: Area Code**
The area code is extracted from a phone number using the functions of Regex
```
#!/usr/bin/python\n

import re
phone = "602-343-8747"
match = re.search(r'^\d\d\d', phone)
if match:
        print 'The area code is:', match.group()
```
Explanation: A phone number is defined within the script. The RegEx syntax to search for a particular section is re.search(r'search criteria', search location). This is stored as match in the code. The regular expression to find the first 3 digits in a sequence of numbers is ^\d\d\d. '^' indicates the beginning of the string and \d indicated digits. 
 
 ### **Code 3: Data Wrangling of Gene Dataset**
A data set containing the information pertaining to a genome is taken as an argument. 

This is an example of a few lines of data in the file: 
```
1	pseudogene	gene	11869	14412	.	+	.	gene_id "ENSG00000223972"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene";

1	processed_transcript	transcript	11869	14409	.	+	.	gene_id "ENSG00000223972"; transcript_id "ENST00000456328"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana";

1	processed_transcript	exon	11869	12227	.	+	.	gene_id "ENSG00000223972"; transcript_id "ENST00000456328"; exon_number "1"; gene_name "DDX11L1"; gene_source "ensembl_havana"; gene_biotype "pseudogene"; transcript_name "DDX11L1-002"; transcript_source "havana"; exon_id "ENSE00002234944";

From this, if the third column contains the element "gene" that line is taken for processing. From those selected lines, the Genename, chromosome number, starting position and ending position are extracted and printed. The output is in JSON format. 
```
The aim of the code is to first check whether the third column contains the type "Gene", then extract the gene name, chromosome number, starting position and ending position and return them in the JSON format. 
An example of JSON format is: 
{"geneName":"OR4G4P","chr":"1","startPos":52473, "endPos":54936},
```
#!/usr/bin/python
import sys
import re

output = {}
f = open(sys.argv[1],'r')
lines=f.readlines()
for line in lines:
        if re.search(r'.*\t.*\tgene\t',line):
                name = re.findall(r'gene_name\s("\S+\")',line)
                print "{\"geneName\":",
                print  name,
                print ",",
                chrom = re.findall(r'^(\d)',line)
                print "\"chr\":",
                print chrom,
                print ",",
                start = re.findall(r'\sgene\s(\d+)\s\d+',line)
                print "\"startPos\":",
                print start,
                print ",",
                end = re.findall(r'(\d+)\s\.',line)
                print "\"endPos\":",
                print end,
                print "}"
```
 Explanation: The functions of Sys and Re are imported. The file is opened and the file containing the data is taken as an argument by accessing location one. This is stored in 'f' and the lines contained in f are read. An iterative loop is created by using a for statement that ensures that the code is carried forward for every line of the data. The condition of having the third column be 'gene' is checked with a regular expression statement. If this holds true, the gene name, chromosome number, starting position and ending position are found. Each element required is printed indivually in the JSON format. Using 'findall' ensures that the data is checked from start to finish for a match. 
 Also, by using grouping combined with findall ensures that the specified group marked by "(search criteria)" is returned, not the entire regular expression. 
