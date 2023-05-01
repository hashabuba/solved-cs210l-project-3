Download Link: https://assignmentchef.com/product/solved-cs210l-project-3
<br>
<table width="719">

 <tbody>

  <tr>

   <td width="719">This document only contains the description of the project and the project problems. For the programming exercises on concepts needed for the project, please refer to the <a href="https://www.swamiiyer.net/cs210/project3_checklist.pdf">project checklist</a>  <a href="https://www.swamiiyer.net/cs210/project3_checklist.pdf">.</a></td>

  </tr>

 </tbody>

</table>

The purpose of this assignment is to write a program to implement <em>autocomplete </em>for a given set of <em>N </em>strings and nonnegative weights. That is, given a prefix, find all strings in the set that start with the prefix, in descending order of weight.

Autocomplete is an important feature of many modern applications. As the user types, the program predicts the complete <em>query </em>(typically a word or phrase) that the user intends to type. Autocomplete is most effective when there are a limited number of likely queries. For example, the <a href="https://www.imdb.com/">Internet Movie Database</a> uses it to display the names of movies as the user types; search engines use it to display suggestions as the user enters web search queries; cell phones use it to speed up text input.

In these examples, the application predicts how likely it is that the user is typing each query and presents to the user a list of the top-matching queries, in descending order of weight. These weights are determined by historical data, such as box office revenue for movies, frequencies of search queries from other Google users, or the typing history of a cell phone user. For the purposes of this assignment, you will have access to a set of all possible queries and associated weights (and these queries and weights will not change).

The performance of autocomplete functionality is critical in many systems. For example, consider a search engine which runs an autocomplete application on a server farm. According to one study, the application has only about 50ms to return a list of suggestions for it to be useful to the user. Moreover, in principle, it must perform this computation <em>for every keystroke typed into the search bar </em>and <em>for every user</em>!

In this assignment, you will implement autocomplete by sorting the queries in lexicographic order; using binary search to find the set of queries that start with a given prefix; and sorting the matching queries in descending order by weight.

<strong>Problem 1. </strong>(<em>Autocomplete Term</em>) Implement an immutable comparable data type Term in Term.java that represents an autocomplete term: a string query and an associated real-valued weight. You must implement the following API, which supports comparing terms by three different orders: lexicographic order by query string (the natural order); in descending order by weight (an alternate order); and lexicographic order by query string but using only the first <em>r </em>characters (a family of alternate orderings). The last order may seem a bit odd, but you will use it in Problem 3 to find all terms that start with a given prefix (of length <em>r</em>).

<h1>method                                                                                          description</h1>

<table width="720">

 <tbody>

  <tr>

   <td width="284">Term(String query)</td>

   <td width="436">initialize a term with the given query string and zero weight</td>

  </tr>

  <tr>

   <td width="284">Term(String query, long weight)</td>

   <td width="436">initialize a term with the given query string and weight</td>

  </tr>

  <tr>

   <td width="284">int compareTo(Term that)</td>

   <td width="436">compare the terms in lexicographic order by query</td>

  </tr>

  <tr>

   <td width="284">static Comparator&lt;Term&gt; byReverseWeightOrder()</td>

   <td width="436">for comparing terms in descending order by weight</td>

  </tr>

  <tr>

   <td width="284">static Comparator&lt;Term&gt; byPrefixOrder(int r)</td>

   <td width="436">for comparing terms in lexicographic order but using only the first <em>r </em>characters of each query</td>

  </tr>

  <tr>

   <td width="284">String toString()</td>

   <td width="436">a string representation of the term</td>

  </tr>

 </tbody>

</table>

<em>Corner cases. </em>The constructor should throw a java.lang.NullPointerException if query is null and a java.lang.IllegalArgumentException if weight is negative. The byPrefixOrder() method should throw a <sub>java.lang.IllegalArgumentException </sub>if <em>r </em>is negative.

<em>Performance requirements. </em>The string comparison functions should take time proportional to the number of characters needed to resolve the comparison.

