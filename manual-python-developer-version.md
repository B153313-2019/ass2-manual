---
description: Manual for python coder
---

# Manual - Python Developer Version

* **Modular design was used in this application. Thus, the codes of this application is easier for developer to modify.** 
* **In order to make it easier for developers to understand the codes of this application, this manual will explain them by using each module as a unit.**

### Unit of two extra function

* **The first unit is about the modules that are imported. Then, there are two useful functions.**
* First function is used to create a progress bar which can monitor and show the progress of a loop. ****Variable "iteration" represent the current loop number, "total" represent whole loop number, and other arguments control the shape of bar.
* Next function is used to ask user whether to continue. It use 2 argument Variable "query" should be a interrogative sentence, this sentence should ask user whether process a task, and variable "ProgramName" should be the name of next program that should be excuted.

```python
# I have written simple comments for each function
# For more details, please see the python developer manual


import os,sys,time,re,numpy
import subprocess as sp
import xml.dom.minidom


# A simple function that create a progress bar which can monitor and show the progress of a loop.
# Variable "iteration" represent the current loop number, "total" represent whole loop number. 
def ProgressBar (iteration, total, prefix = '', suffix = '', decimals = 1, length = 100, fill = '█', printEnd = "\r"):
    percent = ("{0:." + str(decimals) + "f}").format(100 * (iteration / float(total)))
    filledLength = int(length * iteration // total)
    bar = fill * filledLength + '-' * (length - filledLength)
    print('\r%s |%s| %s%% %s' % (prefix, bar, percent, suffix), end = printEnd)
    # Print New Line on Complete
    if iteration == total: 
        print()
    else:
        pass


# The followed function can help you to get a command, and then decide whether execute next code.
'''
	The function use 2 arguments. Variable "query" should be a interrogative sentence, this sentece
	should ask user whether process a task, and variable "ProgrameName" shoule be the name of next 
	programme that should be excuted.
'''
def ProgramExecutionJudgment(query,ProgrameName,symbol):
    startCommand = input(query)
    while startCommand != "yes" and startCommand != "no":
        print("Input is not recognized, please re-enter")
        startCommand = input(query)
    if startCommand == "no":
        print("\n"+"\033[93m"+("The program will quit, thank you for your use".center(70,"*"))+"\033[0m")
        exit()
    elif startCommand == "yes":
        print("\n"+((ProgrameName + " starts executing").center(70,symbol))+"\n")
    else:
        pass
```

### Initiation unit

* **Next unit is initiation part of this application.** 
* The function "Initiation" contains the basic information, simple introduction to user and call function " ProgramExecutionJudgment" to have a start query for user.

```python
# Initiation contains the basic information, simple introduction to user and a start query
def Initiation():
    print("\033[93m"+"\n"+("Programme starts".center(70,"*"))+"\n"+"\033[0m")
    print("="*30+"\n"+"# Assignment 2 of BPSM       #\n"\
        "# Version:2.0.0"+" "*14+"#"\
        +"\n"+"# Updated:2019.11.17"+" "*9+"#"\
        +"\n"+"# Author:LittleTrain"+" "*9+"#"\
        "\n"+"="*30+"\n\n")
    print("This application is really useful!!!".center(70,"="))
    time.sleep(0.5)
    print("# Give the protein family and the taxonomic group")
    time.sleep(0.5)
    print("# Then the relevant protein sequence will be retrived")
    time.sleep(0.5)
    print("# Alignment of these sequences will be completed")
    time.sleep(0.5)
    print("# Similarity graph of aligned sequences will be drawn")
    time.sleep(0.5)
    print("# Motifs of protein sequences will be scanned")
    time.sleep(0.5)
    print("# And other 3 functions wait you to find and use...")
    time.sleep(0.5)
    print("="*70+"\n")
    ProgramExecutionJudgment("Do you want to start task 1 now?(yes or no)","Task 1","=")
```

