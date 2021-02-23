# pytesseract

    import subprocess
    driver.save_screenshot('image.png')

    with open('file.txt', 'w+') as f:
        proc = subprocess.Popen(['pytesseract', '-l', 'eng', 'image.png'], stdout=subprocess.PIPE, bufsize=1)
        for line in iter(proc.stdout.readline, b''):
            if (line.decode().strip() != ''):
                f.write(line.decode())
        proc.communicate()
