# Johnny GUI for John the Ripper: setup and first use

This guide explains what Johnny is, how to install it, and how to use it for a small, authorized password-recovery test. The examples use Ubuntu/Debian Linux; package availability and menu names vary by release.

> **Use only with permission.** Only test files and password hashes that you own or are explicitly authorized to assess. A password hash is sensitive data: keep it private and remove temporary hash files when you no longer need them.

## 1. What is Johnny?

**Johnny** is a graphical front end for **John the Ripper (JtR)**. John the Ripper does the password-recovery work; Johnny provides controls to load a password/hash file, choose an attack and wordlist, start or pause a run, and view results. Johnny therefore needs a compatible John the Ripper installation underneath it. Some file formats require the Jumbo build of John and a separate format-specific extraction utility.

For a password-protected PDF, the usual workflow is:

1. Extract the PDF's password hash into a text file with `pdf2john` (part of, or distributed with, John the Ripper Jumbo in many installations).
2. Load that hash file in Johnny.
3. Run a dictionary or other permitted recovery attempt.
4. Use the recovered password to open your own PDF and verify it.

Johnny does not decrypt a PDF directly, and a successful recovery is not guaranteed. It depends on the PDF's encryption format, the installed John build, and the strength of the password.

## 2. Install on Ubuntu or Debian

Open a terminal and install John the Ripper:

```bash
sudo apt update
sudo apt install john
```

Check whether Johnny is available in the repositories enabled for your release:

```bash
apt-cache policy johnny
```

If a candidate version is shown, install it:

```bash
sudo apt install johnny
```

If no candidate is shown, enable the appropriate official repository for your distribution and check again. Do not download an installer from an untrusted mirror. If Johnny is not packaged for your release, use the upstream Johnny project’s installation instructions, or use John the Ripper from the command line; the hash preparation and supported formats are the same. The upstream project’s exact build dependencies can differ by release.

Confirm John is installed, then launch Johnny from your desktop application menu (search for **Johnny**) or, if the package provides the command, from a terminal:

```bash
john --version
johnny
```

If the `johnny` command is not found but the desktop launcher exists, launch it from the menu. If `john --list=formats` works, John is available; PDF support and the PDF extractor may still require a Jumbo build.

### Windows and macOS

This guide’s commands are for Linux. On Windows, a practical option is an Ubuntu virtual machine or WSL with a supported Linux desktop environment; Johnny is a Linux GUI and may not work in a plain WSL terminal without GUI support. On macOS, check the upstream Johnny and John documentation for a currently supported installation method rather than assuming the Linux package instructions apply.

## 3. Prepare an authorized PDF hash

Use a copy of a PDF that you own or have permission to test. Install or obtain a compatible John the Ripper Jumbo build if your distribution’s John does not include PDF support or the helper script.

Find the extractor supplied with your installation. Common names/locations include `pdf2john.pl` and `pdf2john`:

```bash
command -v pdf2john.pl || command -v pdf2john
```

If the command is not on your `PATH`, locate it in the John Jumbo installation directory (often a `run` directory), or consult that installation’s documentation. Then create a hash file. Substitute the path to your own PDF and use the extractor name/path that exists on your system:

```bash
pdf2john.pl "$HOME/Documents/my-test.pdf" > "$HOME/my-test.hash"
```

Some builds use `pdf2john` instead:

```bash
pdf2john "$HOME/Documents/my-test.pdf" > "$HOME/my-test.hash"
```

Check that the output is non-empty before opening it:

```bash
ls -l "$HOME/my-test.hash"
head -c 160 "$HOME/my-test.hash"; echo
```

The output contains a password hash, not the document contents. Treat it as confidential. If extraction reports an unsupported PDF or produces no output, check that you used the Jumbo extractor and that the PDF is actually password protected.

## 4. Run a recovery attempt in Johnny

Names and layouts may differ slightly between Johnny versions. The toolbar in the application may use labels such as **Open password file**, **Start new attack**, **Resume attack**, and **Pause attack**.

1. Start Johnny.
2. Choose **Open password file** (or the equivalent open/load option) and select `my-test.hash`.
3. Confirm that the loaded entry is identified as a PDF hash. If it is not, stop and verify the extractor output and John format support.
4. Choose an attack. For a first, limited test, select a **wordlist/dictionary** attempt.
5. Select a wordlist that you are allowed to use. For a controlled test, you can make a tiny list of guesses for your own test file:

   ```bash
   printf '%s\n' 'example-one' 'example-two' 'your-test-password' > "$HOME/test-words.txt"
   ```

6. Set the wordlist path in Johnny’s attack options. Use the application’s default rules initially; avoid launching an unnecessarily broad or long run.
7. Start the attack and watch the status/progress and log. Pause or stop it if needed.
8. When Johnny reports a match, note the recovered password securely and verify it by opening your own PDF. Do not publish screenshots or logs that expose passwords or hashes.

A run that finds no match only means the selected candidates did not work; it does not prove that the password is unrecoverable. A strong, unique password may be impractical to recover.

## 5. Save, resume, and clean up

- Save a Johnny/John session if you want to resume a permitted test later. Session controls and default session locations vary by package.
- Keep hash files and recovered passwords in a private location with appropriate access controls.
- After the test, securely remove temporary copies if they are no longer needed. For example, remove only the specific file you created:

  ```bash
  rm -- "$HOME/my-test.hash"
  ```

  This removes the hash file, not the original PDF. Do not run cleanup commands against directories or files you have not verified.

## 6. Troubleshooting

| Problem | What to check |
| --- | --- |
| Johnny does not launch | Confirm the package installed successfully; try the desktop menu; check that your desktop environment supports the application. |
| `johnny` command is missing | The package may provide only a launcher, or Johnny may not be installed. Check the application menu and package manager. |
| PDF hash extraction command is missing | Install/use a compatible John Jumbo distribution and locate its `pdf2john.pl` or `pdf2john` helper. |
| Johnny rejects the hash file | Recreate it from the authorized PDF, ensure the output is not empty, and confirm PDF format support in the installed John build. |
| No password is found | Try a relevant, authorized wordlist and verify the PDF/hash format. Recovery is not guaranteed, especially for strong passwords. |
| Password appears in logs/screenshots | Treat it as exposed; avoid sharing the material and change the password if the document or password is still in use. |

## 7. Key terms

- **Hash extraction:** Converting protected-file metadata into a format John can test. It does not reveal the PDF contents.
- **Wordlist/dictionary attack:** Testing candidate passwords from a list, optionally with transformations.
- **Session:** Saved state that can let John/Johnny continue a previous run.
- **Jumbo:** The feature-rich John the Ripper variant that includes many additional formats and helper scripts; availability depends on how John was packaged.