### Task 1 unit

* **This unit is about the first functional part of this application.**
* In this unit, the first function is aimed to modify the format of a fasta file. As the sequences that we download from NCBI are not in one line, this function can recognize sequences and delete the line break "\n".
* The followed function "GetNames" aim to receive the protein family name and taxonomy group name from use. This function define two global variables for names, thus, they can be used outside this function.
* Next function " ModeChoose" provide 8 modes for user to filter the search result of NCBI. And I define variables "ModeCommand" and "SearchCondition" as global variables. Thus, they could be used in other place.
* The forth function "SeqRequest" can download sequences from NCBI according to the protein name, taxonomy name and filter mode. This function call the shell to use esearch and efetch to download sequences. And then call function "FileFormatting" to modify the downloaded file.
* Fifth function "DataSetCount" call "esearch" to get the information from NCBI and store it in an xml format file. Then analysis this xml file to count the number of search result, and determine whether the number more than 250 or not. Besides, this function have 3 modes that will make different judgments according to different modes.
* **The last function "Task1" is the main function of task 1. In this function, names and filter mode are firstly given by user according to functions "GetNames" and "ModeChoose". Then use "DataSetCount" to judge the number of sequences. If number = 0, re-call "GetNames". If number &gt; 10000, ask user whether continue or re-enter the names and mode. After all things are ready, use "SeqRequest" to download sequences.**