<table width="720">

 <tbody>

  <tr>

   <td width="720">$ java Term data/cities.txt 5 Top 5 by lexicographic order:2200                      ’s Gravenmoer, Netherlands19190                     ’s-Gravenzande, Netherlands134520 ’s-Hertogenbosch, Netherlands3628                      ’t Hofke, Netherlands246056 A Coru a , SpainTop 5 by reverse-weight order:14608512                               Shanghai, China13076300                                  Buenos Aires, Argentina12691836                             Mumbai, India12294193                                          Mexico City, Distrito Federal, Mexico11624219                                Karachi, Pakistan</td>

  </tr>

 </tbody>

</table>

<strong>Problem 2. </strong>(<em>Binary Search Deluxe</em>) When binary searching a sorted array that contains more than one key equal to the search key, the client may want to know the index of either the first or the last such key. Accordingly, implement a library of static methods BinarySearchDeluxe.java with the following API:

<h1>method                                                                                                                       description</h1>

<table width="720">

 <tbody>

  <tr>

   <td width="363">static int firstIndexOf(Key[] a, Key key, Comparator&lt;Key&gt; c)</td>

   <td width="357">the index of the first key in <em>a</em>[] that equals the search key, or -1 if no such key</td>

  </tr>

  <tr>

   <td width="363">static int lastIndexOf(Key[] a, Key key, Comparator&lt;Key&gt; c)</td>

   <td width="357">the index of the last key in <em>a</em>[] that equals the search key, or -1 if no such key</td>

  </tr>

 </tbody>

</table>

<em>Corner cases. </em>Each static method should throw a java.lang.NullPointerException if any of its arguments is null. You should assume that the argument array is in sorted order (with respect to the supplied comparator).

<em>Performance requirements. </em>The firstIndexOf() and lastIndexOf() methods should make at most 1 + dlog<em>N</em>e compares in the worst case, where <em>N </em>is the length of the array. In this context, a <em>compare </em>is one call to comparator.compare().

$ java BinarySearchDeluxe data/wiktionary.txt cook

3

<strong>Problem 3. </strong>(<em>Autocomplete</em>) In this part, you will implement a data type that provides autocomplete functionality for a given set of string and weights, using Term and BinarySearchDeluxe. To do so, <em>sort </em>the terms in lexicographic order; use <em>binary search </em>to find the set of terms that start with a given prefix; and sort the matching terms in descending order by weight. Organize your program by creating an immutable data type Autocomplete in Autocomplete.java with the following API:

<h1>method                                                                description</h1>

<table width="720">

 <tbody>

  <tr>

   <td width="216">Autocomplete(Term[] terms)</td>

   <td width="504">initialize the data structure from the given array of terms</td>

  </tr>

  <tr>

   <td width="216">Term[] allMatches(String prefix)</td>

   <td width="504">all terms that start with the given prefix, in descending order of weight</td>

  </tr>

  <tr>

   <td width="216">int numberOfMatches(String prefix)</td>

   <td width="504">the number of terms that start with the given prefix</td>

  </tr>

 </tbody>

</table>

<em>Corner cases. </em>The constructor and each method should throw a java.lang.NullPointerException if its argument is null.

<em>Performance requirements. </em>The constructor should make proportional to <em>N </em>log<em>N </em>compares (or better) in the worst case, where <em>N </em>is the number of terms. The allMatches() method should make proportional to log<em>N </em>+<em>M </em>log<em>M </em>compares (or better) in the worst case, where <em>M </em>is the number of matching terms. The numberOfMatches() method should make proportional to log<em>N </em>compares (or better) in the worst case. In this context, a <em>compare </em>is one call to any of the compare() or compareTo() methods defined in Term.

<table width="720">

 <tbody>

  <tr>

   <td width="720">$ java Autocomplete data/cities.txt 7M&lt;enter&gt;12691836                             Mumbai, India12294193                                                           Mexico City, Distrito Federal, Mexico10444527                                 Manila, Philippines10381222                              Moscow, Russia3730206 Melbourne, Victoria, Australia3268513 Montr al , Quebec, Canada3255944 Madrid, SpainAl M&lt;enter&gt;431052 Al Ma allah al Kubr , Egypt420195 Al Man rah , Egypt290802 Al Mubarraz, Saudi Arabia258132 Al Mukall , Yemen 227150 Al Miny , Egypt128297 Al Man qil , Sudan99357                     Al Ma ar yah , Egypt&lt;ctrl-d&gt;</td>

  </tr>

 </tbody>

</table>


