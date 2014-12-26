<span style="font-family: Arial;">String in Java is very special class and most frequently used class as
well. There are lot many things to learn about </span><span style="font-family: &quot;Courier New&quot;;">String</span><span style="font-family: Arial;"> in Java
than any other class, and having a good knowledge of different </span><span style="font-family: Courier New, Courier, monospace;">String
</span><span style="font-family: Arial;">functionalities makes you to use it properly. Given heavy use of Java </span><span style="font-family: &quot;Courier New&quot;;">String</span><span style="font-family: Arial;"> in almost
any kind of project, it become even more important to know subtle detail about
String. Though I have shared lot of String related article already here in
<b>Javarevisited</b>, this is an effort to bring some of String feature together. In
this tutorial we will see some important points about Java </span><span style="font-family: &quot;Courier New&quot;;">String</span><span style="font-family: Arial;">, which is
worth remembering. You can also refer my earlier post <a href="http://javarevisited.blogspot.com/2012/10/10-java-string-interview-question-answers-top.html">10
advanced Java String questions</a> to know more about </span><span style="font-family: &quot;Courier New&quot;;">String</span><span style="font-family: Arial;">. Though I
tried to cover lot of things, there are definitely few things, which I might
have missed; please let me know if you have any question or doubt on </span><span style="font-family: &quot;Courier New&quot;;">java.lang.String</span><span style="font-family: Arial;">
functionality and I will try to address them here.</span></div>
<a name='more'></a><br />
<div class="MsoNormal">
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">1)
Strings are not null terminated in Java.</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">Unlike C and C++, String in Java doesn't terminate with null character. Instead
String are Object in Java and backed by character array. You can get the
character array used to represent String in Java by calling </span><span style="font-family: &quot;Courier New&quot;;">toCharArray()</span><span style="font-family: Arial;"> method of </span><span style="font-family: &quot;Courier New&quot;;">java.lang.String</span><span style="font-family: Arial;"> class of
JDK.</span></div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">2)
Strings are immutable and final in Java</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">Strings are immutable in Java it means once created you cannot modify
content of String. If you modify it by using </span><span style="font-family: &quot;Courier New&quot;;">toLowerCase()</span><span style="font-family: Arial;">, </span><span style="font-family: &quot;Courier New&quot;;">toUpperCase()</span><span style="font-family: Arial;"> or any
other method,<span style="mso-spacerun: yes;">&nbsp; </span>It always result in new
String. Since String is final there is no way anyone can extend String or
override any of String functionality. Now if you are puzzled <a href="http://javarevisited.blogspot.com/2010/10/why-string-is-immutable-in-java.html">why
String is immutable or final in Java</a>. checkout the link.</span></div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">3)
Strings are maintained in String Pool</span></u></b></div>
<div class="MsoNormal">
<a href="http://3.bp.blogspot.com/-K6q0DQ1v-tw/TWu8owBtc2I/AAAAAAAAADA/oBoHDBiJ8ag/s1600/17.jpg" imageanchor="1" style="clear: left; float: left; margin-bottom: 1em; margin-right: 1em;"><img alt="Advanced Java String tutorial and example programmers " border="0" src="http://3.bp.blogspot.com/-K6q0DQ1v-tw/TWu8owBtc2I/AAAAAAAAADA/oBoHDBiJ8ag/s1600/17.jpg" title="" /></a><span style="font-family: Arial;">As I Said earlier String is special class in Java and all String literal
e.g. </span><span style="font-family: &quot;Courier New&quot;;">"abc"</span><span style="font-family: Arial;"><span style="mso-spacerun: yes;">&nbsp; </span>(anything
which is inside double quotes are String literal in Java) are maintained in a
separate String pool, special memory location inside Java memory, more
precisely inside <a href="http://javarevisited.blogspot.com/2012/01/tomcat-javalangoutofmemoryerror-permgen.html">PermGen
Space</a>.&nbsp;Any time&nbsp;you create a new String object using String literal, JVM
first checks&nbsp;String pool and if an object with similar content available, than
it returns that and doesn't create a new object. JVM doesn't perform String
pool check if you create object using new operator.</span></div>
<div class="MsoNormal">
<br /></div>
<div class="MsoNormal">
<span style="font-family: Arial;">You may face subtle issues if you are not aware of this String&nbsp;behaviour&nbsp;, here is an example</span></div>
<div class="MsoNormal">
<span style="font-family: Arial;"><br /></span>
<div style="background: #F3F3F3; border: dotted windowtext 1.0pt; mso-border-alt: dotted windowtext .5pt; mso-element: para-border-div; padding: 1.0pt 4.0pt 1.0pt 4.0pt;">