```python
# This function is aimed to modify the format of fasta file
# The sequences that we download from NCBI are not in one line, this fuction can recognize sequences and delete the line break "\n"
def FileFormatting(RawFile,NewFile):
    RawFileData = open(RawFile)
    MidFileData = open("MidFile","w+")
    for line in RawFileData:
        if line[0] == ">":
            MidFileData.write("\n"+line)
        else:
            line = line.replace("\n","")
            MidFileData.write(line)
    RawFileData.close()
    MidFileData.close()
    with open('MidFile', 'r') as Mid:
        data = Mid.read().splitlines(True)
    with open(NewFile, 'w+') as New:
        New.writelines(data[1:])
    New.close()
    Mid.close()
    os.system("rm -f "+ RawFile)
    os.system("rm -f MidFile")


# This function allow user input a protein family name and a taxonomy group name, and store them in two global variables to ensure they can be re-selected	
def GetNames():
    global ProteinFamName,TaxonGroupName
    ProteinFamName = input("Please input a protein family name:")
    TaxonGroupName = input("Please input a taxonomic group name:")


# This function allow user choose the filter condition of sequences search
# And I define variables "ModeCommand" and "SearchCondition" as global variables. Thus they could use in other place.
def ModeChoose():
    print("\nThere are 8 modes to filter sequences:\n"\
          +"1.\"null\"\n2.[TI]\n3.NOT PARTIAL\n4.NOT PREDICTED\n"\
          +"5.[TI] NOT PARTIAL\n6.[TI] NOT PREDICTED\n7.NOT PARTIAL"\
          +"NOT PREDICTED\n8.[TI] NOT PARTIAL NOT PREDICTED\n"\
          +"\nChoose one mode that you want")
    ModeDictionary = {"1":"","2":" [TI] ","3":" NOT PARTIAL ","4":" NOT PREDICTED ","5":" [TI] NOT PARTIAL","6":" [TI] NOT PREDICTED ","7":" NOT PARTIAL NOT PREDICTED ","8":" [TI] NOT PARTIAL NOT PREDICTED "}
    global ModeCommand
    ModeCommand = input("Please enter the index(1-8):")
    while ModeCommand not in ["1","2","3","4","5","6","7","8"]:
        print("Out of options, please re-enter:")
        ModeCommand = input("Please enter the index(1-8):")
    global SearchCondition
    SearchCondition = ModeDictionary.get(ModeCommand)


# This function call the shell to use esearch and efetch to download sequences. And then call function "FileFormatting" to modify the downloaded file
def SeqRequest():
    print("Downloading, please wait.\nIt may take you ten seconds to a few minutes.\nThis is determined by the amount of data.")
    os.system("esearch -db protein -query \""+ TaxonGroupName + "[Organism]+" + ProteinFamName + SearchCondition + "\" | efetch -format fasta > RawProteinSequences.fasta")
    FileFormatting("RawProteinSequences.fasta","ProteinSequences.fasta")
    print("\n"+("Download finished".center(70,"-"))+"\n")
       

# This function count the number of data, and determine whether the number more than 250 or not
# And this function will make different judgments according to different modes
def DataSetCount(Modle = "normal"):
    os.system("esearch -db protein -query \""+ TaxonGroupName + "[Organism]+" + ProteinFamName + SearchCondition + "\" > SeqNum.xml")
    dom = xml.dom.minidom.parse("SeqNum.xml")
    root = dom.documentElement
    itemlist = root.getElementsByTagName('Count')
    global SeqNumber
    SeqNumber = int(itemlist[0].firstChild.data)
    if Modle == 0 :
        if SeqNumber == 0:
            return True
        else:
            return False
    elif Modle == 250:
        if SeqNumber <= 250:
            return True
        else:
            return False
    elif Modle == 10000:
        if SeqNumber > 10000:
            return True
        else:
            return False
    else:
        pass


# Task 1, the beginning of this application.
'''
    Firstly, call function "GetNames()" to let user input the protein family name and taxonomy group name.
    Then, call function "ModeChoose()" to let user choose filter condition.
    Next, call function "DataSetCount()", choose mode 0 to judge the number of sequences corresponding to the names that user given. If it is 0, ask user enter new names, until the number is not 0.
    After that, choose mode 10000 of function "DataSetCount()" to judge the number of sequences wheter it is more than 10000. If it is, give user a warning and let him make a choice.
    Now, all things are ready. Ask user to confirm downloading.
    process the function "SeqRequest()" to download sequences.
'''
def Task1():
    GetNames()
    ModeChoose()
    while DataSetCount(0):
        print("There is no result, please re-enter.")
        GetNames()
        ModeChoose()
    while DataSetCount(10000):
        print("\033[31m" + ("Warning".center(70,"*"))+ "\033[0m")
        print("The number of sequences is "+ str(SeqNumber) +".")
        print("You can continue or exit, but you are highly recommended to change mode or names:")
        print("1.still continue")
        print("2.change to a more precise mode")
        print("3.reassign a protein family and a taxonomy group")
        print("4.exit")
        choice = input("Please make your choice(enter the index of choices):")
        while choice not in ["1","2","3","4"]:
            choice = input("Out of options, please re-enter the index(1-4):")
        if choice == "1":
            break
        elif choice == "2":
            ModeChoose()
        elif choice == "3":
            GetNames()
        elif choice == "4":
            exit()
        else:
            pass
    print("\nThe number of sequences is " + str(SeqNumber))
    ProgramExecutionJudgment("Do you want to download them(yes or no)?","Download","-")
    SeqRequest()
```

### Task 2 unit

* **Next unit is all functions of task 2.**
* The first function is aim to get the header from a blast result. The blast result has a default fixed format, so we can use this function to get the headers of sequences.
* Next is a function that use tool "cons" to get the most conservative sequence of a set of sequences. And it can also calculate the number of conservative amino acid residue and its proportion in this sequence.
* The third function is an important part of "task2" function. It firstly use function "ConservativeDegree" to get a most conservative protein sequences. Then make a blast database by using those protein sequences. Next, we use the most conservative protein sequence to make balst with database. Then will can get a result of the identity between most conservative sequence and sequences in database. Then we retrieve the head of top 250 sequences that have higher identity than others, and use pullseq to pick up these sequences from whole protein sequences. Finally use function "FileFormatting" to modify the format.
* **The forth function is the main part of task 2. Firstly, clustalo is used to make an alignment. Then, re-formatting the result of clustalo. Next, make a judgement. If the number of sequence is less than 250, use "ConservativeDegree" and "plotcon" to get conservative degree and draw graph straigntly. If the number is more than 250, let user make a choice, wheter plotting to all sequences or find out 250 most similar sequences to make graph. If user choose option 1, use "ConservativeDegree" and "plotcon" to get conservative degree and draw graph of all sequences. If user choose option 2, call function "Get250HighestSimilarity" to get 250 most similar sequences, and then getting conservative degree and drawing graph of all sequences.**

