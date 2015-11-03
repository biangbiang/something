sed实现直接修改文件内容
=======================

sed是实现对流的编辑。通常，我们使用sed可以实现内容的编辑后然后保存成另外的一个文件，如果正确的话，才写入到源文件。但是某些时候，我们需要直接修改文件，因为，保存文件到一个文件，然后再覆盖原文件的办法显得很麻烦。

其实很简单，只需要一个 -i 参数就可以了。

比如，我想替换文件中的 properties 为 property ,可以使用

    sed  's/properties/property/g'  build.xml

这种方式，其实并没有修改build.xml文件的内容。如果想保存修改，通常的做法就需要重定向到另外的一个文件

    sed  's/properties/property/g'  build.xml > build.xml.tmp

这样，build.xml.tmp文件就是修改后的文件.

如果无误，那么就可以用

    mv build.xml.tmp build.xml

覆盖原文件。

如果想直接修改源文件，而没有这样的过程，可以用下面的命令

    sed  -i 's/properties/property/g'  build.xml

这样，就直接修改了build.xml文件
 
注：还有一个更简单的方法

    sed -in-place -e 's/abc/cba/g' build.xml