<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String name = </span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Scala"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">; </span><span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//1st String object</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String name_1 = </span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Scala"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">; </span><span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//same object referenced by name variable</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String name_2 = </span><b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">new</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"> String(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Scala"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">) </span><span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//different String object</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//this will return true</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">if</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(name==name_1){<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"both name and name_1 is pointing to same string object"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">);<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">}<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//this will return false</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">if</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(name==name_2){<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"both name and name_2 is pointing to same string object"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">);<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">}<o:p></o:p></span></div>
</div>
<span style="font-family: Arial;"><br /></span>
<span style="font-family: Arial;">if you compare name and </span><span style="font-family: &quot;Courier New&quot;;">name_1</span><span style="font-family: Arial;"> using equality operator </span><span style="font-family: &quot;Courier New&quot;;">"=="</span><span style="font-family: Arial;"> it will
return true because both are pointing to same object. While </span><span style="font-family: &quot;Courier New&quot;;">name==name_2</span><span style="font-family: Arial;"> will
return false because they are pointing to different string object. It's worth
remembering that <a href="http://javarevisited.blogspot.sg/2012/12/difference-between-equals-method-and-equality-operator-java.html">equality
<span style="font-family: &quot;Courier New&quot;;">"=="</span> operator compares
object memory location</a> and not characters of String. By default Java puts
all string literal into string pool, but you can also put any string into pool
by calling </span><span style="font-family: &quot;Courier New&quot;;">intern()</span><span style="font-family: Arial;"> method of </span><span style="font-family: &quot;Courier New&quot;;">java.lang.String</span><span style="font-family: Arial;"> class,
like string created using </span><span style="font-family: &quot;Courier New&quot;;">new()</span><span style="font-family: Arial;"> operator.</span></div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">4) Use
Equals methods for comparing String in Java</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">String class overrides equals method and provides a content equality,
which is based on characters, case and order. So if you want to compare two
String object, to check whether they are same or not, always use </span><span style="font-family: &quot;Courier New&quot;;">equals()</span><span style="font-family: Arial;"> method
instead of equality operator. Like in earlier example if <span style="mso-spacerun: yes;">&nbsp;</span>we use <a href="http://javarevisited.blogspot.com/2011/02/how-to-write-equals-method-in-java.html">equals
method</a> to compare objects, they will be equal to each other because they
all contains same contents. Here is example of comparing String using equals
method.</span><br />
<span style="font-family: Arial;"><br /></span></div>
<div class="MsoNormal">
<div style="background: #F3F3F3; border: dotted windowtext 1.0pt; mso-border-alt: dotted windowtext .5pt; mso-element: para-border-div; padding: 1.0pt 4.0pt 1.0pt 4.0pt;">

<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String name = </span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">; </span><span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//1st String object</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String name_1 = </span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">; </span><span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//same object referenced by name variable</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String name_2 = </span><b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">new</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"> String(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">) </span><span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//different String object</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">if</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(name.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">equals</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(name_1)){<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"name and name_1 are equal String by equals method"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">);<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">}<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//this will return false</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">if</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(name==name_2){<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"name_1 and name_2 are equal String by equals method"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">);<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">}<o:p></o:p></span></div>
</div>
<span style="font-family: Arial;"><br /></span>
<span style="font-family: Arial;">You can also check my earlier post </span><a href="http://javarevisited.blogspot.com/2012/12/difference-between-equals-method-and-equality-operator-java.html" style="font-family: Arial;">difference
between equals() method and == operator</a><span style="font-family: Arial;"> for more detail discussion on
consequences of comparing two string using == operator in Java.</span></div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">5) Use
indexOf() and lastIndexOf() or matches(String regex) method to search inside
String</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">String class in Java provides convenient method to see if a character or&nbsp;sub-string&nbsp;or a pattern exists in&nbsp;current String object. You can use<span style="font-family: Arial,Helvetica,sans-serif;"> </span></span><span style="font-family: Courier New, Courier, monospace;">indexOf()</span><span style="font-family: Arial;"><span style="font-family: Arial,Helvetica,sans-serif;"> </span>which will
return position of character or String, if that exist in current String object
or -1 if character doesn't exists in String. </span><span style="font-family: &quot;Courier New&quot;;">lastIndexOf</span><span style="font-family: Arial;"> is similar
but it searches from end. </span><span style="font-family: &quot;Courier New&quot;;">String.match(String regex)</span><span style="font-family: Arial;"> is even
more powerful, which allows you to search for a <a href="http://javarevisited.blogspot.com/2012/10/regular-expression-example-in-java-to-check-String-number.html">regular
expression pattern</a> inside String. here is examples of </span><span style="font-family: &quot;Courier New&quot;;">indexOf</span><span style="font-family: Arial;">, </span><span style="font-family: &quot;Courier New&quot;;">lastIndexOf</span><span style="font-family: Arial;"> and </span><span style="font-family: &quot;Courier New&quot;;">matches</span><span style="font-family: Arial;"> method
from </span><span style="font-family: &quot;Courier New&quot;;">java.lang.String</span><span style="font-family: Arial;"> class.</span></div>
<div class="MsoNormal">
<span style="font-family: Arial;"><br /></span>
<div style="background: #F3F3F3; border: dotted windowtext 1.0pt; mso-border-alt: dotted windowtext .5pt; mso-element: para-border-div; padding: 1.0pt 4.0pt 1.0pt 4.0pt;">

