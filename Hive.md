# Hive
### Command and Description
 - quit
 - set key=value
   - set
   - set -v
 - reset
 - Add a file(s)/jar(s)/archieves to the hive distributed cache.
   - add FILES <file > <file>*
   - add JAR[S] <file> <file>*
   - add ARCHIVE[S] <file> <file>*
 - List all the files added to the distributed cache.
   - list FILE[S]
   - list JAR[S]
   - list ARCHIVE[S]
 - Check if given resources are already added to distributed cache.
   - list FILE[S] <file>*
   - list JAR[S] <file>*
   - list ARCHIVE[S] <file>*
 - Removes the resource(s) from the distributed cache.
   - delete FILE[S] <file>*
   - delete JAR[S] <file>*
   - delete ARCHIVE[S] <file>*
 - Executes a shell command from the hive shell
   ! <cmd>
 - Executes a dfs command from the hive shell
   dfs
 - Execute a hive query and prints results to standard out
   - <query>
 - Used to execute a script file inside the CLI.
   - source FILE <file>

