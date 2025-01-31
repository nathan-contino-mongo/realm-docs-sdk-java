# Yokedox Javadoc Development Testing Repo

First, clone and build Yokedox: https://github.com/mongodb-university/yokedox

To generate docs, run the following in this directory:

```sh
node path/to/yokedox/cli/build/main run \
  -o source \
  --debugGeneratorResultPath ./javadoc \
  --outputMdastJson=0 \
  javadoc -- -overview ./javadoc/overview.html
```

- `node path/to/yokedox/cli/build/main run`: run the version of Yokedox you built locally.
- `-o source`: output to the `source/` directory ("source" as in "consumed by snooty")
- `-debugGeneratorResultPath ./javadoc`: consume the generator (javadoc) result from the `javadoc/` directory. This is provided so that you don't have to actually set up Javadoc or clone the realm-java repository to work with Yokedox.
- `--outputMdastJson=0`: by default (for now), Yokedox outputs yokedast json files. You can use these to inspect the output and debug. However, they don't serve any purpose for snooty, so they should not be committed. Remove this line if you want to generate them for debugging purposes.
- `javadoc`: tells Yokedox which generator to use - or in this case, tells Yokedox which generator was used to generate the result at `debugGeneratorResultPath`
- `-- `: stop accepting Yokedox args and pass the subsequent arguments directly to javadoc.
- `-overview ./javadoc/overview.html`: Javadoc option to specify the overview page, which gets added to the main index file.

>Remember to rebuild Yokedox whenever you made a change to it.

## Run Doclet on Realm

```sh
cd path/to/realm-java/

find ./realm-annotations ./realm/realm-library/src/main -iname "*.java" | xargs javadoc -d test -docletpath ~/projects/yokedox/plugins/JsonDocletJava8/build/libs/JsonDocletJava8-all.jar -sourcepath android.sourceSets.objectServer.java.srcDirs -classpath ~/Library/Android/sdk/platforms/android-29/android.jar:~/Library/Android/sdk/build-tools/30.0.3/core-lambda-stubs.jar -doclet com.yokedox.JsonDoclet8 -quiet -source 8 io.realm
```
