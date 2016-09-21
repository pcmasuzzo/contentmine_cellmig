**This file explains how to use _your own_ stop-words file in the _ami2_ plugin of the ContentMine pipeline**

 At first, I thought I would just run:

 ami2-word --project cell_mig --w.words wordFrequencies --w.stopwords stopwords_eng.txt

 where stopwords_eng.txt is _my own_ local file.

 But nope, this does not work, because the WordSetWrapper used in the plugin points to the URL: **/org/xmlcml/ami2/wordutil/yourfile.txt**
 So, for a Linux machine, here is what I did:

 - go to _/usr/share/ami-repo/repo/ami2-0.1-SNAPSHOT.jar_ (this name can change depending on your _ami_ version); make sure you have the proper rights and download this .jar file, so that you can modify it.

 - Add your stop-words file to this jar folder, exactly in the location: _ami2-0.1-SNAPSHOT.jar/ami-/org/xmlcml/ami2/wordutil/_

 - Then simply replace your jar file with the new one:

 _sudo mv new_ami_file.jar /usr/share/ami-repo/repo/ami2-0.1-SNAPSHOT.jar_

 It is a bit cumbersome, but does the trick.
 Would be great to have the reference to the stop-words file externally, without having to interfere with the library itself. Absolute or relative path to the file would be ideal.
