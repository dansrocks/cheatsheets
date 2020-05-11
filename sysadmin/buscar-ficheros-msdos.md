# Ficheros MS-DOS en linux

Identificar ficheros con salto de línea "\r" de Windows/MSDOS

Forma #1:

    find . -name "*.php" -exec file {} \; | grep "CRLF"

Forma #2

    find . -name '*.php' -print0 | xargs -0 grep -l '^M$'

Forma #3

    find . -name "*.php" | xargs file | grep "CRLF"
