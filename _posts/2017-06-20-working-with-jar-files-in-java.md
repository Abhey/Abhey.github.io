---
layout: post
title: Working with jar files in java
categories: Java
---

<p>Whenever a developer wants to distribute a version of his software, then all he want is to distribute a single file and not a directory structure filled with class files. JAR files were designed for this purpose. A JAR file can contain both class files and other file types like sound and image files which may be included in the project. All the files in a JAR file are compressed using a format similar to zip.</p>
<p style="text-align: center"><strong>Creating a JAR file &#8211;  more Options</strong></p>
<p>A jar file is created using jar tool. The general command looks somewhat like this:</p>
<pre> jar options jar-file [manifest-file] file1 file2 file3 ...</pre>
<ul>
<li><strong>jar &#8211; file : </strong>name of jar file on which you want to use jar tool.</li>
<li><strong>file1, file2, file3 : </strong> files which you want to add inside a jar file. manifest-file is the name of file which contains manifest of that jar file, giving manifest-file as an argument is entirely optional.</li>
<li><strong>c</strong> : Creates a new or empty archive and adds files to it. If any of the specified file name are directories, then the jar program processes them recursively.</li>
<li><strong>C</strong> : Temporarily changes the directory.</li>
<li><strong>e :</strong> Creates an entry point in the manifest.</li>
<li><strong>f</strong> : Specifies the JAR file name as the second command-line argument. If this parameter is missing, jar will write the result to standard output (when creating a JAR file)or read it from standard input(when extracting or tabulating a JAR file).</li>
<li><strong>i</strong> : Creates an index file.</li>
<li><strong>m</strong> : Adds a manifest file to the JAR file. A manifest is a description of the archive contents and origin. Every archive has a default manifest, but you can supply your own if you want to authenticate the contents of the archive.</li>
<li><strong>M</strong> : Does not create a manifest file for the entries.</li>
<li><strong>t :</strong> Displays the table of contents.</li>
<li><strong>u :</strong> Updates an existing JAR file.</li>
<li><strong>v :</strong> Generates verbose output.</li>
<li><strong>x :</strong> Extract files. If you supply one or more file names, only those files are extracted Otherwise, all files are extracted.</li>
<li><strong>0 :</strong> Stores without zip compression.</li>
</ul>
<p>The options of jar command are almost similar to that of UNIX tar command. In windows you can also get help about various options of jar command just by typing jar in cmd and then pressing enter, the output will be somewhat similar to this:</p>
<p><img src="http://cdncontribute.geeksforgeeks.org/wp-content/uploads/Capture12.png" alt="jar command options" /></p>
<p>Example :</p>
<p>For creating a JAR file which have two classes server.class and client.class and a Jpeg image logo.jpeg, one need to write following command :</p>
<pre> jar cvf chat.jar server.class client.class logo.jpeg</pre>
<p>The output of above command will be somewhat like this:</p>
<p><img src="http://cdncontribute.geeksforgeeks.org/wp-content/uploads/Capture22.png" alt="" /></p>
<p>It&#8217;s a better practice to use <strong>-v</strong> option along with jar command as you will get to know how the things are going on.</p>
<p style="text-align: center"><strong> Manifest File </strong></p>
<p>Each JAR file contains a manifest file that describe the features of the archive. Each JAR file have a manifest file by default. Default manifest file is named as MANIFEST.MF and is present in the META-INF subdirectory of archive. Although the default manifest file contains just two entries, but complex manifest files can have way more. Here, is what a default manifest file looks like &#8211;</p>
<p><img src="http://cdncontribute.geeksforgeeks.org/wp-content/uploads/Capture3.png" alt="Default manifest" /></p>
<p>The entries of manifest files are grouped into sections. Each section have two entries section name and its value. We will see a bit later how these sections can really help us in controlling the properties of our archive. Manifest file can also be updated by using the <strong>m</strong> option of jar command. But there are certain things which need to kept in mind while updating manifest file otherwise you may get the following creepy message.</p>
<pre> java.io.IOException: invalid manifest format</pre>
<p><strong>Things to keep in mind while handling Manifest files:</strong></p>
<ol>
<li>You should leave space between the name and value of any section in manifest file, like Version:1.1 is in valid section instead write Version: 1.1 that space between colon and 1.1 really matters a lot.</li>
<li>While specifying the main class you should not add .class extension at the end of class name. Simply specify the main class by typing:
<pre>Main-Class: Classname</pre>
<p>(I&#8217;ll be briefing about Main-Class section very shortly).</p></li>
<li>You must add newline at the end of file. You need not to write \n for specifying newline instead just leave the last line of your manifest file blank that will serve the purpose.</li>
<li>Text file for manifest must use UTF-8 encoding otherwise you may get into some trouble.</li>
</ol>
<p><strong>Example:</strong></p>
<p>Now let&#8217;s come back and update the contents of our chat.jar archive. To update the manifest file we simply need to write the following command:</p>
<pre> jar uvfm chat.jar manifest.txt</pre>
<p>Here manifest.txt is the new manifest file, which has following contents:</p>
<p><img src="http://cdncontribute.geeksforgeeks.org/wp-content/uploads/Capture5.png" alt="manifest.txt" /></p>
<p>The output of above command will be somewhat like this:</p>
<p><img src="http://cdncontribute.geeksforgeeks.org/wp-content/uploads/Capture6.png" alt="null" /></p>
<p>Here we are getting two warnings because we are trying to overwrite to previously present entries.</p>
<p style="text-align: center"><strong> Executable Jar Files </strong></p>
<p>You can use the <strong>e</strong> option of jar command to specify the entry point of your program, ie. class which you normally want to invoke when launching your Java application.</p>
<p><strong>Example:</strong></p>
<p>To create chat.jar file having client class as main class you need to write following command &#8211;</p>
<pre> jar cvfe chat.jar client client.class server.class logo.jpeg</pre>
<p>The output of above command will be somewhat like this:</p>
<p><img src="http://cdncontribute.geeksforgeeks.org/wp-content/uploads/Capture41.png" alt="" /></p>
<p>Remember not to add .class extension after the name of class which you want to set main class.</p>
<p>Alternatively you can add a Main-Class entry in the manifest file and then update it. For the above example you just need to add this entry:</p>
<pre> Main-Class: client</pre>
<p>With main class being set one can simply run a jar program by writing following command &#8211;</p>
<pre> java -jar chat.jar</pre>
<p>Depending on operating system configuration, users may even be able to launch application by double clicking the JAR file icon.</p>
<p style="text-align: center"><strong> Package Sealing </strong></p>
<p>Finally, we are going to discuss about package sealing in Java. We can seal a package in Java to ensure that no further classes can add themselves to it. You may want to seal a package if you use a package visible classes, methods and fields in your code. Without package sealing, other classes can add themselves to the same package and thereby gain access to package visible features.</p>
<ul>
<li>To achieve package sealing all one need to do is to put all classes of that package into a JAR file.</li>
<li> By default the packages in a jar file are not sealed but one can change the global default by adding few lines in manifest file. </li>
<li>Let&#8217;s again consider the case of our chat.jar archive, now the package of classes client.class and server.class is application and we want to seal this package all we need to do is to add following line in the manifest file and update it.
<pre> Name: application
Sealed: true</pre>
</li>
</ul>
<p>This is all from my side on how to work with  JAR files. Stay Tuned!!<br />
<br />