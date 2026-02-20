# systemslibrarianship26
## LIS 624: Systems Librarianship Spring 2026

---
This page serves to document my work in LIS 624 via a Github repository. This will document my work from week 4 going forward. 
As time goes on in this course, this description will change and include additional information. 

---
**Week 4 Goals:**
1. Choose a Code Line (cl) editor: `micro`
2. Create a GitHub Account and Establish GitHub Repository
3. Configure Git on my server
4. Clone, Edit, Commit, and Push

**Topics Covered:**

 **Documentation Template:**
- Goal: 
- Context:
- Steps:
- Results:
- Verification:
- Notes:

**Documentation:**
- Goal: To add information to this page for Module 2's Check In. 
- Context: I added here some command line notes for this assignment and for my own documentation.



### Command Line Notes:
- To look at local files: ls /usr/local/bin
- To establish global configuration options, connecting to github: cat ~/.gitconfig
- To look at github files, use the command above, then use the command: ls
- To open a particular file there like for this class: cd systemslibrarianship26/
- To look at the file: cat README.md
- To edit using the text editor I chose: micro README.md


### Week 5 Notes:
- Goal: To search using the [grep] command to search through data.
- Context: Utilizing the [grep] command to search and sort through large amounts of bibliographic data.

** [grep] Command Line Notes:**
Note: (filename) in these notes is where you enter name of the file you are searching in, you DO NOT need the file name in paranthesis.
Note: "word" in these notes is what thing you want to search for, you have to have the word in quotation marks.
Note: [grep] is case sensitive but there is a way to make it ignore case sensetivity.
- Basic [grep] search: grep "word" (file name)
- To ignore wordcase sensitivity: grep -i "word" (file name)
- To search lines that do not match out string, use the combination (by having the "i" in there, it ignores case sensitivity): grep -vi "word" (file name)
- To remove the first row (best for spreadsheets with headers), you will need to have the "^" in the quotes:grep -vi "^word" (file name)
- To count the lines with a certain word, use: grep -ic "word" (file title)
- To conduct a Boolean OR search in a data sheet, you will need the quotations and parenthesis as is in this example, use: grep -Ei "(word1|word2)" (file name)
- To search more in a Boolean search, add more vertical bars: grep -Ei "(word1|word2|word3)" (file name)
