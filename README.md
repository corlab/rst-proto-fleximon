rst-proto-fleximon
==================

Robotics Systems Type Library with FlexiMon Extensions

A collection of common and device specific data types for Robotics and Cognitive Systems specified in Google Protocol Buffer's IDL format and a build infrastructure for generating directly usable shared objects.

Shared type specifications are essential for achieving re-usability at the data and the component-level. Hence, the RST project aims at collecting typical data types in the context of intelligent (robotics) systems. From the IDL-based type specifications, shared libraries for Java, C++, Python and Common Lisp are build which are directly usable within RSB components.

Installation and Usage with Maven
=================================

Generally, RST can be configured and build as described in the upstream project (see links below).

To utilize this specific RST version from Java, it has to be deployed in a local Maven repository.

To do so, issue the following commands in RST/build-directory/java:

<pre>
mvn install:install-file -Dfile=rst-0.11.0.jar -DgroupId=rsb -DartifactId=rst-fleximon -Dversion=0.11-SNAPSHOT -Dpackaging=jar
mvn install:install-file -Dfile=rstsandbox-0.11.0.jar -DgroupId=rsb -DartifactId=rst-sandbox-fleximon -Dversion=0.11-SNAPSHOT -Dpackaging=jar
</pre>

The upstream project is hosted at CoR-Lab's [open source server](http://code.cor-lab.org/).

References
----------

* Documentation: http://docs.cor-lab.de/rst-manual/trunk/html/index.html
* Homepage: https://code.cor-lab.org/projects/rst
* Wiki : https://projects.cor-lab.org/projects/rst/wiki
* Upstream SCM Location: https://code.cor-lab.de/git/rst.git.proto.git
* Mailing List: https://lists.techfak.uni-bielefeld.de/cor-lab/mailman/listinfo/rsb

