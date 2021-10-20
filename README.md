# if.05.11-03_Supermarket - Unit Tests

This assignment is supposed to be executed by a team of 2-3 students. 
The team shall work on the repository of one team member; collaboration and release shall be done in that repo.
Members of the team who do not use their repository, shall still note the member who owns the productive repository as well as the members of their team below.

## Team Members:

_Please note the team members here, name the person who owns the working repository._

## Goals
(see Assignment '02-Supermarket)
+ Development of the OOP SW design of a supermarket application according to the requirements specified below.
The SW design shall be expressed using UML. For creating UML diagrams e.g. VS Code with PlantUML extension, Online UML tools like https://www.draw.io, photography or scan of hand-drawn diagrams or evaluation versions of Sparx Enterprise Architect may be used. Other UML tools may also be used.
+ Implementation of the application according to the chosen SW design.

__Extended Goals (Assignment 03)__
+ Implement Unit Tests for all classes.
+ Line Coverage of executed tests shall be at least 90%.
+ Implement unit tests for 'equal' 'hashCode' and 'toString' methods for all classes.
+ Implement unit tests for testing the number of enumeration item for each enum.

## Work Products
+ SW Design: 
    + Design specification as PDF document describing the SW design including UML diagrams (text and graphics). The design specification shall also answer the questions stated at the end of this document.
    + UML diagrams as images
    + SW Design artifacts shall be stored in a dedicated folder aside the the source code.
+ Implementation:
    + Source code of the build- and executable Java application.
+ Unit Tests
    + Unit tests and test report 

## Requirements

__Terminology__:  
As usual for technical specifications, 
+ __shall__ and __must__ specify mandatory requirements (must have),
+ __shall not__ and __must not__ defines absolute prohibitions,
+ __should__ specifies a recommended requirement (should have),
+ __may__ specifies an optional requirement (nice to have),

--


A Java application that is capable of managing products on the stock of a supermarket _shall_ be realized.   

1. In general, an __product__ _shall_ have
    + a name and  
    + a unique __barcode__ according to EAN-8 (see below). 

1. For each product, its current stock quantity _shall_ be maintained.

1. It _shall_ only be allowed to add new products for the supermarket, if the barcode of the given product is valid and there does not exist another product with the same barcode – both cases _shall_ be treated as errors. 

1. Products _may_ be removed from the assortment, but only in case the stock quantity is zero. They _must not_ be removable if they are still in stock.

1. For __food products__, a list of contained __allergens__ _shall_ be maintained. 
    1. Allergens _shall_ be unambiguous throughout the application. 
    1. Name and code of allergens _shall_ be presentable to users.
    1. It _shall not_ be possible to modify the name or code of allergens at run-time.
    1. For allergen types, all 12 allergens which need to be declared according to Austrian law _must_ be handled. 
    1. It _shall_ be possible to add new allergens to a food at any point in time.
    1. It _shall_ be possible to remove allergens from a food at any point in time. 
    1. Further, it _shall_ be possible to prove, if a given food product contains any allergen from a given list of allergens. Example: Does an product contain the allergens milk, egg, and peanuts?

1. For __non-food products__, it _shall_ be possible to store a list of customer __reviews__:
    1. Each review _shall_ have a __star rating__ (from 1 to 5) as well as a review text and a date. 
    1. A star rating of zero _shall_ mean 'no rating' – in this case only a review text is available. 
    1. The list of reviews shall be sorted by date – newest on top (first). 
    1. It _shall_ be possible to add reviews 
    1. It _must not_ be possible to delete reviews. 
    1. A summary rating _shall_ be maintained for a non-food product collected over all its review ratings. 
    1. It _shall_ be possible to ask for all reviews with a given minimum star rating (e.g. 3 stars or more). 

1. Your __application__ _shall_ be able to create a list of all products sorted by product name. 

1. It _shall_ be possible to retrieve a list of all products with a stock quantity below a given limit, sorted by stock quantity descending. 

1. It _shall_ be possible to query for all food products which do not contain a given set of allergens.

1. Technical requirements: 
    1. The 'application shell' _shall_ be designed as class rather than implementing anything (except instantiating the 'application') in the 'main' function.
    1. There _shall_ exist exactly one instance of the application;
    this instance shall be globally accessible.
    1. It shall be possible to perform required operations, such as rating, reviewing, listing, or deleting products interactively via console.
    1. Initial stocks _may_ be read from external sources (e.g. text or JSON files).

## Calculating the Checksum for EAN Code:
__(Excerpt from Wikipedia)__  
The checksum is calculated taking a varying weight value times the value of each number in the barcode to make a sum. The checksum digit is then the digit which must be added to this sum to get a number evenly divisible by 10 (i.e. the additive inverse of the sum, modulo 10). See ISBN check digit calculation for a more extensive description and algorithm. The Global Location Number/GLN also uses the same method.

### Weight
The weight for a specific position in the EAN code is either 3 or 1, which alternates so that the final data digit has a weight of 3; the same algorithm is used in other GTINs and the Serial Shipping container Code (SSCC). In an EAN-13 code, the weight is 3 for even positions and 1 for odd positions; this is reversed in EAN-8 codes. All GTIN and SSCC codes get their weight values for the position of the code from this table, making their code line up to the right:

<table> 
<tr><th></th><th colspan="7">Weights</th></tr>
<tr><td>Position</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td></tr>
<tr><td>Weight</td><td>3</td><td>1</td><td>3</td><td>1</td><td>3</td><td>1</td><td>3</td></tr>
</table>

### Calculation
If we take the EAN code 7351353, we do the following:
1.	We calculate the products for each digit and its weight. 7 * 3 = 21 for the leftmost digit, 3 * 1 for the next, etc.  
The following table shows the digits, weights and their products:

<table> 
<tr><td>Position</td><td>0</td><td>1</td><td>2</td><td>3</td><td>4</td><td>5</td><td>6</td></tr>
<tr><td>Weight  </td><td>3</td><td>1</td><td>3</td><td>1</td><td>3</td><td>1</td><td>3</td></tr>
<tr><td>Code    </td><td>7</td><td>3</td><td>5</td><td>1</td><td>3</td><td>5</td><td>3</td></tr>
<tr><td>Products</td><td>21</td><td>3</td><td>15</td><td>1</td><td>9</td><td>5</td><td>9</td></tr>
</table>

1.	We sum up all products. The sum of products above yields 63.
1.	We need the number to be added to the above sum to get a number evenly dividable by 10. To figure this out, we first get the units column of 63 by 63 % 10 = 3.
1.	Then we calculate 10 minus 3 makes the checksum = 7.  

Therefore, the complete EAN 8 code is then: 73513537

## Collection Design Considerations
__Note__ Answer these questions within your SW design specification and argument why you have chosen that Collection implementation.
1. Collection for allergens:  
Which container type is used to store all allergens of a food and why?

1. Collection for products:  
  a. Which container type is used to store this list, and why?  
  b. Discuss whether the following statements are plausible or implausible. Document the outcome of the discussion including the reasons for your decision:
    + “The number of different products frequently changes for a supermarket.”
    + “If the product is identified via a barcode, we have to store duplicates.”  

1. Collection for reviews  
What are the important questions to be asked when you have to decide the data type to store the reviews of a Non-Food-Product? Ask these questions, agree on answers, document them and decide which Collection type to use.

## Unit Testing
+ To enable Unit Tests, JUnit must be installed as project (or global) library. JUnit5 can be installed from Maven using key 'org.junit.jupiter:junit-jupiter'
+ After JUnit is installed, unit tests can be executed e.g. via context menu 'Run as Unit Test'.
+ To measure coverage of tested code, run unit tests 'with coverage' (e.g. from menu 'Run').