```python
# This function is aim to get the header from a blast result.
def HeaderCatch(File):
    data = open(File).readlines()
    data2 = data[29:279]
    header = open("header.txt","w+")
    data5 = []
    for string in data2:
        data3 = string.split(" ")
        data4 = [i for i in data3 if i != '']
        data5.append(data4[0])
    for item in data5:
        header.write(item+"\n")
    header.close()


# A function that use tool "cons" to get the most conservative sequence of a set of sequences.
# Calculate the number of conservative amino acid residue and its proportion in this sequence.
def ConservativeDegree(MultipleAli):
    os.system("cons "+MultipleAli+" RawAligned.cons")
    FileFormatting("RawAligned.cons","Aligned.cons")
    fasta = open("Aligned.cons","r").readlines()
    file = open("Aligned.cons","w")
    for line in fasta:
        a = re.sub('x','-',line)
        file.writelines(a)
    file.close()
    global NoCS,ConservativeRate
    NoCS = len(fasta[1]) - fasta[1].count("x")
    ConservativeRate = NoCS/len(fasta[1])


# An important module of task2
'''
	This function use function "ConservativeDegree" to get a most conservative protein sequences.
	Then make a blast database by using those protein sequences.
	Next, we use the most conservative protein sequence to make balst with database.
	Then will can get a result of the identity between most conservative sequence and sequences in database.
	Then we retrieve the head of top 250 sequences that have higher identity than others, and use pullseq to pick up these sequences from whole protein sequences.
	Finally use function "FileFormatting" to modify the format.
'''
def Get250HighestSimilarity():
    ConservativeDegree("MultipleSequenceAlignment.fasta")
    os.system("makeblastdb -in ProteinSequences.fasta -dbtype prot -out nem")
    os.system("blastp -db nem -query Aligned.cons -out blastoutput.out")
    HeaderCatch("blastoutput.out")
    sp.call("/localdisk/data/BPSM/Assignment2/pullseq -i ProteinSequences.fasta -n header.txt > Sequences250.fasta",shell = True)
    FileFormatting("Sequences250.fasta","SequencesOf250.fasta")


# Task 2, the second program of this application.
'''
    Firstly, clustalo is used to make an alignment.
    Then, re-formatting the result of clustalo.
    Next, make a judgement. If the number of sequence is less than 250, use "ConservativeDegree" and "plotcon" to get conservative degree and draw graph straigntly.
    If the number is more than 250, let user make a choice, wheter plotting to all sequences or find out 250 most similar sequences to make graph.
    If user choose option 1, use "ConservativeDegree" and "plotcon" to get conservative degree and draw graph of all sequences.
    If user choose option 2, call function "Get250HighestSimilarity" to get 250 most similar sequences, and then getting conservative degree and drawing graph of all sequences.
'''
def Task2():
    print("Data processing, please wait...\n")
    os.system("clustalo -i ProteinSequences.fasta --distmat-out=RawPairwiseDistanceMatrix.fasta --guidetree-out=GuideTree.gt --percent-id --full --force --threads=32 -o RawMultipleSequenceAlignment.fasta")
    FileFormatting("RawMultipleSequenceAlignment.fasta","MultipleSequenceAlignment.fasta")
    if SeqNumber <= 250:
        ConservativeDegree("MultipleSequenceAlignment.fasta")
        os.system("plotcon -sformat fasta MultipleSequenceAlignment.fasta --winsize=20 -graph svg")
        print("\nIn the most conservative sequence, the number of conservative amino acid residue is",NoCS)
        print("The conservative degree of the most conservative sequence is",ConservativeRate)
        print("The conservative graph is stored in file \"graph svg\", please check")
    else:
        print("The number of sequences is " + str(SeqNumber))
        print("You can create a conservation graph of all of sequences")
        print("You can also plotting to no more than the 250 most similar sequences.")
        print("\033[31m"+"Remember, the number you choose will also applied in follwed functions\n"+"\033[0m")
        print("1.plotting to all of sequences\n2.plotting to no more than the 250 most similar sequences\n3.exit\n")
        global PlottingDecision
        PlottingDecision = input("Pleas enter the index of you want(1-3):")
        while PlottingDecision not in ["1","2","3"]:
            PlottingDecision = input("Out of options, please re-enter the index(1-3):")
        if PlottingDecision == "1":
            ConservativeDegree("MultipleSequenceAlignment.fasta")
            os.system("plotcon -sformat fasta MultipleSequenceAlignment.fasta --winsize=20 -graph svg")
            print("\nIn the most conservative sequence, the number of conservative amino acid residue is",NoCS)
            print("The conservative degree of the most conservative sequence is",ConservativeRate)
            print("The conservative graph is stored in file \"graph svg\", please check")
        elif PlottingDecision == "2":
            Get250HighestSimilarity()
            os.system("clustalo -i SequencesOf250.fasta --percent-id --full --force --threads=32 -o AlignmentOf250Sequences.fasta")
            ConservativeDegree("AlignmentOf250Sequences.fasta")
            os.system("plotcon -sformat fasta AlignmentOf250Sequences.fasta --winsize=20 -graph svg")
            print("\nFor these 250 sequences, the number of conservative amino acid residue in the most conservative sequence is",NoCS)
            print("The conservative degree of the most conservative sequence is",ConservativeRate)
            print("The conservative graph is stored in file \"graph svg\", please check")
        else:
            exit()  

```

