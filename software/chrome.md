# chrome

## native dark mod

1. chrome://flags

2. search Dark

3. Enable: Force Dark Mode for Web Contents

4. Enable: Web Platform Controls Dark Mode

## sqlite

### extract time from login data

    select datetime(date_created / 1000000 + (strftime('%s', '1601-01-01')), 'unixepoch', 'localtime') from logins;
