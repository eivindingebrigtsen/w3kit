#w3Kit
is a library of github submodules
some tools for checking, maintaining and validating your javascripts


##Rake
A set of tasks that can be handy for developing w3 technologies,
just type rake followed by 

* _tabs_ -> Makes all tabs into 2 spaces for indentation heaven
* _strip_ -> Strips all console calls from your code, and stores the modified javascripts in the tmp folder
* _packext_ -> Takes all libraries in "ext" folder and strips & minifies them into a file
* _packall_ -> Takes all files in "src" folder and, strips & minifies them
* _tests_ -> Starts the testserver
* _test_ -> Runs the tests in js/tests