### Task 3 unit

* **The unit of task 3 is followed.**
* The most important part of task 3 is function "MotifsScan". Because the tool "patmatmotifs" in EMBOSS can only scan one sequence once, the sequences must be separated to single files. And use a loop to scan all sequences and put all result files together and generate a total file. This function does a good job of these missions, and use a progress bar to monitor the loop process to give user a better experience.
* Next function is "task3". It judge the sequences number and the mode \(whether process all sequences or just choose 250 most similar sequences\), and use function "MotifsScan" to process corresponding file.

```python
# This function seprate sequences that downloaded into single sequence and stored in fasta files
# Then call "patmatmotifs" to scan each sequence and get the result
# Finally put the separated result together getting final result and delete the mid product
def MotifsScan(File):
    RawData = open(File).readlines()
    SingleSeqList = [RawData[x:x+2] for x in range(0, len(RawData),2)]
    MotifTotal = open("MotifTotalResult.txt","w+")
    Cycles = int(len(RawData)/2)
    for i in range(Cycles):
        SubFile = open("SubSeq"+str(i)+".fasta","w+")
        SubFile.write(SingleSeqList[i][0])
        SubFile.write(SingleSeqList[i][1])
        SubFile.close()
        os.system("patmatmotifs SubSeq"+str(i)+".fasta MotifSubResult"+str(i)+".txt"+" -auto")
        MotifSub = open("MotifSubResult"+str(i)+".txt").readlines()
        if MotifSub[14] != "# HitCount: 0\n":
            MotifTotal.writelines(MotifSub[11:])
        else:
            pass
        time.sleep(0.05)
        ProgressBar(i + 1, Cycles, prefix = 'Task 3:', suffix = 'Complete', length = 50)  
    MotifTotal.close()
    os.system("rm -rf MotifSubResult*.txt")
    os.system("rm -rf SubSeq*.fasta")


# Task 3, the third program of this application
'''
    Use fuction "MotifsScan" to scan sequences and get result according to number of sequences and the mode that user choose.
'''
def Task3():
    if SeqNumber <= 250:
        MotifsScan("ProteinSequences.fasta")
    else:
        if PlottingDecision == "1":
            MotifsScan("ProteinSequences.fasta")
        elif PlottingDecision == "2":
            MotifsScan("SequencesOf250.fasta")
        else:
            exit()
```

