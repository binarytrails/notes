# grep

    # recursive inside all directory files
    grep -rni <word> <dir>

    # grep pdf files
    pdfgrep -i "\.html" file.pdf |

    # grep urls and remove duplicates
    cat file.html | grep -Eo "(http|https)://[a-zA-Z0-9./?=_%:-]*"  | sort -u

    # replace strings in found lines (-l)
    grep -rl matchstring somedir/ | xargs sed -i 's/string1/string2/g'

    # multiple greps
    cat file.html | grep -e first -e second
