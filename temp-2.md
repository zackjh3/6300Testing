1. When starting the application, a user can choose whether to (1) log in as a specific student or (2) register as a new student.
-  To register as a new student, the user must provide the following student information:
         i.A unique username
        ii. A major
        iii. A seniority level (i.e., freshman, sophomore, junior, senior, or grad)
        iv. An email address
>  I satisfied this requirement by creating a **Student** class that has the attributes *userName*, *major*, *year*, and *email*. The operations in the **Student** class are detailed later. The **Student** class has a relationship with every other class, as it is used in several various methods. 

   - The newly added student is immediately created in the system.
> Logging in as a new student will create an instance of class **Student** with the attributes listed above, however this is not shown in the diagram as it is a GUI related class.

- For simplicity, there is no password creation/authentication; that is, selecting or entering a student username is sufficient to log in as that student.
- Also for simplicity, student and quiz information is local to a device.
>These are GUI related classes that do not have to be shown

2. The application allows students to (1) add a quiz, (2) remove a quiz they created, (3) practice quizzes, and (4) view the list of quiz score statistics.
>In order to satisfy many of the following requirements, one of the primary classes I added to the design is the **Quiz** class.  This class represents the quizzes created and practiced by the **Student**. The attributes for this class include the quiz name, a description, the **Student** who created the **Quiz** and a list of words and definitions in the quiz. The **Quiz** class is used by the **Student** class, the **PracticeSession** class, and the **QuizList** class, as it is passed as a parameter into the methods of all those classes. It has just one operation - *createWordBank()* which adds the words and definitions input by the **Student** into an ArrayList. To help organize the words and definitions, I created a class called **DictionaryEntry** which has just two attributes - word and definition.  This links the word to its definition. The incorrect definitions that are not tied to words will still be an object of the class, and the word attribute will be null.
>To satisfy the 2. requirement specifically, the student class has 4 methods:
>1.*createQuiz*()
>2.*deleteQuiz*()
>3.*practiceQuiz*()
>4.*viewStats*()
>I also added to the design a class **QuizList** with an ArrayList containing **Quiz** objects. It has a relationship to both the **Student** and **Quiz** class, as it is an aggregation of **Quiz** objects and a **Student** object is a parameter in it's methods. Those methods are *addQuiz()* which adds a **Quiz** to the list, and *removeQuiz()* which removes it. The *createQuiz()* method in **Student** will create an instance of class **Quiz** described below, as well as add that object to the **QuizList** class.

3. To add a quiz, a student must enter the following quiz information:
- Unique name
- Short description

>These pieces of information are attributes of the **Quiz** class

- List of N words, where N is between 1 and 10,  together with their definitions 
- List of N * 3 incorrect definitions, not tied to any particular word, where N is the number of words in the quiz.
>As mentioned previously, the **DictionaryEntry** class will store the word with its definition.  Incorrect definitions will not have a word attribute so they are not tied to any particular word.
4. To remove a quiz, students must select it from the list of the quizzes they created. Removing a quiz must also remove the score statistics associated with that quiz.

> This requirement will be satisfied by the *deleteQuiz()* method I mentioned previously, which will view the objects in the **QuizList** class that the **Student** has created, as well as call the removeQuiz method in the **QuizList** class which will remove the selected **Quiz** from the ArrayList. The *viewQuizList()* can be used to show the correct filtered list of quizzes depending on whether the list is of the creator or other students' quizzes. Within the *deleteQuiz()* method, there will also be a call of the *deleteStats()* method in the **SessionList** class.  This method will have as parameters a **Quiz** object and a **Student** object.  It will remove from the ArrayList all **PracticeSession** objects with that have attributes matching the parameters, which will delete the statistics associated with the quiz

5. To practice a quiz, students must select it from the list of quizzes created by other students.
>Similar to the previous requirement, the student will view the **QuizList** class, however to satisfy this requirement, they will only be able to view objects created by other **Student**s.  
6. When a student is practicing a quiz, the application must do the following:

>To realize the requirements for practicing a quiz, I added to the design a **PracticeSession** class. It has as attributes the date, a **Quiz**, a **Student** object, and the final score for that session. When a **Student** practices a quiz, it will create an instance of the **PracticeSession** class and pass as parameters the **Quiz** being practiced and the **Student** taking the quiz. The primary method is the *takeQuiz()* method that also has as parameters a **Quiz** and **Student** object, which will implement the practice requirements below. The class has relationships with the **Student** class, the **Quiz** class, and the **SessionList** class. 

- Until all words in the quiz have been used in the current practice session: 
>The *takeQuiz()* method will have a while loop that will call methods to satisfy all the following requirements while there are still unused words

1. Display a random word in the quiz word list.

>the *getWord()* method in the class will select a random word from the word bank in the class and display it.

2. Display four definitions, including the correct definition for that word (the other three definitions must be randomly selected from the union of (1) the set of definitions for the other words in the quiz and (2) the set of incorrect definitions for the quiz. 
>I created a method called *getIncorrectAnswers()* that will operate on the ArrayList<DictionaryEntry> attribute of the **Quiz** object to generate a set of the incorrect definitions according to the requirements. The correct definition will be taken from the attribute of the **DictionaryEntry** class.  

3. Let the student select a definition and display “correct” (resp., “incorrect”) if the definition is correct (resp., incorrect).
>I wrote the *getResult()* method that will take the **Student** answer and display either correct or incorrect, as well as save the result for calculating the quiz score.
- After every word in the quiz has been used, the student will be shown the percentage of words they correctly defined, and this information will be saved in the quiz score statistics for that quiz and student.
>The **PracticeSession** class will use local variables to keep track of the number of correct and incorrect answers and then after all the words in the quiz have been used, the *calculateScore()* method will calculate the percentage correct, save that value to the score attribute of the class, and display the result in the GUI.

7. The list of quiz score statistics for a student must list all quizzes, ordered based on when they were last played by the student (most recent first). Clicking on a quiz must display (1) the student’s first score and when it was achieved (date and time), (2) the student’s highest score and when it was achieved (date and time), and (3) the names of the first three students to score 100% on the quiz, ordered alphabetically.
> To realize this requirement, I added to the design a class **SessionList** which has as an attribute an ArrayList of type **PracticeSession**.  **SessionList** has relationships to **Student** and **PracticeSession**.  Instances of **PracticeSession** are passed to **SessionList** method *addSession()* as parameter and stored in the ArrayList. This class also has additional operations to calculate statistics:
>1.*getFirstQuiz()*:
>2.*getHighScore()*:
>3.*getPerfectScores()*:
>Because each practice session will have a date, **Quiz**, **Student** and score attribute, it will be relatively easy to get the statistics mentioned in the requirements

8. The user interface must be intuitive and responsive.
>This is not represented in my design as it will be handled entirely within the GUI implementation
9. The performance of the game should be such that students do not experience any considerable lag between their actions and the response of the application.

>Performance is not represented in my design either, as it can vary widely depending on the actual coding of the methods and classes