### **Wild Card unit**

* **Some extra tools can be used in this part.**
* There are 3 tools in this units. They are applied in functions, these functions will use relative tool to process corresponding data files according to the sequences number and mode that user choose.
* And then the function "WildCardInfo" is a function which can print prompt message according to the list given. If you give a list \["1","2","3","4"\] to this function, it will show all information to user, if you give \["1"\], it will only print the "Option1".
* Next function "WildCardOptions" use the same argument as “WildCardInfo”. This function call "WildCardInfo" and ask user to choose a tool. After user has choose, process relative function.
* The last function is the body of wild card unit. It call "WildCardOption" with argument \["1","2","3","4"\] to list the 3 function in wild card. Ask user to choose an option, and process corresponding function. 

  After one function completed, list other functions that user has not choose and process after user choose one, until all functions has been used.

```python
# An extra function, use "pepwindowall" of EMBOSS
# It can draw Kyte-Doolittle hydropathy plot for a protein alignment
# And the data source is depended on the mode that user choose in task2
def PepWindowAll():
    if SeqNumber <= 250:
        os.system("pepwindowall MultipleSequenceAlignment.fasta -gxtitle=\"Base Number\" -gytitle=\"hydropathy\" -graph svg -normalize")
    else:
        if PlottingDecision == "1":
            os.system("pepwindowall MultipleSequenceAlignment.fasta -gxtitle=\"Base Number\" -gytitle=\"hydropathy\" -graph svg -normalize")
        else:
            os.system("pepwindowall AlignmentOf250Sequences.fasta -gxtitle=\"Base Number\" -gytitle=\"hydropathy\" -graph svg -normalize")


# An extra function, use "charge" in EMBOSS
# It can draw a protein charge plot
# And the data source is depended on the mode that user choose in task2
def Charge():
    if SeqNumber <= 250:
        os.system("charge ProteinSequences.fasta -sformat1 fasta -graph svg -plot")
    else:
        if PlottingDecision == "1":
            os.system("charge ProteinSequences.fasta -sformat1 fasta -graph svg -plot")
        else:
            os.system("charge SequencesOf250.fasta -sformat1 fasta -graph svg -plot")


# An extra function, use "tmap" in EMBOSS
# It can predict and plot transmembrane segments in protein sequences
# And the data source is depended on the mode that user choose in task2
def Tmap():
    if SeqNumber <= 250:
        os.system("tmap MultipleSequenceAlignment.fasta -out tmap.res -graph svg")
    else:
        if PlottingDecision == "1":
            os.system("tmap MultipleSequenceAlignment.fasta -out tmap.res -graph svg")
        else:
            os.system("tmap AlignmentOf250Sequences.fasta -out tmap.res -graph svg")


# A function which can print prompt message according to the list given.
def WildCardInfo(OptionsList):
    Option1 = "1.pepwindowall:Draw Kyte-Doolittle hydropathy plot for a protein alignment\n"
    Option2 = "2.charge:Draw a protein charge plot\n"
    Option3 = "3.tmap:Predict and plot transmembrane segments in protein sequences\n"
    Option4 = "4.exit\n"
    OpDiction = {"1":Option1,"2":Option2,"3":Option3,"4":Option4}
    for op in OptionsList:
        print(OpDiction.get(op))


# Main function of task wildcard.
'''
    To use this function, you need give a list which contain index from 1-4(such as ["1","2","3","4"]) as variable.
    Then this fuction will print the corresponding information and ask pick one index from the list that contained in the variable.
    Finally call corresponding tool.
'''
def WildCardOptions(OptionsList):
    WildCardInfo(OptionsList)
    global WCOCommand
    WCOCommand = input("Please enter your command:")
    while WCOCommand not in OptionsList:
        print("Input is not recognized, please re-enter")
        WCOCommand = input("Please re-enter your command:")
    if WCOCommand == "1":
        print("\n"+("The program pepwindowall starts executing".center(70,"-"))+"\n")
        PepWindowAll()
        print("\n"+("Program pepwindowall completed".center(70,"-"))+"\n")
    elif WCOCommand == "2":
        print("\n"+("The program charge starts executing".center(70,"-"))+"\n")
        Charge()
        print("\n"+("Program charge completed".center(70,"-"))+"\n")
    elif WCOCommand == "3":
        print("\n"+("The program tmap starts executing".center(70,"-"))+"\n")
        Tmap()
        print("\n"+("Program tmap completed".center(70,"-"))+"\n")
    elif WCOCommand == "4":
        print("\n"+"\033[93m"+("The program will quit, thank you for your use".center(70,"-"))+"\033[0m")
        exit()
    else:
        pass


# The last program of this appliction
'''
    Firstly, call function "WildCardOptions" to list the 3 function in wild card, ask user to choose an option, and process corresponding function.
    After one function completed, list other functions that user has not choose and process after user choose one, until all functions has been used.
'''
def WildCard():
    WildCardOptions(["1","2","3","4"])
    if WCOCommand == "1":
        WildCardOptions(["2","3","4"])
        if WCOCommand == "2":
            WildCardOptions(["3","4"])
        else:
            WildCardOptions(["2","4"])
    elif WCOCommand == "2":
        WildCardOptions(["1","3","4"])
        if WCOCommand == "1":
            WildCardOptions(["3","4"])
        else:
            WildCardOptions(["1","4"])
    else:
        WildCardOptions(["2","3","4"])
        if WCOCommand == "2":
            WildCardOptions(["3","4"])
        else:
            WildCardOptions(["2","4"])
```

