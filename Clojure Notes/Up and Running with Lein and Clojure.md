# Up and Running with Lein and Clojure

Leiningen is a sort of configuration automator for Clojure projects. It's good. 

To create a new project with the app template do:

`lein new app app-name`

This creates an empty project with a useful structure. Notable files are:

* project.clj is a config file
* src is a directory for source code
* src/app-name/core.clj is where code will go to start 

### Running a Clojure Project

The function `-main` is the entry point. To run it, simply do `lein run`. This runs `-main` and displays its output. Should you want to package your app into a .jar file, run `lein uberjar` and check the target folder.


