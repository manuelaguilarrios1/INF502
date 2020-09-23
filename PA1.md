# PA1 DNA Similarity
The goal of the of this project is to get two DNA sequences from two files and seeing how many DNA characters match using two different matching methods, number of matches and maximum contiguous chain. 
For both these methods, a user input max shift is needed to add into the mathcing methods. The approach I took with this project is to compare two list of strings with a different function with for each matching method.
For the number of matches method, a for loop check each char in both sequences and see if they match. If they match, a match counter increases and the then final match counter will be given as the score.
For the maximum contiguous method, a for loop will check each char in both sequences and see if they match. A match counter will increase with each match, but if the char don't match the match counter will be put in a match counter list and then be reset to 0. 
To shift the sequences, the appropiate number of spaces will be added to the beginning or the end of the  respective list. The two sequences will then be printed out for each shift along with the score so that the user can see the result. 
Code:
```python
def AskForInfo():
    file1 = "Error"
    while file1 == "Error":
        try:
            a = str(input("Please enter the name of the file for sequence 1: "))
            file1 = OpenFile(a)
        except FileNotFoundError:
            print("That file was not found, please enter a valid file name")
            file1 = "Error"
    file2 = "Error"
    while file2 == "Error":
        try:
            b = str(input("Please enter the name of the file for sequence 2: "))
            file2 = OpenFile(b)
        except FileNotFoundError:
            print("That file was not found, please enter a valid file name")
            file2 = "Error"
    max_shift = "Error"
    while max_shift == "Error":
        try:
            max_shift = int(input("Please enter the maximum shift: "))
            if max_shift > len(file1) or max_shift > len(file2):
                print("Max shift cannot be greater than the length of any of the files")
                max_shift = "Error"
        except (ValueError, TypeError):
            print("Please enter an integer number")
            max_shift = "Error"
    check = checkSequence(file1,file2)
    output = [file1, file2, max_shift, check]
    return output
def split(word):
    return [char for char in word]
def OpenFile(filename):
    f = open(filename, "r")
    file = f.read()
    f.close()
    file = split(file)
    return file
def checkSequence(file1, file2):
    if len(file1) != len(file2):
        return False
    for i in range(len(file1)):
        if file1[i] != 'A' and file1[i] != 'T' and file1[i] != 'G' and file1[i] != 'C':
            return False
        if file2[i] != 'A' and file2[i] != 'T' and file2[i] != 'G' and file2[i] != 'C':
            return False
    return True

def print_sequence(list):
    for item in list:
        print(item, end=" ")
    print(" ")
def match_score(s1, s2):
    score = 0
    for i in range(len(s1)):
        if s1[i] == s2[i]:
            score = score + 1
    return score
def continuous_score(s1, s2):
    score = 0
    score_count = []
    for i in range(len(s1)):
        if s1[i] == s2[i]:
            score = score + 1
        else:
            score_count.append(score)
            score = 0;
    score_count.append(score)
    score_count.sort()
    return score_count[-1]
def compare_sequences_matches(sequence1, sequence2, max_shift, option):
    highest_score = 0
    score_loc = []
    for j in range(max_shift+1):
        if j == 0:
            if option == 1:
                score = match_score(sequence1, sequence2)
                print("With no shifting the match score for the DNA sequences is: "+str(score))
                print_sequence(sequence1)
                print_sequence(sequence2)
                print(" ")
                if score > highest_score:
                    highest_score = score
                    score_loc = [1, j]
            elif option == 2:
                score = continuous_score(sequence1, sequence2)
                print("With no shifting the continous DNA sequence score is: " + str(score))
                print_sequence(sequence1)
                print_sequence(sequence2)
                print(" ")
                if score > highest_score:
                    highest_score = score
                    score_loc = [1, j]
        else:
            #Make copies to avoid changing the original sequences and reset sequences
            s1 = sequence1.copy()
            s2 = sequence2.copy()
            #Left shifting
            for i in range(j):
                s1.insert(0, " ")
                s2.append(" ")
            if option == 1:
                score = match_score(s1,s2)
                print("By shifting sequence 1 by " + str(j) + " the matching score is: " + str(score))
                print(" ")
                if score > highest_score:
                    highest_score = score
                    score_loc = [1, j]
            elif option == 2:
                score = continuous_score(s1, s2)
                print(" ")
                print("By shifting sequence 1 by " + str(j) + " the continous score is: " + str(score))
                if score > highest_score:
                    highest_score = score
                    score_loc = [1, j]
            print_sequence(s1)
            print_sequence(s2)
            print(" ")
            #Reset sequence copies for right shift
            s1 = sequence1.copy()
            s2 = sequence2.copy()
            for i in range(j):
                s2.insert(0, " ")
                s1.append(" ")
            if option == 1:
                score = match_score(s1, s2)
                print("By shifting sequence 2 by " + str(j) + " the matching score is: " + str(score))
                print(" ")
                if score > highest_score:
                    highest_score = score
                    score_loc = [2, j]
            elif option == 2:
                score = continuous_score(s1, s2)
                print(" ")
                print("By shifting sequence 2 by " + str(j) + " the continous score is: " + str(score))
                if score > highest_score:
                    highest_score = score
                    score_loc = [2, j]
            print_sequence(s1)
            print_sequence(s2)
            print(" ")
    print("The highest score, " + str(highest_score) + ", can be found be shifting sequence " + str(score_loc[0])+" by " + str(score_loc[1]))
    print(" ")
menu_choice = 0
while menu_choice != 3:
    print("Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them")

    print('Please pick one of the following options by entering the respective number:')
    print("1. Compare two DNA sequences using the number of matches method.")
    print("2. Compare two DNA sequences using the maximum contiguous chain method.")
    print("3. Exit the program.")
    try:
        menu_choice = int(input("Your input: "))
    except (ValueError, TypeError):
        print("Whoops, you entered an invalid option!")
        menu_choice = 0
    while menu_choice < 1 or menu_choice > 3:
        try:
            menu_choice = input("Please enter a valid option: ")
            menu_choice = int(menu_choice)
        except (ValueError, TypeError):
            print("Whoops, you entered an invalid option!")
            menu_choice = 0
    if menu_choice != 3:
        output = AskForInfo()
        if output[3]:
            compare_sequences_matches(output[0],output[1],output[2], menu_choice)
        else:
            print("There is an error in the sequences, either they do not match in length or one of them has an unknown character in it")
```
Output:
```
Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them
Please pick one of the following options by entering the respective number:
1. Compare two DNA sequences using the number of matches method.
2. Compare two DNA sequences using the maximum contiguous chain method.
3. Exit the program.
Your input: 1
Please enter the name of the file for sequence 1: seq1.txt
Please enter the name of the file for sequence 2: seq2.txt
Please enter the maximum shift: 8
With no shifting the match score for the DNA sequences is: 3
A C T G A C T T T T  
T T T A G C C G A T  
 
By shifting sequence 1 by 1 the matching score is: 3
 
  A C T G A C T T T T  
T T T A G C C G A T    
 
By shifting sequence 2 by 1 the matching score is: 2
 
A C T G A C T T T T    
  T T T A G C C G A T  
 
By shifting sequence 1 by 2 the matching score is: 1
 
    A C T G A C T T T T  
T T T A G C C G A T      
 
By shifting sequence 2 by 2 the matching score is: 1
 
A C T G A C T T T T      
    T T T A G C C G A T  
 
By shifting sequence 1 by 3 the matching score is: 2
 
      A C T G A C T T T T  
T T T A G C C G A T        
 
By shifting sequence 2 by 3 the matching score is: 0
 
A C T G A C T T T T        
      T T T A G C C G A T  
 
By shifting sequence 1 by 4 the matching score is: 3
 
        A C T G A C T T T T  
T T T A G C C G A T          
 
By shifting sequence 2 by 4 the matching score is: 1
 
A C T G A C T T T T          
        T T T A G C C G A T  
 
By shifting sequence 1 by 5 the matching score is: 1
 
          A C T G A C T T T T  
T T T A G C C G A T            
 
By shifting sequence 2 by 5 the matching score is: 2
 
A C T G A C T T T T            
          T T T A G C C G A T  
 
By shifting sequence 1 by 6 the matching score is: 0
 
            A C T G A C T T T T  
T T T A G C C G A T              
 
By shifting sequence 2 by 6 the matching score is: 3
 
A C T G A C T T T T              
            T T T A G C C G A T  
 
By shifting sequence 1 by 7 the matching score is: 1
 
              A C T G A C T T T T  
T T T A G C C G A T                
 
By shifting sequence 2 by 7 the matching score is: 3
 
A C T G A C T T T T                
              T T T A G C C G A T  
 
By shifting sequence 1 by 8 the matching score is: 1
 
                A C T G A C T T T T  
T T T A G C C G A T                  
 
By shifting sequence 2 by 8 the matching score is: 2
 
A C T G A C T T T T                  
                T T T A G C C G A T  
 
The highest score, 3, can be found be shifting sequence 1 by 0
 
Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them
Please pick one of the following options by entering the respective number:
1. Compare two DNA sequences using the number of matches method.
2. Compare two DNA sequences using the maximum contiguous chain method.
3. Exit the program.
Your input: 2
Please enter the name of the file for sequence 1: seq1.txt
Please enter the name of the file for sequence 2: seq2.txt
Please enter the maximum shift: 8
With no shifting the continous DNA sequence score is: 1
A C T G A C T T T T  
T T T A G C C G A T  
 
 
By shifting sequence 1 by 1 the continous score is: 1
  A C T G A C T T T T  
T T T A G C C G A T    
 
 
By shifting sequence 2 by 1 the continous score is: 1
A C T G A C T T T T    
  T T T A G C C G A T  
 
 
By shifting sequence 1 by 2 the continous score is: 1
    A C T G A C T T T T  
T T T A G C C G A T      
 
 
By shifting sequence 2 by 2 the continous score is: 1
A C T G A C T T T T      
    T T T A G C C G A T  
 
 
By shifting sequence 1 by 3 the continous score is: 1
      A C T G A C T T T T  
T T T A G C C G A T        
 
 
By shifting sequence 2 by 3 the continous score is: 0
A C T G A C T T T T        
      T T T A G C C G A T  
 
 
By shifting sequence 1 by 4 the continous score is: 2
        A C T G A C T T T T  
T T T A G C C G A T          
 
 
By shifting sequence 2 by 4 the continous score is: 1
A C T G A C T T T T          
        T T T A G C C G A T  
 
 
By shifting sequence 1 by 5 the continous score is: 1
          A C T G A C T T T T  
T T T A G C C G A T            
 
 
By shifting sequence 2 by 5 the continous score is: 2
A C T G A C T T T T            
          T T T A G C C G A T  
 
 
By shifting sequence 1 by 6 the continous score is: 0
            A C T G A C T T T T  
T T T A G C C G A T              
 
 
By shifting sequence 2 by 6 the continous score is: 3
A C T G A C T T T T              
            T T T A G C C G A T  
 
 
By shifting sequence 1 by 7 the continous score is: 1
              A C T G A C T T T T  
T T T A G C C G A T                
 
 
By shifting sequence 2 by 7 the continous score is: 3
A C T G A C T T T T                
              T T T A G C C G A T  
 
 
By shifting sequence 1 by 8 the continous score is: 1
                A C T G A C T T T T  
T T T A G C C G A T                  
 
 
By shifting sequence 2 by 8 the continous score is: 2
A C T G A C T T T T                  
                T T T A G C C G A T  
 
The highest score, 3, can be found be shifting sequence 2 by 6
 
Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them
Please pick one of the following options by entering the respective number:
1. Compare two DNA sequences using the number of matches method.
2. Compare two DNA sequences using the maximum contiguous chain method.
3. Exit the program.
Your input: 1
Please enter the name of the file for sequence 1: seq1.txt
Please enter the name of the file for sequence 2: seq3.txt
Please enter the maximum shift: 8
With no shifting the match score for the DNA sequences is: 2
A C T G A C T T T T  
T T T T G T C G A T  
 
By shifting sequence 1 by 1 the matching score is: 4
 
  A C T G A C T T T T  
T T T T G T C G A T    
 
By shifting sequence 2 by 1 the matching score is: 2
 
A C T G A C T T T T    
  T T T T G T C G A T  
 
By shifting sequence 1 by 2 the matching score is: 1
 
    A C T G A C T T T T  
T T T T G T C G A T      
 
By shifting sequence 2 by 2 the matching score is: 2
 
A C T G A C T T T T      
    T T T T G T C G A T  
 
By shifting sequence 1 by 3 the matching score is: 2
 
      A C T G A C T T T T  
T T T T G T C G A T        
 
By shifting sequence 2 by 3 the matching score is: 2
 
A C T G A C T T T T        
      T T T T G T C G A T  
 
By shifting sequence 1 by 4 the matching score is: 2
 
        A C T G A C T T T T  
T T T T G T C G A T          
 
By shifting sequence 2 by 4 the matching score is: 3
 
A C T G A C T T T T          
        T T T T G T C G A T  
 
By shifting sequence 1 by 5 the matching score is: 1
 
          A C T G A C T T T T  
T T T T G T C G A T            
 
By shifting sequence 2 by 5 the matching score is: 3
 
A C T G A C T T T T            
          T T T T G T C G A T  
 
By shifting sequence 1 by 6 the matching score is: 0
 
            A C T G A C T T T T  
T T T T G T C G A T              
 
By shifting sequence 2 by 6 the matching score is: 4
 
A C T G A C T T T T              
            T T T T G T C G A T  
 
By shifting sequence 1 by 7 the matching score is: 1
 
              A C T G A C T T T T  
T T T T G T C G A T                
 
By shifting sequence 2 by 7 the matching score is: 3
 
A C T G A C T T T T                
              T T T T G T C G A T  
 
By shifting sequence 1 by 8 the matching score is: 1
 
                A C T G A C T T T T  
T T T T G T C G A T                  
 
By shifting sequence 2 by 8 the matching score is: 2
 
A C T G A C T T T T                  
                T T T T G T C G A T  
 
The highest score, 4, can be found be shifting sequence 1 by 1
 
Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them
Please pick one of the following options by entering the respective number:
1. Compare two DNA sequences using the number of matches method.
2. Compare two DNA sequences using the maximum contiguous chain method.
3. Exit the program.
Your input: 2
Please enter the name of the file for sequence 1: seq1.txt
Please enter the name of the file for sequence 2: seq3.txt
Please enter the maximum shift: 8
With no shifting the continous DNA sequence score is: 1
A C T G A C T T T T  
T T T T G T C G A T  
 
 
By shifting sequence 1 by 1 the continous score is: 2
  A C T G A C T T T T  
T T T T G T C G A T    
 
 
By shifting sequence 2 by 1 the continous score is: 1
A C T G A C T T T T    
  T T T T G T C G A T  
 
 
By shifting sequence 1 by 2 the continous score is: 1
    A C T G A C T T T T  
T T T T G T C G A T      
 
 
By shifting sequence 2 by 2 the continous score is: 1
A C T G A C T T T T      
    T T T T G T C G A T  
 
 
By shifting sequence 1 by 3 the continous score is: 1
      A C T G A C T T T T  
T T T T G T C G A T        
 
 
By shifting sequence 2 by 3 the continous score is: 1
A C T G A C T T T T        
      T T T T G T C G A T  
 
 
By shifting sequence 1 by 4 the continous score is: 2
        A C T G A C T T T T  
T T T T G T C G A T          
 
 
By shifting sequence 2 by 4 the continous score is: 2
A C T G A C T T T T          
        T T T T G T C G A T  
 
 
By shifting sequence 1 by 5 the continous score is: 1
          A C T G A C T T T T  
T T T T G T C G A T            
 
 
By shifting sequence 2 by 5 the continous score is: 3
A C T G A C T T T T            
          T T T T G T C G A T  
 
 
By shifting sequence 1 by 6 the continous score is: 0
            A C T G A C T T T T  
T T T T G T C G A T              
 
 
By shifting sequence 2 by 6 the continous score is: 4
A C T G A C T T T T              
            T T T T G T C G A T  
 
 
By shifting sequence 1 by 7 the continous score is: 1
              A C T G A C T T T T  
T T T T G T C G A T                
 
 
By shifting sequence 2 by 7 the continous score is: 3
A C T G A C T T T T                
              T T T T G T C G A T  
 
 
By shifting sequence 1 by 8 the continous score is: 1
                A C T G A C T T T T  
T T T T G T C G A T                  
 
 
By shifting sequence 2 by 8 the continous score is: 2
A C T G A C T T T T                  
                T T T T G T C G A T  
 
The highest score, 4, can be found be shifting sequence 2 by 6
 
Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them
Please pick one of the following options by entering the respective number:
1. Compare two DNA sequences using the number of matches method.
2. Compare two DNA sequences using the maximum contiguous chain method.
3. Exit the program.
Your input: 1
Please enter the name of the file for sequence 1: seq2.txt
That file was not found, please enter a valid file name
Please enter the name of the file for sequence 1: seq2.txt
Please enter the name of the file for sequence 2: seq3.txt
Please enter the maximum shift: 8
With no shifting the match score for the DNA sequences is: 8
T T T A G C C G A T  
T T T T G T C G A T  
 
By shifting sequence 1 by 1 the matching score is: 4
 
  T T T A G C C G A T  
T T T T G T C G A T    
 
By shifting sequence 2 by 1 the matching score is: 2
 
T T T A G C C G A T    
  T T T T G T C G A T  
 
By shifting sequence 1 by 2 the matching score is: 2
 
    T T T A G C C G A T  
T T T T G T C G A T      
 
By shifting sequence 2 by 2 the matching score is: 1
 
T T T A G C C G A T      
    T T T T G T C G A T  
 
By shifting sequence 1 by 3 the matching score is: 3
 
      T T T A G C C G A T  
T T T T G T C G A T        
 
By shifting sequence 2 by 3 the matching score is: 1
 
T T T A G C C G A T        
      T T T T G T C G A T  
 
By shifting sequence 1 by 4 the matching score is: 1
 
        T T T A G C C G A T  
T T T T G T C G A T          
 
By shifting sequence 2 by 4 the matching score is: 1
 
T T T A G C C G A T          
        T T T T G T C G A T  
 
By shifting sequence 1 by 5 the matching score is: 2
 
          T T T A G C C G A T  
T T T T G T C G A T            
 
By shifting sequence 2 by 5 the matching score is: 0
 
T T T A G C C G A T            
          T T T T G T C G A T  
 
By shifting sequence 1 by 6 the matching score is: 0
 
            T T T A G C C G A T  
T T T T G T C G A T              
 
By shifting sequence 2 by 6 the matching score is: 1
 
T T T A G C C G A T              
            T T T T G T C G A T  
 
By shifting sequence 1 by 7 the matching score is: 1
 
              T T T A G C C G A T  
T T T T G T C G A T                
 
By shifting sequence 2 by 7 the matching score is: 1
 
T T T A G C C G A T                
              T T T T G T C G A T  
 
By shifting sequence 1 by 8 the matching score is: 1
 
                T T T A G C C G A T  
T T T T G T C G A T                  
 
By shifting sequence 2 by 8 the matching score is: 1
 
T T T A G C C G A T                  
                T T T T G T C G A T  
 
The highest score, 8, can be found be shifting sequence 1 by 0
 
Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them
Please pick one of the following options by entering the respective number:
1. Compare two DNA sequences using the number of matches method.
2. Compare two DNA sequences using the maximum contiguous chain method.
3. Exit the program.
Your input: 2
Please enter the name of the file for sequence 1: seq2.tx
That file was not found, please enter a valid file name
Please enter the name of the file for sequence 1: seq2.txt
Please enter the name of the file for sequence 2: seq3.txt
Please enter the maximum shift: 8
With no shifting the continous DNA sequence score is: 4
T T T A G C C G A T  
T T T T G T C G A T  
 
 
By shifting sequence 1 by 1 the continous score is: 3
  T T T A G C C G A T  
T T T T G T C G A T    
 
 
By shifting sequence 2 by 1 the continous score is: 2
T T T A G C C G A T    
  T T T T G T C G A T  
 
 
By shifting sequence 1 by 2 the continous score is: 2
    T T T A G C C G A T  
T T T T G T C G A T      
 
 
By shifting sequence 2 by 2 the continous score is: 1
T T T A G C C G A T      
    T T T T G T C G A T  
 
 
By shifting sequence 1 by 3 the continous score is: 1
      T T T A G C C G A T  
T T T T G T C G A T        
 
 
By shifting sequence 2 by 3 the continous score is: 1
T T T A G C C G A T        
      T T T T G T C G A T  
 
 
By shifting sequence 1 by 4 the continous score is: 1
        T T T A G C C G A T  
T T T T G T C G A T          
 
 
By shifting sequence 2 by 4 the continous score is: 1
T T T A G C C G A T          
        T T T T G T C G A T  
 
 
By shifting sequence 1 by 5 the continous score is: 1
          T T T A G C C G A T  
T T T T G T C G A T            
 
 
By shifting sequence 2 by 5 the continous score is: 0
T T T A G C C G A T            
          T T T T G T C G A T  
 
 
By shifting sequence 1 by 6 the continous score is: 0
            T T T A G C C G A T  
T T T T G T C G A T              
 
 
By shifting sequence 2 by 6 the continous score is: 1
T T T A G C C G A T              
            T T T T G T C G A T  
 
 
By shifting sequence 1 by 7 the continous score is: 1
              T T T A G C C G A T  
T T T T G T C G A T                
 
 
By shifting sequence 2 by 7 the continous score is: 1
T T T A G C C G A T                
              T T T T G T C G A T  
 
 
By shifting sequence 1 by 8 the continous score is: 1
                T T T A G C C G A T  
T T T T G T C G A T                  
 
 
By shifting sequence 2 by 8 the continous score is: 1
T T T A G C C G A T                  
                T T T T G T C G A T  
 
The highest score, 4, can be found be shifting sequence 1 by 0
 
Welcome, this program will open two files of your choosing and compare the DNA sequences found inside them
Please pick one of the following options by entering the respective number:
1. Compare two DNA sequences using the number of matches method.
2. Compare two DNA sequences using the maximum contiguous chain method.
3. Exit the program.
Your input: 3

Process finished with exit code 0
```