<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String str = </span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java is best programming language"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">;<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">if</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(str.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">indexOf</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">) != -</span><b><span style="color: #0000dd; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">1</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">){<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">&nbsp;&nbsp;&nbsp;&nbsp; System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"String
contains Java at index :"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"> + str.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">indexOf</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">));<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">}<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">if</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(str.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">matches</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"J.*"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">)){<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">&nbsp;&nbsp;&nbsp;&nbsp; System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"String
Starts with J"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">);<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">}<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">str =</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Do you like Java ME or Java EE"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">;<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<br /></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<b><span style="color: #008800; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">if</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(str.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">lastIndexOf</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">) != -</span><b><span style="color: #0000dd; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">1</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">){<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"String contains Java lastly at: "</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"> + str.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">lastIndexOf</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">));<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">}<o:p></o:p></span></div>
</div>
<span style="font-family: Arial;"><br /></span>
<span style="font-family: Arial;">As expected </span><span style="font-family: &quot;Courier New&quot;;">indexOf</span><span style="font-family: Arial;"> will return </span><span style="font-family: &quot;Courier New&quot;;">0</span><span style="font-family: Arial;"> because characters
in String are indexed from zero. </span><span style="font-family: &quot;Courier New&quot;;">lastIndexOf</span><span style="font-family: Arial;"> returns
index of second &#8220;</span><span style="font-family: &quot;Courier New&quot;;">Java&#8221;,</span><span style="font-family: Arial;"> which starts at 23 and matches
will return true because </span><span style="font-family: &quot;Courier New&quot;;">J.*</span><span style="font-family: Arial;"> pattern is any String starting with character
</span><span style="font-family: &quot;Courier New&quot;;">J</span><span style="font-family: Arial;"> followed by any character because of</span><span style="font-family: &quot;Courier New&quot;;"> dot(.) </span><span style="font-family: Arial;">and any
number of time due to </span><span style="font-family: &quot;Courier New&quot;;">asterick (*)</span><span style="font-family: Arial;">.</span></div>
<div class="MsoNormal">
<br /></div>
<div class="MsoNormal">
<span style="font-family: Arial;">Remember </span><span style="font-family: &quot;Courier New&quot;;">matches()</span><span style="font-family: Arial;"> is tricky and some time
non-intuitive. If you just put </span><span style="font-family: &quot;Courier New&quot;;">"Java"</span><span style="font-family: Arial;"> in matches
it will return </span><span style="font-family: &quot;Courier New&quot;;">false</span><span style="font-family: Arial;"> because String is not equals to
"Java" i.e. in case of plain text it behaves like equals method. See <a href="http://java67.blogspot.sg/2012/09/java-string-matches-example-regular-expression.html">here</a>
for more examples of String </span><span style="font-family: &quot;Courier New&quot;;">matches()</span><span style="font-family: Arial;"> method.</span></div>
<div class="MsoNormal">
<br /></div>
<div class="MsoNormal">
<span style="font-family: Arial;">Apart from </span><span style="font-family: &quot;Courier New&quot;;">indexOf()</span><span style="font-family: Arial;">, </span><span style="font-family: &quot;Courier New&quot;;">lastIndexOf()</span><span style="font-family: Arial;"> and </span><span style="font-family: &quot;Courier New&quot;;">matches(String
regex)</span><span style="font-family: Arial;"> String also has methods like </span><span style="font-family: &quot;Courier New&quot;;">startsWith()</span><span style="font-family: Arial;"> and </span><span style="font-family: &quot;Courier New&quot;;">endsWidth(),</span><span style="font-family: Arial;"> which can
be used to check an String if it starting or ending with certain character or
String.</span></div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">6) Use
SubString to get part of String in Java</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">Java String provides another useful method called </span><span style="font-family: &quot;Courier New&quot;;">substring(),</span><span style="font-family: Arial;"> which can
be used to get parts of String. basically you specify start and end index and </span><span style="font-family: &quot;Courier New&quot;;">substring()</span><span style="font-family: Arial;"> method
returns character from that range. Index starts from 0 and goes till </span><span style="font-family: &quot;Courier New&quot;;">String.length()-1</span><span style="font-family: Arial;">. By the
way </span><span style="font-family: &quot;Courier New&quot;;">String.length()</span><span style="font-family: Arial;"> returns you number of characters in String,
including white spaces like tab, space. One point which is worth remembering
here is that substring is also backed up by character array, which is used by
original String. This can be dangerous if original string object is very large
and substring is very small, because even a small fraction can hold reference
of complete array and prevents it from being garbage collected even if there is
no other reference for that particular String. Read <a href="http://javarevisited.blogspot.com/2011/10/how-substring-in-java-works.html">How
Substring works in Java</a> for more details. Here is an example of using SubString
in Java:</span></div>
<div class="MsoNormal">
<br /></div>
<div class="MsoNormal">
<div style="background: #F3F3F3; border: dotted windowtext 1.0pt; mso-border-alt: dotted windowtext .5pt; mso-element: para-border-div; padding: 1.0pt 4.0pt 1.0pt 4.0pt;">