* **Next unit is the main function of this application.** 
* This function contains all modules of this application \(Initiation, task 1, task 2, task 3, wild card\) and some prompt message. It process programs step by step, and once finish a task, ask user whether to continue. 

```python
# The main function of this application
'''
    Process programs step by step, and once finish a task, ask user wheter to continue.
'''
def main():
    Initiation()
    Task1()
    print(("Task 1 has been finished".center(70,"="))+"\n")
    ProgramExecutionJudgment("Do you want to continue to process task 2 (yes or no)?","Task 2","=")
    Task2()
    print("\n"+("Task 2 has been finished".center(70,"="))+"\n")
    ProgramExecutionJudgment("Do you want to continue to process task 3 (yes or no)?","Task 3","=")
    Task3()
    print("\033[93m"+"\nProcess completed. Result stored in \"MotifTotalResult.txt\", please check"+"\033[0m")
    print("\n"+("Task 3 has been finished".center(70,"="))+"\n")
    print("\033[93m"+("Now, it's \"wildcard\" time!".center(70,"=")+"\n")+"\033[0m")
    print("There are 3 extra functions in this application.\n")
    ProgramExecutionJudgment("Do you want experience some extra functions (yes or no)?","Wild Card","=")
    WildCard()
    print("\033[93m"+"Congratulations! All the projects have been completed.\n"+"\033[0m")
    print("\033[93m"+("The program will quit, thank you for your use".center(70,"*"))+"\033[0m")
    exit()

```

* **The last but must unit, run the main function!!!!!!**

```python
# Running the main function  
main()
```

