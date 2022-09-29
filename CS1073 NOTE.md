## WEEK1 and WEEK2

### 1.Dos基本命令

|      |                    |                          |
| ---- | -----------------: | ------------------------ |
| 1    |               cd.. | 回滚上级cd               |
| 2    |               cd \ | return to the rootdircls |
| 3    |                dir |                          |
| 4    |                cls | clear screen             |
| 5    |               exit |                          |
| 6    | cd folder1\folder2 |                          |

### 2.CMD COMPLY  THE CODE

**Compile:**  javac. javaCode.java

**execute**:  java javaCode

### 3.algorithm

number the steps

Week3

instance variable

assessor = getter

mutator = setter

## WEEK4

### Javadoc 

#### Form

`/**`
Description is an overview of functionality.

`@author Your Name`
`@paramparamNameDescription of purpose.`
`@return Description of return value.`

*/

#### Generating Documentation

***javadoc–author –private  ClassName.jav***a

***javadoc–author –private -d MyJavadoc ClassName.java***



### Object Oriented (OO) Design 

  Software development consists of 4 basic development activities:

- ***Requirements***
- ***Design***

- ***Implementation***
- Testing

#### Identifying Classes 

represents a group of objects with similar behavior
generally nouns (people, place, thing)
Class names that represent objects should be singular nouns:
Employee, Student, Book

##### Assigning Responsibilities

Each task, activity or behaviour must be defined in the methods of the class.

### Object Variables vs Primitive Data Types 

#### Object Variables

No variable can ever hold an object –it holds a referenceto an object

Multiple reference variables can refer to the same object, but if all references to an object are reassigned then the object is marked for garbage collection.

When one object variable is assigned to another, the referenceis copied (not the object).



![image-20210925223032069](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210925223032069.png)

![image-20210925223404493](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210925223404493.png)

## ![image-20210925223537687](C:\Users\ThinkPad\AppData\Roaming\Typora\typora-user-images\image-20210925223537687.png)Method Overloading

Method Overloading

The compiler decides which method is called based on the number, type, and order of thei**r parameters**.

## **Question 3** (15 points)



Write two Java classes to represent information about conventions and celebrities.

First, write a Celebrity class. For each Celebrity, you need to know:

- the name of the celebrity (e.g. "Chuck Gold").
- the cost of paying for their appearance (e.g. 2480.75)
- the cost of their security expenses (e.g. 950.34)
- whether or not the celebrity is a member of a professional organization.

Use appropriate data types for the 4 instance variables.

Provide a constructor for Celebrity that records the celebrity's name, the cost of their appearance, the cost of their security expenses, and whether or not they are a member of a professional organization.

Provide 2 accessor methods for the name and professional organization membership status information.

Provide an additional accessor method, getCostToBook(), that calculates and returns the total cost of booking this celebrity. Celebrities are paid the full amount for their appearance, but only 60% of their security expenses are covered.

Provide 1 mutator method to set the amount of the celebrity's appearance fee.

Next, create a Convention class to represent a convention. Each convention will have a celebrity appearance during the opening ceremonies and another celebrity appearance during the finale. Include 4 instance variables in the Convention class for:

- the celebrity who will appear during the opening
- the celebrity who will appear during the finale
- the total registration fees that have been collected (e.g. 4728.64)
- the total funds that have been received from advertisers (e.g. 1590.75)

Again, use appropriate data types.

Provide a constructor for Convention that accepts three parameters and uses them to initialize the celebrities and the total funds received from advertisers. When a Convention object is first created, no registration fees have yet been collected (meaning that the starting total amount of fees collected will always be 0.0).

Include 1 mutator method in the Convention class that adds registration fees to the total registration fees that have been collected. The fees to be added will be passed in as a parameter.

Also, provide an isReputable() accessor method in the Convention class. This method determines and returns whether or not the convention is considered reputable. To be reputable, at least one celebrity that has been booked to appear at the convention must be a member of a professional organization and the total cost of the convention cannot exceed the convention's revenue. The convention's revenue includes the total registration fees that have been collected and the total funds that have been received from advertisers. The cost of the convention includes the total amount to be paid to the celebrities.

**Important - How to Answer this Question:** Open up a text editor window on your computer and write the complete Celebrity and Convention classes (each in a separate text file).  Save these as Celebrity.java and Convention.java.  Then, upload those two files to submit as your answer for this question.  (To do that, click on the "Add a File" button below, upload both files, and then click the "Add" button.) 

Additional notes:

- Javadoc comments are not required
- No driver program is required
- No additional accessor or mutator methods are required except those mentioned above