<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String str = </span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Java is best programming language"</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">;<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">&nbsp;&nbsp;&nbsp;&nbsp; <o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #888888; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">//this will return part of
String str from index 0 to 12</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"><o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">String subString = str.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">substring</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><b><span style="color: #0000dd; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">0</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">,</span><b><span style="color: #0000dd; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">12</span></b><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">);<o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">&nbsp;&nbsp;&nbsp;&nbsp; <o:p></o:p></span></div>
<div class="MsoNormal" style="background-position: initial initial; background-repeat: initial initial; border: none; padding: 0in;">
<span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">System.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">out</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">.</span><span style="color: #336699; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">println</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">(</span><span style="background: #FFF0F0; color: #dd2200; font-family: &quot;Courier New&quot;; font-size: 11.0pt;">"Substring: "</span><span style="color: #333333; font-family: &quot;Courier New&quot;; font-size: 11.0pt;"> + subString);<o:p></o:p></span></div>
</div>
</div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">7)
"+" is overloaded for String concatenation</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;"><i>Java
doesn't support Operator overloading</i> but String is special and + operator
can be used to concatenate two Strings. It can even used to convert </span><span style="font-family: &quot;Courier New&quot;;">int</span><span style="font-family: Arial;">, </span><span style="font-family: &quot;Courier New&quot;;">char</span><span style="font-family: Arial;">, </span><span style="font-family: &quot;Courier New&quot;;">long</span><span style="font-family: Arial;"> or </span><span style="font-family: &quot;Courier New&quot;;">double</span><span style="font-family: Arial;"> to convert
into String by simply concatenating with empty
string </span><span style="font-family: &quot;Courier New&quot;;">""</span><span style="font-family: Arial;">. internally + is implemented
using </span><span style="font-family: &quot;Courier New&quot;;">StringBuffer</span><span style="font-family: Arial;"> prior to Java 5 and </span><span style="font-family: &quot;Courier New&quot;;">StringBuilder</span><span style="font-family: Arial;"> from Java
5 onwards. This also brings point of using </span><span style="font-family: &quot;Courier New&quot;;">StringBuffer</span><span style="font-family: Arial;"> or </span><span style="font-family: &quot;Courier New&quot;;">StringBuilder</span><span style="font-family: Arial;"> for
manipulating String. Since both represent mutable object they can be used to
reduce string garbage created because of temporary String. Read more about <a href="http://javarevisited.blogspot.com/2011/07/string-vs-stringbuffer-vs-stringbuilder.html">StringBuffer
vs StringBuilder</a> here.</span></div>
<div class="MsoNormal">
<br /></div>
<div class="MsoNormal">
<span style="font-family: Arial;"><span style="mso-spacerun: yes;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; </span></span></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">8) Use
trim() to remove white spaces from String</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">String in Java provides </span><span style="font-family: &quot;Courier New&quot;;">trim()</span><span style="font-family: Arial;"> method to remove white space
from both end of String. If </span><span style="font-family: &quot;Courier New&quot;;">trim()</span><span style="font-family: Arial;"> removes white spaces it
returns a new </span><span style="font-family: &quot;Courier New&quot;;">String</span><span style="font-family: Arial;"> otherwise it returns same String. Along with </span><span style="font-family: &quot;Courier New&quot;;">trim()</span><span style="font-family: Arial;"> String also provides </span><span style="font-family: &quot;Courier New&quot;;">replace()</span><span style="font-family: Arial;"> and </span><span style="font-family: &quot;Courier New&quot;;">replaceAll()</span><span style="font-family: Arial;"> method for
replacing characters from String. </span><span style="font-family: &quot;Courier New&quot;;">replaceAll</span><span style="font-family: Arial;"> method even
support regular expression. Read more about How to replace String in Java <a href="http://javarevisited.blogspot.com/2011/12/java-string-replace-example-tutorial.html">here</a>.</span></div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">9) Use
split() for splitting String using Regular expression</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">String in Java is feature rich. it has methods like </span><span style="font-family: &quot;Courier New&quot;;">split(regex)</span><span style="font-family: Arial;"> which can
take any String in form of regular expression and split the String based on
that. particularly useful if you dealing with comma separated file (CSV) and
wanted to have individual part in a String array. There are other methods also
available related to splitting String, see this <a href="http://javarevisited.blogspot.com/2011/09/string-split-example-in-java-tutorial.html">Java
tutorial to split string</a> for more details.</span><br />
<span style="font-family: Arial;"><br /></span></div>
<div class="MsoNormal">
<br /></div>
<div class="MsoNormal">
<b><u><span style="font-family: Arial;">10) Don't
store sensitive data in String</span></u></b></div>
<div class="MsoNormal">
<span style="font-family: Arial;">String pose security threat if used for storing sensitive data like
passwords, SSN or any other sensitive information. Since String is immutable in
Java there is no way you can erase contents of String and since they are kept
in String pool (in case of String literal) they stay longer on Java heap ,which
exposes risk of being seen by anyone who has access to Java memory, like
reading from memory dump. Instead </span><span style="font-family: &quot;Courier New&quot;;">char[]</span><span style="font-family: Arial;"> should be
used to store password or sensitive information. See <a href="http://javarevisited.blogspot.com.br/2012/03/why-character-array-is-better-than.html">Why
char[] is more secure than String for storing passwords in Java</a> for more
details.</span><br />
<span style="font-family: Arial;"><br /></span>
<span style="font-family: Arial;"><br /></span>
<span style="font-family: Arial;"><b><u>11) Character Encoding and String</u></b></span><br />
<span style="font-family: Arial;">Apart from all these 10 facts about String in Java, the most critical thing to know is <i>what encoding your String is using</i>. It does not make sense to have a </span><span style="font-family: Courier New, Courier, monospace;">String </span><span style="font-family: Arial;">without knowing what encoding it uses. There is no way to interpret an String if you don't know the encoding it used. You can not assume that </span><span style="font-family: Courier New, Courier, monospace;">"plain"</span><span style="font-family: Arial;"> text is ASCII. If you have a String, in memory or stored in file, you must know what encoding it is in, or you cannot display it correctly. By default Java uses platform encoding i.e. character encoding of your server, and believe me this can cause huge trouble if you are handling Unicode data, especially if you are <a href="http://javarevisited.blogspot.sg/2013/03/convert-and-print-byte-array-to-hex-string-java-example-tutorial.html">converting byte array to XML String</a>. I have faced instances where our program fail to interpret Strings from European language e.g. German, French etc. because our server was not using Unicode encodings like </span><span style="font-family: Courier New, Courier, monospace;">UTF-8</span><span style="font-family: Arial;"> or </span><span style="font-family: Courier New, Courier, monospace;">UTF-16</span><span style="font-family: Arial;">. Thankfully, Java allows you to specify default character encoding for your application using system property file.encoding. See <a href="http://javarevisited.blogspot.com/2012/01/get-set-default-character-encoding.html">here </a>to read more about character encoding in Java</span></div>
<div class="MsoNormal">
<br />
<br /></div>
<div class="MsoNormal">
<span style="font-family: Arial;">That's all about String in Java. As I have said String is very special in
Java, sometime even refer has God class. It has some unique feature like </span><span style="font-family: &quot;Courier New&quot;;">immutability</span><span style="font-family: Arial;">, </span><span style="font-family: &quot;Courier New&quot;;">concatenation
support, caching</span><span style="font-family: Arial;"> etc, and to become a serious Java programmer,
detailed knowledge of String is quite important. Last but not the least don't
forget about <a href="http://javarevisited.blogspot.com/2012/01/get-set-default-character-encoding.html">character
encoding</a> while converting a byte array into String in Java. Good knowledge of</span><span style="font-family: Courier New, Courier, monospace;"> java.lang.String</span><span style="font-family: Arial;"> is must for good Java developers.</span></div>
<div class="MsoNormal">
<br /></div>