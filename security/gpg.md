# GNU Privacy Guard

## Generating your key

Chose RSA and RSA, 4096 and 1y expire (at most).

    gpg --full-generate-key

## Verify password

    gpg --export-secret-keys -a <id> > /dev/null && echo OK

## Change password

    gpg --edit-key <id>
    > passwd
    > save

## Encrypting

The fastest way is to use echo directly.

    echo "dear deer" | gpg --encrypt --armor --recipient "Roger"

Another way is to use [more] to pass the file content to [gpg].

    more message | gpg -e -a -r "Roger"

Encrypt a file:

    gpg -e -r toto -o toto.pgp toto.txt

Multiple recipients:

> GPG encrypts the file once with a symmetric key, then places a header identifying the target keypair and an encrypted version of the symmetric key. When encrypted to multiple recipients, this header is placed multiple times providing a uniquely encrypted version of the same symmetric key for each recipient.

    gpg -e -r alice@example.com -r bob@example.com clear-message.txt

## Decrypting

    echo "-----BEGIN PGP MESSAGE-----
    Version: GnuPG v1.4.12 (GNU/Linux)

    hQIMA8LsxNwWC0LgARAAhZgg+SyNc2SW2GHIclRNbl6534wnWmdrMkBK5LZXvirP
    y1jbCMjSCcGY8kzkuQZ07KfFYv7uwCDcs7RFxgYUwV1O71egLnslG+FYAAA/EY0f
    z4wJBJBtToIN/Ii83pvOvxchwKgynMyQ5/5mT8CFhuRsLrYrS/zxI7bnbAhETDep
    jh3WTVTpg6Bz/rRGqyHVADSXJdrALNzd0fZG5yP+rksgB01SljjhIIAn9vvEzpTS
    QAE26eH2Uz99e2xoRONU4Whs/+Jul9r3v8XLuGlyTIq5thLyCFqdaPQyNp7rFgPM
    tsfiPEo94XJ4z+TFqanNjG8=
    =L9Js
    -----END PGP MESSAGE-----" | gpg --decrypt

You can always export it to file by adding [> output]

    echo "encrypted message" | gpg -d > output

Decrypt a binary file:

    gpg -d <filename>
    gpg -d toto.pgp > toto.txt

## Exporting

    gpg --export -a "Roger" > public.key
    gpg --export-secret-key -a "Roger" > private.key

## Importing

    gpg --import public.key
    gpg --allow-secret-key-import --import private.key

## Deletion

    gpg --delete-key "Name"
    gpg --delete-secret-key "Name"

## Compress & Encrypt

    tar czvpf - dir/ | gpg --symmetric --cipher-algo aes256 -o backup.tar.gz.gpg

## Decrypt & Extract

    gpg -d backup.tar.gz.gpg | tar xzvf -

## Searching keys

    gpg --keyserver hkp://keys.openpgp.org/ --search-keys tasty@waffles.now

## Receiving keys

    gpg --keyserver hkp://keys.openpgp.org --recv-keys <hash>
